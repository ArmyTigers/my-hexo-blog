---
title: Activiti6表结构说明
date: 2022-07-06 19:50:00
tags:
---

# 表结构说明

<!-- more -->

## 表名规则:

### 	Activiti 使用到的表都是 ACT_ 开头的。表名的第二部分用两个字母表明表的用途。

## 表前缀说明

- ACT_GE_  (GE)   表示  general 全局通用数据及设置，各种情况都使用的数据。

- ACT_HI_  (HI)   表示  history 历史数据表，包含着程执行的历史相关数据，如结束的流程实例，变量，任务，等等

- ACT_ID_  (ID)   表示  identity 组织机构，用户记录，流程中使用到的用户和组。这些表包含标识的信息，如用户，用户组，等等。

- ACT_RE_  (RE)   表示  repository 存储，包含的是静态信息，如，流程定义，流程的资源(图片，规则等)。

- ACT_RU_  (RU)   表示  runtime 运行时，运行时的流程变量，用户任务，变量，职责(job)等运行时的数据。Activiti 只存储实例执行期间的运行时数据，当流程实例结束时，将删除这些记录。这就保证了这些运行时的表小且快。

 

## Activiti表总览 

### 全局数据表 (ACT_GE_)

| 表名             | 解释                                                         |
| ---------------- | ------------------------------------------------------------ |
| ACT_GE_BYTEARRAY | 二进制数据表，存储通用的流程定义和流程资源。                 |
| ACT_GE_PROPERTY  | 系统相关属性，属性数据表存储整个流程引擎级别的数据，初始化表结构时，会默认插入三条记录。 |



### 流程历史记录表 (ACT_HI_)

| 表名                  | 解释                           |
| --------------------- | ------------------------------ |
| ACT_ HI_ACTINST       | 历史节点表                     |
| ACT_ HI_ATTACHMENT    | 历史附件表                     |
| ACT_ HI_COMMENT       | 历史意见表                     |
| ACT_ HI_ DETAIL       | 历史详情表，提供历史变量的查询 |
| ACT_ HI_ IDENTITYLINK | 历史流程人员表                 |
| ACT_ HI_ PROCINST     | 历史流程实例表                 |
| ACT_HI_TASKINST       | 历史任务实例表                 |
| ACT_HI_VARINST        | 历史变量表                     |



### 用户用户组表 (ACT_ID_) 

| 表名              | 解释                   |
| ----------------- | ---------------------- |
| ACT_ID_GROUP      | 用户组信息表           |
| ACT_ID_INFO       | 用户扩展信息表         |
| ACT_ID_MEMBERSHIP | 用户与用户组对应信息表 |
| ACT_ID_USER       | 用户信息表             |





### 流程定义表 (ACT_RE_)

| 表名              | 解释               |
| ----------------- | ------------------ |
| ACT_RE_DEPLOYMENT | 部署信息表         |
| ACT_RE_MODEL      | 流程设计模型部署表 |
| ACT_RE_PROCDEF    | 流程定义数据表     |



### 运行实例表 (ACT_RU_)

| 表名                | 解释                                                 |
| ------------------- | ---------------------------------------------------- |
| ACT_RU_EVENT_SUBSCR | 运行时事件 throwEvent、catchEvent 时间监听信息表     |
| ACT_RU_EXECUTION    | 运行时流程执行实例                                   |
| ACT_RU_IDENTITYLINK | 运行时流程人员表，主要存储任务节点与参与者的相关信息 |
| ACT_RU_JOB          | 运行时定时任务数据表                                 |
| ACT_RU_TASK         | 运行时任务节点表                                     |
| ACT_RU_VARIABLE     | 运行时流程变量数据表                                 |



### 其它

| 表名             | 解释                   |
| ---------------- | ---------------------- |
| ACT_EVT_LOG      | 事件日志               |
| ACT_PROCDEF_INFO | 流程定义的动态变更信息 |

 

## Acticiti表详情

### ACT_GE_BYTEARRAY表结构

 	二进制数据表，存储通用的流程定义和流程资源。(act_ge_bytearray)

​	保存流程定义图片和xml、Serializable(序列化)的变量,即保存所有二进制数据，特别注意类路径部署时候，不要把svn等隐藏文件或者其他与流程无关的文件也一起部署到该表中，会造成一些错误(可能导致流程定义无法删除)

| 字段名称       | 字段描述       | 数据类型       | 主键 | 为空 | 取值说明                                            |
| -------------- | -------------- | -------------- | ---- | ---- | --------------------------------------------------- |
| ID_            | ID_            | nvarchar(64)   | Y    |      | 主键ID                                              |
| REV_           | 乐观锁         | int            |      | Y    | Version(版本)                                       |
| NAME_          | 名称           | nvarchar(255)  |      | Y    | 部署的文件名称，如：leave.bpmn.png,leave.bpmn20.xml |
| DEPLOYMENT_ID_ | 部署ID         | nvarchar(64)   |      | Y    | 部署表ID                                            |
| BYTES_         | 字节           | varbinary(max) |      | Y    | 部署文件                                            |
| GENERATED_     | 是否是引擎生成 | tinyint        |      | Y    | 0为用户生成，1为activiti生成                        |

 

