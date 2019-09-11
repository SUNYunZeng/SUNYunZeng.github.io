---
title: Servlet技术
comments: true
toc: true
date: 2019-09-11 18:56:49
categories: Java
tags: Servlet
---

# 什么是Servet?

Servlet (Server Applet)，从字面上看，就是<font color=#f07c82>Java</font>服务器小程序的意思。它确实就像字面意思一样，是<font color=#f07c82>在服务中用于处理网络请求的小程序</font>。

在Web的世界中，客户端会提交各种请求到服务端，服务端如何处理客户端的请求呢？

常规的Java编程方法，好像很难完成这项任务，但是没有事情是难倒程序猿的。

于是，他们设计了<font color=#f07c82>Servlet规范</font>，用来处理网络的各种请求。

具体网络的请求呢，无非就是<font color=#f07c82>get、post等等</font>，这在[HTTP规范系列](http://sunyunzeng.com/categories/HTTP/)里面有讲到。

Servlet规范也没有想象中的那么高大上，其实它就是一个**Java接口**，里面一共就定义了五个方法，如图：

{% asset_img servlet_interface.PNG %}

其中：

> + init() 规定了Servet如何初始化。
> + getServletConfig() 获取Sevvlet的配置。
> + service(ServletRequest, ServletResponse) 接收到请求怎么处理。
> + getServletInfo() 提供有关servlet的信息，如作者、版本、版权等。
> + destroy() 销毁Servlet。

# Servlet的运行

任何合理实现了Servlet接口的类都具有处理HTTP请求的能力，但是就像java类的运行需要在JVM环境中一样，Servlet的运行也要环境，这里称为<font color=#f07c82>容器</font>。

即Servlet的运行需要特定的容器，**该容器负责实现对端口的监听，将请求内容解析，然后实例Servlet对象，给Servlet提供运行环境，然后将Servlet的处理结果发给客户端**。

**Tomcat**就是一个开源的Servlet的容器，它也是一个Web服务器。

<font color=#f07c82>HttpServlet<font>就是已经实现好的一个Servlet类，他对一些方法进行了详细的补充，我们通过doGet()、doPost()等方法很方便地实现处理HTTP请求功能。

它的一些源码如下：

1. service()实现：
```java
protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
 
        //获取http request的method参数，其实就是html的form标签  
        //中method属性对应的字符串 
        String method = req.getMethod();
        long errMsg;
        //判断请求方式
        if(method.equals("GET")) {
            //获取最后被修改时间 
            errMsg = this.getLastModified(req);
            if(errMsg == -1L) {
            /**如果servlet不支持http request header的if-modified-since属性 
             * 则继续处理 
             **/  
                this.doGet(req, resp);
            } else {
               //如果支持这个属性 
                long ifModifiedSince;
                try {
                    ifModifiedSince = req.getDateHeader("If-Modified-Since");
                } catch (IllegalArgumentException var9) {
                    ifModifiedSince = -1L;
                }
               /** 
                * 如果客户端的文件最后修改时间和服务器端的文件最后修改时间一致则返回304不需要修改状态 
                * 这样服务器就不返回html，浏览器读取本地缓存文件，否则重新获取服务器端的对应html文件 
                **/  
                if(ifModifiedSince < errMsg / 1000L * 1000L) {
                    this.maybeSetLastModified(resp, errMsg);
                    this.doGet(req, resp);
                } else {
                    resp.setStatus(304);
                }
            }
        } else if(method.equals("HEAD")) {
            errMsg = this.getLastModified(req);
            this.maybeSetLastModified(resp, errMsg);
            this.doHead(req, resp);
        } else if(method.equals("POST")) {
            this.doPost(req, resp);
        } else if(method.equals("PUT")) {
            this.doPut(req, resp);
        } else if(method.equals("DELETE")) {
            this.doDelete(req, resp);
        } else if(method.equals("OPTIONS")) {
            this.doOptions(req, resp);
        } else if(method.equals("TRACE")) {
            this.doTrace(req, resp);
        } else {
            //如果请求不是以上的所有请求方式，该方法就会响应501错误，也就是不支持这种请求
            String errMsg1 = lStrings.getString("http.method_not_implemented");
            Object[] errArgs = new Object[]{method};
            errMsg1 = MessageFormat.format(errMsg1, errArgs);
            resp.sendError(501, errMsg1);
        }
 
    }
 
public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        HttpServletRequest request;
        HttpServletResponse response;
        try {
            request = (HttpServletRequest)req;
            response = (HttpServletResponse)res;
        } catch (ClassCastException var6) {
            throw new ServletException("non-HTTP request or response");
        }
        this.service(request, response);
}
```

