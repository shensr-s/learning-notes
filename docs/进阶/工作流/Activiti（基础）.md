# Activiti（基础）

## 工作流简介

### 1. 工作流

*工作流*(Workflow)，就是通过计算机对业务流程**自动化**执行管理。它主要解决的是“使在多个参与者之间按照某种预定义的规则自动进行传递文档、信息或任务的过程，从而实现某个预期的业务目标，或者促使此目标的实现”。

### 2. 工作流系统

一个软件系统中具有工作流的功能，我们把它称为工作流系统。 如果一个系统具备**流程的自动化管理功能**，这个系统就可以称为工作流系统

#### 2.1 适用行业

消费品行业，制造业，电信服务业，银证险等金融服务业，物流服务业，物业服务业，物业管理，大中型进出口贸易公司，政府事业机构，研究院所及教育服务业等，特别是大的跨国企业和集团公司。

#### 2.2 具体应用

1. 关键业务流程：订单、报价处理、合同审核、客户电话处理、供应链管理等
2. 行政管理类:出差申请、加班申请、请假申请、用车申请、各种办公用品申请、购买申请、日报
周报等凡是原来手工流转处理的行政表单。
3. 人事管理类：员工培训安排、绩效考评、职位变动处理、员工档案信息管理等。
4. 财务相关类：付款请求、应收款处理、日常报销处理、出差报销、预算和计划申请等。
5. 客户服务类：客户信息管理、客户投诉、请求处理、售后服务管理等。

6. 特殊服务类：ISO系列对应流程、质量管理对应流程、产品数据信息管理、贸易公司报关处理、
物流公司货物跟踪处理等各种通过表单逐步手工流转完成的任务均可应用工作流软件自动规范
地实施。

### 3. 工作流实现方式

**传统**：

- 采用状态字段的值来跟踪流程的变化情况
- 需求变更，代码也要变更

**工作流**：

- 业务流程变化之后，程序可以不用改变
- 业务系统的适应能力就得到了极大提升

### 4. 工作流实现原理分析

使用工作流的方式实现流程管理优势在于与状态字段无关，它始终都是从业务流程图中不断读取下一个节点，当业务更新时，只要更新业务流程图，程序代码无需改动。

结点定义是需要规范的，`bpmn`就是这一套规范，而`activiti`作为一个工作流的软件也实现了这一套规范。

*要点*：表数据与节点数据的转换

![image-20200831221103007](https://gitee.com/szimo/picture_repository/raw/master/images/20200831221103.png)

## Activiti7

### 1. Activiti 介绍

 activiti是一个工作流引擎

官网地址：https://www.activiti.org/

### 2. Activiti 使用

#### 部署 activiti

#### 流程定义

#### 流程定义部署

#### 启动一个流程实例（ProcessInstance ）

#### 用户查询待办任务(Task)

#### 用户办理任务

#### 流程结束

## 环境准备

### 1. 开发环境

*Java环境* Jdk1.8或以上版本

*数据库* Mysql 5 及以上的版本

*Web 容器* 本项目采用的是Tomcat8.5

*开发工具* IDEA

### 2. activiti环境

#### Activiti Designer 流程设计器(IDEA 工具)

IntelliJ IDEA 2019.2.4 (Ultimate Edition)版本在`plugins`中搜索不到*actiBPM*。进入JetBrains 官网下载https://plugins.jetbrains.com/plugin/7429-actibpm/versions进行下载即可

![image-20200901171939461](https://gitee.com/szimo/picture_repository/raw/master/images/20200901171939.png)

IDEA中选择本地安装（Install Plugin from Disk…）即可

![image-20200901172055380](https://gitee.com/szimo/picture_repository/raw/master/images/20200901172055.png)

### 3. Activiti 支持的数据库

![image-20200901173627031](https://gitee.com/szimo/picture_repository/raw/master/images/20200901173627.png)

### 4. 创建mysql数据库

本教程使用`mysql`数据库。
创建 `mysql`数据库 `activiti`（名字任意）：
`CREATE DATABASE activiti DEFAULT CHARACTER SET utf8;`

### 5. 创建表方式

通过运行`java`程序创建表。

#### 5.1 创建`java`工程

#### 5.2 加入`maven`依赖的坐标（jar 包）

在Java工程中加入`ProcessEngine`所需要的jar包，包括：

1) activiti-engine-7.0.0.beta1.jar
2) activiti依赖的 jar 包：mybatis、alf4j、log4j 等
3) activiti依赖的 spring包
4) 数据库驱动
5) 第三方数据连接池 dbcp
6) 单元测试 Junit-4.12.jar

