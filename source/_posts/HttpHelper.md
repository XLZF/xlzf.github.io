---
title: C# HttpHelper
date: 2021-11-06 16:37:42
tags: [Helper]
excerpt: 这段时间在做外部接口，踩了点坑，记录一下.
categories: C#
index_img: https://gitee.com/xlzf/blog-image/raw/master/Gongsi/Csharp.jpeg
---

## 前言

这段时间在做外部接口，踩了点坑，记录一下

​		除了前期对外部接口和现有程序逻辑结合过程中的思路梳理。其中，有个坑，在`net4.6` 上写的demo，接口跑的溜不行，结果把程序放在产品中，嘎，废了。原因是，`net 4`框架下不支持` TLS1.2 `协议，也就是说，访问`http` 接口可以，访问`https`的接口不行，需要把下面`reg` 文件整理并且双击执行，让`net 4` 框架支持 `TLS1.2`协议。

## 执行 net4下安装TLS1.2.reg

``` shell
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\.NETFramework\v4.0.30319]
"SchUseStrongCrypto"=dword:00000001
```

## 代码

``` csharp
public class HttpHelper
{
    /// <summary>
    /// Post
    /// </summary>
    /// <param name="url"></param>
    /// <param name="postData"></param>
    /// <returns></returns>
    public static string PostUrl(string url, string postData)
    {
        HttpWebRequest request = null;
        if (url.StartsWith("https", StringComparison.OrdinalIgnoreCase))
        {
            request = WebRequest.Create(url) as HttpWebRequest;
            ServicePointManager.ServerCertificateValidationCallback = new RemoteCertificateValidationCallback(CheckValidationResult);
            request.ProtocolVersion = HttpVersion.Version11;
            // 这里设置了协议类型。
            ServicePointManager.SecurityProtocol = (SecurityProtocolType)3072;// SecurityProtocolType.Tls1.2; 
            request.KeepAlive = false;
            ServicePointManager.CheckCertificateRevocationList = true;
            ServicePointManager.DefaultConnectionLimit = 100;
            ServicePointManager.Expect100Continue = false;
        }
        else
        {
            request = (HttpWebRequest)WebRequest.Create(url);
        }

        request.Method = "POST";    //使用get方式发送数据
        request.ContentType = "text/plain"; //"application/x-www-form-urlencoded";
        //request.Referer = null;
        //request.AllowAutoRedirect = true;
        //request.UserAgent = "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.2; .NET CLR 1.1.4322; .NET CLR 2.0.50727)";
        request.Accept = "*/*";

        byte[] data = Encoding.UTF8.GetBytes(postData);
        Stream newStream = request.GetRequestStream();
        newStream.Write(data, 0, data.Length);
        newStream.Close();

        //获取网页响应结果
        HttpWebResponse response = (HttpWebResponse)request.GetResponse();
        Stream stream = response.GetResponseStream();
        //client.Headers.Add("Content-Type", "application/x-www-form-urlencoded");
        string result = string.Empty;
        using (StreamReader sr = new StreamReader(stream))
        {
            result = sr.ReadToEnd();
        }

        return result;
    }

    private static bool CheckValidationResult(object sender, X509Certificate certificate, X509Chain chain, SslPolicyErrors errors)
    {
        return true; //总是接受  
    }
}
```