### ACT_GE_PROPERTY表结构

​	属性数据表(act_ge_property)

​	属性数据表。存储整个流程引擎级别的数据。

 

| 字段名称 | 字段描述 | 数据类型      | 主键 | 为空 | 取值说明                              |
| -------- | -------- | ------------- | ---- | ---- | ------------------------------------- |
| NAME_    | 名称     | nvarchar(64)  | √    |      | schema.versionschema.historynext.dbid |
| VALUE_   | 值       | nvarchar(300) |      | √    | 5.**create****(****5.**)              |
| REV_     | 乐观锁   | int           |      | √    | version                               |

 

### ACT_HI_ACTINST表结构

​	历史节点表(act_hi_actinst)

​	历史活动信息。这里记录流程流转过的所有节点，与HI_TASKINST不同的是，taskinst只记录usertask内容

| 字段名称           | 字段描述             | 数据类型      | 主键 | 为空 | 取值说明                                |
| ------------------ | -------------------- | ------------- | ---- | ---- | --------------------------------------- |
| ID_                | ID_                  | nvarchar(64)  | √    |      |                                         |
| PROC_DEF_ID_       | 流程定义ID           | nvarchar(64)  |      |      |                                         |
| PROC_INST_ID_      | 流程实例ID           | nvarchar(64)  |      |      |                                         |
| EXECUTION_ID_      | 执行实例ID           | nvarchar(64)  |      |      |                                         |
| ACT_ID_            | 节点ID               | nvarchar(225) |      |      | 节点定义ID                              |
| TASK_ID_           | 任务实例ID           | nvarchar(64)  |      | √    | 任务实例ID 其他节点类型实例ID在这里为空 |
| CALL_PROC_INST_ID_ | 调用外部的流程实例ID | nvarchar(64)  |      | √    | 调用外部流程的流程实例ID’               |
| ACT_NAME_          | 节点名称             | nvarchar(225) |      | √    | 节点定义名称                            |
| ACT_TYPE_          | 节点类型             | nvarchar(225) |      |      | 如startEvent、userTask                  |
| ASSIGNEE_          | 签收人               | nvarchar(64)  |      | √    | 节点签收人                              |
| START_TIME_        | 开始时间             | datetime      |      |      | 2013-09-15 11:30:00                     |
| END_TIME_          | 结束时间             | datetime      |      | √    | 2013-09-15 11:30:00                     |
| DURATION_          | 耗时                 | numeric(19,0) |      | √    | 毫秒值                                  |
|                    |                      |               |      |      |                                         |

 

### ACT_HI_ATTACHMENT表结构

 	历史附件表( act_hi_attachment )

| 字段名称       | 字段描述   | 数据类型       | 主键 | 为空 | 取值说明             |
| -------------- | ---------- | -------------- | ---- | ---- | -------------------- |
| ID_            | ID_        | nvarchar(64)   | √    |      | 主键ID               |
| REV_           | 乐观锁     | integer        |      | √    | Version              |
| USER_ID_       | 用户ID     | nvarchar(255)  |      | √    | 用户ID               |
| NAME_          | 名称       | nvarchar(255)  |      | √    | 附件名称             |
| DESCRIPTION_   | 描述       | nvarchar(4000) |      | √    | 描述                 |
| TYPE_          | 类型       | nvarchar(255)  |      | √    | 附件类型             |
| TASK_ID_       | 任务实例ID | nvarchar(64)   |      | √    | 节点实例ID           |
| PROC_INST_ ID_ | 流程实例ID | nvarchar(64)   |      | √    | 流程实例ID           |
| URL_           | URL_       | nvarchar(4000) |      | √    | 附件地址             |
| CONTENT_ ID_   | 字节表的ID | nvarchar(64)   |      | √    | ACT_GE_BYTEARRAY的ID |

 

### ACT_HI_COMMENT表结构

 历史意见表( act_hi_comment )

| 字段名称       | 字段描述   | 数据类型       | 主键 | 为空 | 取值说明                             |
| -------------- | ---------- | -------------- | ---- | ---- | ------------------------------------ |
| ID_            | ID_        | nvarchar(64)   | √    |      | 主键ID                               |
| TYPE_          | 类型       | nvarchar(255)  |      | √    | 类型：event(事件)comment(意见)       |
| TIME_          | 时间       | datetime       |      |      | 填写时间’                            |
| USER_ID_       | 用户ID     | nvarchar(64)   |      | √    | 填写人                               |
| TASK_ID_       | 节点任务ID | nvarchar(64)   |      | √    | 节点实例ID                           |
| PROC_INST_ ID_ | 流程实例ID | nvarchar(255)  |      | √    | 流程实例ID                           |
| ACTION_        | 行为类型   | nvarchar(64)   |      | √    | 见备注1                              |
| MESSAGE_       | 基本内容   | nvarchar(4000) |      | √    | 用于存放流程产生的信息，比如审批意见 |
| FULL_MSG_      | 全部内容   | varbinary(max) |      | √    | 附件地址                             |

 

