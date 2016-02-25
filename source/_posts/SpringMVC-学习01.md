title: SpringMVC-学习01
date: 2015-12-10 01:19:19
categories: 
- Spring-MVC
tags: 
- Spring-MVC
---
* 下载[springframework](http://maven.springframework.org/release/org/springframework/spring/)
* 新建web项目，拷贝`spring-framework-4.1.2.RELEASE/libs`下面所有的`*.RELEASE.jar`到`lib`目录下。
* 配置`web.xml`

		<?xml version="1.0" encoding="UTF-8"?>
		<web-app version="3.1" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd">
			<servlet>
				<servlet-name>hello</servlet-name>
				<servlet-class>org.springframework.web.servlet.DispatcherServlet </servlet-class>
				<load-on-startup>1</load-on-startup> // 启动就加载
			</servlet>
			<servlet-mapping>
				<servlet-name>hello</servlet-name>
				<url-pattern>/</url-pattern> // 监听所有的请求
			</servlet-mapping>
		</web-app>
* 在`/WEB-INF`下新建`hello-servlet.xml`(hello取决于web.xml中servlet-name的配置),配置如下

		<?xml version="1.0" encoding="UTF-8"?>
		<beans xmlns="http://www.springframework.org/schema/beans"
			xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			xmlns:context="http://www.springframework.org/schema/context"
			xmlns:mvc="http://www.springframework.org/schema/mvc"
			xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.1.xsd
				http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
				http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.1.xsd">

			<!-- 这两项启动annotation配置 -->
			<context:component-scan base-package="com.demo.controller"/>
			<mvc:annotation-driven/>

			<!-- 配置视图文件的路径和后缀 -->
			<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
				<property name="prefix" value="/WEB-INF/jsp/"/>
				<property name="suffix" value=".jsp"></property>
			</bean>

		</beans>
* 新建一个`controller`文件，`package=hello-servlet.xml`的`base-package`

		package com.demo.controller;

		import org.springframework.stereotype.Controller;
		import org.springframework.web.bind.annotation.RequestMapping;

		@Controller
		public class HelloController {

			@RequestMapping("/")
			// 当访问http://localhost:8080/APP/时就会进入该方法，方法映射到hello视图
			public String hello() {
				return "hello";
			}
		}


