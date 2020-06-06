---
title: spark-note
tags: 
grammar_cjkRuby: true
---
## 安装spark
1. 下载spark：https://www.apache.org/dyn/closer.lua/spark/spark-2.4.4/spark-2.4.4-bin-hadoop2.7.tgz
2. 解压缩下载的spark文件到随意目录(${SPARK_HOME})。
3. spark 依赖于java 1.8， 如果主机上没有java 1.8 需要另外配置。
4. 配置环境变量。

```shell
JAVA_HOME="/usr/bin/java" 
#SCALA_HOME="$HOME/scala-2.12.4" 
SPARK_HOME="$HOME/spark-2.4.4-bin-hadoop2.7" 
SPARK_LOCAL_IP="127.0.0.1" PATH="$HOME/bin:$HOME/.local/bin:$SCALA_HOME/bin:$SPARK_HOME/bin: $HOME/apache-maven-3.5.2/bin:$PATH"
```
5. 验证安装成功，到解压缩目录下， 运行`bin/pyspark`:
```console
~/Tools/spark/spark-2.4.4-bin-hadoop2.7/bin$ ./pyspark
Python 2.7.17 (default, Apr 15 2020, 17:20:14)
[GCC 7.5.0] on linux2
Type "help", "copyright", "credits" or "license" for more information.
20/06/06 21:13:00 WARN Utils: Your hostname, DESKTOP-8E9V0FS resolves to a loopback address: 127.0.1.1; using 192.168.1.100 instead (on interface eth1)
20/06/06 21:13:00 WARN Utils: Set SPARK_LOCAL_IP if you need to bind to another address
20/06/06 21:13:01 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /__ / .__/\_,_/_/ /_/\_\   version 2.4.4
      /_/

Using Python version 2.7.17 (default, Apr 15 2020 17:20:14)
SparkSession available as 'spark'.
>>>
```

## 提交一个python spark应用

1. 示例代码：

```python?linenums
from pyspark import SparkConf, SparkContext
conf = SparkConf() 
sc = SparkContext(conf=conf) 
print "%d lines" % sc.textFile(sys.argv[1]).count()
```
2. 提交应用：

	* 使用单线程方式执行程序
	```shell?linenums
	./bin/spark-submit  ~/workSpace/spark_learning/myaap.py <file path that you want to count line numbers>
	```

	* 使用10个线程执行程序

	```shell?linenums
	./bin/spark-submit --master 'local[10]'  ~/workSpace/spark_learning/myaap.py <file path that you want to count line numbers>
	
	```
	* 使用与机器线程数相等的线程数量执行程序。

	```shell
	./bin/spark-submit --master 'local[*]'  ~/workSpace/spark_learning/myaap.py <file path that you want to count line numbers>
	```
3. 执行结果（部分）,其中包含了统计的文件行数信息。

	```console
	20/06/06 21:35:43 INFO DAGScheduler: ResultStage 0 (count at /home/qigao/workSpace/spark_learning/myaap.py:6) finished in 0.535 s
	20/06/06 21:35:43 INFO DAGScheduler: Job 0 finished: count at /home/qigao/workSpace/spark_learning/myaap.py:6, took 0.580001 s
	124787 lines
	20/06/06 21:35:43 INFO SparkContext: Invoking stop() from shutdown hook
	20/06/06 21:35:43 INFO SparkUI: Stopped Spark web UI at http://192.168.1.100:4040
	20/06/06 21:35:43 INFO MapOutputTrackerMasterEndpoint: MapOutputTrackerMasterEndpoint stopped!
	20/06/06 21:35:43 INFO MemoryStore: MemoryStore cleared
	20/06/06 21:35:43 INFO BlockManager: BlockManager stopped
	20/06/06 21:35:43 INFO BlockManagerMaster: BlockManagerMaster stopped
	20/06/06 21:35:43 INFO OutputCommitCoordinator$OutputCommitCoordinatorEndpoint: OutputCommitCoordinator stopped!
	20/06/06 21:35:43 INFO SparkContext: Successfully stopped SparkContext
	20/06/06 21:35:43 INFO ShutdownHookManager: Shutdown hook called
	20/06/06 21:35:43 INFO ShutdownHookManager: Deleting directory /tmp/spark-d142b0ab-8269-49cd-af40-b8f875d98e48
	20/06/06 21:35:43 INFO ShutdownHookManager: Deleting directory /tmp/spark-8a7a58d4-c035-4078-833d-7b60588d0935
	20/06/06 21:35:43 INFO ShutdownHookManager: Deleting directory /tmp/spark-d142b0ab-8269-49cd-af40-b8f875d98e48/pyspark-57df02ef-642e-4dec-9376-45d4504db37a
	```