2. doGet()实现：

```Java
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取协议 
        String protocol = req.getProtocol();
        //获取http.method_get_not_supported的国际化字符串
        String msg = lStrings.getString("http.method_get_not_supported");
        if(protocol.endsWith("1.1")) {
        //如果是HTTP/1.1，返回405禁止访问方法错误
            resp.sendError(405, msg);
        } else {
        //如果不是HTTP/1.1，返回400错误的请求错误  
            resp.sendError(400, msg);
        }
 
    }
```

2. doPost()实现：
```java
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String protocol = req.getProtocol();
        String msg = lStrings.getString("http.method_post_not_supported");
        if(protocol.endsWith("1.1")) {
            resp.sendError(405, msg);
        } else {
            resp.sendError(400, msg);
        }
 
}
```

**其实在工业界，几乎没有直接利用Servlet开发Web应用的了，但是它是一个基础，很多开源框架都是基于Servlet开发的，如大名鼎鼎的[Spring](https://spring.io/)**

# 如何实现？

1. 首先我写一个Servlet类,作用是得到get()请求，然后返回“Hello Servlet!”与打印当前时间。

```java
public class HelloServlet extends HttpServlet{
 
    public void doGet(HttpServletRequest request, HttpServletResponse response){
         
        try {
            response.getWriter().println("<h1>Hello Servlet!</h1>");
            response.getWriter().println(new Date().toLocaleString());
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    } 
}
```

2. 然后配置web.xml文件，建立URL与Servlet处理类 **HelloServlet** 之间的关联关系。

```java
<?xml version="1.0" encoding="UTF-8"?>
<web-app>

	<servlet>
		<servlet-name>HelloServlet</servlet-name>
		<servlet-class>HelloServlet</servlet-class>
	</servlet>

	<servlet-mapping>
		<servlet-name>HelloServlet</servlet-name>
		<url-pattern>/hello</url-pattern>
	</servlet-mapping>

</web-app>
```
其中**servlet-name**是类名简写，**servlet-class** 是类名。

**servlet-mapping**实现URL与类的映射，这里实现了访问路径 http://127.0.0.1//hello 时调用HelloServlet类的相应方法。

http://127.0.0.1 是一个监听地址，需要服务器实现监听，此时就需要Tomcat。

然后修改Tomcat的conf/server.xml的路径配置。

```xml
<Context path="/" docBase="F:\\eclipse-workspace\\j2ee\\web" debug="0" reloadable="false" />
```

# Servlet的特点

**1. Servlet是单实例多线程。**
>1) 当Web服务器启动或者用户请求抵达服务器时，Servlet被实例化且只存在一个实例。
>2) 当请求抵达时，Servlet容器(如Tomcat)会调度线程 (Dispathchaer Thread)调度它管理下的线程池中等待执行的线程 (Worker Thread) 给请求者。
>3) 线程执行Servlet中的sercive方法。
>4) 请求结束，该线程放回线程池，等待被调用。

**2. Servet使用标准API，可被更多Web服务器调用。**

## 线程安全

由于Servlet是单实例多线程，当多个线程的用户同时访问共享资源时，就会出现<font color=#f07c82>线程安全的问题</font>。

**解决方法:**

首先，定义在 doPost() 和 doGet()里的方法由于是**局部变量**，再每个用户调用实例方法时都会初始化，所以**不存在线程安全**。（一些属性尽量定义在实例的局部方法中）

实在需要共享的资源，只需加<font color=#f07c82>synchronized同步机制</font>，在共享资源被某一线程占用后，该线程就拥有锁，其它线程只有等待该线程执行完毕才能使用该资源（抢占锁后一一执行）。