```xml
<properties>
        <slf4j.version>1.6.6</slf4j.version>
        <log4j.version>1.2.12</log4j.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.activiti</groupId>
            <artifactId>activiti-engine</artifactId>
            <version>7.0.0.Beta1</version>
        </dependency>

        <dependency>
            <groupId>org.activiti</groupId>
            <artifactId>activiti-spring</artifactId>
            <version>7.0.0.Beta1</version>
        </dependency>

        <dependency>
            <groupId>org.activiti</groupId>
            <artifactId>activiti-bpmn-model</artifactId>
            <version>7.0.0.Beta1</version>
        </dependency>

        <dependency>
            <groupId>org.activiti</groupId>
            <artifactId>activiti-bpmn-converter</artifactId>
            <version>7.0.0.Beta1</version>
        </dependency>

        <dependency>
            <groupId>org.activiti</groupId>
            <artifactId>activiti-json-converter</artifactId>
            <version>7.0.0.Beta1</version>
        </dependency>

        <dependency>
            <groupId>org.activiti</groupId>
            <artifactId>activiti-bpmn-layout</artifactId>
            <version>7.0.0.Beta1</version>
        </dependency>

        <dependency>
            <groupId>org.activiti.cloud</groupId>
            <artifactId>activiti-cloud-services-api</artifactId>
            <version>7.0.0.Beta1</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.40</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>

        <!-- log start -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>${log4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <!-- log end -->

        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>

        <dependency>
            <groupId>commons-dbcp</groupId>
            <artifactId>commons-dbcp</artifactId>
            <version>1.4</version>
        </dependency>
    </dependencies>
```

#### 5.3 log4j.properties

```properties
# Set root category priority to INFO and its only appender to CONSOLE.
#log4j.rootCategory=INFO, CONSOLE            debug   info   warn error fatal
log4j.rootCategory=debug, CONSOLE, LOGFILE

# Set the enterprise logger category to FATAL and its only appender to CONSOLE.
log4j.logger.org.apache.axis.enterprise=FATAL, CONSOLE

# CONSOLE is set to be a ConsoleAppender using a PatternLayout.
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.CONSOLE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n

# LOGFILE is set to be a File appender using a PatternLayout.
log4j.appender.LOGFILE=org.apache.log4j.FileAppender
log4j.appender.LOGFILE.File=D:/axis.log
log4j.appender.LOGFILE.Append=true
log4j.appender.LOGFILE.layout=org.apache.log4j.PatternLayout
log4j.appender.LOGFILE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n

```

#### 5.4 activiti.cfg.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
						http://www.springframework.org/schema/contex http://www.springframework.org/schema/context/spring-context.xsd
						http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--数据源配置dbcp-->
    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/activiti"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </bean>
    <!--activiti单独运行的ProcessEngine配置对象(processEngineConfiguration),使用单独启动方式
        默认情况下：bean的id=processEngineConfiguration-->

    <bean id="processEngineConfiguration" class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">
        <!--代表数据源-->
        <property name="dataSource" ref="dataSource"/>
        <!-- dataSource 和 和 processEngineConfiguration 也可以使用一次性配置出来-->
        <!--<property name="jdbcDriver" value="com.mysql.jdbc.Driver"/>-->
        <!--<property name="jdbcUrl" value="jdbc:mysql://localhost:3306/activiti"/>-->
        <!--<property name="jdbcUsername" value="root"/>-->
        <!--<property name="jdbcPassword" value="root"/>-->
        <!--activiti数据库表处理策略
                            false（默认）：检查数据库表的版本和依赖库的版本， 如果版本不匹配就抛出异常。
                            true: 构建流程引擎时，执行检查，如果需要就执行更新。 如果表不存在，就创建。
                            create-drop: 构建流程引擎时创建数据库表， 关闭流程引擎时删除这些表。
                            drop-create：先删除表再创建表。
                            create: 构建流程引擎时创建数据库表， 关闭流程引擎时不删除这些表。
        -->
        <property name="databaseSchemaUpdate" value="true"/>
    </bean>
