---
title: Winform使用FontAwesome字体库
date: 2021-11-19 14:25:09
tags: [winform, DevExpress]
excerpt: 之前做BS时候，前端框架中就有相关的支持库，现在做CS，也可以用字体库啦。
categories: C#
index_img: https://gitee.com/xlzf/blog-image/raw/master/Gongsi/Csharp.jpeg
---

## Nuget FontAwesomeNet

![image-20211119113023849](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20211119113023849.png)

## 界面

放 Dev的俩 simpleButton 和一个 imageCollection

![image-20211119113135384](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20211119113135384.png)

## 代码

构造函数

``` csharp
public UC_UseFontAwesomeNet()
{
    InitializeComponent();

    // get FontAwesome icon class names(type is Dictionary<string, int>)
    string[] names = FontAwesome.TypeDict.Select(v => v.Key).ToArray();

    // use FontAwesome icon class name get FontAwesome icon Unicode value
    int val = FontAwesome.TypeDict["fa-heart"];//0xf004

    // defalut:
    Bitmap bmp = FontAwesome.GetImage(val);//0xf004
    Icon ico = FontAwesome.GetIcon(val);//0xf004

    // custom:
    FontAwesome.IconSize = 128;//change icon size
    FontAwesome.ForeColer = Color.Purple;//change icon forecolor
    Bitmap bmp1 = FontAwesome.GetImage(val);//0xf004
    Icon ico1 = FontAwesome.GetIcon(val);//0xf004

    imageCollection1.AddImage(bmp);
    imageCollection1.AddImage(bmp1);

    simpleButton1.ImageList = imageCollection1;
    simpleButton2.ImageList = imageCollection1;

    simpleButton1.ImageIndex = 0;
    simpleButton2.ImageIndex = 1;
}
```

## 结果

![成果](https://gitee.com/xlzf/blog-image/raw/master/Gongsi/image-20211119113430451.png)

## 查看图标编码

如下两个地址都可以。

https://www.bootcss.com/p/font-awesome/design.html  
http://www.fontawesome.com.cn/cheatsheet/ 