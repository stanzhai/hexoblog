title: 使用HttpWebRequest下载图片,保存到本地
date: 2013-07-03 16:27:40
categories: 学习
tags:
---

不废话了，直接上代码吧。

<!--more-->

```
// 以下载百度首页的logo为例
string url = "http://www.baidu.com/img/bdlogo.gif";
HttpWebRequest request = WebRequest.Create(url) as HttpWebRequest;
var response = request.GetResponse() as HttpWebResponse;
var stream = response.GetResponseStream();
// 保存到本地
FileStream fs = new FileStream("ok.jpg", FileMode.Create);
stream.CopyTo(fs, (int)response.ContentLength);
fs.Close();
```