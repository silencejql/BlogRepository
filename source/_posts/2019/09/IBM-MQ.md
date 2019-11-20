---
title: IBM MQ
permalink: IBM MQ
comments: true
copyright: true
abstract: 交钱！
message: Enter password to read！
date: 2019-09-12 19:48:12
description: C# IBMMQ
categories:
- MQ
tags:
- C#
- IBMMQ
top:
password:
---
<center>写在前面</center>
文中代码部分参考
https://blog.csdn.net/java_sparrow/article/details/80626705

**若只做客户端部分的设计不需要安装客户端**

网上关于IBMMQ相关资源实在太少，感谢前人的努力，站在前人的肩膀实在上太舒服了
<!--more-->
## IBMMQ 软件安装设置
### 安装
安装包(WebSphereMQ8.0)已上传网盘，下载后直接安装即可。
链接：https://pan.baidu.com/s/1P9Dz74tvf8_SExH0Dl0hHA
提取码：0a87
### 服务端配置
下文中未提到的部分默认参数即可
#### 创建队列管理器

![](IBM%20MQ/QM.PNG)
##### 设置侦听端口

![](IBM%20MQ/QM3.png)

#### 创建通道

![](IBM%20MQ/CHANNEL.PNG)
#### 创建队列

![](IBM%20MQ/Q.PNG)
#### 添加用户组
将当前用户添加到IBMMQ管理组
![](IBM%20MQ/USER.png)

其中对象名为下图所示安装IBMMQ后自动创建的组mqm
![](IBM%20MQ/GROUP.png)

至此，IBMMQ环境设置完毕
## C# 连接MQ
### 准备

#### 已安装IBMMQ软件
程序中引用：amqmdnet.dll库文件（IBMMQ客户端安装后生成）
路径分别为：
C:\\Program Files\\IBM\WebSphere MQ\\bin\\amqmdnet.dll
程序中添加using IBM.WMQ;
程序安装完成后可能缺少mqdc.dll文件，可下载后放在安装路径
C:\\Program Files\\IBM\\WebSphere MQ\\bin64 下
链接：https://pan.baidu.com/s/19XMuE1q46K1E3BfO8lPPIQ
提取码：5qhq
#### 免安装IBMMQ
准备好amqmdnet.dll在程序中直接引用即可下载地址：
链接：https://pan.baidu.com/s/1Qb4bdDpqXmyQO5_Jx4hmnA
提取码：soua
若缺少其他文件请自取：IBMMQ8.0安装路径Bin文件夹
链接：https://pan.baidu.com/s/1vdg4J2I1-1qLcHh_hnfXBA
提取码：kgi9
### C#代码
#### 初始化
##### 方式一
```csharp
static MQQueueManager qMgr;
static int CCSID = 437;
MQEnvironment.Hostname = "10.91.232.46";
MQEnvironment.Channel = "CHANNEL1";
MQEnvironment.Port = 8802;
MQEnvironment.UserId = "user";
MQEnvironment.Password = "Password";
// 队列管理器
qMgr = new MQQueueManager("LG_2IN1_QMGR");
```

##### 方式二
```csharp
static MQQueueManager qMgr;
Hashtable queueProperties = new Hashtable();
queueProperties[MQC.TRANSPORT_PROPERTY] = MQC.TRANSPORT_MQSERIES_MANAGED;
queueProperties[MQC.HOST_NAME_PROPERTY] = "192.168.1.9";
queueProperties[MQC.PORT_PROPERTY] = 8802;
queueProperties[MQC.CHANNEL_PROPERTY] = "CHANNEL1";
queueProperties[MQC.USER_ID_PROPERTY] = "user";
queueProperties[MQC.PASSWORD_PROPERTY] = "Password";
queueProperties[MQC.CCSID_PROPERTY] = "1381";
qMgr = new MQQueueManager("LG_2IN1_QMGR", queueProperties);
```
#### 发送数据
```csharp
public static void sendMsg(String msgStr)
{
    int openOptions = MQC.MQOO_INPUT_AS_Q_DEF | MQC.MQOO_OUTPUT | MQC.MQOO_INQUIRE;
    MQQueue queue = null;
    try {
        // 建立通道的连接
        queue = qMgr.AccessQueue(queueString, openOptions, null, null, null);
        MQMessage msg = new MQMessage();// 要写入队列的消息
        msg.Format = MQC.MQFMT_STRING;
        msg.CharacterSet = CCSID;
        msg.Encoding = CCSID;
        // msg.writeObject(msgStr);
        msg.WriteString(msgStr); //将消息写入消息对象中
        MQPutMessageOptions pmo = new MQPutMessageOptions();
        msg.Expiry = -1; // 设置消息用不过期
        queue.Put(msg, pmo);// 将消息放入队列
    } catch (Exception e) {
        XmlFO.LogOut("IBMMQ",e.ToString());
    } finally {
        if (queue != null) {
            try {
                queue.Close();
                // qMgr.disconnect();
                XmlFO.LogOut("IBMMQ","写入的消息为：" + msgStr);
            } catch (MQException e) {
                XmlFO.LogOut("IBMMQ", e.ToString());
            }
        }
    }
}
```
#### 读取数据
```csharp
public static void getMsg()
{
    int openOptions = MQC.MQOO_INPUT_AS_Q_DEF | MQC.MQOO_OUTPUT | MQC.MQOO_INQUIRE;
    MQQueue queue = null;
    try {
        queue = qMgr.accessQueue(queueString, openOptions, null, null, null);
        System.out.println("===========================");
        System.out.println("该队列当前的深度为:" + queue.getCurrentDepth());
        System.out.println("===========================");
        int depth = queue.getCurrentDepth();
        // 将队列的里的消息读出来
        while (depth-- > 0) {
            MQMessage msg = new MQMessage();// 要读的队列的消息
            MQGetMessageOptions gmo = new MQGetMessageOptions();
            queue.get(msg, gmo);
            System.out.println("消息的大小为：" + msg.getDataLength());
            System.out.println("消息的内容：" + msg.readStringOfByteLength(msg.getDataLength()));
            System.out.println("---------------------------");
        }
    } catch (Exception e) {
        XmlFO.LogOut("IBMMQ", e.ToString());
    } finally {
        if (queue != null) {
            try {
                queue.close();
                qMgr.disconnect();
            } catch (MQException e) {
                XmlFO.LogOut("IBMMQ", e.ToString());
            }
        }
    }
}
```
