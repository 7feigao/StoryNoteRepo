---
title: Hadoop
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

### HDFS

1. 查看文件目录

``` shell?linenums
#查看根目录
hadoop fs -ls .

#从本地文件系统copy文件到HDFS
hadoop fs -copyFromLocal <localFilePath> hdfs://hadoop-master:9000/<target_path_in_hdfs>
#或者
hadoop fs -copyFromLocal ./844550-99999-2019.op.gz ./NCDC/
```

