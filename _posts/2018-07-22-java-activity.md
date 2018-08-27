---
layout: post
title:  "小白学习activity"
date:   2018-07-22
categories: Java
tags: Java
excerpt: 使用activity
---

---

* 文档结构
{:toc}

使用activity，首先要定义流程，接下来对流程进行部署，部署完之后，启动一个流程实例，然后执行流程实例的中的任务。其中定义的流程为.bpmn文件和.png文件。

---

2018-07-22

---

# activity

### 部署流程三种方式

1.通过classpath路径进行部署

	ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
    processEngine.getRepositoryService()
                   .createDeployment()
                   .addClasspathResource("qingjia.bpmn")
                   .addClasspathResource("qingjia.png")
                   .deploy();

2.通过inputstream完成部署

	InputStream in = this.getClass().getClassLoader().getResourceAsStream("qingjia.bpmn");

	ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();

  	processEngine.getRepositoryService().createDeployment().addInputStream("qingjia.bpmn", bpmnStream).deploy();

3.通过zipInputStream完成部署

	  InputStream in = this.getClass().getClassLoader().getResourceAsStream("qingjia.bpmn");

      ZipInputStream zipInputStream = new ZipInputStream(in);

      ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();

      processEngine.getRepositoryService().createDeployment().addZipInputStream(zipInputStream);

4.小结部署流程涉及到的表及解释

	1.act_ge_bytearray:
		1.表名解释
			act: activity
			ge:general总的
			bytearray: 二进制
		2.字段解释
			name_:存放文件路径及名称
			deployment_id_:存放部署id
			bytes_:存放文件内容
		3.简要说明
			如果要查询文件（bpmn和png）,需要知道deployment_id_

	2.act_re_deployment
		1.表名解释
			act: activity
			re: repository仓库
			deployment: 部署
		2.字段解释
			id_: 部署id
			name_:流程名称

	3.act_re_procdef
		1、表名解释
    		procdef: process definition  流程定义
    	2、字段解释
    		id_: 简称（pdid）组成pdkey:pdversion:随机数
    		name:名称
    		key:名称
    		version:版本号
			deployment_id_:部署ID
		3.简要说明
    		如果名称不变，每次部署，版本号加1
    		如果名称改变，则版本号从1开始计算
    
### 删除流程

	ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
      
    processEngine.getRepositoryService()
                   .deleteDeployment("id", true);
	
	说明：
		deleteDeploymeng包含参数true时，表示级联删除

### 启动流程实例

	ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
		processEngine.getRuntimeService()
		.startProcessInstanceById("qingjia:1:4");
	//参数流程定义id

### 查询当前正在执行任务

	ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
	List<Task> tasks = processEngine.getTaskService()
		.createTaskQuery()
		.taskAssignee("assignee")
		.list();
		for (Task task : tasks) {
			System.out.println(task.getName());
		}
	//assingee 节点name

### 完成任务

	ProcessEngine engine = ProcessEngines.getDefaultProcessEngine();
		engine.getTaskService()
		.complete("202");
	//任务id

### 查询流程部署

	ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
		List<Deployment> deployments = processEngine.getRepositoryService()
		.createDeploymentQuery()
		.orderByDeploymenTime()//按照部署时间排序
		.desc()//按照降序排序
		.list();
		for (Deployment deployment : deployments) {
			System.out.println(deployment.getId());
		}

### 根据名称查询流程部署

	ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
		List<Deployment> deployments = processEngine.getRepositoryService()
		.createDeploymentQuery()
		.orderByDeploymenTime()//按照部署时间排序
		.desc()//按照降序排序
		.deploymentName("请假流程")
		.list();
		for (Deployment deployment : deployments) {
			System.out.println(deployment.getId());
		}

### 查询所有的流程定义

	ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();

	List<ProcessDefinition> pdList = processEngine.getRepositoryService()
		.createProcessDefinitionQuery()
		.orderByProcessDefinitionVersion()
		.desc()
		.list();
		for (ProcessDefinition pd : pdList) {
			System.out.println(pd.getVersion());
		}

### 查看流程图,根据deploymentId和name
	
	ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();

	InputStream inputStream = processEngine.getRepositoryService()
				/**
				 * deploymentID
				 * 文件的名称和路径
				 */
		.getResourceAsStream("501","qingjia.png");

	OutputStream outputStream3 = new FileOutputStream("e:/processimg.png");
		for(int b=-1;(b=inputStream.read())!=-1;){
			outputStream3.write(b);
		}
		inputStream.close();
		outputStream3.close();

### 根据pdid(流程定义id)查看图片
	
	ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();

	InputStream inputStream = processEngine.getRepositoryService()
		.getProcessDiagram("qingjia1:1:804");

	OutputStream outputStream3 = new FileOutputStream("e:/processimg.png");
		for(int b=-1;(b=inputStream.read())!=-1;){
			outputStream3.write(b);
		}
		inputStream.close();
		outputStream3.close();
	
### 查看bpmn文件

	ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();

	InputStream inputStream = processEngine.getRepositoryService()
		.getProcessModel("qingjia1:1:804");

	OutputStream outputStream3 = new FileOutputStream("e:/processimg.bpmn");
		for(int b=-1;(b=inputStream.read())!=-1;){
			outputStream3.write(b);
		}
		inputStream.close();
		outputStream3.close();