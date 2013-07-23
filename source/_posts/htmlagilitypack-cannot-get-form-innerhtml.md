title: 解决HtmlAgilityPack无法获取form标签子节点的问题
date: 2013-07-23 15:08:51
categories: 问题修复
tags: 
---

## 问题描述

今天使用HtmlAgilityPack提取Form表单下的input节点，发现提取的form节点没有子节点，InnerHtml也是为空，起初以为是标签不全导致，后来分析html代码发现不可能是这个问题，提取div标签正常，偏偏form标签有问题，最终从网上找到了答案。

<!--more-->

## 解决方案

在将html转为htmlDoc之前，添加：

```
HtmlNode.ElementsFlags.Remove("form");
```

就可以正常提取到子节点的内容了，也就是：

```
HtmlNode.ElementsFlags.Remove("form");
HtmlDocument doc = new HtmlDocument();
doc.LoadHtml(html);

// 提取form表单节点
var formLinks = doc.DocumentNode.SelectNodes("//form[@action]");
```

## 参考资料

<http://www.crifan.com/htmlagilitypack_html_tag_form_option_no_child_via_sibling_get_innertext/>

## 额外收获

发现了另外一个用户html文档解析的工具`SgmlReader`，抽空了解一下。

到目前为止已经用过了HtmlAgilityPack和Tidy，感觉还是HtmlAgilityPack用起来方便。