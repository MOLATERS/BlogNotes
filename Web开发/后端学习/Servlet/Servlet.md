---
title: Servlet 简介
categories:
- 网页开发
- 后端
tags:
- Servlet
---



开发工具如下：

Netbeans 曾经的官方开发工具

Eclipse 起源于IBM

IntelliJ IDEA 商用软件

## 什么是Servlet

服务端小程序

- 运行在Web容器里面的Javac程序
- 接收客户端的请求，向客户端响应

```java
package cn.edu.hit;
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.*;
public class HelloServlet extends HttpServlet{
	public void doGet(HttpServletRequest request,HttpServletResponse response)
	throws ServletException, IOException{
	response.setContentType("text/html;charset=UTF-8");
	PrintWriter out = response.getWriter();
	out.printIn("Hello 这是一个Servlet!");
	}
}//helloServlet.java
```

Sevlet的部署

Tomcat安装目录

- webapps

  - 项目文件夹

    WEB-INF

    - classes\cn\edu\hit\HelloServlet.calss

Servlet的运行

- http://localhost:8080/test/Hello

Servlet的配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app 此处省去XXX个字符>
<servlet>
<servlet-name>Hello</servlet-name>
<servlet-class>cn.edu.hit.HelloServlet</servlet-class>
</servlet>
<servlet-mapping>
<servlet-name>Hello</servlet-name>
<url-pattern>/Hello</url-pattern>
</servlet-mapping>
</web-app>
```

Servlet 3+使用注解@WebServlet无需配置 web.xml

```java
package cn.edu.hit;
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.*;
import javax.servlet.annotation.WebServlet;
@WebServlet("/Hello")
public class HelloServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
	throws ServletException,IOException {
		response.setContentType("text/html; charset=UTF-8");
		PrintWriter out = response.getWriter();
		out.println("Hello, 这是一个Servlet");
	}
}
```

## Servlet 生命周期

### 加载并实例化

- Servlet容器负责加载Servlet类和实例化一个Servlet对象

### 初始化

- **init**方法负责初始化工作，该方法由Servlet容器调用完成

### 处理客户请求

- **service**方法负责处理客户端请求，并返回响应结果

### 卸载

- **destroy**方法在Servet容器卸载Servlet之前被调用

## init方法

init方法只在Servlet被容器加载后执行一次

init方法用于初始化Servlet，如读取配置信息和初始化参数等

init方法已经在父类中提供缺省实现，可以不用重写

### 两个init方法

void init(ServletConfig config)

- config参数用于读取初始化参数后，调用没有参数的init方法

void init（）

- 通常重写该方法，使用getServketConfig方法获取ServletConfig对象

### 读取初始化参数

```java
public class HelloServlet extends HttpServlet{
	public void init(){
	//方法1
	String username = getInitParameter("username");
	String password = getInitParameter("password");
	//方法2
	ServletConfig config=getServletConfig();
	String username = config.getInitParameter("username");
	String password = config.getInitParaameter("password");
	}
}
```

### 配置初始化参数

```java
@WebServlet(name="HelloServlet",value="/HelloServlet", initParams = {
        @WebInitParam(name="username",value="admin"),
        @WebInitParam(name="pwd",value="123")
})
```



```xml
<servlet>
<servlet-name>Hello</servlet-name>
<servlet-class>cn.edu.hit.HelloServlet</servlet-class>
<init-param>
<param-name>username</param-name>
<param-value>admin</param-value>
</init-param>
</servlet>
<servlet-mapping>
<servlet-name>Hello</servlet-name>
<url-pattern>/Hello</url-pattern>
</servlet-mapping>
```

### response 对象常用方法

PrintWriter getWriter()

- 获取文本输出流，向客户输出文本数据

ServletOutputSystem getOutputStream()

- 获取二进制输出流，向客户端输出二进制数据

void setContentType(String type)

- 设置响应消息的消息体内容类型

void sendRedirect(String location)

- 告知客户端重定向到新的位置

### 请求和响应

#### 重定向

统治浏览器想另一个地址重新发送请求的响应称为重定向

使用Response类的SenRedirect方法进行重定向

void sendRedirect(String location)

在执行该方法钱不能有数据已经返回给客户端，否则方法会抛出illegalStateException异常

重定向后的浏览器地址栏将变成新的URL，Web容器为该请求分配新的request和response

### 请求转发

请求转发发生在服务器端，在转发后的新资源里还通过request对象获取客户端最初的请求参数

使用RequestDispatcher类的forward方法进行请求转发

```java
public void doGet(HttpServletRequest request, HttpServletResponse response)
	throws ServletException, IOException {
	RequestDispatcher rd = request.getRequestDispatcher("new.jsp");
	rd.forward(request, response);
	}
}
```

### 中文乱码问题解决办法

用request.setCharacterEncoding方法制定客户端字符编码

- 该方法只对POST请求有效，对GET请求无效
- 该方法必须在和获取参数前使用，否则无效

使用String类的getBytes方法将字符串转化为特定编码的字节数组，再用字节数组按照特定的编码重新构造字符串

```java
String name = request.getParameter("name");
byte[] buf = name.getBytes("ISO-8859-1");
name = new String(buf, "UTF-8"); // 或 GBK、GB2312
```

## 数据共享

Servlet范围数据共享

Servlet范围数据共享

- Servlet标准规定，Web容器只为每个Servlet创建一个实例,Web容器为每一个请求分配一个独立的线程，所有请求都适用这个实例的成员变量，因此将要共享的数据申明为成员变量，即可实现Servlet范围内的数据共享
- 当两个或者多个线程同时访问一个Servlet时，可能会发生多线程冲突，数据可能会编的不一致，造成资源冲突，实例变量不正确的使用是造成Servlet县城不安全的主要与暗影
- 应避免使用实例变量实现数据共享

请求期间的数据共享

RequestDispatcher可以将客户的请求转发给其他资源，可以在这些不同的资源建实现数据共享可以使用request对象提供如下方法

- void setAttribute(Stirng name,Object value)
- Object get Attribute(Sting name)
- void removeAttribute(String name)

在上游setAttribute

在下游setAttribute

### Web程序范围数据共享

使用ServletContext对象的如下方法

- void setAtrribute(String name, Object value)
- Object getAtrribute(String name)
- void removeAtrribute(String name)

获取ServletContext对象

- getServletContext()
- getServletConfig().getServletContext()

同样存在线程安全问题

### 全局初始化参数

使用ServletContext对象的如下方法获取全局初始化参数

- String getInitParameter(String name)

```xml
<context-param>
<param-name>参数名1</param-name>
<param-value>参数值1</param-value>
</context-param>
<context-param>
<param-name>参数名2</param-name>
<param-value>参数值2</param-value>
</context-param>
```

## 会话跟踪

### 什么是会话和会话跟踪

- 会话（Session)是一系列相关的请求和响应
- 回话耿总（Session Tracking)是在这些请求和响应中维护需要的数据星系，是的这些相关请求和相应得到正确的运作

### 为什么需要会话跟踪

- Web应用基于Http协议，Http协议是一个无状态的协议

- 每次HTTP通信过程是独立的，不维护状态信息，要实现回话耿总新西必须有客户和服务器另外保存

### 实现会话跟踪的请求和响应过程

- 客户端发送请求钱聪客户端存储区去除和本次请求相关的会话信息
- 客户端将会话信息连同请求信息一起发送到服务器
- 服务器从服务端存储区去除和当前请求相关的会话信息，联通客户端发送过来的信息一起进行处理
- 处理过程中可能改变会话信息，这时需要更新服务器端存储区的会话数据
- 服务器返回响应，其中可能包括只是客户端更新会话信息的内容
- 客户端根据只是，更新客户端存储区中的会话信息
