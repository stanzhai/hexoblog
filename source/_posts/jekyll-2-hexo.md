title: 将博客从jekyll迁移到了hexo
date: 2013-04-18 19:09:01
categories: 工具
tags: hexo
---

## 关于hexo和jekyll

hexo和jekyll一样都是个静态网站生成工具，hexo是一个台湾小伙使用nodejs开发的，jekyll则是用ruby开发，github内置了jekyll，可以直接将jekyll相关的文件提交到github，github会自动给你生成静态页面。

<!--more-->

hexo由于采用nodejs开发的，因此需要在本地生成静态页面后在提交到github，不过hexo内置了`hexo deploy`命令，提交博客也是挺方便的。

之前使用jekyll搭建的个人博客，由于jekyll对分页和文章摘要支持的不是很好，了解一下hexo这个新东西，感觉其页面生成速度还是蛮不错的，对分页和文章摘要也支持的挺好，主要是小清新的light主题吸引了我，于是乎，马上有种把博客迁移到hexo的念头。

## 安装hexo

首先需要安装[nodejs](http://nodejs.org/),安装成功后，在命令行下通过`npm install hexo -g`安装hexo。

根据hexo[官方文档](http://zespia.tw/hexo/),创建一个网站，生成页面，并通过`hexo server`本地查看网站，都挺简单的，不在啰嗦了。

## 迁移jekyll到hexo

这个对于程序员就比较简单了，对比一下两种markdown文件的差异，写个程序处理一下就OK了，我用c#处理的，贴出代码：

```
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;

namespace Jekyll2Hexo
{
    class Program
    {
        // 将jekyll中包含md文件的文件夹拖放到生成的exe上，处理后的文档保存在文件夹的上级目录
        // 本程序是针对jekyll-bootstrap博客中的文档做的处理
        static void Main(string[] args)
        {
            if (args.Length == 0)
            {
                return;
            }
            string folder = args[0];
            if (Directory.Exists(folder))
            {
                foreach (var file in Directory.GetFiles(folder, "*.md"))
                {
                    FileInfo fi = new FileInfo(file);
                    string date = fi.Name.Substring(0, 10);
                    StringBuilder sb = new StringBuilder();

                    FileStream fs = new FileStream(file, FileMode.Open);
                    using (StreamReader sr = new StreamReader(fs, Encoding.UTF8))
                    {
                        int lineCount = 0;
                        while (!sr.EndOfStream)
                        {
                            var line = sr.ReadLine();
                            if (line.StartsWith("tagline:"))
                            {
                                continue;
                            }
                            lineCount++;

                            if (lineCount == 1 || lineCount == 2 || lineCount == 4 || lineCount == 8)
                            {
                                continue;
                            }
                            if (lineCount == 3)
                            {
                                sb.AppendLine(line.Replace("\"", ""));
                                sb.AppendLine("date: " + date);
                                continue;
                            }
                            if (lineCount == 5)
                            {
                                sb.AppendLine("categories:" + line.Split(':')[1]);
                                continue;
                            }
                            if (lineCount == 16)
                            {
                                sb.AppendLine("<!-- more -->");
                            }
                            sb.AppendLine(line);
                        }
                    }
                    // 保存处理后的md文件
                    File.WriteAllText(Path.Combine(fi.Directory.Parent.FullName, fi.Name.Substring(11)),
                        sb.ToString(),
                        Encoding.UTF8);
                }
            }
        }
    }
}
```

## 遇到的一些问题

** 文章摘要设置 **

hexo和jekyll一样，都支持使用markdown编写文章，hexo的文章保存在source/_post目录下。

需要注意的是，在编写markdown文档是，在文档中插入`<!--more-->`就可以将文章切分开了，more以上的部分会已摘要的形式显示，当查看全文是more一下的部分也会显示出来。

** 图片路径问题 **

这个也很简单，直接将图片文件夹放到source目录下即可。

同理将favicon.ico和CNAME（github支持在CNAME文件中加入自定义域名，通过自定义的域名访问自己的网站）也放到source目录。