</beans>
```

#### 5.5 编写程序

创建 `ProcessEngineConfiguration`，通过 `ProcessEngineConfiguration` 创建 `ProcessEngine`，在创建`ProcessEngine` 时会自动创建数据库。

```java
public class ActivitiTest {
    @Test
    public void testCreateTable() {
        // 1. 创建ProcessEngineConfiguration 第二个参数如果在activiti.cfg.xml文件中配置为processEngineConfiguration，可不填写
        ProcessEngineConfiguration configuration = ProcessEngineConfiguration.createProcessEngineConfigurationFromResource("activiti.cfg.xml","processEngineConfiguration");
        // 2. 通过ProcessEngineConfiguration创建ProcessEngine，此时会创建数据库
        ProcessEngine processEngine = configuration.buildProcessEngine();
    }
}
```

#### 5.6 数据库表的命名规则

`Activiti` 的表都以 `ACT`开头。 第二部分是表示表的用途的两个字母标识。 用途也和服务的 `API` 对应。

- `ACT_RE`: '`RE`'表示 `repository`。 这个前缀的表包含了流程定义和流程静态资源 （图片，规则，等等）。
- `ACT_RU_`: '`RU`'表示 `runtime`。 这些运行时的表，包含流程实例，任务，变量，异步任务，等运行中的数据。 `Activiti`只在流程实例执行过程中保存这些数据， 在流程结束时就会删除这些记录。 这样运行时表可以一直很小速度很快。
- `ACT_HI`: '`HI`'表示 `history`。 这些表包含历史数据，比如历史流程实例， 变量，任务等等。
- `ACT_GE`: `GE`表示 `general`。通用数据， 用于不同场景下。

## Activiti 服务架构图

![image-20200901182916078](https://gitee.com/szimo/picture_repository/raw/master/images/20200901182916.png)

> 在新版本中，我们通过实验可以发现 `IdentityService`，`FormService` 两个`Serivce` 都已经删除了。
>
> 所以后面我们对于这两个 `Service` 也不讲解了，但老版本中还是有这两个 `Service`。

### 1. activiti.cfg.xml

`activiti`的引擎配置文件，包括：`ProcessEngineConfiguration`的定义、数据源定义、事务管理器等，此文件其实就是一个`spring`配置文件

### 2. ProcessEngineConfiguration 

流程引擎的配置类，通过`ProcessEngineConfiguration`可以创建工作流引擎`ProceccEngine`，常用的两种方法如下：

#### 2.1 StandaloneProcessEngineConfiguration

通过 `org.activiti.engine.impl.cfg.Stan daloneProcessEngineConfiguration`

`Activiti` 可以单独运行，使用它创建的 `ProcessEngine`，`Activiti` 会自己处理事务。

> 配置文件方式：
> 通常在 `activiti.cfg.xml` 配置文件中定义一个 id为 `processEngineConfiguration` 的 bean，这里会使用 `spring`的依赖注入来构建引擎。
> 方法如下：

```xml
<bean id="processEngineConfiguration" class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">
    <!--代表数据源-->
    <property name="dataSource" ref="dataSource"/>
    <!--数据库策略-->
    <property name="databaseSchemaUpdate" value="true"/>
