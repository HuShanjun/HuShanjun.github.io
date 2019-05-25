---
title: MySql触发器
tags: mysql,数据库
grammar_cjkRuby: true
---


# 一. 概述
1. 定义： 触发器是与表有关的对象在满足定义的条件时触发，并执行相关的语句
2. 作用： 协助应用在数据库端确保数据的完整性。

# 二. 语法
## 2.1创建触发器
```sql
CREATE TRIGER 
		trigger_name 
		trigger_time 
		trigger_event
ON table_name
FOR_EACH_ROW trigger_stmt
```
***trigger_name :***  触发器名称
***trigger_name :***  触发时间 可以是 **BEFORE** 或者 **AFTER**   before 的含义是指检查约束之前触发， after 是指在检查约束之后触发
***trigger_event :***  触发器事件，可以是INSERT，UPDATE，DELETE。
**注意：**  
1. 对同一张表相同触发时间的相同触发事件，只能定义一个触发器，在触发器中通过判断更新的字段进行相应的处理； 
2. 使用OLD和NEW来引用触发器中发生变化的记录内容。
3. 触发器只支持行及触发，不支持表级触发。


## 2.2 删除触发器




