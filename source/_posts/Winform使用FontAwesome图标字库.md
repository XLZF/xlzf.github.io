---
title: Winform使用FontAwesome字体库
date: 2021-11-19 14:25:09
tags: [winform, DevExpress]
excerpt: 之前做BS时候，前端框架中就有相关的支持库，现在做CS，也可以用字体库啦。
categories: C#
index_img: https://gitee.com/MyHexo/blog-image/raw/master/Gongsi/Csharp.jpeg
---

# 方式1

## Nuget FontAwesomeNet

![image-20211119113023849](https://gitee.com/MyHexo/blog-image/raw/master/Gongsi/image-20211119113023849.png)

## 界面

放 Dev的俩 simpleButton 和一个 imageCollection

![image-20211119113135384](https://gitee.com/MyHexo/blog-image/raw/master/Gongsi/image-20211119113135384.png)

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

![成果](https://gitee.com/MyHexo/blog-image/raw/master/Gongsi/image-20211119113430451.png)

## 查看图标编码

如下两个地址都可以。

https://www.bootcss.com/p/font-awesome/design.html  
http://www.fontawesome.com.cn/cheatsheet/ 

**需要将 &#x 替换成 \u 最终是\ue603**

# 方式2

## 添加按钮

通方式1中一样，都用imageCollection 去指向

![再添加一个按钮](https://gitee.com/MyHexo/blog-image/raw/master/Gongsi/image-20211119161159228.png)

## 代码

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
	// ***********************上面使用的是FontAwesomeNet****************
	// ***********************simpleButton3 使用转换方法****************
    //simpleButton3 
    Image img = GetFontImage("\uf2cd", Color.Red, 20);
    imageCollection1.AddImage(img);
	simpleButton3.ImageList = imageCollection1;
    simpleButton3.ImageIndex = 2;

    // ***********************这里使用的是直接设置**********************************
    //PrivateFontCollection pfc = new PrivateFontCollection();

    //string path = Path.Combine(AppDomain.CurrentDomain.BaseDirectory, "font/fontawesome-webfont.ttf");

    //pfc.AddFontFile(path);

    //simpleButton3.Text = "\uf2cd";

    //simpleButton3.Font = new Font(pfc.Families[0], 20);

    //simpleButton3.ForeColor = Color.Red;

}
```

帮助转换方法

```csharp
//https://www.bootcss.com/p/font-awesome/design.html  图标代码
//http://www.fontawesome.com.cn/cheatsheet/ 图标代码
/// <summary>
/// 通过FontAwesome设置字体
/// </summary>
public class FontAwesomeHelper
{
    /// <summary>
    /// 字体图标生成图标
    /// </summary>
    /// <param name="fontIco">图标编码</param>
    /// <param name="color">颜色</param>
    /// <param name="size">大小</param>
    /// <returns></returns>
    public static Image GetFontImage(string fontIco, Color color, int size)
    {
        Bitmap bmp = new Bitmap(size, size);
        Graphics g = Graphics.FromImage(bmp);
        g.TextRenderingHint = TextRenderingHint.AntiAliasGridFit;
        g.InterpolationMode = InterpolationMode.HighQualityBilinear;
        g.PixelOffsetMode = PixelOffsetMode.HighQuality;
        g.SmoothingMode = SmoothingMode.HighQuality;

        string ch = fontIco;
        Font font = GetAdjustedFont(g, ch, size, size, 4, true);
        SizeF stringSize = g.MeasureString(ch, font, size);
        float w = stringSize.Width;
        float h = stringSize.Height;
        float left = (size - w) / 2;
        float top = (size - h) / 2;
        // Draw string to screen.
        SolidBrush brush = new SolidBrush(color);
        g.DrawString(ch, font, brush, new PointF(left, top));
        return bmp;
    }

    /// <summary>
    /// 找字体图片
    /// </summary>
    /// <param name="g">图片矢量</param>
    /// <param name="graphicString">图标编码</param>
    /// <param name="containerWidth">宽</param>
    /// <param name="maxFontSize">最大Size</param>
    /// <param name="minFontSize">最小Size</param>
    /// <param name="smallestOnFail">找不到最小找最大</param>
    /// <returns></returns>
    private static Font GetAdjustedFont(Graphics g, string graphicString, int containerWidth, int maxFontSize, int minFontSize, bool smallestOnFail)
    {
        for (double adjustedSize = maxFontSize; adjustedSize >= minFontSize; adjustedSize = adjustedSize - 0.5)
        {
            Font testFont = GetIconFontFromResource((float)adjustedSize);
            SizeF adjustedSizeNew = g.MeasureString(graphicString, testFont);
            if (containerWidth > Convert.ToInt32(adjustedSizeNew.Width))
            {
                return testFont;
            }
        }
        return GetIconFontFromResource(smallestOnFail ? minFontSize : maxFontSize);
    }

    /// <summary>
    /// 去文件夹中去找字体文件
    /// </summary>
    /// <param name="size"></param>
    /// <returns></returns>
    private static Font GetIconFontFromFilePath(float size)
    {
        PrivateFontCollection pfc = new PrivateFontCollection();

        string path = Path.Combine(AppDomain.CurrentDomain.BaseDirectory, "font/fontawesome-webfont.ttf");

        pfc.AddFontFile(path);

        return new Font(pfc.Families[0], size, GraphicsUnit.Point);
    }

    /// <summary>
    /// 去项目资源中去找字体文件
    /// 记录坑，fontawesome-webfont.ttf 加到资源中名称变成 fontawesome_webfont里
    /// </summary>
    /// <param name="size"></param>
    /// <returns></returns>
    private static Font GetIconFontFromResource(float size)
    {
        PrivateFontCollection pfc = new PrivateFontCollection();

        Assembly myAssem = Assembly.GetEntryAssembly();

        ResourceManager rm = new ResourceManager(myAssem.GetName().Name.ToString() + ".Properties.Resources", myAssem);

        byte[] rgbyt = (byte[])rm.GetObject("fontawesome_webfont");

        IntPtr pbyt = Marshal.AllocCoTaskMem(rgbyt.Length);

        if (null != pbyt)
        {
            Marshal.Copy(rgbyt, 0, pbyt, rgbyt.Length);

            pfc.AddMemoryFont(pbyt, rgbyt.Length);

            Marshal.FreeCoTaskMem(pbyt);

            return new Font(pfc.Families[0], size, GraphicsUnit.Point);
        }
        else
        {
            return null;
        }
    }
```

## 结果

![成果](https://gitee.com/MyHexo/blog-image/raw/master/Gongsi/image-20211119161929305.png)



