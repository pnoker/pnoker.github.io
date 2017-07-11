---
layout: post
title: Quartz作业调度框架
date: 2017-03-19
tag: Java
---

Quartz是一个开源的作业调度框架（在GitHub上你可一找到[Quartz](https://github.com/quartz-scheduler/quartz)），它是完全由Java编写的，并设计用于J2EE和J2SE应用中。它提供了巨大的灵活性而不牺牲简单性，用它能够可执一个作业，并创建简单或者复杂的调度。

这是[Quartz官网](http://www.quartz-scheduler.org/)，更多详情可以去官网查看,在官网你可以下载最新版 [Jar](http://www.quartz-scheduler.org/downloads/)、[Doc](http://www.quartz-scheduler.org/documentation/) 等文件。

![](/images/posts/quartz/quartz_org_index.png)

### Quartz简介
简单地创建一个实现org.quartz.Job接口的java类。Job接口包含唯一的方法：
```Java
public void execute(JobExecutionContext context) throws JobExecutionException;
```
在你的Job接口实现类里面，添加一些逻辑到execute()方法。一旦你配置好Job实现类并设定好调度时间表，Quartz将密切注意剩余时间。当调度程序确定该是通知你的作业的时候，Quartz框架将调用你Job实现类（作业类）上的execute()方法并允许做它该做的事情。无需报告任何东西给调度器或调用任何特定的东西。仅仅执行任务和结束任务即可。如果配置你的作业在随后再次被调用，Quartz框架将在恰当的时间再次调用它。

### 架构
在规模方面，Quartz跟大多数开源框架类似。大约有300个java类和接口，并被组织到12个包中。这可以和Apache Struts把大约325个类和接口以及组织到11个包中相比。尽管规模几乎不会用来作为衡量框架质量的一个特性，但这里的关键是quartz内含很多功能，这些功能和特性集是否成为、或者应该成为评判一个开源或非开源框架质量的因素。
![](/images/posts/quartz/quartz_framework.png)

Quartz对任务调度的领域问题进行了高度的抽象，提出了调度器、任务和触发器这3个核心的概念，并在org.quartz通过接口和类对重要的这些核心概念进行描述 
Quartz 调度包的两个基本单元是作业和触发器。
作业是能够调度的可执行任务，触发器提供了对作业的调度.

>* **调度器**：调度器用于将与作业触发器关联,一个作业可关联多个触发器,这样每个触发器被可以触发的作业执行;一个触发器可用于控制多个作业，触发触发时， 
全部作业将获得调度。Quartz的调度器由Scheduler接口体现. 

>* **作业**：只需实现org.quartz.job接口即可.Job接口包含一个方法 execute(),execute方法体是被调度的作业体。一旦实现Job接口和execute()方法， 
Quartz确定作业运作的时候，它将调用 execute()方法体。 

>* **触发器**：有SimpleTrigger和CronTrigger两种类型 

Quartz对任务调度的领域问题进行了高度的抽象，提出了调度器、任务和触发器这3个核心的概念，并在org.quartz通过接口和类对重要的这些核心概念进行描述：

**Job**：是一个接口，只有一个方法void execute(JobExecutionContext context)，开发者实现该接口定义运行任务，JobExecutionContext类提供了调度上下文的各种信息。Job运行时的信息保存在JobDataMap实例中；

**JobDetail**：Quartz在每次执行Job时，都重新创建一个Job实例，所以它不直接接受一个Job的实例，相反它接收一个Job实现类，以便运行时通过newInstance()的反射机制实例化Job。因此需要通过一个类来描述Job的实现类及其它相关的静态信息，如Job名字、描述、关联监听器等信息，JobDetail承担了这一角色。

通过该类的构造函数可以更具体地了解它的功用：JobDetail(java.lang.String name, java.lang.String group, java.lang.Class jobClass)，该构造函数要求指定Job的实现类，以及任务在Scheduler中的组名和Job名称；

**Trigger**：是一个类，描述触发Job执行的时间触发规则。主要有SimpleTrigger和CronTrigger这两个子类。当仅需触发一次或者以固定时间间隔周期执行，SimpleTrigger是最适合的选择；而CronTrigger则可以通过Cron表达式定义出各种复杂时间规则的调度方案：如每早晨9:00执行，周一、周三、周五下午5:00执行等；

**Calendar**：org.quartz.Calendar和java.util.Calendar不同，它是一些日历特定时间点的集合（可以简单地将org.quartz.Calendar看作java.util.Calendar的集合——java.util.Calendar代表一个日历时间点，无特殊说明后面的Calendar即指org.quartz.Calendar）。一个Trigger可以和多个Calendar关联，以便排除或包含某些时间点。

假设，我们安排每周星期一早上10:00执行任务，但是如果碰到法定的节日，任务则不执行，这时就需要在Trigger触发机制的基础上使用Calendar进行定点排除。针对不同时间段类型，Quartz在org.quartz.impl.calendar包下提供了若干个Calendar的实现类，如AnnualCalendar、MonthlyCalendar、WeeklyCalendar分别针对每年、每月和每周进行定义；

**Scheduler**：代表一个Quartz的独立运行容器，Trigger和JobDetail可以注册到Scheduler中，两者在Scheduler中拥有各自的组及名称，组及名称是Scheduler查找定位容器中某一对象的依据，Trigger的组及名称必须唯一，JobDetail的组和名称也必须唯一（但可以和Trigger的组和名称相同，因为它们是不同类型的）。Scheduler定义了多个接口方法，允许外部通过组及名称访问和控制容器中Trigger和JobDetail。

Scheduler可以将Trigger绑定到某一JobDetail中，这样当Trigger触发时，对应的Job就被执行。一个Job可以对应多个Trigger，但一个Trigger只能对应一个Job。可以通过SchedulerFactory创建一个Scheduler实例。Scheduler拥有一个SchedulerContext，它类似于ServletContext，保存着Scheduler上下文信息，Job和Trigger都可以访问SchedulerContext内的信息。SchedulerContext内部通过一个Map，以键值对的方式维护这些上下文数据，SchedulerContext为保存和获取数据提供了多个put()和getXxx()的方法。可以通过Scheduler# getContext()获取对应的SchedulerContext实例；

**ThreadPool**：Scheduler使用一个线程池作为任务运行的基础设施，任务通过共享线程池中的线程提高运行效率。

Job有一个StatefulJob子接口，代表有状态的任务，该接口是一个没有方法的标签接口，其目的是让Quartz知道任务的类型，以便采用不同的执行方案。无状态任务在执行时拥有自己的JobDataMap拷贝，对JobDataMap的更改不会影响下次的执行。而有状态任务共享共享同一个JobDataMap实例，每次任务执行对JobDataMap所做的更改会保存下来，后面的执行可以看到这个更改，也即每次执行任务后都会对后面的执行发生影响。

正因为这个原因，无状态的Job可以并发执行，而有状态的StatefulJob不能并发执行，这意味着如果前次的StatefulJob还没有执行完毕，下一次的任务将阻塞等待，直到前次任务执行完毕。有状态任务比无状态任务需要考虑更多的因素，程序往往拥有更高的复杂度，因此除非必要，应该尽量使用无状态的Job。

如果Quartz使用了数据库持久化任务调度信息，无状态的JobDataMap仅会在Scheduler注册任务时保持一次，而有状态任务对应的JobDataMap在每次执行任务后都会进行保存。

Trigger自身也可以拥有一个JobDataMap，其关联的Job可以通过JobExecutionContext#getTrigger().getJobDataMap()获取Trigger中的JobDataMap。不管是有状态还是无状态的任务，在任务执行期间对Trigger的JobDataMap所做的更改都不会进行持久，也即不会对下次的执行产生影响。

Quartz拥有完善的事件和监听体系，大部分组件都拥有事件，如任务执行前事件、任务执行后事件、触发器触发前事件、触发后事件、调度器开始事件、关闭事件等等，可以注册相应的监听器处理感兴趣的事件。

### Hello Quartz
**Job**：是一个接口只有一个方法void execute(JobExecutionContext context)，开发者实现该接口定义运行任务，JobExecutionContext类提供了调度上下文的各种信息。Job运行时的信息保存在JobDataMap实例中 
定义一个HelloJob.java类：
```java
import java.util.Date;  
import org.slf4j.Logger;  
import org.slf4j.LoggerFactory;  
import org.quartz.Job;  
import org.quartz.JobExecutionContext;  
import org.quartz.JobExecutionException;  
  
public class HelloJob implements Job {  
  
    private static Logger _log = LoggerFactory.getLogger(HelloJob.class);  
     
    public HelloJob() {  
          
    }  
     
    public void execute(JobExecutionContext context)  
        throws JobExecutionException {  
          
        _log.error(" Error: " + new Date());  
          
    }  
}
```
定义一个SimpleExample.java测试类:
```java
import static org.quartz.JobBuilder.newJob;  
import static org.quartz.TriggerBuilder.newTrigger;  
import static org.quartz.DateBuilder.*;  
import java.util.Date;  
import org.quartz.JobDetail;  
import org.quartz.Scheduler;  
import org.quartz.SchedulerFactory;  
import org.quartz.Trigger;  
import org.quartz.impl.StdSchedulerFactory;  
import org.slf4j.Logger;  
import org.slf4j.LoggerFactory;  
public class SimpleExample {  
  
    private static Logger log = LoggerFactory.getLogger(SimpleExample.class);  
  
    public void run() throws Exception {  
        // 通过SchedulerFactory获取一个调度器实例  
        SchedulerFactory sf = new StdSchedulerFactory();      
          
        Scheduler sched = sf.getScheduler();  
          
        Date runTime = evenMinuteDate(new Date());  
          
        // 通过过JobDetail封装HelloJob，同时指定Job在Scheduler中所属组及名称，这里，组名为group1，而名称为job1。  
        JobDetail job = newJob(HelloJob.class).withIdentity("job1", "group1").build();  
  
        // 创建一个SimpleTrigger实例，指定该Trigger在Scheduler中所属组及名称。  
        // 接着设置调度的时间规则.当前时间运行  
        Trigger trigger = newTrigger().withIdentity("trigger1", "group1").startAt(runTime).build();  
  
        // 注册并进行调度  
        sched.scheduleJob(job, trigger);  
  
        // 启动调度器  
        sched.start();  
  
        try {  
            //当前线程等待65秒  
            Thread.sleep(65L * 1000L);  
        } catch (Exception e) {  
              
        }  
          
        //调度器停止运行  
        sched.shutdown(true);  
          
        log.error("结束运行。。。。");  
          
    }  
  
    public static void main(String[] args) throws Exception {  
        SimpleExample example = new SimpleExample();  
        example.run();  
    }  
} 
```

### Cron表达式

Quartz使用类似于Linux下的Cron表达式定义时间规则，Cron表达式由6或7个由空格分隔的时间字段组成，如表1所示：
表1 Cron表达式时间字段

| 时间域名         | 允许值           | 允许的特殊字符  |
| -------------  |  :-------------: | ------------ |
| 秒              | 0-59             | , - * /     |
| 分              | 0-59             | , - * /     |
| 时              | 0-23             | , - * /     |
| 日              | 1-31             | , - * ? / L W C|
| 月              | 1-12             | , - * /     |
| 星期            | 1-7              | , - * ? / L C # |
| 年              | 1970-2099        | , - * /     |

Cron表达式的时间字段除允许设置数值外，还可使用一些特殊的字符，提供列表、范围、通配符等功能，细说如下：

>* 星号(*)：可用在所有字段中，表示对应时间域的每一个时刻，例如，*在分钟字段时，表示“每分钟”；

>* 问号（?）：该字符只在日期和星期字段中使用，它通常指定为“无意义的值”，相当于点位符；

>* 减号(-)：表达一个范围，如在小时字段中使用“10-12”，则表示从10到12点，即10,11,12；

>* 逗号(,)：表达一个列表值，如在星期字段中使用“MON,WED,FRI”，则表示星期一，星期三和星期五；

>* 斜杠(/)：x/y表达一个等步长序列，x为起始值，y为增量步长值。如在分钟字段中使用0/15，则表示为0,15,30和45秒，而5/15在分钟字段中表示5,20,35,50，你也可以使用*/y，它等同于0/y；

>* L：该字符只在日期和星期字段中使用，代表“Last”的意思，但它在两个字段中意思不同。L在日期字段中，表示这个月份的最后一天，如一月的31号，非闰年二月的28号；如果L用在星期中，则表示星期六，等同于7。但是，如果L出现在星期字段里，而且在前面有一个数值X，则表示“这个月的最后X天”，例如，6L表示该月的最后星期五；

>* W：该字符只能出现在日期字段里，是对前导日期的修饰，表示离该日期最近的工作日。例如15W表示离该月15号最近的工作日，如果该月15号是星期六，则匹配14号星期五；如果15日是星期日，则匹配16号星期一；如果15号是星期二，那结果就是15号星期二。但必须注意关联的匹配日期不能够跨月，如你指定1W，如果1号是星期六，结果匹配的是3号星期一，而非上个月最后的那天。W字符串只能指定单一日期，而不能指定日期范围；

>* LW组合：在日期字段可以组合使用LW，它的意思是当月的最后一个工作日；

>* 井号(#)：该字符只能在星期字段中使用，表示当月某个工作日。如6#3表示当月的第三个星期五(6表示星期五，#3表示当前的第三个)，而4#5表示当月的第五个星期三，假设当月没有第五个星期三，忽略不触发；

>* C：该字符只在日期和星期字段中使用，代表“Calendar”的意思。它的意思是计划所关联的日期，如果日期没有被关联，则相当于日历中所有日期。例如5C在日期字段中就相当于日历5日以后的第一天。1C在星期字段中相当于星期日后的第一天。

Cron表达式对特殊字符的大小写不敏感，对代表星期的缩写英文大小写也不敏感。

表2下面给出一些完整的Cron表示式的实例：
表2 Cron表示式示例

|表达式            |说明                                     |
| --------------- | -------------------------------------- |
|"0 0 12 * * ? "  | 每天12点运行 |
| "0 15 10 ? * *" | 每天10：15运行 |
| "0 15 10 * * ?" | 每天10：15运行 |
| "0 15 10 * * ? *" | 每天10：15运行 |
| "0 15 10 * * ? 2008" | 2008年每天10：15运行 |
| "0 * 14 * * ?"  | 每天14-15点每分钟运行一次，从14：00开始，14：59结束 |
| "0 0/5 14 * * ?" | 每天14：00开始，每隔5分钟运行一次 |
| "0 0/5 14,18 * * ?" | 每天14：00-14：59和18：00-18：59之间，每隔5分钟运行一次 |
| "0 0-5 14 * * ?" | 每天14：00开始，从14：00-14：05之间每隔1分钟运行一次 |
| "0 10,44 14 ? 3 WED" | 每年3月份的星期三14:10和14:44各执行一次 |
| "0 15 10 ? * MON-FRI" | 周一至周五每天10：15运行|
| "0 15 10 15 * ?" | 每月15号10:15运行 |
| "0 15 10 L * ?"  | 每月最后一天的10:15运行 |
| "0 15 10 ? * 6L" | 每月最后一个星期五10:15运行一次 |
| "0 15 10 ? * 6L 2007-2009" | 2007-2009年的每月最后一个星期五10:15运行一次 |
| "0 15 10 ? * 6#3" | 每月的第三个星期五10:15运行一次 |