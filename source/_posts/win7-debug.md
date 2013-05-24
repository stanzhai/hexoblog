title: Win7_64位,通过DosBox使用debug
date: 2013-05-20 08:42:04
categories: 工具
tags: 汇编
---

最新学习汇编，以前接触汇编的时候用的还是XP，当时在XP下使用debug做过简单的练习，3年后，发现Win7下的debug不见了。

微软在64位系统中，把16位的支持全部删除了，只能兼容32位, 微软认为64位的升级，可以把16位的历史累赘剔除，因此在Win7中也没有了debug这个命令。

<!--more-->

## 难道Win7下就不能用debug这个彪悍的命令了么？？

最近老弟也在学汇编，听他说可以用[DosBox](http://www.dosbox.com/download.php?main=1)，到网上找了一下这个东东，发现果然OK。

下载DosBox并安装。

还需要下载个Win7下可用的[debug](http://u.115.com/file/b3mmegwu)程序。

假设下载的debug程序放到了D盘根目录。

打开DosBox，通过一下命令使用debug：

```
mount c d:\
c:\debug.exe
```
