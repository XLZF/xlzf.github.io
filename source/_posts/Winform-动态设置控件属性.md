---
title: Winform 动态设置控件属性
date: 2021-11-01 21:25:09
tags: [winform, DevExpress]
excerpt: Winform 动态设置控件属性
categories: C#
index_img: https://gitee.com/MyHexo/blog-image/raw/master/Gongsi/Csharp.jpeg
---

## Winform 动态设置控件属性

本文用到的是 Winform + Dev 控件

主要功能：可以读取配置（从数据库或者配置文件中读取），通过配置控制前面控件的属性

* 放点控件

![](https://gitee.com/MyHexo/blog-image/raw/master/521993-20210830135410324-1392804038.png)

* 先准备用到的实体

``` CSharp

/// <summary>
/// 控件配置
/// </summary>
public class ControlList
{
    /// <summary>
    /// 控件NAME
    /// </summary>
    public string ControlName { get; set; }
    /// <summary>
    /// 控件类型
    /// </summary>
    public string ControlTypeName { get; set; }
    /// <summary>
    /// 控件默认值
    /// </summary>
    public string ControlDefaultVal { get; set; }
    /// <summary>
    /// 控件可用
    /// </summary>
    public bool ControlEnable { get; set; }
    /// <summary>
    /// 控件显示
    /// </summary>
    public ControlvisibleInfo ControlVisible { get; set; }
}

public class ControlvisibleInfo
{
    /// <summary>
    /// 显示值 
    /// </summary>
    public DevExpress.XtraLayout.Utils.LayoutVisibility IsVisible { get; set; }

    /// <summary>
    /// 包控件的Item
    /// </summary>
    public string PatientItem { get; set; }
}

```

* 正文

``` Csharp
public partial class Form5 : Form
{
    public Form5()
    {
        InitializeComponent();
    }

    private void Form5_Load(object sender, EventArgs e)
    {
        BindData();

        SetControlsPropertyVal();
    }

    public List<ControlList> listName = new List<ControlList>();

    public void SetControlsPropertyVal()
    {
        foreach (ControlList item in listName)
        {
            Control[] ctrols = layoutControl1.Controls.Find(item.ControlName, true);

            if (ctrols.Length > 0)
            {
                Type t = ctrols[0].GetType();

                if (t.Name == item.ControlTypeName && ctrols[0].Name == item.ControlName)
                {
                    object o = t.GetProperty("EditValue").GetValue(ctrols[0], null);

                    t.GetProperty("EditValue").SetValue(ctrols[0], item.ControlDefaultVal);

                    t.GetProperty("Enabled").SetValue(ctrols[0], item.ControlEnable);

                    if (item.ControlVisible != null)
                    {
                        DevExpress.XtraLayout.BaseLayoutItem obj = layoutControlGroup1.Items.FindByName(item.ControlVisible.PatientItem);

                        if (obj != null)
                        {
                            obj.Visibility = item.ControlVisible.IsVisible;
                        }
                    }
                }
            }
        }
    }

    public void BindData()
    {
        #region 绑定控件
        List<SeachLookUpEditDataBind> list = new List<SeachLookUpEditDataBind>
        {
            new SeachLookUpEditDataBind { ID = "1", NAME = "张三" },
            new SeachLookUpEditDataBind { ID = "2", NAME = "李四" },
            new SeachLookUpEditDataBind { ID = "3", NAME = "王五" },
            new SeachLookUpEditDataBind { ID = "4", NAME = "赵六" }
        };

        searchLookUpEdit1.Properties.ValueMember = "ID";
        searchLookUpEdit1.Properties.DisplayMember = "NAME";
        searchLookUpEdit1.Properties.DataSource = list;

        searchLookUpEdit2.Properties.ValueMember = "ID";
        searchLookUpEdit2.Properties.DisplayMember = "NAME";
        searchLookUpEdit2.Properties.DataSource = list;
        #endregion

        #region 设置控件属性值

        listName.Add(new ControlList
        {
            ControlName = "textEdit1",
            ControlTypeName = "TextEdit",
            ControlDefaultVal = "234",
            ControlEnable = true,
            ControlVisible = new ControlvisibleInfo
            {
                IsVisible = DevExpress.XtraLayout.Utils.LayoutVisibility.Always,
                PatientItem = "layoutControlItem2"
            }
        });
        listName.Add(new ControlList
        {
            ControlName = "searchLookUpEdit1",
            ControlTypeName = "SearchLookUpEdit",
            ControlDefaultVal = "1",
            ControlEnable = false,
            ControlVisible = new ControlvisibleInfo
            {
                IsVisible = DevExpress.XtraLayout.Utils.LayoutVisibility.Always,
                PatientItem = "layoutControlItem1"
            }
        });
        listName.Add(new ControlList
        {
            ControlName = "searchLookUpEdit2",
            ControlTypeName = "SearchLookUpEdit",
            ControlDefaultVal = "2",
            ControlEnable = true,
            ControlVisible = new ControlvisibleInfo
            {
                IsVisible = DevExpress.XtraLayout.Utils.LayoutVisibility.Never,
                PatientItem = "layoutControlItem3"
            }
        });
        listName.Add(new ControlList
        {
            ControlName = "radioGroup1",
            ControlTypeName = "RadioGroup",
            ControlDefaultVal = "1",
            ControlEnable = true,
            ControlVisible = new ControlvisibleInfo
            {
                IsVisible = DevExpress.XtraLayout.Utils.LayoutVisibility.Always,
                PatientItem = "layoutControlItem4"
            }
        });

        #endregion
    }
}
```

