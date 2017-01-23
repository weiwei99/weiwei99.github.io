pinterest abtest

平台整体看3个部分：
* UI
* QA workflow
* simplify api support multi platform

目标：
* realtime configure change
* 易于操作
* dashborad做决策
* scale online/offline 

步骤：
 1. 创建实验初始配置
 2. 创建目标用户分组
 3. 根据效果全量或者rollback
 
 要点：
 * 配置和代码分离，speedup
 * 实时下发到experiment机器
 * 质量保证，有review，测试通过才能上线
 
 
简洁的api(support java python js app)：
 ```
 # Get the experiment group given experiment name and gatekeeper object, without actually triggering the experiment.
group = gk.get_group("example_experiment_name")
 
# Activate/trigger experiment. It will return experiment group if any.
group = gk.activate_experiment("example_experiment_name")
 
# Application code showing treatment based on group.
if group in ['enabled', 'employees']:
  # behavior for enabled group
  pass
else:
  # behavior for control group
  pass
```

架构：
https://engineering.pinterest.com/sites/engineering/files/article/fields/Screen%20Shot%202016-04-08%20at%2010.57.37%20AM.png

* Extensible and generic enough
* Scalable处理大量的实验
* dashboard指标

使用的基本处理引擎跟之前的日志分析类似，kafka + hadoop + storm + spark

pinball


Batch-processing MapReduce workflow
每天等数据入hdfs之后，开始跑mapreduce任务，包括对原始数据的清洗，按照day-in来分析，按照分隔开的group来分析feature的影响

怎么设计存储，这样方便查询？
* rowkey
* columnfamily(分t,a,c)
* range/group/non-aggregate

实时计算：
* Cohort analysis不同组之间的差别
* day-in 按照实验开始日期来分析衰减
* statistical significant 通过t-检验来提前知道异常
* group validation 分组是否符合预期
