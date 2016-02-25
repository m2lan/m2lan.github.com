title: SpringMVC-学习02
date: 2015-12-10 01:21:56
categories: 
- Spring-MVC
tags: 
- Spring-MVC
---
## 传值和接受传值

	package com.demo.controller;

	import java.util.HashMap;
	import java.util.Map;

	import org.springframework.stereotype.Controller;
	import org.springframework.ui.Model;
	import org.springframework.web.bind.annotation.RequestMapping;
	import org.springframework.web.bind.annotation.RequestParam;

	@Controller
	public class HelloController {

	@RequestMapping("/")
	public String hello() {
		return "hello";
	}
	
	/**
	 * @param username
	 * @requestParam 表示参数username为必须
	 * request http://localhost:8080/demo?username=aaa
	 * @return
	 */
	@RequestMapping("/say")
	public String say(@RequestParam("username") String username) {
		System.out.println(username);
		return "say";
	}
	
	/**
	 * @param username
	 * username参数可传可不传，不传就为null
	 * @return
	 */
	@RequestMapping("/say1")
	public String say1(String username) { 
		System.out.println(username);
		return "say";
	}
	
	/**
	 * @param username
	 * @param context
	 * 传值给视图 01: 使用map方式
	 * 视图使用${username}
	 * @return
	 */
	@RequestMapping("/say2")
	public String say2(String username, Map<String, Object> context) {
		context.put("username", username);
		return "say2";
	}
	
	/**
	 * @param username
	 * @param context
	 * 传值给视图 02: 使用model方式
	 * 视图使用${username}
	 * @return
	 */
	@RequestMapping("/say3")
	public String say3(String username, Model model) {
		
	//		01
	//		model.addAttribute("username", username);
			
	//		02
	//		Map<String, Object> map = new HashMap<String, Object>();
	//		map.put("username", username);
	//		model.addAllAttributes(map);
			
	//		03(key为参数的类型，username是String类型，则视图${string})
	//		model.addAttribute(username);
			return "say2";
		}
	}


