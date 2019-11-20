---
title: 委托-匿名-Lambda表达式
permalink: 委托-匿名-Lambda表达式
comments: true
copyright: false
abstract: 交钱！
message: Enter password to read！
date: 2019-06-26 15:05:21
description: C# Lambda表达式(转载)
categories:
- C#
tags:
- 委托
- 匿名方法
- Lambda
- Func<T>委托
top:
password:
---
### 委托
```csharp
delegate int calculator(int x, int y); //委托类型
static void Main()
{
    calculator add = new calculator(Addition);
    int AddResult = add(1, 1);
    Console.Write(AddResult);

    calculator dec = new calculator(Subtraction);
    int SubResult = dec(2,1);
    Console.write(SubResult);
}

/// <summary>
/// 加法
/// </summary>
/// <param name="x"></param>
/// <param name="y"></param>
/// <returns>x+y</returns>
public static int Addition(int x, int y)
{
    return x + y;
}

/// <summary>
/// 减法
/// </summary>
/// <param name="x"></param>
/// <param name="y"></param>
/// <returns>x-y</returns>
public static int Subtraction(int x, int y)
{
    return x - y;
}
```

### 匿名方法
```csharp
delegate int calculator(int x, int y); //委托
static void Main()
{
   calculator add = delegate(int num1,int num2)
   {
       return num1 + num2;
   };
   calculator dec = delegate(int num1,int num2)
   {
       return num1 - num2;
   };
   int AddResult = dec(1, 1);
   int SubResult = dec(2, 1);
   Console.Write(AddResult);
   Console.Write(SubResult);
}
```
### Lambda表达式
```csharp
delegate bool MyBol(int x, int y);
delegate bool MyBol_2(int x, string y);
delegate int calculator(int x, int y); //委托类型
delegate void VS();
static void Main()
{
    MyBol Bol = (x, y) => x == y;
    MyBol_2 Bol_2 = (x, s) => s.Length > x;
    calculator C = (X, Y) => X * Y;
    VS S = () => Console.Write("我是无参数Labada表达式");
    int[] numbers = { 5, 4, 1, 3, 9, 8, 6, 7, 2, 0 };
    int oddNumbers = numbers.Count(n => n % 2 == 1);
    List<People> people = LoadData();//初始化
    IEnumerable<People> results = people.Where(delegate(People p) { return p.age > 20; });
}
private static List<People> LoadData()
{
    List<People> people = new List<People>();   //创建泛型对象  
    People p1 = new People(21, "guojing");      //创建一个对象  
    People p2 = new People(21, "wujunmin");     //创建一个对象  
    People p3 = new People(20, "muqing");       //创建一个对象  
    People p4 = new People(23, "lupan");        //创建一个对象  
    people.Add(p1);                     //添加一个对象  
    people.Add(p2);                     //添加一个对象  
    people.Add(p3);                     //添加一个对象  
    people.Add(p4);
    return people;
}
public class People
{
  public int age { get; set; }             //设置属性  
  public string name { get; set; }         //设置属性  
  public People(int age, string name)      //设置属性(构造函数构造)  
  {
      this.age = age;                 //初始化属性值age  
      this.name = name;               //初始化属性值name  
  }
}
```
### Func<T>委托
```csharp
//最后一个参数为返回值类型
static void Main(string[] args)
{
    Func<int, int, bool> gwl = (p, j) =>
        {
            if (p + j == 10)
            {
                return true;
            }
            return false;
        };
    Console.WriteLine(gwl(5,5) + "");   //打印‘True’
    Console.ReadKey();
}
```
