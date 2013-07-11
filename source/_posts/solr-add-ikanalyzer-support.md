title: Solr4.3添加IKAnalyzer中文分词器
date: 2013-07-02 18:22:45
categories: 
tags:
---

## IKAnalyzer解压后的存放目录

solr-4.3.1/example/solr-webapp/webapp/WEB-INF/lib

## 修改schema.xml

文件路径: `solr-4.3.1/example/solr/collection1/conf/schema.xml`

添加text_ik类型,内容如下：

```xml
<fieldType name="text_ik" class="solr.TextField">  
    <analyzer type="index" isMaxWordLength="false" class="org.wltea.analyzer.lucene.IKAnalyzer"/>  
    <analyzer type="query" isMaxWordLength="true" class="org.wltea.analyzer.lucene.IKAnalyzer"/>  
</fieldType>  
```

<!--more-->

## lucene4.0与IKAnalyzer的冲突问题

运行之后发现异常：Exception in thread "main" java.lang.VerifyError: class org.wltea.analyzer.lucene.IKAnalyzer overrides final method tokenStream.(Ljava/lang/String;Ljava/io/Reader;)Lorg/apache/lucene/analysis/TokenStream;
原因IKAnalyzer中参考手册中的例子是使用的lucene3.4，与4.0已经是不兼容了。

从google 上面下载 [IK Analyzer 2012FF_hf1.zip](https://ik-analyzer.googlecode.com/files/IK%20Analyzer%202012FF_hf1.zip) 就可以解决问题。

## 启动，并查看分析效果

启动命令：

```
cd solr-4.3.1/example
java -jar start.jar
```

查看：

打开：http://10.6.154.250:8983/solr/#/collection1/analysis

FieldType选择： text_ik

FieldValue(Query)输入：中华人民共和国，点Analyse Values 显示结果中如果不是单字断开表示正常。

