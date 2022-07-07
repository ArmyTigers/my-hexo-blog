---
title: Activiti6工作流Api文档
date: 2022-07-06 19:48:55
tags:
---


# Activiti6工作流Api文档 

<!-- more -->

1、activiti 核心api

![img](file:////private/var/folders/m4/cdq8ymt567d_pnlb0620n4_40000gn/T/com.kingsoft.wpsoffice.mac/wps-junhu/ksohtml//wps5.png) 

 

1.1  流程存储服务 (RepositoryService)

​	•  管理流程定义文件xml及静态资源服务

​	•  对流程定义文件对暂停激活

​	•  流程定义启动权限管理

​	•  部署文件构造器DeploymentBuilder

​	•  部署文件查询器DeploymentQuery

​	•  流程定义文件查询对象ProcessDefinitionQuery

 

| 序号 | 方法                                                         | 含义                 | 描述                           | 库表字段                                              |
| ---- | ------------------------------------------------------------ | -------------------- | ------------------------------ | ----------------------------------------------------- |
| 1    | repositoryService.createDeployment().addClasspathResource("参 数") .deploy() | 部署流程             | resources文件下面的xml流程文件 | 省略                                                  |
| 2    | repositoryService.createDeploymentQuery().list()             | 查询所有部署         | 省略                           | 省略                                                  |
| 3    | repositoryService.createProcessDefinitionQuery().list()      | 查询所有部署流程     | 省略                           | 省略                                                  |
| 4    | repositoryService.suspendProcessDefinitionById(id)或ByKey    | 挂起流程             | 根据流程id挂起流程             | 修改表ACT_RE_PROCDEF字段SUSPENSION_STATE_:1激活 2挂起 |
| 5    | repositoryService.activateProcessDefinitionById(id)或ByKey   | 启动流程             | 根据流程id激活流程             | 修改表ACT_RE_PROCDEF字段SUSPENSION_STATE_:1激活 2挂起 |
| 6    | repositoryService.addCandidateStarterUser(流程id,用户id)     | 流程与用户对应关系   | 添加流程与用户关系             | 操作ACT_RU_IDENTITYLINK表                             |
| 7    | repositoryService.deleteCandidateStarterGroup(流程id,用户组id) | 流程与用户组对应关系 | 添加流程与用户组关系           | 操作ACT_RU_IDENTITYLINK表                             |
| 8    | repositoryService.deleteCandidateStarterUser(流程id,用户id)  | 流程与用户对应关系   | 删除流程与用户关系             | 操作ACT_RU_IDENTITYLINK表                             |
| 9    | repositoryService.deleteCandidateStarterGroup(流程id,用户组id) | 流程与用户对应关系   | 删除流程与用户组关系           | 操作ACT_RU_IDENTITYLINK表                             |
| 10   | repositoryService.getIdentityLinksForProcessDefinition(流程id) | 查询流程对应关系     | 查询流程对应用户跟组关系       | 操作ACT_RU_IDENTITYLINK表                             |



1.2 流程运行控制服务  (RuntimeService)

​	•  启动流程及对流程数据对控制

​	•  流程实例(ProcessInstance)与执行流(Execution)查询

​	•  触发流程操作、接收消息和信号

1.3 RuntimeService启动流程变量管理

​	•  启动流程的常用方式(id,key,message)

​	•  启动流程可选参数(businesskey,variables,tenantId)

​	•  变量(variables)的设置和获取

1.4  流程实例与执行流

​	•  流程实例(ProcessInstance)表示一次工作流业务的数据实体

​	•  执行流(Execution)表示流程实例中具体的执行路径

​	•  流程实例接口继承与执行流

1.5  流程触发

​	•  使用trigger触发ReceiveTask节点

​	•  触发信号捕获事件signalEvenReceived

​	•  触发消息捕获事件messageEventReceived



| 序号 | 方法                                                         | 含义                               | 描述                   |
| ---- | ------------------------------------------------------------ | ---------------------------------- | ---------------------- |
| 1    | runtimeService.startProcessInstanceByKey(String processDefinitionKey, Map<String, Object> variables) | 根据部署流程key启动一个流程        | 省略                   |
| 2    | runtimeService.startProcessInstanceById(String processDefinitionId, Map<String, Object> variables) | 根据部署流程id启动一个流程         | 省略                   |
| 3    | runtimeService.createProcessInstanceBuilder().businessKey("businessKey001") .processDefinitionKey(String processDefinitionKey).variables( Map<String, Object> variables) .start() | 根据processInstanceBuilder启动流程 | 省略                   |
| 4    | runtimeService.getVariables(processInstance.getId())         | 根据流程实例id获取传参             | 省略                   |
| 5    | runtimeService.setVariable(processInstance.getId(),"key3","value3") | 新增或修改参数                     | 省略                   |
| 6    | runtimeService.createProcessInstanceQuery().processInstanceId(processInstance.getId()) | 查询流程实例                       | 根据流程id获取流程实例 |
| 7    | runtimeService.createExecutionQuery()                        | 获取流程执行对象                   | 省略                   |



1.4  任务管理服务

+ TaskService
  + 对用户任务(UserTask)管理和流程控制
  +  设置用户任务(UserTask)对权限信息(拥有者,候选人,办理人)
  + 针对用户任务添加任务附件、任务；评价和事件记录
+ TaskService对Task管理与流程控制
  + Task对象对创建,删除
  + 查询Task,并驱动Task节点完成执行
  + Task相关参数变量(variable)设置

| 序号 | 方法                                               | 含义         | 描述 |
| ---- | -------------------------------------------------- | ------------ | ---- |
| 1    | taskService.createTaskQuery().list()               | 查询所有任务 | 省略 |
| 2    | taskService.setVariable("任务id","键","值")        | 设置普通变量 | 省略 |
| 3    | taskService.setVariableLocal("任务id","键","值")   | 设置本地变量 | 省略 |
| 4    | taskService.getVariables("任务id")                 | 获取普通变量 | 省略 |
| 5    | runtimeService.getVariables(task.getExecutionId()) | 获取本地变量 | 省略 |
| 6    | taskService.complete("任务id","传值Map")           | 到下一个节点 | 省略 |



+ TaskService设置Task权限信息
  + https://blog.51cto.com/u_3423936/2771360
  + 指定拥有人(Owner)和办理人(Assignee)
  + 通过claim设置办理人

| 序号 | 方法                                                         | 含义                                     | 描述 |
| ---- | ------------------------------------------------------------ | ---------------------------------------- | ---- |
| 1    | taskService.setOwner("taskId","user")                        | 设置流程发起人                           | 省略 |
| 2    | taskService.claim(""taskId"","user")                         | 指定代办人                               |      |
| 3    | taskService.addCandidateUser("user")                         | 添加候选人                               |      |
| 4    | taskService.addCandidateGroup("group")                       | 添加候选组                               |      |
| 5    | taskService.createTaskQuery().taskCandidateUser("user").taskUnassigned().list() | 查询候选人列表有user但是没指定代办人任务 |      |
| 6    | taskService.createTaskQuery().taskCandidateUser("user").taskUnassigned().list() | 查询候选人列表有我但是没指定代办人任务   |      |
| 7    | taskService.createTaskQuery().taskAssignee("user").list()    | 查询代办人为user的任务                   |      |
| 8    | taskService.getIdentityLinksForTask("taskId")                | 查询任务与人员之间的关系                 |      |



+ TaskService设置Task附加信息
  + 任务附件(Attachment)创建与查询
  + 任务评价(Comment)创建与查询

| 序号 | 方法                                                         | 含义             | 描述 |
| ---- | ------------------------------------------------------------ | ---------------- | ---- |
|      | taskService.createAttachment("类型","任务id","流程Id","附件名称","附件描述","流或者url) | 上传附件         |      |
|      | taskService.getTaskAttachments("任务id")                     | 上传附件         |      |
|      | taskService.addComment("任务id","流程id","批注1")            | 添加审批批注     |      |
|      | taskService.getTaskComments("任务id")                        | 查询审批批注     |      |
|      | taskService.getTaskEvents("任务id")                          | 查询任务日志记录 |      |

 

1.5 身份管理服务

+ IdentityService
  + 管理用户(User)
  + 管理用户组(Group)
  + 用户与用户组关系(Membership)

| 序号 | 方法                                | 含义             | 描述 |
| ---- | ----------------------------------- | ---------------- | ---- |
|      | identityService.newUser("userid")   | 创建一个用户     |      |
|      | identityService.newGroup("groupid") | 创建一个组       |      |
|      | identityService.saveUser(user)      | 保存或者更新用户 |      |
|      | identityService.saveGroup(group)    | 保存或者更新组   |      |
|      | identityService.createUserQuery()   | 查询用户         |      |
|      | identityService.createGroupQuery()  | 查询组           |      |

 

1.6 表单服务管理

+ FormService
  + 解析流程定义中表单项的配置
  + 提交表单的方式驱动用户节点流转
  + 获取自定义外部表单key

 

| 序号 | 方法                                                         | 含义                           | 描述 |
| ---- | ------------------------------------------------------------ | ------------------------------ | ---- |
| 1    | formService.getStartFormKey(processDefinition.getId())       | 部署流程的id获取表单key        |      |
| 2    | formService.getStartFormData(processDefinition.getId()).getFormProperties() | 获取开始节点表单内容           |      |
| 3    | formService.getStartFormData(processDefinition.getId()).getFormProperties() | 获取开始节点表单内容           |      |
| 4    | formService.submitStartFormData(processDefinition.getId(), "传值参数") | 通过formservice启动流程        |      |
| 5    | formService.submitTaskFormData("taskId","传参数")            | 通过formservice提交task表单    |      |
| 6    | formService.getTaskFormData("taskId")                        | 通过taskid获取task节点表单内容 |      |

 

1.7  历史管理服务

+ HistoryService
  + 管理流程实例结束后的历史数据
  + 构建历史数据查询对象
  + 根据流程实例id删除流程历史数据

![img](file:////private/var/folders/m4/cdq8ymt567d_pnlb0620n4_40000gn/T/com.kingsoft.wpsoffice.mac/wps-junhu/ksohtml//wps6.png) 

+ HistoryService构建历史查询对象
  + create[历史数据实体]Query
  + createNative[历史数据实体]Query | 通过原生sql查询
  + createProcessInstanceHistoryLogQuery | 查询一个流程实例的所有其他数据

+ HistoryService删除历史操作
  + deleteHistoricProcessInstance | 删除历史流程实例及联删除其他信息
  + deleteHistoricTaskInstance | 删除历史的task实例

 

| 序号 | 方法                                                         | 含义                           | 描述 |
| ---- | ------------------------------------------------------------ | ------------------------------ | ---- |
| 1    | historyService.createHistoricProcessInstanceQuery()          | 查询流程实例变量               |      |
| 2    | historyService.createHistoricActivityInstanceQuery()         | 查询活动节点                   |      |
| 3    | historyService.createHistoricTaskInstanceQuery()             | 查询任务实例                   |      |
| 4    | historyService.createHistoricVariableInstanceQuery()         | 查询流程任务变量               |      |
| 5    | historyService.createHistoricDetailQuery()                   | 历史任务流程活动详细信息       |      |
| 6    | historyService.createProcessInstanceHistoryLogQuery("流程实例id") | 查询一个流程实例的所有其他数据 |      |
| 7    | historyService.deleteHistoricProcessInstance("流程实例id")   | 删除历史流程实例               |      |
| 8    | historyService.deleteHistoricTaskInstance("taskid")          | 删除历史任务                   |      |



1.8 其他管理服务

+ 其他管理服务
  + 管理服务ManagementService
  + 动态流程定义服务DynamicBpmnService

+ ManagementService
  + job任务管理
  + 数据库相关通用操作
  + 执行流程引擎命令(Command)

+ Job任务查询
  + JobQuery 查询一般工作
  + TimerJobQuery 查询定时工作
  + SuspendedJobQuery 查询中断工作
  + DeadLetterJobQuery 查询无法执行的工作

 

| 序号 | 方法                                         | 含义               | 描述 |
| ---- | -------------------------------------------- | ------------------ | ---- |
| 1    | managementService.createTimerJobQuery(       | 查询定时任务       |      |
| 2    | managementService.createJobQuery()           | 查询一般工作       |      |
| 3    | managementService.createSuspendedJobQuery()  | 查询中断工作       |      |
| 4    | managementService.createDeadLetterJobQuery() | 查询无法执行的工作 |      |

 

+ 数据库相关操作
  + 查询表结构元数据(TableMetaData)
  + 通用查询(TablePageQuery)
  + 执行自定义Sql查询(executeCustomSql)

 

| 序号 | 方法                                                         | 含义               | 描述 |
| ---- | ------------------------------------------------------------ | ------------------ | ---- |
| 1    | managementService.createTablePageQuery().tableName(managementService.getTableName(class)) | 查询实体到所有数据 |      |
| 2    | managementService.executeCustomSql()                         | 自定义sql查询      |      |



1.9 异常策略

+ ActivitiEXception
  + ActivitiWrongDbException 引擎与数据库版本不匹配
  + ActivitiOptimisticLockingException 并发导致乐观锁异常
  + ActivitiClassLoadingException 加载类异常
  + ActivitiObjectNotFoundException 操作对象不存在
  + ActivitilllegalArgumentException 非法的参数
  + ActivitiTaskAlreadyClaimedException 任务被重新声明代理人
  + BpmnError 定义业务异常控制流程