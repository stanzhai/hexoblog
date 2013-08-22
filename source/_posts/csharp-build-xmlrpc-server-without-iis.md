title: C#无需IIS构建XmlRpc服务器
date: 2013-08-22 10:58:01
categories: 学习
tags: rpc
---

## 准备

我们使用CookComputing.XmlRpcServerV2 3.0.0来构建XmlRpc服务器。

新建一个控制台项目，在项目中添加对CookComputing.XmlRpcServerV2 3.0.0的引用，可以使用nuget来安装。

```
PM> Install-Package xmlrpcnet
PM> Install-Package xmlrpcnet-server
```

<!--more-->

## 编写服务

我这里写了个非常简单的服务，代码如下：

```
public class SimpleService : XmlRpcListenerService
{
  [XmlRpcMethod]
  public int Add(int a, int b)
  {
    return a + b;
  }
}
```

## 编写Service Host相关代码，也就是XmlRpc服务代码

这里我们通过HttpListener类处理XmlRpc客户端的请求，HttpListener使用的是异步处理，代码如下：

```
class Program
{
  private static XmlRpcListenerService _svc = new SimpleService();

  static void Main(string[] args)
  {
      HttpListener listener = new HttpListener();
      listener.Prefixes.Add("http://127.0.0.1:11000/");
      listener.Start();
      listener.BeginGetContext(new AsyncCallback(ProcessRequest), listener);
      Console.ReadLine();
  }

  static void ProcessRequest(IAsyncResult result)
  {
      HttpListener listener = result.AsyncState as HttpListener;
      // 结束异步操作
      HttpListenerContext context = listener.EndGetContext(result);
      // 重新启动异步请求处理
      listener.BeginGetContext(new AsyncCallback(ProcessRequest), listener);
      try
      {
          Console.WriteLine("From： " + context.Request.UserHostAddress);
          // 处理请求
          _svc.ProcessRequest(context);
      }
      catch (Exception ex)
      {
          Console.WriteLine(ex.Message);
      }
  }
}
```

启动程序后，打开浏览器访问：<http://127.0.0.1:11000/>就可以看到如下的页面，现在就可以调用XmlRpc服务了。

![XmlRpc服务](/images/xmlrpc.png)