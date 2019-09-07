---
title: spark-note
tags: 
grammar_cjkRuby: true
---

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


