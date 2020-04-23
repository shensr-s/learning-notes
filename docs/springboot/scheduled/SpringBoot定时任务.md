# Spring Boot 定时任务

> 作者：子墨同学
>
> 时间：2020-04-15

[TOC]



## Scheduled定时任务器

Scheduled定时任务器：是Spring3.0以后以后自带的一个定时任务器。

### 导入坐标

```xml
<!--scheduled坐标-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context-support</artifactId>
</dependency>
```

### 编写定时任务类

```java
/**
 * Scheduled 定时任务
 **/
@Component
public class ScheduledDemo {

    private Logger logger = LoggerFactory.getLogger(Scheduled.class);

    /**
     * 定时任务方法
     * 注解 @Scheduled：设置定时任务
     * cron 属性： cron表达式：定时任务触发事件的一个字符串表达形式
     */
    @Scheduled(cron = "0/2 * * * * ?")
    public void scheduledmethod() 
        logger.info("定时任务触发，每两秒执行一次");
    }
}

```

### 在启动类中开启定时任务

在启动类中添加@EnableScheduling注解即可

```java
@SpringBootApplication
@EnableScheduling //开启定时任务
@MapperScan(basePackages = "cn.edu.nwafu.fd.application.*.mapper")
public class CourseApplication {
    public static void main(String[] args) {
        SpringApplication.run(CourseApplication.class, args);
    }
}
```

### cron表达式

