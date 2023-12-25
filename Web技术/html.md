# HTML

<img src="https://www.runoob.com/wp-content/uploads/2013/06/02A7DD95-22B4-4FB9-B994-DDB5393F7F03.jpg" alt="img" style="zoom: 33%;" />

`<!DOCTYPE>` 是一种特殊的标记，位于 HTML 文档的最前面，用来告诉浏览器文档使用的是哪个 HTML 或 XHTML 规范。这个声明是用来确保你的网页在不同的浏览器中以尽可能相同的方式渲染。

**<!DOCTYPE> 的作用**

1. **指定文档类型和版本**：它告诉浏览器页面使用的是哪个 HTML/XHTML 规范，例如 HTML5 或 XHTML 1.0。
2. **确保标准模式渲染**：有助于触发浏览器的标准模式，而不是怪异模式（quirks mode）。标准模式更严格地遵循 W3C 规范，而怪异模式可能会呈现不一致的样式和行为。

**常见的 <!DOCTYPE> 声明**

- **HTML5**：`<!DOCTYPE html>`，这是当前最新版本的 HTML，简洁且适用于所有类型的 HTML 文档。
- **XHTML 1.0 Strict**：`<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">`，这种类型遵守所有 XHTML 规范，不包括展示性和废弃的元素。
- **HTML 4.01 Transitional**：`<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">`，这种类型包含所有 HTML 4.01 元素和属性，包括展示性和废弃的。

## 基础标签

### 头部标签

常用于在头部区域的元素标签有：`<title>`, `<style>`, `<meta>`,` <link>`, ` <script>`, `<noscript>` 和 `<base>`。

#### `<base>`

**HTML `<base>` 元素** 指定用于一个文档中包含的所有相对 URL 的根 URL。一份中只能有一个 `<base>` 元素。

`baseURI` 默认为 `document.location.href`。

合法的父级：任何不带有任何其他 `<base>` 元素的`<head>` 元素

使用说明

- 多个 `<base>` 元素
- 如果指定了多个 `<base>` 元素，只会使用第一个 href 和 target 值，其余都会被忽略。

页内锚

- 指向文档中某个片段的链接，例如 `<a href="#some-id">` 用 `<base>` 解析，触发对带有附加片段的基本 URL 的 HTTP 请求。

- 例如：给定 `<base href="https://example.com">`

- 以及此链接 `<a href="#anchor">Anker</a>`

- 链接指向 `https://example.com/#anchor`



#### `<link>`

HTML 外部资源链接元素 (`<link>`) 规定了当前文档与外部资源的关系。该元素最常用于链接样式表，此外也可以被用来创建站点图标 (比如 PC 端的“favicon”图标和移动设备上用以显示在主屏幕的图标) 。

`<link>` 标签通常用于链接到样式表:

```html
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```

- **href**：指定被链接文档的 URL。这是最常用的属性。`<link href="styles.css" rel="stylesheet">`

- **rel**：定义当前文档与被链接文档之间的关系（relationship）。对于样式表，通常设为 `"stylesheet"`。

- **media**：指定被链接文档将显示或用于的设备类型或媒体查询。常用于响应式设计。

  - 例如：`<link href="print.css" rel="stylesheet" media="print">`（仅在打印时应用这个样式表）

- **type**：指定被链接文档的 MIME 类型。对于 CSS 样式表，这通常是 `"text/css"`。

  - 例如：`<link href="styles.css" rel="stylesheet" type="text/css">`

- **title**：指定可选的样式表的标题。

  - 例如：`<link href="styles.css" rel="stylesheet" title="Default Style">`

- **sizes**：仅当 `rel="icon"` 时使用，指定图标的尺寸。

  `<link href="favicon.ico" rel="icon" type="image/x-icon" sizes="16x16">`

  

**type常用：**

1. **CSS**: `type="text/css"`
   - 用于链接样式表。
   - 例：`<link rel="stylesheet" type="text/css" href="style.css">`
2. **Preload Script**: `type="text/javascript"` 或 `type="application/javascript"`
   - 用于预加载 JavaScript 脚本。
   - 例：`<link rel="preload" as="script" type="text/javascript" href="script.js">`
3. **Preload Font**: `type="font/woff2"`, `type="font/woff"`, `type="application/font-woff"`
   - 用于预加载字体文件。
   - 例：`<link rel="preload" as="font" type="font/woff2" href="font.woff2" crossorigin="anonymous">`
4. **Favicon**: `type="image/x-icon"` 或 `type="image/png"`
   - 用于指定网站的 favicon（收藏夹图标）。
   - 例：`<link rel="icon" type="image/png" href="favicon.png">`
5. **Manifest**: `type="application/manifest+json"`
   - 用于指向 web 应用程序的 manifest 文件。
   - 例：`<link rel="manifest" href="/manifest.json">`



#### `<meta>`

`<meta>` 标签在 HTML 中用于提供有关页面的元信息，它不会显示在页面上，但对于搜索引擎优化（SEO）、页面的显示效果以及控制浏览器行为等方面至关重要。以下是一些常用的 `<meta>` 标签设置：

`<meta charset="UTF-8">`：声明网页使用的字符编码。

`<meta name="description" content="这是一个关于HTML学习的页面。">`：提供一个简短描述

`<meta name="keywords" content="HTML, CSS, JavaScript">`：关键字

`<meta name="author" content="John Doe">`：指明页面的作者

`<meta http-equiv="X-UA-Compatible" content="IE=edge">`：兼容性提示

`<meta http-equiv="content-language" content="en-US">`：语言



### HTML 格式化标签

HTML 使用标签 `<b>`("bold") 与 `<i>`("italic") 对输出的文本进行格式, 如：**粗体** or *斜体*

**通常标签 `<strong>` 替换加粗标签 `<b>` 来使用, `<em>` 替换 `<i>`标签使用。**

```html
<big>这个文本字体放大</big><em>这个文本是斜体的</em><i>这个文本是斜体的</i><small>这个文本是缩小的</small>
这个文本包含
<sub>下标</sub>
这个文本包含
<sup>上标</sup><del>删除字</del>
```

