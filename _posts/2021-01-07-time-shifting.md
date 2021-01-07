---
layout:     post
title:      "标准时间转换"
subtitle:   "不拼接方式转换"
date:       2021-01-07 12:00:00
author:     "callrenjie"
header-img: "img/bg/hello_world.jpg"
catalog: true
tags:
    - 记录
---

### 不用拼接转换标准时间格式

如何将new Date（）转换为标准的时间格式（YYYY-MM-DD HH:mm:ss），直接在js里面，不使用moment,也不使用getFullYear()，getMonth()这样拼接的方法。

![](D:\renjBlog\img\401577057-5cef4130d33b8_articlex.png)

执行结果：

<p>toISOString 2019-05-30T02:31:44.618Z<br>toUTCString Thu, 30 May 2019 02:31:44 GMT<br>toString() Thu May 30 2019 10:31:44 GMT+0800 (中国标准时间)<br>toDateString() Thu May 30 2019<br>toTimeString() 10:31:44 GMT+0800 (中国标准时间)<br>toLocaleString() 2019-5-30 10:31:44<br>toLocaleTimeString() 10:31:44<br>toLocaleDateString() 2019-5-30<br>format() 2019-05-30 10:31:44</p>

