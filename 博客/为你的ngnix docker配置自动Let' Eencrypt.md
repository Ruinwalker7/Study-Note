# 为你的nginx docker配置自动Let' Eencrypt认证

这是前一篇配置 Nginx Docker 的后续内容，在这一片博客当中，会记录如何使用Let' Eencrypt给予网站https认证，以及自动更新脚本。

## 获取免费证书

1. 安装Certbot客户端

   ```shell
   apt install certbot
   ```

2. 获取证书

   ```
   certbot certonly --standalone -d example.com -d www.example.com
   ```

   > 我们的一些服务并没有根目录，例如一些微服务，这时候使用 `--webroot` 就走不通了。certbot 还有另外一种模式 `--standalone` ， 这种模式不需要指定网站根目录，他会自动启用服务器的443端口，来验证域名的归属。我们有其他服务（例如nginx）占用了443端口，就必须先停止这些服务，在证书生成完毕后，再启用。

证书生成完毕后，我们可以在 `/etc/letsencrypt/live/` 目录下看到对应域名的文件夹，里面存放了指向证书的一些**快捷方式**。但我们使用 Nginx docker 的时候，我们需要将源文件拷贝到容器的映射目录，源文件目录为`/etc/letsencrypt/archive/`。

```shell
cp -r /etc/letsencrypt/archive/* /nginx/cert/
```

## Nginx配置文件

在配置文件中添加Server内容如下：

```conf
server {
        listen 443 ssl http2;
        listen [::]:443 ssl http2;
        server_name  .atoposlibra.top;
        ssl_protocols TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers on;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
        ssl_session_tickets off;
        ssl_session_timeout 1d;
        ssl_session_cache shared:SSL:10m;
        add_header Strict-Transport-Security
            "max-age=31536000; includeSubDomains"
            always;
        ssl_certificate /cert/你的网站域名/fullchain1.pem;
        ssl_certificate_key /cert/你的网站域名/privkey1.pem; 
        location / {
            proxy_pass http://127.0.0.1:你的网站端口;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto https;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection $connection_upgrade;
        }
    }
```

## 自动更新 SSL 证书

Let' Eencrypt的认证只有90天的有效期，所以我们至少需要每隔90天就要进行一次认证，否则会导致https认证失效。

在Ubuntu上创建这样一个脚本，我们可以选择Shell脚本来实现，因为它可以直接运行终端命令。下面是一个基本的脚本示例，该脚本会停止Docker容器中的Nginx服务，更新Let's Encrypt证书，将新证书复制到指定位置，然后重启Nginx的Docker容器。此脚本使用了`certbot`来更新Let's Encrypt证书，这是一个常用的工具来自动化管理Let's Encrypt证书。

```bash
#!/bin/bash

# 定义你的Nginx Docker容器名
NGINX_CONTAINER_NAME="nginx-container"

# 定义证书复制的目标路径
CERT_DESTINATION_PATH="/nginx/cert"

# 停止Nginx容器
docker stop $NGINX_CONTAINER_NAME

# 更新Let's Encrypt证书
# 假设你的域名是example.com
certbot renew --standalone --preferred-challenges http

# 检查certbot命令是否成功执行
if [ $? -ne 0 ]; then
    echo "Certbot renewal failed."
    exit 1
fi

# 将新证书复制到容器映射文件夹
cp -r /etc/letsencrypt/archive/* $CERT_DESTINATION_PATH/

# 重启Nginx容器
docker start $NGINX_CONTAINER_NAME
```

将此脚本保存为一个文件，例如`update_nginx_cert.sh`，然后赋予它执行权限：

```bash
chmod +x update_nginx_cert.sh
```

要每两个月自动运行这个脚本，你可以使用`cron`来安排它。打开当前用户的crontab文件：

```bash
crontab -e
```

然后添加以下行来安排任务。这行命令意味着每两个月的第一天的午夜12点执行脚本：

```
0 0 1 */2 * /path/to/your/script/update_nginx_cert.sh
```

请确保替换`/path/to/your/script/update_nginx_cert.sh`为你脚本的实际路径。这样，你的系统就会每两个月自动更新Let's Encrypt证书，并且相应地更新你的Nginx Docker容器了。