> cron在线生成工具：[cron在线生成工具](http://www.bejson.com/othertools/cron/)

cron表达式是一个字符串，`字符串以5或6个空格隔开，分为6或7个域，每一个域代表一个含义`，Cron有如下两种语法格式：
　　（1） Seconds Minutes Hours DayofMonth Month DayofWeek Year
　　（2）Seconds Minutes Hours DayofMonth Month DayofWeek

> 推荐使用6个域的表达式

#### 结构

cron从左到右（用空格隔开）：`秒 分 小时 月份中的时期 月份 星期中的日期 年`

#### 字段含义

| 位置 |   字段名    | 允许的值         | 允许的特殊字符 |
| :--: | :---------: | ---------------- | :------------- |
|  1   |    `秒`     | 0-59             | ,- * /         |
|  2   |    `分`     | 0-59             | ,- * /         |
|  3   |    `时`     | 0-23             | ,- * /         |
|  4   |    `日`     | 1-31             | ,- * ? / L W C |
|  5   |    `月`     | 1-12 or JAN-DEC  | ,- * /         |
|  6   |    `周`     | 1-7 or SUN-SAT   | ,- * ? / L C # |
|  7   | `年` (可选) | empty，1970-2099 | ,- * /         |

#### 符号说明

- `,`：表示列出枚举值。

  > 例如在`分`使用`5,20`，则意味着在第5分钟和第20分钟触发。

- `-`：表示范围。

  > 例如在`分`使用`5-20`，表示从5分钟到20分钟每分钟触发一次。

- `*`：表示匹配该域的任意值。

  > 假如在`分`域使用`*`，即表示每分钟都会触发事件。

- `/` ：表示**每多长时间**执行一次。

  > 例如在`Minutes`域使用`5/20`,则意味着第5分钟开始触发，第25分钟、第45分钟分别触发一次，在一个小时中一共触发了三次。

- `?` ：只能用在周和日。因为周和日会相互影响。

  > 例如想`在每月的20日15点13分13秒`触发调度，不管20日到底是星期几，则只能使用如下写法： `13 13 15 20 * ?`,其中最后一位只能用`？`。

- `L` ： 表示最后，只能出现在日和周。

  > 如果在`日`使用`5L`,意味着在最后的一个星期四触发。

- `W`：表示有效工作日(周一到周五),只能出现在周域，系统将在离指定日期的最近的有效工作日触发事件。

  > 例如：在日使用5W，如果5日是星期六，则将在最近的工作日：星期五，即4日触发。如果5日是星期天，则在6日触发；如果5日在星期一到星期五中的一天，则就在5日触发。另外一点，W的最近寻找不会跨过月份。

- `#`：用于确定每个月第几个星期几，只能出现在周。

  > 例如在4#2，表示某月的第二个星期三。



#### 例子

`0 0 1 1 1 ？` ：每年的1月1日1点0分0秒执行一次

`0 0 1 1 1,6 ？`：每年的1月和6月的1号的1:00:00执行一次

`0 0 1 1,4,7,10 ?`：每个季度的第一个月的1号的1:00:00执行一次

`0 0 1 1 * ?`：每个月的1号的一点执行一次

`0 0 1 * * ?`：每天的一点执行一次

## 整合Quartz定时任务框架

### Quartz介绍以及使用思路

#### Quartz介绍

`Quartz`是`OpenSymphony`开源组织在`Job scheduling`领域又一个开源项目，它可以与J2EE与J2SE应用程序相结合也可以单独使用。Quartz可以用来创建简单或为运行十个，百个，甚至是好几万个Jobs这样复杂的程序。Jobs可以做成标准的Java组件或 EJBs。Quartz的最新版本为Quartz 2.3.2。

#### Quartz使用思路

1. Job---任务---你要做什么？
2. Trigger---触发器---你什么时候去做？
3. Scheduler---任务调度---你什么时候去做什么事？

#### Quartz基本使用方式

##### 简单使用

###### 引入坐标

```xml
<!--quartz定时任务-->
<dependency>
    <groupId>org.quartz-scheduler</groupId>
    <artifactId>quartz</artifactId>
    <version>2.3.1</version>
</dependency>
```

###### 创建Job类

```java
import org.quartz.Job;
import org.quartz.JobExecutionContext;
import org.quartz.JobExecutionException;

import java.util.Date;

/**
 * Job 类 ：定义任务类
 **/

public class QuartzDemo implements Job {
    /**
     * 任务被触发时所执行的方法
     */
    @Override
    public void execute(JobExecutionContext jobExecutionContext) throws JobExecutionException {
        System.out.println("Execute...." + new Date());
    }
}
```

###### 编写测试代码

```java
import org.quartz.*;
import org.quartz.impl.StdSchedulerFactory;

public class QuartzMain {
    public static void main(String[] args) throws SchedulerException {
        // 1.创建Job对象，你要做什么？
        JobDetail job = JobBuilder.newJob(QuartzDemo.class).build();

        // 2.创建Trigger对象，在什么时候做？
        /*
        简单的trigger触发时间：通过Quartz中的一个方法来完成简单的重复调用
        Trigger trigger = TriggerBuilder.newTrigger().withSchedule(SimpleScheduleBuilder.repeatSecondlyForever()).build();
        cron Trigger :按照Cron的表达式来给定触发的时间
         */

        // Trigger trigger = TriggerBuilder.newTrigger().withSchedule(SimpleScheduleBuilder.repeatSecondlyForever()).build();
        Trigger trigger = TriggerBuilder.newTrigger().withSchedule(CronScheduleBuilder.cronSchedule("0/2 * * * * ?")).build();

        // 3.创建Scheduled对象，在什么时间做什么事？
        Scheduler scheduler = StdSchedulerFactory.getDefaultScheduler();

        scheduler.scheduleJob(job,trigger);

        // 4.启动
        scheduler.start();
    }
}

```

##### Springboot整合(2.1.7)

###### 坐标导入

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!--quartz定时调度依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-quartz</artifactId>
</dependency>
```



## 参考资料

1. [Cron表达式](https://www.cnblogs.com/duhy/p/12066858.html)

2. [SpringBoot2.x开发案例之整合Quartz任务管理系统](https://blog.51cto.com/itstyle/2097213)

3. [springboot2.x集成quartz定时任务实战](https://blog.csdn.net/nikoHuang/article/details/100017910)
4. [springboot2整合Quartz持久化定时任务管理界面](https://blog.csdn.net/haoxiaoyong1014/article/details/83339169?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-4&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-4)

5. [Spring Boot 2.x 集成 Quartz，动态管理定时任务（集群管理）](https://www.jianshu.com/p/a1c51fa9a972)
6. [第四十七章：SpringBoot2.0新特性 - Quartz自动化配置集成](https://www.jianshu.com/p/056281e057b3)