### ACT_HI_DETAIL表结构

 	历史详情表( act_hi_detail )

​	流程中产生的变量详细，包括控制流程流转的变量，业务表单中填写的流程需要用到的变量等。

| 字段名称      | 字段描述   | 数据类型         | 主键 | 为空 | 取值说明                                            |
| ------------- | ---------- | ---------------- | ---- | ---- | --------------------------------------------------- |
| ID_           | ID_        | nvarchar(64)     | √    |      | 主键                                                |
| TYPE_         | 类型       | nvarchar(255)    |      |      | 见备注2                                             |
| PROC_INST_ID_ | 流程实例ID | nvarchar(64)     |      | √    | 流程实例ID                                          |
| EXECUTION_ID_ | 执行实例ID | nvarchar(64)     |      | √    | 执行实例ID                                          |
| TASK_ID_      | 任务实例ID | nvarchar(64)     |      | √    | 任务实例ID                                          |
| ACT_INST_ID_  | 节点实例ID | nvarchar(64)     |      | √    | ACT_HI_ACTINST表的ID                                |
| NAME_         | 名称       | nvarchar(255)    |      |      | 名称                                                |
| VAR_TYPE_     | 参数类型   | nvarchar(255)    |      | √    | 见备注3                                             |
| REV_          | 乐观锁     | int              |      | √    | Version                                             |
| TIME_         | 时间戳     | datetime         |      |      | 创建时间                                            |
| BYTEARRAY_ID_ | 字节表ID   | nvarchar         |      | √    | ACT_GE_BYTEARRAY表的ID                              |
| DOUBLE_       | DOUBLE_    | double precision |      | √    | 存储变量类型为Double                                |
| LONG_         | LONG_      | numeric          |      | √    | 存储变量类型为long                                  |
| TEXT_         | TEXT_      | nvarchar         |      | √    | 存储变量值类型为String                              |
| TEXT2_        | TEXT2_     | nvarchar         |      | √    | 此处存储的是JPA持久化对象时，才会有值。此值为对象ID |

 

 ### ACT_HI_IDENTITYLINK表结构

​	历史流程人员表( act_ru_identitylink )

​	任务参与者数据表。主要存储历史节点参与者的信息 

| 字段名称      | 字段描述   | 数据类型      | 主键 | 为空 | 取值说明   |
| ------------- | ---------- | ------------- | ---- | ---- | ---------- |
| ID_           | ID_        | nvarchar(64)  | √    |      | ID_        |
| GROUP_ID_     | 组ID       | nvarchar(255) |      | √    | 组ID       |
| TYPE_         | 类型       | nvarchar(255) |      | √    | 备注4      |
| USER_ID_      | 用户ID     | nvarchar(255) |      | √    | 用户ID     |
| TASK_ID_      | 节点实例ID | nvarchar(64)  |      | √    | 节点实例ID |
| PROC_INST_ID_ | 流程实例ID | nvarchar(64)  |      | √    | 流程实例ID |

 

### ACT_HI_PROCINST表结构

 	历史流程实例表(act_hi_procinst)

| 字段名称                   | 字段描述     | 数据类型       | 主键 | 为空 | 取值说明               |
| -------------------------- | ------------ | -------------- | ---- | ---- | ---------------------- |
| ID_                        | ID_          | nvarchar(64)   | √    |      | 主键ID                 |
| PROC_INST_ID_              | 流程实例ID   | nvarchar(64)   |      |      | 流程实例ID             |
| BUSINESS_KEY_              | 业务主键     | nvarchar(255)  |      | √    | 业务主键，业务表单的ID |
| PROC_DEF_ID_               | 流程定义ID   | nvarchar(64)   |      |      | 流程定义ID             |
| START_TIME_                | 开始时间     | datetime       |      |      | 开始时间               |
| END_TIME_                  | 结束时间     | datetime       |      | √    | 结束时间               |
| DURATION_                  | 耗时         | Numeric(19)    |      | √    | 耗时                   |
| START_USER_ID_             | 起草人       | nvarchar(255)  |      | √    | 起草人                 |
| START_ACT_ID_              | 开始节点ID   | nvarchar(255)  |      | √    | 起草环节ID             |
| END_ACT_ID_                | 结束节点ID   | nvarchar(255)  |      | √    | 结束环节ID             |
| SUPER_PROCESS_INSTANCE_ID_ | 父流程实例ID | nvarchar(64)   |      | √    | 父流程实例ID           |
| DELETE_REASON_             | 删除原因     | nvarchar(4000) |      | √    | 删除原因               |

 

### ACT_HI_TASKINST表结构

 	历史任务实例表( act_hi_taskinst )

