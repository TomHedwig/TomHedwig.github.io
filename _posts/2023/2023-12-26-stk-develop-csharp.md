---
title: STK二次开发(C#)指北
date: 2023-12-26 11:45:42 +0800
categories: [Programming, Development-Logs]
tags: [STK, C#]
math: true
description: 记录在使用C#进行STK二次开发时的操作
---

## 准备工作

在新建项目前，对于 STK 9 和 STK 11，均需以管理员权限运行`STK Install Folder\bin\AgPluginReg.exe`以注册所有`.dll`文件

![](/img/2023-12-26-stk-develop-csharp/register_dll.png)

## 新建项目

~~新建项目时，注意 STK 9 和 STK 11 均只支持 **.NET Framework** 而不支持 **.NET Core**；但是，有时需要应用到 C# 的新语法或特性时，又不得不使用 **.NET Core**（比如要使用 `CommunityToolkit.Mvvm` 的生成器，就必须使用 **.NET Core** ）~~

~~在这种情况下，一种方式是将需要使用新语法或特性的内容从主程序中抽离出来，用 **.NET Standard 2.0** 的类库包装这些内容，再在类库中应用新语法或特性；另一种方式则是使用 **.NET Core** 的主程序，但将目标平台设置为 **.Net Framework**~~

实际上关于 `.NET Core` 和 `.NET Framework` 的选择在 `STK 11`中并没有那么重要，根据笔者的开发过程来看，在二种情况下，可视化组件貌似都可以正常使用；不过，以防万一，还是建议使用 `.NET Framework` 或是使用 `.NET Core` 的主程序，但将目标平台设置为 `.Net Framework`为好

### 构建平台选择

在项目属性（`Properties`）中，对于构建（`Build`）中的目标平台（`Platform target`）设置，STK 9 和 STK 11 有所区别：

+ STK 9 应选择 **x86**

![](/img/2023-12-26-stk-develop-csharp/build_stk9.png)

+ STK 11 应选择 **any CPU** 并保持 **Prefer 32-bit** 选项处于未勾选的状态

![](/img/2023-12-26-stk-develop-csharp/build_stk11.png)

### 项目引用选择

#### Assemblies

设置项目引用时，由于 STK 的可视化控件属于 **WinForm** 组件，因此对于 **.NET Framework** 的 **WPF** 应用，STK 9 和 STK 11 均需要在`Reference Manager > Assemblies > Framework`中勾选以下内容：

+ **System.Windows.Forms**

+ **WindowsFormsIntegration**

![](/img/2023-12-26-stk-develop-csharp/reference_assembly.png)

而对于 **.NET Core** 应用，只需在 `.csproj` 文件中添加：

```xaml
<UseWindowsForms>true</UseWindowsForms>
<LangVersion>12.0</LangVersion>
```

并修改 `TargetFramework` 为：

```xaml
<TargetFramework>net472</TargetFramework>
```

#### COM

只有 STK 9 需要设置 `COM` 引用，在`Reference Manager > COM > Type Libraries`中勾选以下内容即可：

+ **AGI STK Objects 9**
+ **AGI STK Util 9**
+ **AGI STK VGT 9**
+ **AGI STK X 9**

![](/img/2023-12-26-stk-develop-csharp/reference_com_stk9.png)

#### Browse

对于 STK 9，除了 `COM` 引用外，还需在`Peference Manager > Browse` 中打开`STK 9 Install Folder\bin\Primary Interop Assemblies`以添加引用以下文件：

+ **AGI.STKX.Interop.dll**
+ **AxAGI.STKX.Interop.dll**

![](/img/2023-12-26-stk-develop-csharp/reference_browse_stk9.png)

对于 STK 11，只需在`Peference Manager > Browse` 中打开`STK 11 Install Folder\bin\Primary Interop Assemblies`以添加引用以下文件：

+ **AGI.STKX.Controls.Interop.dll**

+ **AGI.STKXGraphics.Interop.dll**

+ **AGI.STKUtil.Interop.dll**

+ **AGI.STKX.Interop.dll**

+ **AGI.STKVgt.Interop.dll**

+ **AGI.STKObjects.Interop.dll**

![](/img/2023-12-26-stk-develop-csharp/reference_browse_stk11.png)

### 引用属性设置

完成全部引用后，必须将上述引用的 **全部内容** 的 **Embed Interop Types** 属性设置为 **false**

![](/img/2023-12-26-stk-develop-csharp/properties.png)

## 抛出异常

![](/img/2023-12-26-stk-develop-csharp/exception_instance.png)

**原因**：程序中第一次实例化`AgStkObjectRoot`时，若不存在以下内容中的任意一个：

+ `AxAgUiAxVOCntrl` 控件
+ `AxAgUiAx2DCntrl` 控件
+ `AgSTKXApplication` 实例

则会抛出该异常

**解决方法**：在程序开始时创建`AgSTKXApplication`实例即可：

```c#
services.AddSingleton(new AgSTKXApplication());
```

或：

```c#
AgSTKXApplication stk_instance = new AgSTKXApplication();
```

![](/img/2023-12-26-stk-develop-csharp/exception_thread.png)

**原因**：程序开始时`AgSTKXApplication`和`AgStkObjectRoot`的实例化必须是单线程的，如果使用异步方式实例化，如：

```c#
await Task.Run(() =>
{
    services.AddSingleton(new AgSTKXApplication());
    services.AddSingleton(new AgStkObjectRoot());
});
```

则会抛出该异常

**解决方法**：不使用异步方式实例化