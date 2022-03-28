---
title: Dev GridControl VS SeachLookUpEdit
date: 2021-11-01 21:38:50
tags: [winform, DevExpress]
excerpt: DevGridControl中，两SeachlookUpEdit联动.
categories: C#
index_img: https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/DevExpress.jpeg
---

为了实现在DevGridControl控件中，两列都用seachlookUpEdit 编辑，并且这两列需要联动,实现效果如下

https://blogimage-1255495010.cos.ap-beijing.myqcloud.com/521993-20210818221241151-2085448174.gif

*由于图放在Gitee，超过1M的就不能及时显示了*

``` CSharp
using DevExpress.XtraEditors.Repository;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace Dev10_2_8
{
    public partial class Form4 : Form
    {
        public Form4()
        {
            InitializeComponent();
        }

        private List<Objectes> list = new List<Objectes>();

        private List<SeachLookUpEditData> SLEPLIST = new List<SeachLookUpEditData>();

        private List<SeachLookUpEditData> SLESLIST = new List<SeachLookUpEditData>();

        private void Form4_Load(object sender, EventArgs e)
        {
            Init();
        }

        public void Init()
        {
            #region 构建基础测试数据

            list.Add(new Objectes
            {
                ID = "1",
                NAME = "张三",
                CLASSP = "a",
                CLASSSON = "a1"
            });

            list.Add(new Objectes
            {
                ID = "2",
                NAME = "李四",
                CLASSP = "b",
                CLASSSON = "b1"
            });

            #endregion

            #region 构建SeachLookUpedit 数据源

            SLEPLIST.Add(new SeachLookUpEditData
            {
                ID = "a",
                NAME = "父节点A",
                PID = "0"
            });

            SLEPLIST.Add(new SeachLookUpEditData
            {
                ID = "b",
                NAME = "父节点B",
                PID = "0"
            });

            SLESLIST.Add(new SeachLookUpEditData
            {
                ID = "a1",
                NAME = "子节点A1",
                PID = "a"
            });

            SLESLIST.Add(new SeachLookUpEditData
            {
                ID = "a2",
                NAME = "子节点A2",
                PID = "a"
            });

            SLESLIST.Add(new SeachLookUpEditData
            {
                ID = "b1",
                NAME = "子节点B1",
                PID = "b"
            });

            SLESLIST.Add(new SeachLookUpEditData
            {
                ID = "b2",
                NAME = "子节点B2",
                PID = "b"
            });

            #endregion

            #region 绑定

            this.repositoryItemSearchLookUpEdit1.DataSource = SLEPLIST;

            this.repositoryItemSearchLookUpEdit2.DataSource = SLESLIST;

            this.gridControl1.DataSource = list;

            #endregion
        }

        /// <summary>
        /// 关键方法
        /// </summary>
        /// <param name="sender"></param>
        /// <param name="e"></param>
        private void gridView1_CustomRowCellEditForEditing(object sender, DevExpress.XtraGrid.Views.Grid.CustomRowCellEditEventArgs e)
        {
            if (e.Column.FieldName == "CLASSSON")
            {
                object oid = gridView1.GetFocusedRowCellValue("CLASSP");

                if (oid == null) return;

                string Pid = oid.ToString();

                RepositoryItemSearchLookUpEdit lookup = ReturnLook(Pid);

                e.RepositoryItem = lookup;
            }
        }

        /// <summary>
        /// 生产控件
        /// </summary>
        /// <param name="Pid"></param>
        /// <returns></returns>
        private RepositoryItemSearchLookUpEdit ReturnLook(string Pid)
        {
            var list = SLESLIST.Where(p => p.PID == Pid).ToList();

            RepositoryItemSearchLookUpEdit obj = new RepositoryItemSearchLookUpEdit
            {
                ValueMember = "ID",

                DisplayMember = "NAME",

                DataSource = list
            };

            return obj;
        }

        /// <summary>
        /// 防止选择完父节点后，对应的子节点字段与父节点字段冲突 比如 选择的父节点是b，但是这个时候子节点还是 a的子节点。
        /// </summary>
        /// <param name="sender"></param>
        /// <param name="e"></param>
        private void repositoryItemSearchLookUpEdit1_EditValueChanged(object sender, EventArgs e)
        {
            var val = ((DevExpress.XtraEditors.BaseEdit)sender).EditValue;

            if (val != null)
            {
                object Sid = gridView1.GetFocusedRowCellValue("CLASSSON");

                var list = SLESLIST.Where(p => p.PID == val.ToString() && p.ID == Sid.ToString()).ToList();

                if (list.Count == 0)
                {
                    gridView1.SetFocusedRowCellValue("CLASSSON", null);
                }
            }
        }
    }

    public class Objectes
    {
        public string ID { get; set; }

        public string NAME { get; set; }

        public string CLASSP { get; set; }

        public string CLASSSON { get; set; }
    }

    public class SeachLookUpEditData
    {
        public string ID { get; set; }

        public string NAME { get; set; }

        public string PID { get; set; }
    }
}
```
