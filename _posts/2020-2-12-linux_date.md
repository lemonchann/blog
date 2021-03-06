---
layout: post
title: "多面手linux date命令"
date:   2020-1-27
tags: [后台开发]
comments: true
author: lemon
toc : true
---

今天给项目写了个脚本需要获取前一天的时间，本来先获取今天的然后减一下，如果是1号的话还要考虑大小月份挺复杂的，于是去查了一下手册`date`命令原生支持，喜出望外，今天就详细说说这个看起来不起眼的`date`命令。

使用Linux的同学应该对linux的`date`命令不会陌生，经常需要在命令行敲一下这个命令获取当前时间。然而这只是他的能力冰山一角。

```bash
[lemon@localhost ~]$ date 
2020年 02月 12日 星期三 19:51:46 CST
```



## 常规操作

#### 获取时间戳，1970年1月1日0点0分0秒到现在历经的秒数

```bash
[lemon@localhost ~]$ date +%s
1581508426
```



#### 时间戳还原，把刚才的秒数还原成时间字符串

```bash
[lemon@localhost ~]$ date -d "@1581508426"
2020年 02月 12日 星期三 19:53:46 CST
```



#### 指定的时间字符串转换成时间戳

```bash
[lemon@localhost ~]$ date -d '02/22/2222 07:21:22' +%s
7956832882
#或者
[lemon@localhost ~]$ date -d '2222-02-22 07:21:22' +"%s"
7956832882
```



#### 格式化输出时间格式

```bash
[lemon@localhost ~]$ date "+%Y-%m-%d"
2020-02-12
[lemon@localhost ~]$ date "+%H:%M:%S"
20:01:53
[lemon@localhost ~]$ date "+%Y-%m-%d %H:%M:%S"
2020-02-12 20:02:06
```

具体的格式参考man手册：

```bash
 格式 FORMAT 控制着输出格式. 仅当选项指定为全球时间时本格式才有效。 分别解释如下:

       %%     文本的 %

       %a     当前区域的星期几的简写 (Sun..Sat)

       %A     当前区域的星期几的全称 (不同长度) (Sunday..Saturday)

       %b     当前区域的月份的简写 (Jan..Dec)

       %B     当前区域的月份的全称(变长) (January..December)

       %c     当前区域的日期和时间 (Sat Nov 04 12:02:33 EST 1989)

       %d     (月份中的)几号(用两位表示) (01..31)

       %D     日期(按照 月/日期/年 格式显示) (mm/dd/yy)

       %e     (月份中的)几号(去零表示) ( 1..31)

       %h     同 %b

       %H     小时(按 24 小时制显示，用两位表示) (00..23)

       %I     小时(按 12 小时制显示，用两位表示) (01..12)

       %j     (一年中的)第几天(用三位表示) (001..366)

       %k     小时(按 24 小时制显示，去零显示) ( 0..23)

       %l     小时(按 12 小时制显示，去零表示) ( 1..12)

       %m     月份(用两位表示) (01..12)

       %M     分钟数(用两位表示) (00..59)

       %n     换行

       %p     当前时间是上午 AM 还是下午 PM
       
       %r     时间,按 12 小时制显示 (hh:mm:ss [A/P]M)

       %s     从 1970年1月1日0点0分0秒到现在历经的秒数 (GNU扩充)

       %S     秒数(用两位表示)(00..60)

       %t     水平方向的 tab 制表符

       %T     时间,按 24 小时制显示(hh:mm:ss)

       %U     (一年中的)第几个星期，以星期天作为一周的开始(用两位表示) (00..53)

       %V     (一年中的)第几个星期，以星期一作为一周的开始(用两位表示) (01..52)

       %w     用数字表示星期几 (0..6); 0 代表星期天

       %W     (一年中的)第几个星期，以星期一作为一周的开始(用两位表示) (00..53)

       %x     按照 (mm/dd/yy) 格式显示当前日期

       %X     按照 (%H:%M:%S) 格式显示当前时间

       %y     年的后两位数字 (00..99)

       %Y     年(用 4 位表示) (1970...)

       %z     按照 RFC-822 中指定的数字时区显示(如, -0500) (为非标准扩充)

       %Z     时区(例如, EDT (美国东部时区)), 如果不能决定是哪个时区则为空
```



## 下面就是比较骚的操作，我今天用到了。



#### 获取相对当前时间的明天的时间

```bash
[lemon@localhost ~]$ date -d next-day
2020年 02月 13日 星期四 20:08:35 CST

#你可以指定输出格式，比如
[lemon@localhost ~]$ date -d next-day +%Y%m%d
20200213
```



#### 获取相对于当前时间的昨天的时间

```bash
[lemon@localhost ~]$ date -d last-day
2020年 02月 11日 星期二 20:11:35 CST

#你也可以指定输出格式，比如
[lemon@localhost ~]$ date -d last-day +%Y%m%d
20200211
```



#### 获取相对当前时间的上个月的时间

```bash
[lemon@localhost ~]$ date -d last-month
2020年 01月 12日 星期日 20:13:20 CST

#同样的你也可以指定输出格式，比如
[lemon@localhost ~]$ date -d last-month +%Y-%m-%d
2020-01-12
```



#### 获取相对当前时间的下个月的时间

```bash
[lemon@localhost ~]$ date -d next-month
2020年 03月 12日 星期四 20:15:44 CST

[lemon@localhost ~]$ date -d next-month "+%Y-%m-%d %H:%M:%S"
2020-03-12 20:15:38
```



#### 获取相对当前时间的明年的时间

```bash
[lemon@localhost ~]$ date -d next-year
2021年 02月 12日 星期五 20:17:21 CST
```



#### 获取相对当前时间的上一年的时间

```bash
[lemon@localhost ~]$ date -d last-year
2019年 02月 12日 星期二 20:17:29 CST
```

