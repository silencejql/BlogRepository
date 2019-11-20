---
title: C# XML
permalink: C# XML
comments: true
copyright: false
abstract: 交钱！
message: Enter password to read！
date: 2019-07-11 12:56:29
description: C#对XML的操作
categories:
- C#
tags:
- XML
top:
password:
---

## XML创建、读取配置
```csharp
static public void WirteConfig(string FileName, string KeyName, string Value)
{
    try
    {
        string FilePath = OnlyOneStartUp.UseConfigPath;
        string AllFileName = FilePath + "\\" + FileName + ".xml";
        if (!Directory.Exists(FilePath))
            Directory.CreateDirectory(FilePath);

        DataSet ds = new DataSet();
        if (File.Exists(AllFileName))
            ds.ReadXml(AllFileName);
        if (ds.Tables.Count < 1)
            ds.Tables.Add();
        if (ds.Tables[0].Rows.Count < 1)
            ds.Tables[0].Rows.Add();

        if (!ds.Tables[0].Columns.Contains(KeyName))
            ds.Tables[0].Columns.Add(KeyName);
        ds.Tables[0].Rows[0][KeyName] = Value;

        ds.WriteXml(AllFileName);
    }
    catch (Exception e)
    {
        string sError = string.Format("写入配置信息Error:{0}", e.Message);
        ErrorOut(MethodInfo.GetCurrentMethod().Name, sError);
    }
}
/// <summary>
/// 读出配置文件
/// </summary>
/// <param name="FileName">配置文件名称</param>
/// <param name="KeyName">键名</param>
/// <param name="Value">返回的值</param>
static public bool ReadConfig(string FileName, string KeyName, ref string Value)
{
    try
    {
        string FilePath = OnlyOneStartUp.UseConfigPath;
        string AllFileName = FilePath + "\\" + FileName + ".xml";
        if (!Directory.Exists(FilePath))
            return false;

        DataSet ds = new DataSet();
        if (File.Exists(AllFileName))
            ds.ReadXml(AllFileName);
        else return false;
        if (ds.Tables.Count < 1)
            return false;
        if (ds.Tables[0].Rows.Count < 1)
            return false;

        if (!ds.Tables[0].Columns.Contains(KeyName))
            return false;
        Value = Convert.ToString(ds.Tables[0].Rows[0][KeyName]);
        return true;
    }
    catch (Exception e)
    {
        string sError = string.Format("读取配置信息Error:{0}", e.Message);
        ErrorOut(MethodInfo.GetCurrentMethod().Name, sError);
        return false;
    }
}

static public void ReadConfigEx(string FileName, string KeyName, ref string Value)
{
    if (!ReadConfig(FileName, KeyName, ref Value))
        WirteConfig(FileName, KeyName, Value);
}
```
