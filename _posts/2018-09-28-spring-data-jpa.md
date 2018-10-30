---
layout: post
title:  "spring data JPA"
date:   2018-09-28
categories: spring boot
tags: 框架
excerpt: spring boot JPA
---

---

* 文档结构
{:toc}

---

2018-09-28

---

## spring data JPA

### 1.什么是spring data JPA
	
	JPA是一个基于对象关系映射的标准规范。

### 2.定义数据访问层（继承JpaRepository接口）

### 3.配置使用spring data JPA

### 4.定义查询方法

	1.根据属性名查询。
		（1）常规查询
		（2）限制结果数量
	2.使用JPA的@NamedQuery查询
	3.使用@Query查询
		（1）使用参数索引(@Query)
		（2）使用命名参数(@Query)
		（3）更新查询（@Modifying和@Query）
		（4）Specification
			1)定义Criterial查询
			2）定义接口实现JpaSpecificationExecutor接口
		（5）排序与分页（Sort类，Page接口，Pageable接口）
			1）定义接口
			2）使用排序
			3）使用分页

### 自定义Repository的实现

	1.定义自定义Repository接口
	2.定义接口实现（继承SimpleJpaRepository实现1中自定义接口）
	3.自定义RepositoryFactoryBean.
	4.开启自定义支持使用@EnableJpaRepositories的repositoryFactoryBeanClass来指定FactoryBean