| 字段名称        | 字段描述                | 数据类型       | 主键 | 为空 | 取值说明                             |
| --------------- | ----------------------- | -------------- | ---- | ---- | ------------------------------------ |
| ID_             | ID_                     | nvarchar(64)   | √    |      | 主键ID                               |
| PROC_DEF_ID_    | 流程定义ID              | nvarchar(64)   |      | √    | 流程定义ID                           |
| TASK_DEF_KEY_   | 节点定义ID              | nvarchar(255)  |      | √    | 节点定义ID                           |
| PROC_INST_ID_   | 流程实例ID              | nvarchar(64)   |      | √    | 流程实例ID                           |
| EXECUTION_ID_   | 执行实例ID              | nvarchar(64)   |      | √    | 执行实例ID                           |
| NAME_           | 名称                    | varchar(255)   |      | √    | 名称                                 |
| PARENT_TASK_ID_ | 父节点实例ID            | nvarchar(64)   |      | √    | 父节点实例ID                         |
| DESCRIPTION_    | 描述                    | nvarchar(400)  |      | √    | 描述                                 |
| OWNER_          | 实际签收人 任务的拥有者 | nvarchar(255)  |      | √    | 签收人(默认为空，只有在委托时才有值) |
| ASSIGNEE_       | 签收人或被委托          | nvarchar(255)  |      | √    | 签收人或被委托                       |
| START_TIME_     | 开始时间                | datetime       |      |      | 开始时间                             |
| CLAIM_TIME_     | 提醒时间                | datetime       |      | √    | 提醒时间                             |
| END_TIME_       | 结束时间                | datetime       |      | √    | 结束时间                             |
| DURATION_       | 耗时                    | numeric(19)    |      | √    | 耗时                                 |
| DELETE_REASON_  | 删除原因                | nvarchar(4000) |      | √    | 删除原因(completed,deleted)          |
| PRIORITY_       | 优先级别                | int            |      | √    | 优先级别                             |
| DUE_DATE_       | 过期时间                | datetime       |      | √    | 过期时间，表明任务应在多长时间内完成 |
| FORM_KEY_       | 节点定义的formkey       | nvarchar(255)  |      | √    | desinger节点定义的form_key属性       |

 

### ACT_HI_VARINST表结构 

 历史变量表( act_hi_varinst )

| 字段名称      | 字段描述   | 数据类型      | 主键 | 为空 | 取值说明                                            |
| ------------- | ---------- | ------------- | ---- | ---- | --------------------------------------------------- |
| ID_           | ID_        | nvarchar(64)  | √    |      | ID_                                                 |
| PROC_INST_ID_ | 流程实例ID | nvarchar(64)  |      | √    | 流程实例ID                                          |
| EXECUTION_ID_ | 执行实例ID | nvarchar(255) |      | √    | 执行实例ID                                          |
| TASK_ID_      | 任务实例ID | nvarchar(64)  |      | √    | 任务实例ID                                          |
| NAME_         | 名称       | nvarchar(64)  |      |      | 参数名称(英文)                                      |
| VAR_TYPE_     | 参数类型   | varchar(255)  |      | √    | 备注5                                               |
| REV_          | 乐观锁     | nvarchar(64)  |      | √    | 乐观锁 Version                                      |
| BYTEARRAY_ID_ | 字节表ID   | nvarchar(400) |      | √    | ACT_GE_BYTEARRAY表的主键                            |
| DOUBLE_       | DOUBLE_    | nvarchar(255) |      | √    | 存储DoubleType类型的数据                            |
| LONG_         | LONG_      | nvarchar(255) |      | √    | 存储LongType类型的数据                              |
| TEXT_         | TEXT_      | datetime      |      | √    | 备注6                                               |
| TEXT2_        | TEXT2_     | datetime      |      |      | 此处存储的是JPA持久化对象时，才会有值。此值为对象ID |

 

### ACT_ID_GROUP表结构

 用户组信息表( act_id_group )

| 字段名称 | 字段描述 | 数据类型      | 主键 | 为空 | 取值说明      |
| -------- | -------- | ------------- | ---- | ---- | ------------- |
| ID_      | ID_      | nvarchar(64)  | √    |      | 主键ID        |
| REV_     | 乐观锁   | int           |      | √    | 乐观锁Version |
| NAME_    | 名称     | nvarchar(255) |      | √    | 组名称        |
| TYPE_    | 类型     | nvarchar(255) |      | √    | 类型          |

 

### ACT_ID_INFO表结构

 用户扩展信息表( act_id_info )

| 字段名称   | 字段描述 | 数据类型      | 主键 | 为空 | 取值说明      |
| ---------- | -------- | ------------- | ---- | ---- | ------------- |
| ID_        | ID_      | nvarchar(64)  | √    |      | 主键ID        |
| REV_       | 乐观锁   | int           |      | √    | 乐观锁Version |
| USER_ID_   | 用户ID   | nvarchar(64)  |      | √    |               |
| TYPE_      | 类型     | nvarchar(64)  |      | √    |               |
| KEY_       |          | nvarchar(255) |      | √    |               |
| VALUE_     |          | nvarchar(255) |      | √    |               |
| PASSWORD_  |          | Image         |      | √    |               |
| PARENT_ID_ |          | nvarchar(255) |      | √    |               |

 

