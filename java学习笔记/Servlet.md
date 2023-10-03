# Servlet

客户端和服务器的数据库或应用程序中的中间层

![Servlet 架构](assets/servlet-arch.jpg)



## Servlet 生命周期

Servlet 生命周期可被定义为从创建直到毁灭的整个过程。以下是 Servlet 遵循的过程：

- Servlet 初始化后调用 **init ()** 方法。
- Servlet 调用 **service()** 方法来处理客户端的请求。
- Servlet 销毁前调用 **destroy()** 方法。
- 最后，Servlet 是由 JVM 的垃圾回收器进行垃圾回收的。



### `init()`

在第一次创建Servlet时被调用，Servlet创建于用户第一次调用对应该Servlet的URL时候，也可以指定在服务器第一次启动时被调用



### service() 

service() 方法是执行实际任务的主要方法。Servlet 容器（即 Web 服务器）调用 service() 方法来处理来自客户端（浏览器）的请求，并把格式化的响应写回给客户端。

<img src="https://www.runoob.com/wp-content/uploads/2014/07/Servlet-LifeCycle.jpg" alt="Servlet 生命周期" style="zoom: 67%;" />



Servlet 是服务 HTTP 请求并实现 `javax.servlet.Servlet` 接口的 Java 类。Web 应用程序开发人员通常编写 Servlet 来扩展`javax.servlet.http.HttpServlet`，并实现 Servlet 接口的抽象类专门用来处理 HTTP 请求。



## Servlet 表单数据

### 使用 Servlet 读取表单数据

Servlet 处理表单数据，这些数据会根据不同的情况使用不同的方法自动解析：

- `getParameter()`：您可以调用 `request.getParameter()` 方法来获取表单参数的值。
- `getParameterValues()`：如果参数出现一次以上，则调用该方法，并返回多个值，例如复选框。
- `getParameterNames()`：如果您想要得到当前请求中的所有参数的完整列表，则调用该方法。



