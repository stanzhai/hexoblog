title: cubieboard 编译nodejs 0.10.15
date: 2013-07-28 00:17:59
categories: 
tags: cubieboard2
---

## 准备

下载nodejs源码:<http://nodejs.org/dist/v0.10.15/node-v0.10.15.tar.gz> 并解压。

<!--more-->

## 创建针对Arm处理器的编译脚本

1. 在nodejs源码目录下添加install.sh，内容如下：

```
#for 0.10.5
export GYPFLAGS="-Darmeabi=hard -Dv8_use_arm_eabi_hardfloat=true -Dv8_can_use_vfp3_instructions=true -Dv8_can_use_vfp2_instructions=true -Darm7=1"
./configure --without-snapshot --with-arm-float-abi=hard

#for 0.8.22
#export GYPFLAGS="-Darmeabi=hard -Dv8_use_arm_eabi_hardfloat=true -Dv8_can_use_vfp3_instructions=true -Dv8_can_use_vfp2_instructions=true -Darm7=1"
#./configure --prefix=/home/bigmusic/opt/node-v0.8.22 --without-snapshot --with-arm-float-abi=hard


#for 0.6.19
#./configure --prefix=/home/bigmusic/opt/node-v0.6.19 --without-snapshot --openssl-libpath=/usr/lib/ssl --openssl-includes=/usr/include/openssl

read -n 1 -p "Press ENTER to Continue or Press Ctrl+C to exit..."

make clean
make
make install
```

`chmod 777 install.sh`修改install.sh为可执行权限，运行`./install.sh`进行编译安装。

在cubieboard下编译比较慢，慢慢等吧，貌似得半小时左右吧。