### ACT_ID_MEMBERSHIP表结构

 用户与分组对应信息表( act_id_membership )

用来保存用户的分组信息。

| 字段名称 | 字段描述 | 数据类型     | 主键 | 为空 | 取值说明 |
| -------- | -------- | ------------ | ---- | ---- | -------- |
| USER_ID  | 用户ID   | nvarchar(64) | √    |      |          |
| GROUP_ID | 用户组ID | nvarchar(64) | √    |      |          |

 

### ACT_ID_USER表结构

 用户信息表( act_id_user )

| 字段名称     | 字段描述 | 数据类型      | 主键 | 为空 | 取值说明      |
| ------------ | -------- | ------------- | ---- | ---- | ------------- |
| ID_          | ID_      | nvarchar(64)  | √    |      | 主键ID        |
| REV_         | 乐观锁   | int           |      | √    | 乐观锁Version |
| FIRST_       | 姓       | nvarchar(255) |      | √    |               |
| LAST_        | 名       | nvarchar(255) |      | √    |               |
| EMAIL_       | EMAIL_   | nvarchar(255) |      | √    |               |
| PWD_         | 密码     | nvarchar(255) |      | √    |               |
| PICTURE_ ID_ | 图片ID   | nvarchar(64)  |      | √    |               |

 

### ACT_RE_DEPLOYMENT表结构

部署信息表( act_re_deployment )

部署流程定义时需要被持久化保存下来的信息。

| 字段名称     | 字段描述 | 数据类型      | 主键 | 为空 | 取值说明   |
| ------------ | -------- | ------------- | ---- | ---- | ---------- |
| ID_          | ID_      | nvarchar(64)  | √    |      | 主键ID     |
| CATEGORY_    | 分类     | nvarchar(255) |      | √    | 类别       |
| DEPLOY_TIME_ | 部署时间 | datetime      |      | √    | 部署时间   |
| NAME_        | 部署名称 | nvarchar(255) |      | √    | 部署文件名 |

 

### ACT_RE_MODEL表结构

流程设计模型部署表( act_re_model )

流程设计器设计流程后，保存数据到该表。

| 字段名称                      | 字段描述     | 数据类型      | 主键 | 为空 | 取值说明                     |
| ----------------------------- | ------------ | ------------- | ---- | ---- | ---------------------------- |
| ID_                           | ID_          | nvarchar(64)  | √    |      | ID_                          |
| REV_                          | 乐观锁       | int           |      | √    | 乐观锁                       |
| NAME_                         | 名称         | nvarchar(255) |      | √    | 名称                         |
| KEY_                          | KEY_         | nvarchar(255) |      | √    | key                          |
| CATEGORY_                     | 分类         | nvarchar(255) |      | √    | 分类                         |
| CREATE_TIME_                  | 创建时间     | datetime      |      | √    | 创建时间                     |
| LAST_UPDATE_TIME_             | 最新修改时间 | datetime      |      | √    | 最新修改时间                 |
| VERSION_                      | 版本         | int           |      | √    | 版本                         |
| META_INFO_                    | META_INFO_   | nvarchar(255) |      | √    | 以json格式保存流程定义的信息 |
| DEPLOYMENT_ID_                | 部署ID       | nvarchar(255) |      | √    | 部署ID                       |
| EDITOR_SOURCE_VALUE_ID_       |              | datetime      |      | √    |                              |
| EDITOR_SOURCE_EXTRA_VALUE_ID_ |              | datetime      |      | √    |                              |

 

### ACT_RE_PROCDEF表结构

流程定义数据表( act_re_procdef )

业务流程定义数据表。此表和 ACT_RE_DEPLOYMENT 是多对一的关系，即，一个部署的bar包里可能包含多个流程定义文件，每个流程定义文件都会有一条记录在 ACT_REPROCDEF 表内，每个流程定义的数据，都会对于 ACT_GE_BYTEARRAY 表内的一个资源文件和 PNG 图片文件。和 ACT_GE_BYTEARRAY 的关联是通过程序用ACT_GE_BYTEARRAY.NAME 与 ACT_RE_PROCDEF.NAME 完成的，在数据库表结构中没有体现。 