<big>这个文本字体放大</big>
<em>这个文本是斜体的</em>
<i>这个文本是斜体的</i>
<small>这个文本是缩小的</small>
这个文本包含
<sub>下标</sub>
这个文本包含
<sup>上标</sup>
<del>删除字</del>

```html
<h1>标题</h1>
<p>段落</p>
<a href="www.baidu.com">链接</a>
<img decoding="async" src="/images/logo.png" width="258" height="39" />  <!-- 图片 -->
<hr><!--水平线-->
<br> <!--换行-->
```

### 计算机输出标签

```html
<code>代码</code>
<var>变量</var>
<pre>预格式文本</pre>   <!-- 文本啥样就啥样-->
```



### 引文, 引用, 及标签定义

```html
<p><bdo dir="rtl">该段落文字从右到左显示。</bdo></p>  
```

<p>该段落文字从左到右显示。</p>  
<p><bdo dir="rtl">该段落文字从右到左显示。</bdo></p>  

#### 地址

```html
<address>Written by <a href="mailto:webmaster@example.com">Jon Doe</a>.<br> 
Visit us at:<br></address>
```

<address>
Written by <a href="mailto:webmaster@example.com">Jon Doe</a>.<br> 
Visit us at:<br>
</address>


### 链接

```html
<a href="https://www.runoob.com/" target="_blank" rel="noopener noreferrer">访问菜鸟教程!</a>
```

如果你将 target 属性设置为 _blank, 链接将在新窗口打开。

[`target`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a#target)

该属性指定在何处显示链接的 URL，作为*浏览上下文*的名称（标签、窗口或 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe)）。以下关键词对加载 URL 的位置有特殊含义：

- `_self`：当前页面加载。（默认）
- `_blank`：通常在新标签页打开，但用户可以通过配置选择在新窗口打开。
- `_parent`：当前浏览环境的父级浏览上下文。如果没有父级框架，行为与 `_self` 相同。
- `_top`：最顶级的浏览上下文（当前浏览上下文中最“高”的祖先）。如果没有祖先，行为与 `_self` 相同。

