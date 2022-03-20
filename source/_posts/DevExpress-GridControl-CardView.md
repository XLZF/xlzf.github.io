---
title: DevExpress GridControl CardView
date: 2021-11-06 15:47:02
tags: [winform, DevExpress]
excerpt: DevGridControl中使用CardView 
categories: C#
index_img: https://gitee.com/xlzf/blog-image/raw/master/DevExpress.jpeg
---

# DevExpress GridControl 中使用 CardView

## 添加一个GridControl

![ConvertCardView](https://gitee.com/xlzf/blog-image/raw/master/image-20211106155039479.png)

## GridView 转 CardView

![CardView](https://gitee.com/xlzf/blog-image/raw/master/image-20211106155147102.png)

## 设置字段

![设置](https://gitee.com/xlzf/blog-image/raw/master/image-20211106155745779.png)

## 代码

``` csharp
// 实体类
public class PicEntity
{
    public string FILENAME { get; set; }

    public string FILEPATH { get; set; }

    public Byte[] PIC { get; set; }
}
```

``` csharp
/// <summary>
/// 选择图片
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void Btn_ChoosePic_Click(object sender, EventArgs e)
{
    OpenFileDialog fileDialog = new OpenFileDialog
    {
        Multiselect = true,

        Title = "请选择文件",

        Filter = "图片|*.jpg;*.png;*.gif;*.jpeg;*.bmp" //设置要选择的文件的类型
    };

    List<PicEntity> list = new List<PicEntity>();

    if (fileDialog.ShowDialog() == DialogResult.OK)
    {
        foreach (string file in fileDialog.FileNames)
        {
            string[] arr = file.Split('\\');

            using (Image img = Image.FromFile(file))
            {
                list.Add(new PicEntity { FILENAME = arr[arr.Length - 1], FILEPATH = file, PIC = FileContent(file) });//arr[arr.Length - 1]
            }
        }

        gridControl1.DataSource = list;
    }
}
```

``` Cs
/// <summary>
/// 文件转Byte数组
/// </summary>
/// <param name="fileName"></param>
/// <returns></returns>
private byte[] FileContent(string fileName)
{
    using (FileStream fs = new FileStream(fileName, FileMode.Open, FileAccess.Read))
    {
        try
        {
            byte[] buffur = new byte[fs.Length];
            fs.Read(buffur, 0, (int)fs.Length);
            return buffur;
        }
        catch (Exception ex)
        {
            throw ex;
        }
    }
}
```

``` csharp
/// <summary>
/// 构造函数
/// </summary>
public UC_PicList()
{
    InitializeComponent();
	
    //自适应高度
    cardView1.OptionsBehavior.FieldAutoHeight = true;

    //设置图片控件高度
    repositoryItemPictureEdit1.CustomHeight = 300;
}
```

## 成品

选择图片文件，把图片信息放到数据源中，展示到视图中。

![image-20211106161313136](https://gitee.com/xlzf/blog-image/raw/master/image-20211106161313136.png)

