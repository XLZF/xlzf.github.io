---
title: DotNet & DotNet Core 区别
date: 2022-02-12 17:25:29
tags: [Interview]
excerpt: DotNet & DotNet Core 区别
categories: C#
index_img: https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/Csharp.jpeg
---

# DotNet 与 DotNet Core 的区别

![image-20220212182101257](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20220212182101257.png)

**DotNet 包含 DotNet Core** 

**DotNet** 内有 **DotNet Framework** 、**DotNet Core** 、**Xamarin**

**DotNet Framework** 支持Windows和Web应用程序。今天，您可以使用Windows窗体，WPF和UWP在.NET Framework中构建Windows应用程序。ASP.NET MVC用于在.NET Framework中构建Web应用程序。

**DotNet Core** 是一种新的开源和跨平台框架，用于为包括Windows，Mac和Linux在内的所有操作系统构建应用程序。.NET Core仅支持UWP和ASP.NET Core。UWP用于构建Windows 10目标Windows和移动应用程序。ASP.NET Core用于构建基于浏览器的Web应用程序。 

**Xamarin** 无庸置疑，当您想使用C#构建移动（iOS，Android和Windows Mobile）应用程序时，Xamarin是您唯一的选择。

**DotNet Standard**  是一套官方定义的API规范，DotNet Framework 和 DotNet Core 都实现了这套规范。

|              | DotNet Framework              | DotNet Core                  |
| ------------ | ----------------------------- | ---------------------------- |
| 跨平台       | windows                       | windows、Linux、Mac          |
| 微服务       | 基于对Docker的支持，不建议    | 支持，有些中间件天然支持Core |
| 容器化       | 可以，但笨重                  | 轻                           |
| 多版本并行   | net 4.0 支持并行了也只是和3.5 | 全版本并行                   |
| 高性能可扩展 | 一般                          | 天然支持                     |
| 开源         | 否                            | 是                           |

再结合下图

![族谱](https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/image-20220212182623858.png)



## **.NET Core和ASP.NET Core区别**

1. .NET Core是运行时。它可以执行为其构建的应用程序。ASP.NET Core是构成一个用于构建Web应用程序的框架的库的集合。ASP.NET Core库可以在.NET Core和“完整.NET Framework”（Windows附带许多年）上使用。

2. 使用.NET Core的 ASP.NET CORE-所有依赖项都是自包含的，可以使用大多数Nuget包，不能使用Windows特定的包，可以在Windows，Linux，Mac上执行

3. 使用.NET Framework的 ASP.NET CORE-大多数依赖项都是自包含的，仅在Windows上执行，将有权访问Windows特定的Nuget软件包，需要在计算机上安装有针对性的.NET Framework版本

## **ASP.NET Core (.NET Core) and ASP.NET Core (.NET Framework)区别**

**ASP.NET Core (.NET Core)**

> 使用.Net Core运行时的ASP.NET Core可以支持跨平台(Windows, Mac, and Linux (包括Docker)),服务器不需要安装.Net Core，它的依赖与应用程序捆绑在一起。而且它是高性能的开源的框架。它能够在您自己的进程中托管IIS，Nginx，Apache，Docker或自托管。ASP.NET Core完全作为NuGet包发布。这允许您优化您的应用程序，使其仅包含必要的NuGet包。实际上，面向.NET Core的ASP.NET Core 2.x应用程序只需要一个NuGet包。应用程序表面积较小的好处，可以有更严格的安全性，更少的服务和更高的性能。可以使用 Kestrel web server。可以使用Visual Studio Code写代码。它现在还不支持Aspx, WPF, WCF and WebServices。它内置依赖注入的支持。可以使用coreclr，它是带有.net core的asp.net核心的运行时。

**ASP.NET Core (.NET Framework)**

> 使用.NET Framework运行时的ASP.NET Core可以支持桌面应用，也可以说是完整版。但这些应用程序只能在Windows上运行，但有关ASP.NET Core的其他所有内容的行为方式都相同。另一方面，.Net框架在2005年之前就开始了，它不断添加新功能，使其成为一个复杂的框架和更重的框架。它不是跨平台的。它支持Aspx，WPF，WCF和WebServices。

参考资料

1. [ASP.NET Core (.NET Core) and ASP.NET Core (.NET Framework)区别-CJavaPy](https://www.cjavapy.com/article/73/)
2. [.NET Framework， .NET Core 和.NET Standard的区别和联系_Will Wang0715的博客-CSDN博客](https://blog.csdn.net/qq_21209307/article/details/104891873?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2~default~CTRLIST~Rate-1.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2~default~CTRLIST~Rate-1.pc_relevant_default&utm_relevant_index=1)