</bean>
```

#### 2.2 SpringProcessEngineConfiguration

通过 `org.activiti.spring.SpringProcessEngineConfiguration` 与 `Spring` 整合。

创建 spring与 activiti的整合配置文件：

activity-spring.cfg.xml（名称不固定）

### 3. Service

#### 3.1 Service 创建方式

> 通过 `ProcessEngine` 创建 `Service`，`Service` 是工作流引擎提供用于进行工作流部署、执行、管理的服务接口。
> 方式如下：

```java
RuntimeService runtimeService = processEngine.getRuntimeService();
RepositoryService repositoryService = processEngine.getRepositoryService();
TaskService taskService = processEngine.getTaskService();
//......
```

#### 3.2 Service 总览

| Service             | 描述                      |
| :------------------ | :------------------------ |
| *RepositoryService* | activiti 的资源管理类     |
| *RuntimeService*    | activiti 的流程运行管理类 |
| *TaskService*       | activiti 的任务管理类     |
| *HistoryService*    | activiti 的历史管理类     |
| ManagerService      | activiti 的引擎管理类     |

> 注：红色标注为常用 service。

## Activiti 入门体验

项目目录结构：

![image-20200902170714804](https://gitee.com/szimo/picture_repository/raw/master/images/20200902170714.png)

### 1. 流程定义部署

将线下定义的流程部署到`activiti`数据库中，这就是流程定义部署，通过调用`activiti`的`api`将流程定义的`bpmn`和`png`两个文件一个一个添加部署到`activiti`中，也可以将两个文件打成`zip`包进行部署。

#### 1.1 单文件部署

```java
/**
 * 流程定义的部署
 * activiti 表有哪些
 *      act_re_deployment   部署信息
 *      act_re_procdef      流程定义的一些信息
 *      act_ge_bttearray    流程定义的bpmn文件以及陪你过文件
 **/

public class ActivitiDeployment {

    public static void main(String[] args) {
        // 1.创建定义部署
        ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
        // 2.得到RepositoryService 实例
        RepositoryService repositoryService = processEngine.getRepositoryService();
        // 3. 进行部署
        Deployment deployment = repositoryService.createDeployment()
                .addClasspathResource("bpmn/test.bpmn")
                .addClasspathResource("bpmn/test.png")
                .name("请假流程")
                .deploy();
        // 4.输出部署的而一些信息
        System.out.println(deployment.getId());
        System.out.println(deployment.getName());

    }
}
```

#### 1.2 压缩包部署

……

#### 1.3 操作数据表

流程定义部署后操作`activiti`数据表如下：

`SELECT * FROM act_re_deployment` #流程定义部署表，记录流程部署信息

`SELECT * FROM act_re_procdef` #流程定义表，记录流程定义信息

`SELECT * FROM act_ge_bytearray` #资源表

*说明*：

- act_re_deployment 和 act_re_procdef 一对多关系，一次部署在流程部署表生成一条记录，但一次部署可以部署多个流程定义，每个流程定义在流程定义表生成一条记录。每一个流程定义在`act_ge_bytearray`会存在两个资源记录bpmn 和 png。

*建议*：

- 一次部署一个流程，这样部署表和流程定义表是一对一的关系，方便读取流程部署及流程定义信息。

### 2. 流程定义查询

```java
/**
 * 查询当前用户的任务列表
 *
 */
public class ActivitiTaskQuery {

    //wangwu完成自己任务列表的查询
    public static void main(String[] args) {
        //1.得到ProcessEngine对象
        ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();

        //2.得到TaskService对象
        TaskService taskService = processEngine.getTaskService();

        //3.根据流程定义的key,负责人assignee来实现当前用户的任务列表查询
        Task task = taskService.createTaskQuery()
                .processDefinitionKey("myProcess_1")
                .taskAssignee("zs")
                .singleResult();

        //4.任务列表的展示
        System.out.println("流程实例ID:" + task.getProcessInstanceId());
        System.out.println("任务ID:" + task.getId());  
        System.out.println("任务负责人:" + task.getAssignee());
        System.out.println("任务名称:" + task.getName());


    }
}
```

### 3. 流程定义删除

```java
public void deleteDeployment() {
        // 流程部署id
        String deploymentId = "8801";
        // 通过流程引擎获取repositoryService
        RepositoryService repositoryService = processEngine.getRepositoryService();
        //删除流程定义，如果该流程定义已有流程实例启动则删除时出错
        repositoryService.deleteDeployment(deploymentId);
        //设置true 级联删除流程定义，即使该流程有流程实例启动也可以删除，设置为false非级别删除方式，如果流程
        //repositoryService.deleteDeployment(deploymentId, true);
    }