| 字段名称            | 字段描述                | 数据类型       | 主键 | 为空 | 取值说明                         |
| ------------------- | ----------------------- | -------------- | ---- | ---- | -------------------------------- |
| ID_                 | ID_                     | nvarchar(64)   | √    |      | ID_                              |
| REV_                | 乐观锁                  | int            |      | √    | 乐观锁                           |
| CATEGORY_           | 分类                    | nvarchar(255)  |      | √    | 流程定义的Namespace就是类别      |
| NAME_               | 名称                    | nvarchar(255)  |      | √    | 名称                             |
| KEY_                | 定义的KEY               | nvarchar(255)  |      |      | 流程定义ID                       |
| VERSION_            | 版本                    | int            |      |      | 版本                             |
| DEPLOYMENT_ID_      | 部署表ID                | nvarchar(64)   |      | √    | 部署表ID                         |
| RESOURCE_NAME_      | bpmn文件名称            | nvarchar(4000) |      | √    | 流程bpmn文件名称                 |
| DGRM_RESOURCE_NAME_ | png图片名称             | nvarchar(4000) |      | √    | 流程图片名称                     |
| DESCRIPTION_        | 描述                    | nvarchar(4000) |      | √    | 描述                             |
| HAS_START_FORM_KEY_ | 是否存在开始节点formKey | tinyint        |      | √    | start节点是否存在formKey 0否 1是 |
| SUSPENSION_STATE_   | 是否挂起                | tinyint        |      | √    | 1 激活 2挂起                     |

 

### ACT_RU_EVENT_SUBSCR表结构

 

 

| 字段名称       | 字段描述   | 数据类型      | 主键 | 为空 | 取值说明                               |
| -------------- | ---------- | ------------- | ---- | ---- | -------------------------------------- |
| ID_            | 事件ID     | nvarchar(64)  | √    |      | 事件ID                                 |
| REV_           | 版本       | int           |      | √    | 乐观锁Version                          |
| EVENT_TYPE_    | 事件类型   | nvarchar(255) |      |      | 事件类型                               |
| EVENT_NAME_    | 事件名称   | nvarchar(255) |      | √    | 事件名称                               |
| EXECUTION_ID_  | 执行实例ID | nvarchar(64)  |      | √    | 执行实例ID                             |
| PROC_INST_ID_  | 流程实例ID | nvarchar(64)  |      | √    | 流程实例ID                             |
| ACTIVITY_ID_   | 活动实例ID | nvarchar(64)  |      | √    | 活动实例ID                             |
| CONFIGURATION_ | 配置       | nvarchar(255) |      | √    | 配置                                   |
| CREATED_       | 是否创建   | datetime      |      |      | 默认值 当前系统时间戳CURRENT_TIMESTAMP |



### ACT_RU_EXECUTION表结构

运行时流程执行实例表( act_ru_execution )

| 字段名称          | 字段描述        | 数据类型      | 主键 | 为空 | 取值说明                       |
| ----------------- | --------------- | ------------- | ---- | ---- | ------------------------------ |
| ID_               | ID_             | nvarchar(64)  | √    |      | ID_                            |
| REV_              | 乐观锁          | int           |      | √    | 乐观锁                         |
| PROC_INST_ID_     | 流程实例ID      | nvarchar(64)  |      |      | 流程实例ID                     |
| BUSINESS_KEY_     | 业务主键ID      | nvarchar(255) |      | √    | 业务主键ID                     |
| PARENT_ID_        | 父节点实例ID    | nvarchar(64)  |      | √    | 父节点实例ID                   |
| PROC_DEF_ID_      | 流程定义ID      | nvarchar(64)  |      | √    | 流程定义ID                     |
| SUPER_EXEC_       | SUPER_EXEC_     | nvarchar(64)  |      | √    | SUPER_EXEC_                    |
| ACT_ID_           | 节点实例ID      | nvarchar(255) |      | √    | 节点实例ID即ACT_HI_ACTINST中ID |
| IS_ACTIVE_        | 是否存活        | tinyint       |      | √    | 是否存活                       |
| IS_CONCURRENT_    | 是否并行        | tinyint       |      | √    | 是否为并行(true/false)         |
| IS_SCOPE_         | IS_SCOPE_       | tinyint       |      | √    | IS_SCOPE_                      |
| IS_EVENT_SCOPE_   | IS_EVENT_SCOPE_ | tinyint       |      | √    | IS_EVENT_SCOPE_                |
| SUSPENSION_STATE_ | 是否挂起        | tinyint       |      | √    | 挂起状态 1激活 2挂起           |
| CACHED_ENT_STATE_ |                 | int           |      | √    |                                |

 



### ACT_RU_IDENTITYLINK表结构

运行时流程人员表( act_ru_identitylink )

任务参与者数据表。主要存储当前节点参与者的信息。

| 字段名称      | 字段描述   | 数据类型      | 主键 | 为空 | 取值说明   |
| ------------- | ---------- | ------------- | ---- | ---- | ---------- |
| ID_           | ID_        | nvarchar(64)  | √    |      | ID_        |
| REV_          | 乐观锁     | int           |      | √    | 乐观锁     |
| GROUP_ID_     | 组ID       | nvarchar(64)  |      | √    | 组ID       |
| TYPE_         | 类型       | nvarchar(255) |      | √    | 备注7      |
| USER_ID_      | 用户ID     | nvarchar(64)  |      | √    | 用户ID     |
| TASK_ID_      | 节点实例ID | nvarchar(64)  |      | √    | 节点实例ID |
| PROC_INST_ID_ | 流程实例ID | nvarchar(64)  |      | √    | 流程实例ID |
| PROC_DEF_ID_  | 流程定义ID | nvarchar(255) |      | √    | 流程定义ID |

 

