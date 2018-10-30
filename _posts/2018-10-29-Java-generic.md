---
layout: post
title:  "Java泛型设计"
date:   2018-10-29
categories: Java
tags: 泛型
excerpt: 学习java泛型程序设计
---

* 文档结构
{:toc}

学习java泛型程序设计，知识来源java核心技术第十版。

---


# java泛型程序设计

##  1.泛型代码和虚拟机
		
	虚拟机没有泛型类型对象，所有对象都属于普通类。

### 1.1 类型擦除

### 1.2 翻译泛型表达式

### 1.3 翻译泛型方法

### 1.4 调用遗留代码

    

## 2.约束与局限性

1. class对象的获取

- 第一种方式 通过对象getClass方法

        Person person = new Person();
        
        Class<?> class1 = person.getClass();
    
- 第二种方式 通过类的class属性

         class1 = Person.class;
         
- 第三种方式 通过Class类的静态方法——forName()来实现

        try {
            class1 = Class.forName("com.whoislcj.reflectdemo.Person");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
       