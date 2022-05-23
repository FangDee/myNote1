### Quarz的3个核心

#### 1.调度器

```
//使用工厂模式创建调度器:scheduler
Scheduler scheduler = StdSchedulerFactory.getDefaultScheduler();
```

#### 2.触发器

```
//触发器
        Trigger trigger = TriggerBuilder.newTrigger()
            	//定义name/group
                .withIdentity("trigger1", "group1") 
                .startAt(new GregorianCalendar(2020,5,30,17,1,0).getTime())
                //.startNow()//一旦加入scheduler，立即生效，即开始计时
                .withSchedule(SimpleScheduleBuilder.simpleSchedule()
				//.withIntervalInMinutes(1)
                .withIntervalInSeconds(2) //每隔一秒执行一次
                .repeatForever()) //一直执行，直到结束时间
            	//设置结束时
               .endAt(newGregorianCalendar(2020,6,30,16,59,0).getTime()).build();
```

#### 3.任务

```
//1首先定义一个任务类
public class OrderJob implements Job {

    public void execute(JobExecutionContext context) throws JobExecutionException {

        //提供当前任务的一些信息;
        //创建工作详情
        JobDetail jobDetail=context.getJobDetail();
        //获取工作的名称
        String jobName = jobDetail.getKey().getName();//任务名
        String jobGroup = jobDetail.getKey().getGroup();//任务group
        System.out.println("job执行，job："+jobName+" group:"+jobGroup);


        System.out.println(new Date());

        //假设现在是要取消一笔订单

    }
    
 
   
//通过任务类的类对象 创建任务对象
JobDetail jd = JobBuilder.newJob(OrderJob.class).build();
```

### 4.执行任务

```
//在调度表里面加上了 xx时间要去执行xx任务
scheduler.scheduleJob(jd,trigger);

//启动调度
scheduler.start();
```

### 到此为止就是对Quartz的简单应用