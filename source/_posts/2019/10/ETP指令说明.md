---
title: ETP指令说明
permalink: ETP指令说明
comments: true
copyright: true
abstract: 交钱！
message: Enter password to read！
date: 2019-10-25 18:14:02
description:
categories:
- 工作
tags:
- ISO15031-5
- 15765-4
top:
password:
---
<center>写在前面</center>
&emsp; 国六排放测试过程中常用通讯指令说明

详细资料请参考ISO15031-5或J1979-DA
<!--more-->

## ISO15031-5
### 15765-4
#### Service 01
请求当前动力系统诊断数据
01后最多可以读取6个PID(Parameter Identification)
请求：01 PID PID PID PID PID PID
回复：41 PID Data PID Data PID Data PID Data PID Data PID Data
##### PID
|PID|说明|数据长度（byte）|计算|
|:-:|:-:|:-:|:-:|
|00 |查看支持的PID|   |   |
|01 |清除故障诊断码后的监视器状态|4 |   |
|04 |负载值  | 1  |y=x/255   |
|<font color="#FF0000"> 05  </font>    |<font color="#FF0000"> 冷却液温度  </font>|1 |y=x-40|
|0B |进气歧管绝对压力| 1  |y=x   |
|<font color="#FF0000"> 0C  </font>|<font color="#FF0000"> 发动机转速  </font> |2 |y=x/4 |
|<font color="#FF0000"> 0D  </font> |<font color="#FF0000"> 车速  </font> |1 |y=x |
|10 |空气流量  |2 |y=x/100 |
|11 |节气门位置|1 |y=x/255 |
|44  |Commanded Equivalence Ratio |2|y=2x/65535  |
|<font color="#FF0000"> 5C  </font>   |<font color="#FF0000"> 油温  </font>   | 1  |y=x-40|
|6A   |   |   |   |
|6E   |   |   |   |
|78   |尾气温度   |   |   |
|7A   |   |   |   |
|83   |NOx传感器   |   |   |
|85   |NOx控制系统   |   |   |

###### PID 00
请求：01 00
回复：
> 41 00 A B C D //PID 01\~20
41 20 A B C D //PID 21\~40
41 40 A B C D //PID 41\~60
41 60 A B C D //PID 61\~80
41 80 A B C D //PID 81\~A0
41 A0 A B C D //PID A1\~C0
41 C0 A B C D //PID C1\~E0
41 E0 A B C D //PID E1\~FF

`41 00 A B C D`
Data A bit7 ~ Data D bit0 表示是否支持PID 01~20

如：`41 00 90 39 00 00`
Data A = 90 = 1001 0000
bit7 = 1; bit4 = 1
表示支持 PID 01、PID 04
Data B = 39 = 0011 1001
bit5 = bit4 = bit3 = bit0 = 1
表示支持 PID 0B、PID 0C、PID 0D、PID 10

#### Service 09
##### PID
|PID|说明|数据长度（byte）|计算|
|:-:|:-:|:-:|:-:|
|02 |VIN|17 |   |
|04   |Cal.ID   |   |   |
|06   |Cal.CVN   |   |   |
|08   |IPT   |   |   |

## ISO27145
#### Service 22
22 F4 对应 ISO15031的01服务
22 F8 对应 ISO15031的09服务