```

> *说明*：
>
> 1) 使用 `repositoryService` 删除流程定义
>
> 2) 如果该流程定义下没有正在运行的流程，则可以用普通删除。
>
> 3) 如果该流程定义下存在已经运行的流程，使用普通删除报错，可用级联删除方法将流程及相关记录全部删除。项目开发中使用级联删除的情况比较多，**删除操作一般只开放给超级管理员使用**。

### 4. 启动流程

```java
/**
 * 启动流程实例： 前提已经完成流程定义的部署工作
 * <p>
 * 背后影响的表：
 *          act_hi_actinst     已完成的活动信息
 *          act_hi_identitylink   参与者信息
 *          act_hi_procinst   流程实例
 *          act_hi_taskinst   任务实例
 *          act_ru_execution   执行表
 *          act_ru_identitylink   参与者信息
 *          act_ru_task  任务
 **/
public class ActivitiStartInstance {
    public static void main(String[] args) {
        // 1.得到ProcessEngine对象
        ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
        // 2.得到RuntimeService
        RuntimeService runtimeService = processEngine.getRuntimeService();
        // 3.创建流程实例，流程定义的可以需要知道myProcess_1
        ProcessInstance processInstance = runtimeService.startProcessInstanceByKey("myProcess_1");

        System.out.println("流程部署ID：" + processInstance.getDeploymentId());
        System.out.println("流程实例ID：" + processInstance.getId());
        System.out.println("活动ID：" + processInstance.getActivityId());
    }
}

```

### 5. 处理任务

```java
/**
 * 处理当前用户的任务
 * 背后操作的表：
 *          act_hi_actinst
 *          act_hi_identitylink
 *          act_hi_taskinst
 *          act_ru_identitylink
 *          act_ru_task
 */
public class ActivitiCompleteTask {

    //查询当前用户wangwu的任务并处理掉
    public static void main(String[] args) {
        //1.得到ProcessEngine对象
        ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();

        //2.得到TaskService对象
        TaskService taskService = processEngine.getTaskService();

        //3.查询当前用户的任务
        Task task = taskService.createTaskQuery()
                .processDefinitionKey("myProcess_1")
                .taskAssignee("ls")
                .singleResult();

        //4.处理任务,结合当前用户任务列表的查询操作的话,任务ID:task.getId()
        taskService.complete(task.getId());

        //5.输出任务的id
        System.out.println(task.getId());
    }
}
```

## 总结一

![Diagram 1](https://gitee.com/szimo/picture_repository/raw/master/images/20200902170526.png)

### 1. Activiti的架构说明

`ProcessEngineConfiguration`类，主要作用：加载`activiti.cfg.xml`配置文件

`ProcessEngine`类，主要作用：获取`service`接口，并且可以生产`activiti`的工作环境， 25张表

`Service`接口：   主要作用：快速操作25张表

- RepositoryService
- RuntimeService
- TaskService
- HistoryService
- ……

### 2. BPMN的Activiti Designer插件

IDEA工具中安装actiNPM插件的注意细节

### 3. 画出流程定义图

### 4. 部署流程定义

- 方式一：单个文件（bpmn文件，png文件）
- 方式二：zip压缩包

### 5. 启动流程实例

`runtimeService.startProcessInstanceByKey("myProcess_1");`

### 6. 查看任务

`taskService.createTaskQuery()`

### 7. 完成任务

`taskService.complete(task.getId());`