### 提交一个java spark应用

1. 构建一个Java maven项目: https://github.com/7feigao/csv246_hw0
	* 单词计数应用：https://github.com/7feigao/csv246_hw0/tree/cs_246_hw0_section4_words_count
2. 在有`pom.xml`的folder下执行`mvn clean package`生成Jar包。
3. 提交并执行生成的jar包:

	```shell?linenums
	./spark-submit --master 'local[*]' /mnt/c/Users/gaoqi/IdeaProjects/cs246_hw0/target/cs246_hw0-1.0-SNAPSHOT.jar /mnt/e/User/gaoqi/Downloads/hw0-bundle.tar/hw0-bundle/pg100.txt
	```
4. 部分执行log：

		```prolog?linenums
		0/06/06 22:13:02 INFO HadoopRDD: Input split: file:/mnt/e/User/gaoqi/Downloads/hw0-bundle.tar/hw0-bundle/pg100.txt:2794944+2794945
		20/06/06 22:13:02 INFO HadoopRDD: Input split: file:/mnt/e/User/gaoqi/Downloads/hw0-bundle.tar/hw0-bundle/pg100.txt:0+2794944
		20/06/06 22:13:02 INFO LineRecordReader: Found UTF-8 BOM and skipped it
		20/06/06 22:13:02 INFO Executor: Finished task 1.0 in stage 0.0 (TID 1). 875 bytes result sent to driver
		20/06/06 22:13:02 INFO Executor: Finished task 0.0 in stage 0.0 (TID 0). 875 bytes result sent to driver
		20/06/06 22:13:02 INFO TaskSetManager: Finished task 1.0 in stage 0.0 (TID 1) in 208 ms on localhost (executor driver) (1/2)
		20/06/06 22:13:02 INFO TaskSetManager: Finished task 0.0 in stage 0.0 (TID 0) in 222 ms on localhost (executor driver) (2/2)
		20/06/06 22:13:02 INFO TaskSchedulerImpl: Removed TaskSet 0.0, whose tasks have all completed, from pool
		20/06/06 22:13:02 INFO DAGScheduler: ResultStage 0 (count at MyApp.java:9) finished in 0.286 s
		20/06/06 22:13:02 INFO DAGScheduler: Job 0 finished: count at MyApp.java:9, took 0.327254 s
		124787 lines

		20/06/06 22:13:02 INFO SparkContext: Invoking stop() from shutdown hook
		20/06/06 22:13:02 INFO SparkUI: Stopped Spark web UI at http://192.168.1.100:4040
		20/06/06 22:13:02 INFO MapOutputTrackerMasterEndpoint: MapOutputTrackerMasterEndpoint stopped!
		20/06/06 22:13:02 INFO MemoryStore: MemoryStore cleared
		20/06/06 22:13:02 INFO BlockManager: BlockManager stopped
		20/06/06 22:13:02 INFO BlockManagerMaster: BlockManagerMaster stopped
		20/06/06 22:13:02 INFO OutputCommitCoordinator$OutputCommitCoordinatorEndpoint: OutputCommitCoordinator stopped!
		20/06/06 22:13:02 INFO SparkContext: Successfully stopped SparkContext
		20/06/06 22:13:02 INFO ShutdownHookManager: Shutdown hook called
		20/06/06 22:13:02 INFO ShutdownHookManager: Deleting directory /tmp/spark-5b084d3c-bdfc-43ed-a9ad-57a475ba66a6
		20/06/06 22:13:02 INFO ShutdownHookManager: Deleting directory /tmp/spark-0328e11e-f318-4db3-a8c2-02f58b904736

		```


## Spark 配置

### [修改端口](https://blog.csdn.net/qq839177306/article/details/78727072)

1. cd SPARK_HOME/sbin
2. vi start-master.sh
3. 定位到下面部分内容：

		``` bash
		if [ "$SPARK_MASTER_WEBUI_PORT" = "" ]; then

		  SPARK_MASTER_WEBUI_PORT=8080

		fi
		```

## Spark 使用

### 运行示例


