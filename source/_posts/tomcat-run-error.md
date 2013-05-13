title: tomcat: org.apache.catalina.LifecycleException
date: 2013-03-17
categories: 问题修复
tags: [Tomcat]
---

## 问题描述

本机安装环境：jdk1.7 x64, Tomcat 7 x64, Eclipse Java EE IDE 64

运行jsp网站时出现如下错误：

<!-- more -->
	严重: A child container failed during start
	java.util.concurrent.ExecutionException: org.apache.catalina.LifecycleException: Failed to start component [StandardEngine[Catalina].StandardHost[localhost]]
	at java.util.concurrent.FutureTask$Sync.innerGet(Unknown Source)
	at java.util.concurrent.FutureTask.get(Unknown Source)
	at org.apache.catalina.core.ContainerBase.startInternal(ContainerBase.java:1142)
	at org.apache.catalina.core.StandardEngine.startInternal(StandardEngine.java:302)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
	at org.apache.catalina.core.StandardService.startInternal(StandardService.java:443)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
	at org.apache.catalina.core.StandardServer.startInternal(StandardServer.java:732)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
	at org.apache.catalina.startup.Catalina.start(Catalina.java:675)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(Unknown Source)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(Unknown Source)
	at java.lang.reflect.Method.invoke(Unknown Source)
	at org.apache.catalina.startup.Bootstrap.start(Bootstrap.java:322)
	at org.apache.catalina.startup.Bootstrap.main(Bootstrap.java:451)
	Caused by: org.apache.catalina.LifecycleException: Failed to start component [StandardEngine[Catalina].StandardHost[localhost]]
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:154)

## 解决方案

Tomcat7，目前支持的最高版本的jdk是1.6，安装个1.6版的jdk即可。