[`rel`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a#rel)

该属性指定了目标对象到链接对象的关系。



#### name 属性

name 属性规定锚（anchor）的名称。

您可以使用 name 属性创建 HTML 页面中的书签。

书签不会以任何特殊方式显示，它对读者是不可见的。

当使用命名锚（named anchors）时，我们可以创建直接跳至该命名锚（比如页面中某个小节）的链接，这样使用者就无需不停地滚动页面来寻找他们需要的信息了。

##### 命名锚的语法：

```
<a name="label">锚（显示在页面上的文本）</a>
```

**提示：**锚的名称可以是任何你喜欢的名字。

**提示：**您可以使用 id 属性来替代 name 属性，命名锚同样有效。

##### 实例

首先，我们在 HTML 文档中对锚进行命名（创建一个书签）：

```
<a name="tips">基本的注意事项 - 有用的提示</a>
```

然后，我们在同一个文档中创建指向该锚的链接：

```
<a href="#tips">有用的提示</a>
```

您也可以在其他页面中创建指向该锚的链接：

```
<a href="http://www.w3school.com.cn/html/html_links.asp#tips">有用的提示</a>
```

在上面的代码中，我们将 # 符号和锚名称添加到 URL 的末端，就可以直接链接到 tips 这个命名锚了。

##### 表格

HTML 表格由 `<table>` 标签来定义。

- **tr**：tr 是 table row 的缩写，表示表格的一行。
- **td**：td 是 table data 的缩写，表示表格的数据单元格。
- **th**：th 是 table header的缩写，表示表格的表头单元格。

1. 一个可选的`<captive>`
2. 0或多个`<colgroup>`
3. 一个可选的`<thead>`
4. 下列任意一个：
   1. 零个或多个`<tbody>`
   2. 零个或多个`<tr>`

### 列表

```html
<ul>
<li>Coffee</li>
<li>Milk</li>
</ul>
```

<ul>
<li>Coffee</li>
<li>Milk</li>
</ul>
type：

**disc:** 实心圆

square：实心正方形

circle：空心圆


```html
<h4>嵌套列表：</h4>
<ul>
  <li>Coffee</li>
  <li>Tea
    <ul>
      <li>Black tea</li>
      <li>Green tea
        <ul>
          <li>China</li>
          <li>Africa</li>
        </ul>
      </li>
    </ul>
  </li>
  <li>Milk</li>
</ul>
```



### 表单

HTML 表单用于收集用户的输入信息，此区域包含交互控件，将用户收集到的信息发送到 Web 服务器。

表单元素是允许用户在表单中输入内容，比如：文本域（textarea）、下拉列表（select）、单选框（radio-buttons）、复选框（checkbox） 等等。

表单元素的类型时使用`type`指定的



#### `<form>`

`action`：处理表单提交的 URL。这个值可被 `<button>、<input type="submit"> 或 <input type="image"> `元素上的 formaction 属性覆盖。

[`method`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/form#method)

浏览器使用这种HTTP方式来提交 表单。可能的值有：

- `post`：表单数据会包含在表单体内然后发送给服务器。
- `get`：表单数据会附加在 `action` 属性的 URL 中，并以 '?' 作为分隔符，[没有副作用](https://developer.mozilla.org/zh-CN/docs/Glossary/Idempotent) 时使用这个方法。
- `dialog`：如果表单在 `dialog`元素中，提交时关闭对话框。



#### `<input>`

##### `<input type="button">`

> 备注： button 类型的 `<input> `元素仍然是合法的 HTML 代码，但是新的 `<button>` 元素是创建按钮的更好的方式。鉴于 `<button>` 的标签文字可以插入至开闭标签之间，你可以在标签中包含 HTML 代码，甚至是图像。

###### 属性

`value`：设置按钮中的文字

###### 使用

`<input type="button">` 元素没有默认行为（与之类似的 `<input type="submit">` 和 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/input/reset) 分别用于提交和重置表单）。要让按钮做任何事情，你必须编写 JavaScript 代码

```js
const button = document.querySelector("input");
const paragraph = document.querySelector("p");

button.addEventListener("click", updateButton);

function updateButton() {
  if (button.value === "开动机器") {
    button.value = "停止机器";
    paragraph.textContent = "机器启动了！";
  } else {
    button.value = "开动机器";
    paragraph.textContent = "机器已经停下了。";
  }
}
```

##### `<input type="text"> `

需要输入文本的时候，默认20字符

##### `<input type="password">`

使用星号代替字符

##### `<input type="checkbox">`

属性有name、value

#### `<textarea>`

```html
<textarea rows="10" cols="30">
我是一个文本框。
</textarea>
```

rows：列数

cols：行数

#### `<select>`

```html
   <select id="country" name="country">
        <option value="cn">CN</option>
        <option value="usa">USA</option>
        <option value="uk">UK</option>
    </select>
```

#### `<label>`

`for` 属性规定标签绑定到哪个表单元素。



### 图片

#### `<img>` 

```html
<img
  class="fit-picture"
  src="/media/cc0-images/grapefruit-slice-332-332.jpg"
  alt="Grapefruit slice atop a pile of other slices" />
```

- `src` 属性是**必须的**，它包含了你想嵌入的图片的路径。

- `alt` 属性包含一条对图像的文本描述，这不是强制性的，但对无障碍而言，它**难以置信地有用**——屏幕阅读器会将这些描述读给需要使用阅读器的使用者听，让他们知道图像的含义。如果由于某种原因无法加载图像，普通浏览器也会在页面上显示 `alt` 属性中的备用文本：例如，网络错误、内容被屏蔽或链接过期。

- [`height`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img#height)

  图像的固有高度，以像素为单位。必须是没有单位的整数值。

  > **备注：** 同时包括 `height` 和 [`width`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img#attr-width) 使浏览器在加载图像之前计算图像的长宽比。此长宽比用于保留显示图像所需的空间，减少甚至防止在下载图像并将其绘制到屏幕上时布局的偏移。减少布局偏移是良好用户体验和 Web 性能的主要组成部分。



# HTTP

**HTTP 是一个在计算机世界里专门在「两点」之间「传输」文字、图片、音频、视频等「超文本」数据的「约定和规范」。**

**简单、灵活和易于扩展、应用广泛和跨平台**

**1. 简单**
HTTP 基本的报文格式就是 header + body，头部信息也是 key-value 简单文本的形式，易于理解，降低了学习和使用的门槛。

**2. 灵活和易于扩展**
HTTP协议里的各类请求方法、URI/URL、状态码、头字段等每个组成要求都没有被固定死，都允许开发人员自定义和扩充。

同时 HTTP 由于是工作在应用层（ OSI 第七层），则它下层可以随意变化。
HTTPS 也就是在 HTTP 与 TCP 层之间增加了 SSL/TLS 安全传输层，HTTP/3 甚至把 TCPP 层换成了基于 UDP 的 QUIC。

**3. 应用广泛和跨平台**



## 简介

标准形式：[协议类型]://[服务器地址]:[端口号]/[资源层级UNIX文件路径] [文件名]?[查詢]#[片段ID]

**HTTP协议：HyperText Transfer Protocol (超文本传输协议)**  **TCP协议**

**HTTP 三点注意事项：**

- HTTP 是无连接：无连接的含义是限制每次连接只处理一个请求，服务器处理完客户的请求，并收到客户的应答后，即断开连接，采用这种方式可以节省传输时间。
- HTTP 是媒体独立的：这意味着，只要客户端和服务器知道如何处理的数据内容，任何类型的数据都可以通过HTTP发送，客户端以及服务器指定使用适合的 MIME-type 内容类型。
- HTTP 是无状态：HTTP 协议是无状态协议，无状态是指协议对于事务处理没有记忆能力，缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大，另一方面，在服务器不需要先前信息时它的应答就较快。

**HTTPS协议**：HyperText Transfer Protocol over Secure Socket Layer

HTTP的安全版，HTTPS 的主要作用是在不安全的网络上**创建一个安全信道**，并可在使用适当的加密包和服务器证书可被验证且可被信任时，对窃听和中间人攻击提供合理的防护。

浏览器显示的内容都有 HTML、XML、GIF、Flash 等，浏览器是通过 MIME Type 区分它们

> **注释：**MIME Type 是该资源的媒体类型，MIME Type 不是个人指定的，是经过互联网（IETF）组织协商，以 RFC（是一系列以编号排定的文件，几乎所有的互联网标准都有收录在其中） 的形式作为建议的标准发布在网上的，大多数的 Web 服务器和用户代理都会支持这个规范。

媒体类型通常通过 HTTP 协议，由 Web 服务器告知浏览器的，通过 Content-Type 来表示的。例如：**Content-Type：text/HTML**。

通常只有一些在互联网上获得广泛应用的格式才会获得一个 **MIME Type**，如果是某个客户端自己定义的格式，一般只能以 **application/x-** 开头。

### URI 的组成

一个典型的 URI 可以分为以下几个部分：

1. **方案（Scheme）**：指定访问资源所使用的协议，如 `http`、`https`、`ftp`、`mailto` 等。
2. **授权机构（Authority）**：通常包括认证信息、服务器地址和端口号。
3. **路径（Path）**：资源在服务器上的具体位置。
4. **查询（Query）**：以 `?` 开始，后面跟随一系列的参数，用于提供额外的指令。
5. **片段（Fragment）**：以 `#` 开始，通常用于指向网页内的某个部分。



## HTTP 消息结构

- *客户端/服务端（C/S）的架构模型*
- *无状态的请求/响应协议*

请求：

<img src="assets/2012072810301161.png" alt="img" style="zoom:150%;" />

响应消息：

![img](assets/httpmessage.jpg)



### HTTP 协议的 8 种请求类型介绍

HTTP 协议中共定义了八种方法或者叫“动作”来表明对 Request-URI 指定的资源的不同操作方式，具体介绍如下：

-  **OPTIONS**：返回服务器针对特定资源所支持的HTTP请求方法。也可以利用向Web服务器发送'*'的请求来测试服务器的功能性。
-  **HEAD**：向服务器索要与GET请求相一致的响应，只不过响应体将不会被返回。这一方法可以在不必传输整个响应内容的情况下，就可以获取包含在响应消息头中的元信息。
-  **GET**：向特定的资源发出请求。
-  **POST**：向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的创建和/或已有资源的修改。
-  **PUT**：向指定资源位置上传其最新内容。
-  **DELETE**：请求服务器删除 Request-URI 所标识的资源。
-  **TRACE**：回显服务器收到的请求，主要用于测试或诊断。
-  **CONNECT**：HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器。

虽然 HTTP 的请求方式有 8 种，但是我们在实际应用中常用的也就是 **get** 和 **post**

## HTTP 响应状态行

状态行也由三部分组成，包括HTTP协议的版本，状态码，以及对状态码的文本描述。例如：

```text
HTTP/1.1 200 OK （CRLF）
```



## HTTP 响应头信息

| 应答头           | 说明                                                         |
| :--------------- | :----------------------------------------------------------- |
| Allow            | 服务器支持哪些请求方法（如GET、POST等）。                    |
| Content-Encoding | 文档的编码（Encode）方法。只有在解码之后才可以得到Content-Type头指定的内容类型。利用gzip压缩文档能够显著地减少HTML文档的下载时间。Java的GZIPOutputStream可以很方便地进行gzip压缩，但只有Unix上的Netscape和Windows上的IE 4、IE 5才支持它。因此，Servlet应该通过查看Accept-Encoding头（即request.getHeader("Accept-Encoding")）检查浏览器是否支持gzip，为支持gzip的浏览器返回经gzip压缩的HTML页面，为其他浏览器返回普通页面。 |
| Content-Length   | 表示内容长度。只有当浏览器使用持久HTTP连接时才需要这个数据。如果你想要利用持久连接的优势，可以把输出文档写入 ByteArrayOutputStream，完成后查看其大小，然后把该值放入Content-Length头，最后通过byteArrayStream.writeTo(response.getOutputStream()发送内容。 |
| Content-Type     | 表示后面的文档属于什么MIME类型。Servlet默认为text/plain，但通常需要显式地指定为text/html。由于经常要设置Content-Type，因此HttpServletResponse提供了一个专用的方法setContentType。 |
| Date             | 当前的GMT时间。你可以用setDateHeader来设置这个头以避免转换时间格式的麻烦。 |
| Expires          | 应该在什么时候认为文档已经过期，从而不再缓存它？             |
| Last-Modified    | 文档的最后改动时间。客户可以通过If-Modified-Since请求头提供一个日期，该请求将被视为一个条件GET，只有改动时间迟于指定时间的文档才会返回，否则返回一个304（Not Modified）状态。Last-Modified也可用setDateHeader方法来设置。 |
| Location         | 表示客户应当到哪里去提取文档。Location通常不是直接设置的，而是通过HttpServletResponse的sendRedirect方法，该方法同时设置状态代码为302。 |
| Refresh          | 表示浏览器应该在多少时间之后刷新文档，以秒计。除了刷新当前文档之外，你还可以通过setHeader("Refresh", "5; URL=http://host/path")让浏览器读取指定的页面。 注意这种功能通常是通过设置HTML页面HEAD区的＜META HTTP-EQUIV="Refresh" CONTENT="5;URL=http://host/path"＞实现，这是因为，自动刷新或重定向对于那些不能使用CGI或Servlet的HTML编写者十分重要。但是，对于Servlet来说，直接设置Refresh头更加方便。  注意Refresh的意义是"N秒之后刷新本页面或访问指定页面"，而不是"每隔N秒刷新本页面或访问指定页面"。因此，连续刷新要求每次都发送一个Refresh头，而发送204状态代码则可以阻止浏览器继续刷新，不管是使用Refresh头还是＜META HTTP-EQUIV="Refresh" ...＞。  注意Refresh头不属于HTTP 1.1正式规范的一部分，而是一个扩展，但Netscape和IE都支持它。 |
| Server           | 服务器名字。Servlet一般不设置这个值，而是由Web服务器自己设置。 |
| Set-Cookie       | 设置和页面关联的Cookie。Servlet不应使用response.setHeader("Set-Cookie", ...)，而是应使用HttpServletResponse提供的专用方法addCookie。参见下文有关Cookie设置的讨论。 |
| WWW-Authenticate | 客户应该在Authorization头中提供什么类型的授权信息？在包含401（Unauthorized）状态行的应答中这个头是必需的。例如，response.setHeader("WWW-Authenticate", "BASIC realm=＼"executives＼"")。 注意Servlet一般不进行这方面的处理，而是让Web服务器的专门机制来控制受密码保护页面的访问（例如.htaccess）。 |



## HTTP 状态码

**HTTP Status Code**。

HTTP 状态码由三个十进制数字组成，第一个十进制数字定义了状态码的类型。响应分为五类：信息响应(100–199)，成功响应(200–299)，重定向(300–399)，客户端错误(400–499)和服务器错误 (500–599)：

| 分类 | 分类描述                                       |
| :--- | :--------------------------------------------- |
| 1**  | 信息，服务器收到请求，需要请求者继续执行操作   |
| 2**  | 成功，操作被成功接收并处理                     |
| 3**  | 重定向，需要进一步的操作以完成请求             |
| 4**  | 客户端错误，请求包含语法错误或无法完成请求     |
| 5**  | 服务器错误，服务器在处理请求的过程中发生了错误 |



HTTP状态码列表:

| 状态码 | 状态码英文名称                  | 中文描述                                                     |
| :----- | :------------------------------ | :----------------------------------------------------------- |
| 100    | Continue                        | 继续。客户端应继续其请求                                     |
| 101    | Switching Protocols             | 切换协议。服务器根据客户端的请求切换协议。只能切换到更高级的协议，例如，切换到HTTP的新版本协议 |
| 200    | OK                              | 请求成功。一般用于GET与POST请求                              |
| 201    | Created                         | 已创建。成功请求并创建了新的资源                             |
| 202    | Accepted                        | 已接受。已经接受请求，但未处理完成                           |
| 203    | Non-Authoritative Information   | 非授权信息。请求成功。但返回的meta信息不在原始的服务器，而是一个副本 |
| 204    | No Content                      | 无内容。服务器成功处理，但未返回内容。在未更新网页的情况下，可确保浏览器继续显示当前文档 |
| 205    | Reset Content                   | 重置内容。服务器处理成功，用户终端（例如：浏览器）应重置文档视图。可通过此返回码清除浏览器的表单域 |
| 206    | Partial Content                 | 部分内容。服务器成功处理了部分GET请求                        |
| 300    | Multiple Choices                | 多种选择。请求的资源可包括多个位置，相应可返回一个资源特征与地址的列表用于用户终端（例如：浏览器）选择 |
| 301    | Moved Permanently               | 永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替 |
| 302    | Found                           | 临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI |
| 303    | See Other                       | 查看其它地址。与301类似。使用GET和POST请求查看               |
| 304    | Not Modified                    | 未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源 |
| 305    | Use Proxy                       | 使用代理。所请求的资源必须通过代理访问                       |
| 306    | Unused                          | 已经被废弃的HTTP状态码                                       |
| 307    | Temporary Redirect              | 临时重定向。与302类似。使用GET请求重定向                     |
| 400    | Bad Request                     | 客户端请求的语法错误，服务器无法理解                         |
| 401    | Unauthorized                    | 请求要求用户的身份认证                                       |
| 402    | Payment Required                | 保留，将来使用                                               |
| 403    | Forbidden                       | 服务器理解请求客户端的请求，但是拒绝执行此请求               |
| 404    | Not Found                       | 服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面 |
| 405    | Method Not Allowed              | 客户端请求中的方法被禁止                                     |
| 406    | Not Acceptable                  | 服务器无法根据客户端请求的内容特性完成请求                   |
| 407    | Proxy Authentication Required   | 请求要求代理的身份认证，与401类似，但请求者应当使用代理进行授权 |
| 408    | Request Time-out                | 服务器等待客户端发送的请求时间过长，超时                     |
| 409    | Conflict                        | 服务器完成客户端的 PUT 请求时可能返回此代码，服务器处理请求时发生了冲突 |
| 410    | Gone                            | 客户端请求的资源已经不存在。410不同于404，如果资源以前有现在被永久删除了可使用410代码，网站设计人员可通过301代码指定资源的新位置 |
| 411    | Length Required                 | 服务器无法处理客户端发送的不带Content-Length的请求信息       |
| 412    | Precondition Failed             | 客户端请求信息的先决条件错误                                 |
| 413    | Request Entity Too Large        | 由于请求的实体过大，服务器无法处理，因此拒绝请求。为防止客户端的连续请求，服务器可能会关闭连接。如果只是服务器暂时无法处理，则会包含一个Retry-After的响应信息 |
| 414    | Request-URI Too Large           | 请求的URI过长（URI通常为网址），服务器无法处理               |
| 415    | Unsupported Media Type          | 服务器无法处理请求附带的媒体格式                             |
| 416    | Requested range not satisfiable | 客户端请求的范围无效                                         |
| 417    | Expectation Failed              | 服务器无法满足Expect的请求头信息                             |
| 500    | Internal Server Error           | 服务器内部错误，无法完成请求                                 |
| 501    | Not Implemented                 | 服务器不支持请求的功能，无法完成请求                         |
| 502    | Bad Gateway                     | 作为网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到了一个无效的响应 |
| 503    | Service Unavailable             | 由于超载或系统维护，服务器暂时的无法处理客户端的请求。延时的长度可包含在服务器的Retry-After头信息中 |
| 504    | Gateway Time-out                | 充当网关或代理的服务器，未及时从远端服务器获取请求           |
| 505    | HTTP Version not supported      | 服务器不支持请求的HTTP协议的版本，无法完成处理               |



## HTTP 的发展和历史

**HTTP**（HyperText Transfer Protocol）是万维网（World Wide Web）的基础协议。自 Tim Berners-Lee 博士和他的团队在 1989-1991 年间创造出它以来，HTTP 已经发生了太多的变化，在保持协议简单性的同时，不断扩展其灵活性。如今，HTTP 已经从一个只在实验室之间交换文件的早期协议进化到了可以传输图片，高分辨率视频和 3D 效果的现代复杂互联网协议。

### 万维网的发明

1989 年，当时在 CERN 工作的 Tim Berners-Lee 博士写了一份关于建立一个通过网络传输超文本系统的报告。这个系统起初被命名为 *Mesh*，在随后的 1990 年项目实施期间被更名为*万维网*（World Wide Web）。它在现有的 TCP 和 IP 协议基础之上建立，由四个部分组成：

- 一个用来表示超文本文档的文本格式，*超文本标记语言*（HTML）。
- 一个用来交换超文本文档的简单协议，超文本传输协议（HTTP）。
- 一个显示（以及编辑）超文本文档的客户端，即网络浏览器。第一个网络浏览器被称为 *WorldWideWeb。*
- 一个服务器用于提供可访问的文档，即 *httpd* 的前身。

HTTP 在应用的早期阶段非常简单，后来被称为 HTTP/0.9，有时也叫做单行（one-line）协议。

### HTTP/0.9——单行协议

最初版本的 HTTP 协议并没有版本号，后来它的版本号被定位在 0.9 以区分后来的版本。HTTP/0.9 极其简单：请求由单行指令构成，以唯一可用方法 [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 开头，其后跟目标资源的路径（一旦连接到服务器，协议、服务器、端口号这些都不是必须的）。

```HTTP
GET /mypage.html
```

响应也极其简单的：只包含响应文档本身。

```html
<html>
  这是一个非常简单的 HTML 页面
</html>
```

跟后来的版本不同，HTTP/0.9 的响应内容并不包含 HTTP 头。这意味着只有 HTML 文件可以传送，无法传输其他类型的文件。也没有状态码或错误代码。一旦出现问题，一个特殊的包含问题描述信息的 HTML 文件将被发回，供人们查看。

### HTTP/1.0——构建可扩展性

由于 HTTP/0.9 协议的应用十分有限，浏览器和服务器迅速扩展内容使其用途更广：

- 协议版本信息现在会随着每个请求发送（`HTTP/1.0` 被追加到了 `GET` 行）。
- 状态码会在响应开始时发送，使浏览器能了解请求执行成功或失败，并相应调整行为（如更新或使用本地缓存）。
- 引入了 HTTP 标头的概念，无论是对于请求还是响应，允许传输元数据，使协议变得非常灵活，更具扩展性。
- 在新 HTTP 标头的帮助下，具备了传输除纯文本 HTML 文件以外其他类型文档的能力（凭借 [`Content-Type`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Type) 标头）。

一个典型的请求看起来就像这样：

```HTTP
GET /mypage.html HTTP/1.0
User-Agent: NCSA_Mosaic/2.0 (Windows 3.1)

200 OK
Date: Tue, 15 Nov 1994 08:12:31 GMT
Server: CERN/3.0 libwww/2.17
Content-Type: text/html
<HTML>
一个包含图片的页面
  <IMG SRC="/myimage.gif">
</HTML>
```

接下来是第二个连接，请求获取图片（并具有相同的响应）：

```HTTP
GET /myimage.gif HTTP/1.0
User-Agent: NCSA_Mosaic/2.0 (Windows 3.1)

200 OK
Date: Tue, 15 Nov 1994 08:12:32 GMT
Server: CERN/3.0 libwww/2.17
Content-Type: text/gif
(这里是图片内容)
```

在 1991-1995 年，这些新扩展并没有被引入到标准中以促进协助工作，而仅仅作为一种尝试。服务器和浏览器添加这些新扩展功能，但出现了大量的互操作问题。直到 1996 年 11 月，为了解决这些问题，一份新文档（RFC 1945）被发表出来，用以描述如何操作实践这些新扩展功能。

### HTTP/1.1——标准化的协议

HTTP/1.0 多种不同的实现方式在实际运用中显得有些混乱。自 1995 年开始，即 HTTP/1.0 文档发布的下一年，就开始修订 HTTP 的第一个标准化版本。在 1997 年初，HTTP1.1 标准发布。

HTTP/1.1 消除了大量歧义内容并引入了多项改进：

- 连接可以复用，节省了多次打开 TCP 连接加载网页文档资源的时间。
- 增加管线化技术，允许在第一个应答被完全发送之前就发送第二个请求，以降低通信延迟。
- 支持响应分块。
- 引入额外的缓存控制机制。
- 引入内容协商机制，包括语言、编码、类型等。并允许客户端和服务器之间约定以最合适的内容进行交换。
- 凭借 [`Host`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Host) 标头，能够使不同域名配置在同一个 IP 地址的服务器上。

一个典型的请求流程，所有请求都通过一个连接实现，看起来就像这样：

```http
GET /zh-CN/docs/Glossary/Simple_header HTTP/1.1
Host: developer.mozilla.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://developer.mozilla.org/zh-CN/docs/Glossary/Simple_header

200 OK
Connection: Keep-Alive
Content-Encoding: gzip
Content-Type: text/html; charset=utf-8
Date: Wed, 20 Jul 2016 10:55:30 GMT
Etag: "547fa7e369ef56031dd3bff2ace9fc0832eb251a"
Keep-Alive: timeout=5, max=1000
Last-Modified: Tue, 19 Jul 2016 00:59:33 GMT
Server: Apache
Transfer-Encoding: chunked
Vary: Cookie, Accept-Encoding

(content)

GET /static/img/header-background.png HTTP/1.1
Host: developer.mozilla.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://developer.mozilla.org/zh-CN/docs/Glossary/Simple_header

200 OK
Age: 9578461
Cache-Control: public, max-age=315360000
Connection: keep-alive
Content-Length: 3077
Content-Type: image/png
Date: Thu, 31 Mar 2016 13:34:46 GMT
Last-Modified: Wed, 21 Oct 2015 18:27:50 GMT
Server: Apache

(image content of 3077 bytes)
```

### 超过 15 年的扩展

由于 HTTP 协议的可扩展性使得创建新的头部和方法是很容易的。即使 HTTP/1.1 协议进行过两次修订，已经稳定使用超过 15 年了。

#### HTTP 用于安全传输

HTTP 最大的变化发生在 1994 年底。HTTP 在基本的 TCP/IP 协议栈上发送信息，网景公司在此基础上创建了一个额外的加密传输层：SSL。SSL 1.0 没有在公司以外发布过，但 SSL 2.0 及其后继者 SSL 3.0 允许通过加密来保证服务器和客户端之间交换消息的真实性，来创建电子商务网站。SSL 在标准化道路上最终成为了 TLS。

与此同时，人们对一个加密传输层的需求也愈发高涨：因为 Web 最早几乎是一个学术网络，相对信任度很高，但如今不得不面对一个险恶的丛林：广告客户、随机的个人或者犯罪分子争相劫取个人信息，将信息占为己有，甚至改动将要被传输的数据。随着通过 HTTP 构建的应用程序变得越来越强大，可以访问越来越多的私人信息，如地址簿、电子邮件或用户的地理位置，即使在电子商务使用之外，对 TLS 的需求也变得普遍。

#### HTTP 用于复杂应用

大约 1996 年，HTTP 被扩展到允许创作，并且创建了一个名为 WebDAV 的标准。它进一步扩展了某些特定的应用程序，如 CardDAV 用来处理地址簿条。但所有这些 DAV 扩展有一个缺陷：它们必须由要使用的服务器来实现，这是非常复杂的。并且他们在网络领域的使用必须保密。

在 2000 年，一种新的使用 HTTP 的模式被设计出来：[具象状态传输（representational state transfer）](https://developer.mozilla.org/zh-CN/docs/Glossary/REST) (或者说 REST)。由 API 发起的操作不再通过新的 HTTP 方法传达，而只能通过使用基本的 HTTP / 1.1 方法访问特定的 URI。这允许任何 Web 应用程序通过提供 API 以允许查看和修改其数据，而无需更新浏览器或服务器。所有需要的内容都被嵌入到由网站通过标准 HTTP/1.1 提供的文件中。REST 模型的缺点在于每个网站都定义了自己的非标准 RESTful API，并对其进行了全面的控制。不同于 DAV 扩展，客户端和服务器是可互操作的。RESTful API 在 2010 年变得非常流行。

自 2005 年以来，可用于 Web 页面的 API 大大增加，其中几个 API 为特定目的扩展了 HTTP 协议，大部分是新的特定 HTTP 头：

- [Server-sent events](https://developer.mozilla.org/zh-CN/docs/Web/API/Server-sent_events)，服务器可以偶尔推送消息到浏览器。
- [WebSocket](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSockets_API)，一个新协议，可以通过升级现有 HTTP 协议来建立。

### HTTP/2——为了更优异的表现

网页愈渐变得的复杂，甚至演变成了独有的应用，可见媒体的播放量，增进交互的脚本大小也增加了许多：更多的数据通过 HTTP 请求被传输。HTTP/1.1 链接需要请求以正确的顺序发送，理论上可以用一些并行的链接，带来的成本和复杂性堪忧。比如，HTTP 管线化（pipelining）就成为了 Web 开发的负担。为此，在 2010 年早期，谷歌通过实践了一个实验性的 SPDY 协议。这种在客户端和服务器端交换数据的替代方案引起了在浏览器和服务器上工作的开发人员的兴趣。明确了响应数量的增加和解决复杂的数据传输，SPDY 成为了 HTTP/2 协议的基础。

HTTP/2 在 HTTP/1.1 有几处基本的不同：

- HTTP/2 是二进制协议而不是文本协议。不再可读，也不可无障碍的手动创建，改善的优化技术现在可被实施。
- 这是一个多路复用协议。并行的请求能在同一个链接中处理，移除了 HTTP/1.x 中顺序和阻塞的约束。
- 压缩了标头。因为标头在一系列请求中常常是相似的，其移除了重复和传输重复数据的成本。
- 其允许服务器在客户端缓存中填充数据，通过一个叫服务器推送的机制来提前请求。

在 2015 年 5 月正式标准化后，HTTP/2 取得了极大的成功，在 2022 年 1 月达到峰值，占所有网站的 46.9%。高流量的站点最迅速的普及，在数据传输上节省了可观的成本和支出。

这种迅速的普及率很可能是因为 HTTP2 不需要站点和应用做出改变：使用 HTTP/1.1 和 HTTP/2 对他们来说是透明的。拥有一个最新的服务器和新点的浏览器进行交互就足够了。只有一小部分群体需要做出改变，而且随着陈旧的浏览器和服务器的更新，而不需 Web 开发者做什么，用的人自然就增加了。

### 后 HTTP/2 进化

随着 HTTP/2.的发布，就像先前的 HTTP/1.x 一样，HTTP 没有停止进化，HTTP 的扩展性依然被用来添加新的功能。特别的，我们能列举出 2016 年里 HTTP 的新扩展：

- 对 Alt-Svc 的支持允许了给定资源的位置和资源鉴定，允许了更智能的 CDN 缓冲机制。
- 客户端提示（client hint） 的引入允许浏览器或者客户端来主动交流它的需求，或者是硬件约束的信息给服务端。
- 在 Cookie 头中引入安全相关的的前缀，现在帮助保证一个安全的 Cookie 没被更改过。

### HTTP/3——基于 QUIC 的 HTTP

HTTP 的下一个主要版本，HTTP/3 有着与 HTTP 早期版本的相同语义，但在传输层部分使用 [QUIC (en-US)](https://developer.mozilla.org/en-US/docs/Glossary/QUIC) 而不是 TCP。到 2022 年 10 月，26% 的网站正在使用 HTTP/3。

QUIC 旨在为 HTTP 连接设计更低的延迟。类似于 HTTP/2，它是一个多路复用协议，但是 HTTP/2 通过单个 TCP 连接运行，所以在 TCP 层处理的数据包丢失检测和重传可以阻止所有流。QUIC 通过 UDP 运行多个流，并为每个流独立实现数据包丢失检测和重传，因此如果发生错误，只有该数据包中包含数据的流才会被阻止。

### 总结

**HTTP/1.1、HTTP/2、HTTP/3 演变**

说说 HTTP/1.1 相比 HTTP/1.0 提高了什么性能？
HTTP/1.1 相比 HTTP/1.0 性能上的改进：

- 使用 TCP 长连接的方式改善了 HTTP/1.0 短连接造成的性能开销。
- 支持 管道（pipeline）网络传输，只要第一个请求发出去了，不必等其回来，就可以发第二个请求出去，可以减少整体的响应时间。

但 HTTP/1.1 还是有性能瓶颈：

- 请求 / 响应头部（Header）未经压缩就发送，首部信息越多延迟越大。只能压缩 Body 的部分；
- 发送冗长的首部。每次互相发送相同的首部造成的浪费较多；
- 服务器是按请求的顺序响应的，如果服务器响应慢，会招致客户端一直请求不到数据，也就是队头阻塞；
- 没有请求优先级控制；
- 请求只能从客户端开始，服务器只能被动响应。

那上面的 HTTP/1.1 的性能瓶颈，HTTP/2 做了什么优化？
HTTP/2 协议是基于 HTTPS 的，所以 HTTP/2 的安全性也是有保障的。
那 HTTP/2 相比 HTTP/1.1 性能上的改进：

**1. 头部压缩**
HTTP/2 会压缩头（Header）如果你同时发出多个请求，他们的头是一样的或是相似的，那么，协议会帮你消除重复的分。
这就是所谓的 HPACK 算法：在客户端和服务器同时维护一张头信息表，所有字段都会存入这个表，生成一个索引号，以后就不发送同样字段了，只发送索引号，这样就提高速度了。

**2. 二进制格式**
HTTP/2 不再像 HTTP/1.1 里的纯文本形式的报文，而是全面采用了二进制格式。
头信息和数据体都是二进制，并且统称为帧（frame）：头信息帧和数据帧。

![img](https://pic1.zhimg.com/80/v2-6ea02bd0dea7eae3fee729897df02f94_720w.webp)

这样虽然对人不友好，但是对计算机非常友好，因为计算机只懂二进制，那么收到报文后，无需再将明文的报文转成二进制，而是直接解析二进制报文，这增加了数据传输的效率。

**3. 数据流**

HTTP/2 的数据包不是按顺序发送的，同一个连接里面连续的数据包，可能属于不同的回应。因此，必须要对数据包做标记，指出它属于哪个回应。
每个请求或回应的所有数据包，称为一个数据流（Stream）。
每个数据流都标记着一个独一无二的编号，其中规定客户端发出的数据流编号为奇数， 服务器发出的数据流编号为偶数
客户端还可以指定数据流的优先级。优先级高的请求，服务器就先响应该请求。

![img](https://pic4.zhimg.com/80/v2-bad1522712684f79322c1bd50133b617_720w.webp)

**4. 多路复用**

HTTP/2 是可以在一个连接中并发多个请求或回应，而不用按照顺序一一对应。
移除了 HTTP/1.1 中的串行请求，不需要排队等待，也就不会再出现「队头阻塞」问题，降低了延迟，大幅度提高了连接的利用率。

举例来说，在一个 TCP 连接里，服务器收到了客户端 A 和 B 的两个请求，如果发现 A 处理过程非常耗时，于是就回应 A 请求已经处理好的部分，接着回应 B 请求，完成后，再回应 A 请求剩下的部分。

![img](https://pic4.zhimg.com/80/v2-325c064984c2454c4bfb7bf83afad1eb_720w.webp)

**5. 服务器推送**

HTTP/2 还在一定程度上改善了传统的「请求 - 应答」工作模式，服务不再是被动地响应，也可以主动向客户端发送消息。
举例来说，在浏览器刚请求 HTML 的时候，就提前把可能会用到的 JS、CSS 文件等静态资源主动发给客户端，减少延时的等待，也就是服务器推送（Server Push，也叫 Cache Push）。
HTTP/2 有哪些缺陷？HTTP/3 做了哪些优化？
HTTP/2 主要的问题在于：多个 HTTP 请求在复用一个 TCP 连接，下层的 TCP 协议是不知道有多少个 HTTP 请求的。
所以一旦发生了丢包现象，就会触发 TCP 的重传机制，这样在一个 TCP 连接中的所有的 HTTP 请求都必须等待这个丢了的包被重传回来。

- HTTP/1.1 中的管道（ pipeline）传输中如果有一个请求阻塞了，那么队列后请求也统统被阻塞住了
- HTTP/2 多请求复用一个TCP连接，一旦发生丢包，就会阻塞住所有的 HTTP 请求。

这都是基于 TCP 传输层的问题，所以 HTTP/3 把 HTTP 下层的 TCP 协议改成了 UDP！

![img](https://pic1.zhimg.com/80/v2-86a7d45bbe7308f105bc28e720e69d20_720w.webp)

UDP 发生是不管顺序，也不管丢包的，所以不会出现 HTTP/1.1 的队头阻塞 和 HTTP/2 的一个丢包全部重传问题。
大家都知道 UDP 是不可靠传输的，但基于 UDP 的 QUIC 协议 可以实现类似 TCP 的可靠性传输。

- QUIC 有自己的一套机制可以保证传输的可靠性的。当某个流发生丢包时，只会阻塞这个流，其他流不会受到影响。
- TL3 升级成了最新的 1.3 版本，头部压缩算法也升级成了 QPack。
- HTTPS 要建立一个连接，要花费 6 次交互，先是建立三次握手，然后是 TLS/1.3 的三次握手。QUIC 直接把以往的 TCP 和 TLS/1.3 的 6 次交互合并成了 3 次，减少了交互次数。

![img](https://pic2.zhimg.com/80/v2-feda45b9cfcd67c810760ebea083a6bd_720w.webp)

所以， QUIC 是一个在 UDP 之上的伪 TCP + TLS + HTTP/2 的多路复用的协议。
QUIC 是新协议，对于很多网络设备，根本不知道什么是 QUIC，只会当做 UDP，这样会出现新的问题。所以 HTTP/3 现在普及的进度非常的缓慢，不知道未来 UDP 是否能够逆袭 TCP。
唠叨



## HTTP首部

### 通用首部字段（General Header Fields）

这些首部字段既出现在请求中，也出现在响应中。

- `Cache-Control`：指定缓存指令 `no-cache`, `no-store`, `max-age=0`, `public`
- `Connection`：控制是否保持网络连接，`keep-alive`, `close`.
- `Date`：消息创建日期和时间。`Tue, 15 Nov 1994 08:12:31 GMT`
- `Pragma`：用于向后兼容HTTP/1.0缓存指令。
- `Transfer-Encoding`：传输编码方式。
- `Via`：显示代理服务器中间环节。

### 请求首部字段（Request Header Fields）

这些首部字段用于HTTP请求。

- `Accept`：指定客户端接受哪些类型的信息`text/html`, `application/json`, `image/jpeg`, `*/*`（任何类型）。
- `Accept-Charset`：指定客户端接受的字符集。`utf-8`, `iso-8859-1`
- `Accept-Encoding`：指定客户端接受的内容编码。`gzip`, `deflate`, `br`
- `Accept-Language`：指定客户端接受的语言。`en-US`, `en;q=0.5`, `de`
- `Authorization`：认证信息。
- `Host`：请求的服务器地址。
- `User-Agent`：客户端的信息。 `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36`
- `Cookie`：服务器发给用户的cookie。
- `Referer`：引用当前请求的前一个文档的地址。

### 响应首部字段（Response Header Fields）

这些首部字段用于HTTP响应。

- `Location`：用于重定向接收方到一个新的位置。
- `Server`：服务器的软件信息。`Apache/2.4.1 (Unix)`, `nginx/1.10.3`
- `Set-Cookie`：服务器发送的cookie信息。
- `WWW-Authenticate`：表示认证方案和参数。

### 实体首部字段（Entity Header Fields）

这些首部字段定义了关于实体正文（如MIME信息）的信息。

- `Content-Encoding`：实体正文的编码方式。
- `Content-Language`：实体正文的语言。
- `Content-Length`：实体正文的长度。
- `Content-Type`：实体正文的媒体类型。 `text/html; charset=UTF-8`, `application/json`, `image/png`.
- `Last-Modified`：资源的最后修改时间。
- `Expires`：实体的过期时间。
