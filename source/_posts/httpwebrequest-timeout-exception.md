title: 解决.NET中HttpWebRequest第一次请求超时、缓慢问题
date: 2013-07-08 15:54:42
categories: 问题修复
tags: CSharp
---

## 问题描述

Windows7下写的一个小工具，用到了HttpWebRequest，第一次请求经常超时，而第二次就很快。

可能是和系统不同的“默认代理和用户验证策略”有关。大家有知道准确原因的请指教。

<!--more-->

## 解决方案

修改app.config文件，在configuration节点中添加

```xml
<system.net>    
   <defaultProxy   
      enabled="false"    
      useDefaultCredentials="false">    
     <proxy/>    
     <bypasslist/>    
     <module/>    
   </defaultProxy>
</system.net>
```

禁用默认代理。