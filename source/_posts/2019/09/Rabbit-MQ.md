---
title: Rabbit MQ
permalink: Rabbit MQ
comments: true
copyright: true
abstract: 交钱！
message: Enter password to read！
date: 2019-09-19 18:38:53
description: C# Rabbit MQ
categories:
- MQ
tags:
- C#
- Rabbit MQ
top:
password:
---
<center>写在前面</center>

<!--more-->
## 安装
### Erlang安装
Rabbit MQ安装前需先安装Erlang语言环境安装包下载地址：
链接：https://pan.baidu.com/s/14_Z6JUdvQfY8PGQ4NBb73A
提取码：abtt
### Rabbit MQ服务端安装
#### 安装
软件安装在C盘(第一次安装到D盘后进入命令行程序不能执行，有兴趣可以研究研究)安装包下载地址：
链接：https://pan.baidu.com/s/1NbHfVoGyo_oVAXzaTnZuAw
提取码：9am0

#### 启用管理工具
在服务程序中确保RabbitMQ服务已启用
安装完成后打开所有程序中的RabbitMQ Command Prompt (sbin dir)
输入命令：'rabbitmq-plugins enable rabbitmq_management'
![](Rabbit%20MQ/安装成功.png)
打开浏览器输入http://localhost:15672
默认账号、密码均为guest

## C# Producer
项目中需要引用RabbitMQ.Client.dll下载链接为：
链接：https://pan.baidu.com/s/1aJ-8RVJnJWibeqtPcTn9hw
提取码：9d8n
此版本支持.Net4.0，全版本请自行下载
下载地址：http://www.rabbitmq.com/releases/rabbitmq-dotnet-client/
```csharp
using RabbitMQ.Client;
var factory = new ConnectionFactory();
factory.HostName = "localhost";//RabbitMQ服务在本地运行127.0.0.1
factory.UserName = "guest";//用户名
factory.Password = "guest";//密码
using (var connection = factory.CreateConnection())
{
    using (var channel = connection.CreateModel())//创建一个Channel
    {
      try
      {
          channel.QueueDeclarePassive(mqqueueString);//判断队列是否存在
      }
      catch (Exception e)
      {
          Log(mqqueueString + "队列未创建!");
          throw e;
          //return false;
      }
        /*在创建队列的时候，只有RabbitMQ上该队列不存在，才会去创建。
        消息是以二进制数组的形式传输的，所以如果消息是实体对象的话，需要序列化和然后转化为二进制数组。*/
        IBasicProperties properties = channel.CreateBasicProperties();
        properties.ContentType = "application/json";
        properties.ContentEncoding = "UTF-8";
        properties.Headers = new Dictionary<string, object>();
        properties.Headers.Add("srcSystem", "EQP");
        properties.Headers.Add("desSystem", "IMES");
        properties.Headers.Add("msgType", "EQP_IMES_EM");
        properties.Headers.Add("msgID", msgID);
        channel.ConfirmSelect();
        channel.QueueDeclare(mqqueueString, true, false, false, null);//消息队列
        var body = Encoding.UTF8.GetBytes(msgStr);
        channel.BasicPublish(mqExchange, mqRoutingKey, properties, body); //开始传递
        if (channel.WaitForConfirms())
        {
            Log("数据发送成功");
            return true;
        }
        else
        {
            Log("数据发送成功，但未收到确认消息");
            return false;
        }
    }
}
```
发送成功后可登录网页在Queues中查看到上传的消息队列及内容
### 函数说明
#### QueueDeclare
转自https://blog.csdn.net/vbirdbest/article/details/78670550
```csharp
queueDeclare(String queue,
			boolean durable,
			boolean exclusive,
			Map<String, Object> arguments);
```
queue: 队列名称

durable： 是否持久化, 队列的声明默认是存放到内存中的，如果rabbitmq重启会丢失，如果想重启之后还存在就要使队列持久化，保存到Erlang自带的Mnesia数据库中，当rabbitmq重启之后会读取该数据库

exclusive：是否排外的，有两个作用，一：当连接关闭时connection.close()该队列是否会自动删除；二：该队列是否是私有的private，如果不是排外的，可以使用两个消费者都访问同一个队列，没有任何问题，如果是排外的，会对当前队列加锁，其他通道channel是不能访问的，如果强制访问会报异常：com.rabbitmq.client.ShutdownSignalException: channel error; protocol method: #method<channel.close>(reply-code=405, reply-text=RESOURCE_LOCKED - cannot obtain exclusive access to locked queue 'queue_name' in vhost '/', class-id=50, method-id=20)一般等于true的话用于一个队列只能有一个消费者来消费的场景

autoDelete：是否自动删除，当最后一个消费者断开连接之后队列是否自动被删除，可以通过RabbitMQ Management，查看某个队列的消费者数量，当consumers = 0时队列就会自动删除

