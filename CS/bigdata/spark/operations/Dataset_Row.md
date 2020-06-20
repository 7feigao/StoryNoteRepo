---
title: Dataset<Row>
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---
## 创建Dataset\<Row\> 对象
```java
Dataset<Row> rows = sparkSession.read().csv("./src/main/resources/sample_with_header.csv");
```

## Dataset\<Row\> 对象方法

### `rows.withColumn()`
```java
/**
   * Returns a new Dataset by adding a column or replacing the existing column that has
   * the same name.
   *
   * `column`'s expression must only refer to attributes supplied by this Dataset. It is an
   * error to add a column that refers to some other Dataset.
   *
   * @group untypedrel
   * @since 2.0.0
   */
def withColumn(colName: String, col: Column): DataFrame = withColumns(Seq(colName), Seq(col))
```
1. 转换列类型

```java
#转换列"计划起飞时间"并替换原来列内容
#转换列"实际到达时间"并生成新的列“到达时间”
rows.withColumn("计划起飞时间", col("计划起飞时间").cast(DataTypes.TimestampType))
                .withColumn("到达时间", col("实际到达时间").cast(DataTypes.TimestampType));
```

### `rows.groupBy()`
```java 
  /**
   * Groups the Dataset using the specified columns, so that we can run aggregation on them.
   * See [[RelationalGroupedDataset]] for all the available aggregate functions.
   *
   * This is a variant of groupBy that can only group by existing columns using column names
   * (i.e. cannot construct expressions).
   *
   * {{{
   *   // Compute the average for all numeric columns grouped by department.
   *   ds.groupBy("department").avg()
   *
   *   // Compute the max age and average salary, grouped by department and gender.
   *   ds.groupBy($"department", $"gender").agg(Map(
   *     "salary" -> "avg",
   *     "age" -> "max"
   *   ))
   * }}}
   * @group untypedrel
   * @since 2.0.0
   */
  @scala.annotation.varargs
  def groupBy(col1: String, cols: String*): RelationalGroupedDataset
```
1. 进行聚合操作
```java
rows.groupBy("出发机场").agg(
                col("出发机场")
                , max(col("飞机编号"))
                , count("*")
                , min("飞机编号")
                , sum("计划到达时间")
                , avg("实际到达时间")
                , avg(col("计划到达时间").minus(col("实际到达时间")))
        ).show();
```

### `rows.orderBy()`
```java
/**
   * Returns a new Dataset sorted by the given expressions.
   * This is an alias of the `sort` function.
   *
   * @group typedrel
   * @since 2.0.0
   */
  @scala.annotation.varargs
  def orderBy(sortExprs: Column*): Dataset[T] = sort(sortExprs : _*)
```

1. 排序

```java 
 rows.orderBy(desc("计划到达时间"),asc("实际到达时间")).show();
```