### ACT_RU_JOB表结构

运行时定时任务数据表( act_ru_job )

| 字段名称             | 字段描述     | 数据类型       | 主键 | 为空 | 取值说明     |
| -------------------- | ------------ | -------------- | ---- | ---- | ------------ |
| ID_                  | 标识         | nvarchar(64)   | √    |      | 标识         |
| REV_                 | 版本         | int            |      | √    | 版本         |
| TYPE_                | 类型         | nvarchar(255)  |      |      | 类型         |
| LOCK_EXP_TIME_       | 锁定释放时间 | datetime       |      | √    | 锁定释放时间 |
| LOCK_OWNER_          | 挂起者       | nvarchar(255)  |      | √    | 挂起者       |
| EXCLUSIVE_           |              | bit            |      | √    |              |
| EXECUTION_ID_        | 执行实例ID   | nvarchar(64)   |      | √    | 执行实例ID   |
| PROCESS_INSTANCE_ID_ | 流程实例ID   | nvarchar(64)   |      | √    | 流程实例ID   |
| PROC_DEF_ID_         | 流程定义ID   | nvarchar(64)   |      | √    | 流程定义ID   |
| RETRIES_             |              | int            |      | √    |              |
| EXCEPTION_STACK_ID_  | 异常信息ID   | nvarchar(64)   |      | √    | 异常信息ID   |
| EXCEPTION_MSG_       | 异常信息     | nvarchar(4000) |      | √    | 异常信息     |
| DUEDATE_             | 到期时间     | datetime       |      | √    | 到期时间     |
| REPEAT_              | 重复         | nvarchar(255)  |      | √    | 重复         |
| HANDLER_TYPE_        | 处理类型     | nvarchar(255)  |      | √    | 处理类型     |
| HANDLER_CFG_         |              | nvarchar(4000) |      | √    | 标识         |

 

### ACT_RU_TASK表结构

运行时任务节点表( act_ru_task )

| 字段名称          | 字段描述       | 数据类型       | 主键 | 为空 | 取值说明                                   |
| ----------------- | -------------- | -------------- | ---- | ---- | ------------------------------------------ |
| ID_               | ID_            | nvarchar(64)   | √    |      | ID_                                        |
| REV_              | 乐观锁         | int            |      | √    | 乐观锁                                     |
| EXECUTION_ID_     | 执行实例ID     | nvarchar(64)   |      | √    | 执行实例ID                                 |
| PROC_INST_ID_     | 流程实例ID     | nvarchar(64)   |      | √    | 流程实例ID                                 |
| PROC_DEF_ID_      | 流程定义ID     | nvarchar(64)   |      | √    | 流程定义ID                                 |
| NAME_             | 节点定义名称   | nvarchar(255)  |      | √    | 节点定义名称                               |
| PARENT_TASK_ID_   | 父节点实例ID   | nvarchar(64)   |      | √    | 父节点实例ID                               |
| DESCRIPTION_      | 节点定义描述   | nvarchar(4000) |      | √    | 节点定义描述                               |
| TASK_DEF_KEY_     | 节点定义的KEY  | nvarchar(255)  |      | √    | 任务定义的ID                               |
| OWNER_            | 实际签收人     | nvarchar(255)  |      | √    | 拥有者(一般情况下为空，只有在委托时才有值) |
| ASSIGNEE_         | 签收人或委托人 | nvarchar(255)  |      | √    | 签收人或委托人                             |
| DELEGATION_       | 委托类型       | nvarchar(64)   |      | √    | 备注8                                      |
| PRIORITY_         | 优先级别       | int            |      | √    | 优先级别，默认为：50                       |
| CREATE_TIME_      | 创建时间       | datetime       |      | √    | 创建时间                                   |
| DUE_DATE_         | 过期时间       | datetime       |      | √    | 耗时                                       |
| SUSPENSION_STATE_ | 是否挂起       | int            |      | √    | 1代表激活 2代表挂起                        |



### ACT_RU_VARIABLE表结构

运行时流程变量数据表( act_ru_variable ) 

