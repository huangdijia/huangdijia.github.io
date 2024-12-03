---
title: crontab 几个特殊字符的含义
date: 2019-05-06 08:12:02
tags: [crontab, linux, centos, ubuntu, debian, macos]
categories: 后端
---

## 含义

|字符|含义|
|--|--|
|`*`&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|全部。意思是在该时间的任意点都应当执行|
|`?`|不指定，任意。仅用于 日(月)和日(周)。`0 0 5 * ?` 代表每个月的第5天零点，不论星期几。|
|`0`|`0 ? * 1` 代表每周一，不论是当月的哪天。|
|`,`|多个值的分隔符，例如`1,5,10` - 代表连续值，例如`1-20`|
|`/`|步长。例如 `5/15`，代表从5开始，以15为步长。因此，当`5/15`位于分钟的位置时，表示小时内的第5、20、35和50分钟|
|`L`|最后一天。可以是每月最后一天或者每周最后一天。如果用在 天(周)字段，并且前面加数字，则表示最后一个周N。例如`5L`，表示最后一个周五（5表示周五，L表示最后）。|
|`W`|工作日，指周一到周五的任意一天|
|`#`|表示第几个的意思，例如 `6#3`，表示当月第3个星期六（6表示周六，3表示第3个）|

## 例子

~~~bash
* 0 9 5L 最后一个周五早上9点
* 0 9 6#3 当月第3个星期六
~~~