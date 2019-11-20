---
title: C# 常识
permalink: C# 常识
comments: true
copyright: false
abstract: 交钱！
message: Enter password to read！
date: 2019-07-05 10:59:00
description:
categories:
- C#
tags:
- C#
top:
password:
---
记录写bug过程中遇到的一些疑问
<!--more-->

## 数据类型
值类型存储在堆栈上，引用类型存储在堆上。
C#类为引用类型，结构为值类型。
从值类型转化为引用类型称为装箱，如果方法需要把一个对象作为参数，而且传送一个值类型，装箱操作会自动进行；装箱的值类型可以使用拆箱操作转换为值类型，在拆箱时，**需要使用类型转换运算符**。
## 装箱和拆箱
装箱是一种通过将变量存储到System.Object中来显式地将值类型转换为引用类型的机制。当您装入值时，CLR会将新对象分配到堆中，并将值类型的值复制到该实例中。例如：
```csharp
int a = 20;  
object b = a; //装箱
```
相反的操作是拆箱，它是将引用类型转换回值类型的过程。此过程验证接收数据类型是否与装箱类型一致;
`int c = (int)b; // 拆箱`

## 泛型
>特征：
>>类型安全
性能
二进制代码复用

详见： {% post_link 泛型的特点 %}

### 为什么不用object代替泛型
由于Object为所有类型的基类，所以可以处理任何数据类型的数据，但是其中存在这拆箱和装箱，如果数据太多会影响到程序的性能。
在使用泛型的时候程序会在编译阶段根据我们提供的类型生成相应的二进制代码，无须进行装箱和拆箱操作。
## 接口
### 为什么要用接口
接口一般由上层人员发起，下层人员实现。
写接口并不是为了扩展，而是为了扩展以后的模块仍然跟项目模块保持高度一致，为了扩展后的规范化。
### 实例化接口对象
#### 接口回调
接口不仅可以声明对象，而且可以把对象实例化，还可以当做参数被传入。
即继承中的向上转型，父类 FL=new 子类()，只不过这里的父类就是interface接口。
```csharp
interface Itemp
{
  double plus();
}

public class num : Itemp
{
  double aa, bb;
  public num(double a, double b)
  {
    this.bb = b;
    this.aa = a;
  }
  public double plus()
  {
    return (aa + bb);
  }
}

static void Main(string[] args)
{
  Itemp tm = null;//声明接口对象引用
  tm = new num(1, 2);//接口回调(向上转型)
  Console.WriteLine(tm.plus());
}
```

## 类型参数约束
&emsp;&emsp;在定义泛型类时，可以对客户端代码能够在实例化类时用于类型参数的类型种类施加限制。如果客户端代码尝试使用某个约束所不允许的类型来实例化类，则会产生编译时错误。这些限制称为约束。
类型参数约束.NET支持的类型参数约束有以下五种：
```csharp
where T : struct  类型参数必须是值类型；可以指定除 Nullable 以外的任何值类型
where T : class   类型参数必须是引用类型；这一点也适用于任何类、接口、委托或数组类型
where T : new()   类型参数必须具有无参数的公共构造函数。当与其他约束一起使用时，new() 约束必须最后指定
where T : NameOfBaseClass   类型参数必须是指定的基类或派生自指定的基类
where T : NameOfInterface   类型参数必须是指定的接口或实现指定的接口。可以指定多个接口约束。约束接口也可以是泛型的
where T : U    为 T 提供的类型参数必须是为 U 提供的参数或派生自为 U 提供的参数
```