| 字段名称      | 字段描述   | 数据类型       | 主键 | 为空 | 取值说明                                                     |
| ------------- | ---------- | -------------- | ---- | ---- | ------------------------------------------------------------ |
| ID_           | ID_        | nvarchar(64)   | √    |      | 主键标识                                                     |
| REV_          | 乐观锁     | int            |      | √    | 乐观锁                                                       |
| TYPE_         | 类型       | nvarchar(255)  |      |      | 备注9                                                        |
| NAME_         | 名称       | nvarchar(255)  |      |      | 变量名称                                                     |
| EXECUTION_ID_ | 执行实例ID | nvarchar(64)   |      | √    | 执行的ID                                                     |
| PROC_INST_ID_ | 流程实例ID | nvarchar(64)   |      | √    | 流程实例ID                                                   |
| TASK_ID_      | 节点实例ID | nvarchar(64)   |      | √    | 节点实例ID(Local)                                            |
| BYTEARRAY_ID_ | 字节表ID   | nvarchar(64)   |      | √    | 字节表的ID(ACT_GE_BYTEARRAY)                                 |
| DOUBLE_       | DOUBLE_    | float          |      | √    | 存储变量类型为Double                                         |
| LONG_         | LONG_      | numeric(19)    |      | √    | 存储变量类型为long                                           |
| TEXT_         | TEXT_      | nvarchar(4000) |      | √    | ‘存储变量值类型为String 如此处存储持久化对象时，值jpa对象的class |
| TEXT2_        | TEXT2_     | nvarchar(4000) |      | √    | 此处存储的是JPA持久化对象时，  才会有值。此值为对象ID        |

 

### ACT_RU_EXECUTION表结构

运行时流程执行实例表( act_ru_execution )	 

| 字段名称          | 字段描述        | 数据类型      | 主键 | 为空 | 取值说明                       |
| ----------------- | --------------- | ------------- | ---- | ---- | ------------------------------ |
| ID_               | ID_             | nvarchar(64)  | √    |      | ID_                            |
| REV_              | 乐观锁          | int           |      | √    | 乐观锁                         |
| PROC_INST_ID_     | 流程实例ID      | nvarchar(64)  |      |      | 流程实例ID                     |
| BUSINESS_KEY_     | 业务主键ID      | nvarchar(255) |      | √    | 业务主键ID                     |
| PARENT_ID_        | 父节点实例ID    | nvarchar(64)  |      | √    | 父节点实例ID                   |
| PROC_DEF_ID_      | 流程定义ID      | nvarchar(64)  |      | √    | 流程定义ID                     |
| SUPER_EXEC_       | SUPER_EXEC_     | nvarchar(64)  |      | √    | SUPER_EXEC_                    |
| ACT_ID_           | 节点实例ID      | nvarchar(255) |      | √    | 节点实例ID即ACT_HI_ACTINST中ID |
| IS_ACTIVE_        | 是否存活        | tinyint       |      | √    | 是否存活                       |
| IS_CONCURRENT_    | 是否并行        | tinyint       |      | √    | 是否为并行(true/false)         |
| IS_SCOPE_         | IS_SCOPE_       | tinyint       |      | √    | IS_SCOPE_                      |
| IS_EVENT_SCOPE_   | IS_EVENT_SCOPE_ | tinyint       |      | √    | IS_EVENT_SCOPE_                |
| SUSPENSION_STATE_ | 是否挂起        | tinyint       |      | √    | 挂起状态 1激活 2挂起           |
| CACHED_ENT_STATE_ |                 | int           |      | √    |                                |

 

## 操作activiti的各个环节涉及到的关键表

\# 部署流程定义涉及到的表

\# 流程部署表

SELECT * FROM `act_re_deployment`

\# 流程定义表

SELECT * FROM `act_re_procdef`

\# 资源文件表

SELECT * FROM `act_ge_bytearray`

\# 系统配置表

SELECT * FROM `act_ge_property`

 

\################################

\# 启动流程实例涉及到的表

\# 流程实例运行时 执行对象表

SELECT * FROM `act_ru_execution`

\# 流程实例运行时 身份联系表

SELECT * FROM `act_ru_identitylink`

\# 流程实例运行时 用户任务表

SELECT * FROM `act_ru_task`

\# 活动节点历史表

SELECT * FROM `act_hi_actinst`

\# 身份联系表 历史

SELECT * FROM `act_hi_identitylink`

\# 流程实例表 历史

SELECT * FROM `act_hi_procinst`

\# 历史任务表

SELECT * FROM `act_hi_taskinst`

\# 结束流程实例涉及到的表

\# 运行时 表数据全部清空

\# 历史表 表数据修改 或者增加了数据

2. **流程定****义与图数据库保存结构**

会更新ACT_RE_MODEL和ACT_GE_BYTEARRAY表，

ACT_GE_BYTEARRAY表保存了PNG图片和xml文件信息(其实是json格式的字符串)，并关联到ACT_RE_MODEL表。

ACT_GE_BYTEARRAY表中NAME_列值source-extra行中存储的是：PNG图片

ACT_GE_BYTEARRAY表中NAME_列值source行中存储的是：xml文件信息

 

流程设计器设计流程后，保存数据到ACT_RE_MODEL 表。

EDITOR_SOURCE_VALUE_ID(流程文件放在ACT_GE_BYTEARRAY中的ID)；

EDITOR_SOURCE_EXTRA_VALUE_ID(流程文件图像放在ACT_GE_BYTEARRAY中的ID)。 

 