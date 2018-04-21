---
layout:     post
title: "ElasticSearch理解（二）"
subtitle:   ""
date:       2018-04-01
author:     "Kanon"
header-img: "https://images.pexels.com/photos/843256/pexels-photo-843256.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940"
tags:
- ES
---
## 倒排索引
ES使用倒排索引来加速全文检索。一个倒排索引由两部分组成：在文档中出现的所有不同的词以及对每个词，它所出现的文档的列表。例如：如果我们有两个文档，每一个文档有一个content字段，包含的内容如下：

1.The quick brown fox jumped over the lazy dog

2.Quick brown foxes leap over lazy dogs in summer

要创建一个倒排索引，首先要将文本内容分割成一个个单独的词，然后罗列每一个词出现的文档。此过程称为tokenization。

之后，将这些词条进行标准化操作，如大写变成小写，去掉没用的词a、and、the，同义词转换等等。此过程称为normalization。

tokenization和normalization统称为analysis，ES里提供了标准的分析器来实现分析操作。

经过分析后倒排索引建立如下：
```
Term      Doc_1  Doc_2
-------------------------
brown   |   X   |  X
dog     |   X   |  X
fox     |   X   |  X
in      |       |  X
jump    |   X   |  X
lazy    |   X   |  X
over    |   X   |  X
quick   |   X   |  X
summer  |       |  X
the     |   X   |  X
------------------------
```

当倒排索引被加载进内存后，就会一直存在。只要内存空间足够大，所有的读操作就都是在内存中进行，速度之快可想而知。

## 索引更新机制
（1）新增的文档被提交到in-memory buffer，Transaction log会同步记下所有ES的操作

![](https://www.elastic.co/guide/en/elasticsearch/guide/current/images/elas_1106.png)

（2）in-memory buffer里的文档被refresh进新的segment，注意，此时新增的文档已经可以被搜索到，但仍在内存里还没持久化到磁盘中。

![](https://www.elastic.co/guide/en/elasticsearch/guide/current/images/elas_1107.png)

（3）不断有新文档提交，Transaction log文件越来越大

![](https://www.elastic.co/guide/en/elasticsearch/guide/current/images/elas_1108.png)

（4）进行flush操作也就是将数据写进磁盘，新的segment被记录进commit point，删除旧的translog，生成新的translog，清空in-memory buffer。

![](https://www.elastic.co/guide/en/elasticsearch/guide/current/images/elas_1109.png)

## segment merging

## 文档删除与修改

## 两个文件
Commit point：a file that lists all known segments

Translog：The translog provides a persistent record of all operations that have not yet been flushed to disk. When starting up, 
Elasticsearch will use the last commit point to recover known segments from disk, and will then replay all operations in the 
translog to add the changes that happened after the last commit.  
The translog is also used to provide real-time CRUD. When you try to retrieve, update, or delete a document by ID, it first 
checks the translog for any recent changes before trying to retrieve the document from the relevant segment. This means that 
it always has access to the latest known version of the document, in real-time.

## 三个关键操作Refresh、Flush、Segment Merging
### 触发时机
- Refresh：默认每秒进行一次
- Flush：默认每30分钟或当translog大于512MB
- Segment Merging：官网上并没明确说明进行时机，摘抄下原话：It happens automatically while you are indexing and searching. （这里面涉及到参数的调整，参考[文章](https://www.jianshu.com/p/9b872a41d5bb)）

## 为什么说ES的搜索时近实时（Near Real-time）
> Elasticsearch has near real-time search: document changes are not visible to search immediately, but will become visible within 1 second.

## 参考
[Inverted Index](https://www.elastic.co/guide/en/elasticsearch/guide/current/inverted-index.html)  
[Translog](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules-translog.html)  

<br><br><br><br>