arguments：
队列中的消息什么时候会自动被删除
>Message TTL(x-message-ttl)：设置队列中的所有消息的生存周期(统一为整个队列的所有消息设置生命周期), 也可以在发布消息的时候单独为某个消息指定剩余生存时间,单位毫秒, 类似于redis中的ttl，生存时间到了，消息会被从队里中删除，注意是消息被删除，而不是队列被删除， 特性Features=TTL, 单独为某条消息设置过期时间AMQP.BasicProperties.Builder properties = new AMQP.BasicProperties().builder().expiration(“6000”);
channel.basicPublish(EXCHANGE_NAME, “”, properties.build(), message.getBytes(“UTF-8”));

>Auto Expire(x-expires): 当队列在指定的时间没有被访问(consume, basicGet, queueDeclare…)就会被删除,Features=Exp

>Max Length(x-max-length): 限定队列的消息的最大值长度，超过指定长度将会把最早的几条删除掉， 类似于mongodb中的固定集合，例如保存最新的100条消息, Feature=Lim

>Max Length Bytes(x-max-length-bytes): 限定队列最大占用的空间大小， 一般受限于内存、磁盘的大小, Features=Lim B

>Dead letter exchange(x-dead-letter-exchange)： 当队列消息长度大于最大长度、或者过期的等，将从队列中删除的消息推送到指定的交换机中去而不是丢弃掉,Features=DLX

>Dead letter routing key(x-dead-letter-routing-key)：将删除的消息推送到指定交换机的指定路由键的队列中去, Feature=DLK

>Maximum priority(x-max-priority)：优先级队列，声明队列时先定义最大优先级值(定义最大值一般不要太大)，在发布消息的时候指定该消息的优先级， 优先级更高（数值更大的）的消息先被消费,

>Lazy mode(x-queue-mode=lazy)： Lazy Queues: 先将消息保存到磁盘上，不放在内存中，当消费者开始消费的时候才加载到内存中

>Master locator(x-queue-master-locator)

#### basicPublish
```csharp
basicPublish(String exchange,
             String routingKey,
            BasicProperties props,
            byte[] body)
```
String exchange -- 交换机名称
String routingKey -- 路由关键字
BasicProperties props -- 消息的基本属性，例如路由头等
byte[] body -- 消息体
```csharp
basicPublish(String exchange,
             String routingKey,
             boolean mandatory,
             BasicProperties props,
             byte[] body)
```
boolean mandatory -- 是否为强制性
```csharp
basicPublish(String exchange,
             String routingKey,
             boolean mandatory,
             boolean immediate,
             BasicProperties props,
             byte[] body)
```
boolean immediate -- 消息是否立即发送出去
#### BasicProperties
```csharp
public static class BasicProperties
{
  private String contentType; //上下文类型
  private String contentEncoding; //编码集
  private Map<String,Object> headers; //消息头
  private Integer deliveryMode; //消息的投递模式
  private Integer priority; //优先级
  private String correlationId; //
  private String replyTo; //
  private String expiration; //过期时间
  private String messageId; //消息编号
  private Date timestamp; //发送消息时的时间戳
  private String type; // 消息类型
  private String userId;
  private String appId;
  private String clusterId;
}
```
## C# Consumer
编写客户端链接RabbitMQ读取信息
```csharp
using RabbitMQ.Client;
using RabbitMQ.Client.Events;
var factory = new ConnectionFactory();
factory.HostName = "localhost";
factory.UserName = "guest";
factory.Password = "guest";

using (var connection = factory.CreateConnection())
{
    using (var channel = connection.CreateModel())
    {
        channel.QueueDeclare("hello", false, false, false, null);

        var consumer = new EventingBasicConsumer(channel);
        channel.BasicConsume("hello", false, consumer);
        consumer.Received += (model, ea) =>
        {
            var body = ea.Body;
            var message = Encoding.UTF8.GetString(body);
            Console.WriteLine("已接收： {0}", message);   
        };
        Console.ReadLine();
    }
}
```
### 函数说明
#### BasicConsume
```csharp
String basicConsume(String queue,
                    Consumer callback)

String basicConsume(String queue,
                    boolean autoAck,
                    Consumer callback)

String basicConsume(String queue,
                    boolean autoAck,
                    Map<String, Object> arguments,
                    Consumer callback)

String basicConsume(String queue,
                    boolean autoAck,
                    String consumerTag,
                    Consumer callback)

String basicConsume(String queue,
                    boolean autoAck,
                    String consumerTag,
                    boolean noLocal,
                    boolean exclusive,
                    Map<String, Object> arguments,
                    Consumer callback)
```
queue 队列名
autoAck 是否自动确认消息,true自动确认,false 不自动要手动调用,建立设置为false
consumerTag 消费者标签，用来区分多个消费者
noLocal 设置为true，表示 不能将同一个Conenction中生产者发送的消息传递给这个Connection中 的消费者
exclusive 是否排他
arguments 消费者的参数
callback 消费者 DefaultConsumer建立使用，重写其中的方法
