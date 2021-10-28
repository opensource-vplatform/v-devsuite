---
description: 把一天分割以半小时为最小粒度的定时器(相当于一天24小时分为48粒度)，如果还有更高时间要求的场景，可能不合适使用这个定时器的设计范围。
---

# 定时任务

## 任务场景

任务场景本地任务、分布式任务；

**本地任务**:指处理本地的垃圾文件与公共资源无关的操作，每个实例都会执行。清理临时本地文件功能适合这场景。

**分布式任务**:处理公共资源的任务,整个集群范围只会执行一次，例如数据库处理等公共数据。分布式任务需要集群缓存服务支持。

## 执行时间

为了减轻CPU压力，定时器的执行时间是不精确的。

**执行一次的任务：**很好理解，就是每天执行一次的任务。符合清理临时文件、数据备份等场景。

**执行多次的任务：**有些任务并不是每天执行的，可能是半小时，可能是一小时，或者四小时，或者每二天，或者每周。这情况都可以使用多次执行的任务接口，然后执行时自己定义执行逻辑就可以。但是小于半小时的，就需要自己设计了。

```java
class ExampleTask implements ITimerTask{ 
	@Override
	public void run(ITimeVo vo) { 
		if(vo.getDay() % 2 == 0) {
			//每2天执行
		}
		if(vo.getHour() % 4 ==0) {
			//每4小时执行
		}
		// 自己定义执行条件
	}	
}

```

## 其他说明

**运行超时：**任务执行时间超一个周期的情况，是不会重复执行的。例如每天清除临时文件的任务，如果24小时都没有运行结束，那么下周期是不会启动。

## 实例代码

注册定时任务

```java
	/**
	 * 注册定时任务
	 * @return
	 */
	private int doRegister() {
		ITimerManager tm = VDS.getIntance().getTimerManager();//取得任务管理器
		String[] tasks = {"repeatTask","singleTaskHalf","singleTask","distributedTask"};
		int time =1;//凌晨1点
		tm.registerRepeatTask(new LocalTask(tasks[0])); //多次执行的任务
		tm.registerSingleTaskHalf(new LocalTask(tasks[1]),time); //凌晨1:30执行
		tm.registerSingleTask(new LocalTask(tasks[2]),time);//凌晨1:00执行
		tm.registerSingleTask(new DistributedTask(tasks[3]),time);//凌晨1:00执行分布式任务
		
		return time;
	}
```

定义执行代码

```java
class LocalTask implements ITimerTask{ 
	private static final Logger log = LoggerFactory.getLogger(LocalTask.class);
	 
	private final String taskName;
	public LocalTask(String task) {
		taskName = task;
	}
	@Override
	public String getTaskName() { 
		return taskName;
	}
	/**本地任务*/
	@Override
	public TaskScene getTaskScene() { 
		return TaskScene.local;
	}

	@Override
	public void run(ITimeVo vo) { 
		String s =getTaskName() + "--start--" +  vo.getDate();
		log.info(s); 
		
	}	
}

class DistributedTask implements ITimerTask{
	private static final Logger log = LoggerFactory.getLogger(DistributedTask.class);
	private final String taskName;
	public DistributedTask(String task) {
		taskName = task;
	}
	@Override
	public String getTaskName() { 
		return taskName;
	}

	/**分布式任务*/
	@Override
	public TaskScene getTaskScene() { 
		return TaskScene.distributed;
	}

	@Override
	public void run(ITimeVo vo) { 
		String s =getTaskName() + "--start--" +  vo.getDate();
		log.info(s);
		
	}	
}
```
