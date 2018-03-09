---
layout:     post
title: "ElasticSearch理解(一)"
subtitle:   ""
date:       2018-03-09
author:     "Kanon"
header-img: "https://images.pexels.com/photos/106400/pexels-photo-106400.jpeg?w=1260&h=750&auto=compress&cs=tinysrgb"
tags:
- ES
---
ElasticSearch是一个基于Lucene的分布式全文搜索引擎。Elastic 的底层是开源库 Lucene，但是，你没法直接用 Lucene，必须自己写代码去调用它的接口。Elastic 是 Lucene 的封装，提供了 REST API 的操作接口，开箱即用。

## ES优点
1. 全文搜索引擎。有一个score对搜索结果的相关度进行评估。
2. 搜索速度快，近实时。
3. 高亮搜索。可以在搜索结果中高亮部分文本片段。
4. 聚合aggregation。与关系型数据库的GROUP BY类似。
5. 分布式，具有横向扩展性。但这个分布式对用户却是透明的，你不用去关心索引的分片在结点上如何进行分配，也不用关心聚合搜索时是如何查询并聚合数据的。

## 集群（cluster）
集群是一系列节点（node）的集合。集群通过名字来区分，默认是叫“elasticsearch”，节点是通过集群的名字来加入的。

## 节点（node）
每一个节点也通过名字来唯一标识，默认情况下节点在启动时会被分配一个随机的标识。

## 分片（shard）
- 一个分片就是一个Lucene的实例，它本身就是一个完整的搜索引擎
- Elasticsearch 是利用分片将数据分发到集群内各处的。分片是数据的容器，文档保存在分片内，分片又被分配到集群内的各个节点里。
- 技术上来说，一个主分片最大能够存储 Integer.MAX_VALUE - 128 个文档。
- 在索引建立的时候就已经确定了主分片数，但是副本分片数可以随时修改。
- 副本（replica）是数据的完整拷贝，副本分片只是某个主分片的拷贝
- 在同一个节点上既保存原始数据又保存副本是没有意义的，因为一旦失去了那个节点，我们也将丢失该节点上的所有副本数据。
- 如果增加副本分片而节点数目却不变，并不能提高性能，因为每个分片从节点上获得的资源会变少。你需要更多的硬件资源来提升吞吐量。
- 但是更多的副本分片数提高了数据冗余量：按照上面的节点配置，我们可以在失去2个节点的情况下不丢失任何数据。

## 映射（mapping）
[映射](https://www.elastic.co/guide/cn/elasticsearch/guide/current/mapping-intro.html)定义了类型中的域，每个域的数据类型，以及Elasticsearh如何处理这些域。  
如果在建立文档前未设置映射，则ES会使用动态映射，尝试猜测域类型并自动帮我们建立映射。
```
GET case/_mapping
```

## 索引（index）
- 在ES中，数据是存储和索引在分片中的，而一个索引仅仅是逻辑上的命名，而类型则允许你在索引中进一步对数据进行逻辑上的划分。
- 注意理解索引，从动词和名词的角度。
- 在 Elasticsearch 中， 每个字段的所有数据都是默认被索引的。即每个字段都有为了快速检索设置的专用倒排索引。而且，不像其他多数的数据库，它能在相同的查询中使用所有这些倒排索引，并以惊人的速度返回结果。
- 默认情况下，一个index会被分成5个primary shard，每个primary shard都有自己的replica shard，这些replica shard合起来刚好是索引的一份完整副本。

## 文档（document）
一个文档本质上就是一个被序列化为键值对的 JSON 对象。它是指最顶层或者根对象, 这个根对象被序列化成 JSON 并存储到 Elasticsearch 中，指定了唯一ID。

#### 文档三个必须的元数据
使用以下三个组合就可以唯一确定一个文档。当你创建一个新的文档时，要么提供自己的_id，要么让ES帮你生成。
- _index：文档在哪存放
- _type:文档表示的对象类别
- _id:文档唯一标识

#### 删除文档
删除文档不会立即将文档从磁盘中删除，只是将文档标记为已删除状态。随着你不断的索引更多的数据，Elasticsearch 将会在后台清理标记为已删除的文档。

#### 并发控制
ES本身提供了版本机制，每一个文档都有一个version字段。所有文档的更新或删除 API，都可以接受 version 参数。举个例子，在更新数据时，先读取这个版本号，在更新时指定要更新的版本号，如果最新版本号大于之前读取的版本号，说明有人改过，我们可能就要重新读取数据来修改。  
[官网文章](https://www.elastic.co/guide/cn/elasticsearch/guide/current/version-control.html)

## 集群健康
查询集群健康的命令。也可以在health后面加上具体的索引名称
```
GET _cluster/health
```
返回结果
```
{
  "cluster_name": "elasticsearch",
  "status": "yellow",
  "timed_out": false,
  "number_of_nodes": 1,
  "number_of_data_nodes": 1,
  "active_primary_shards": 26,
  "active_shards": 26,
  "relocating_shards": 0,
  "initializing_shards": 0,
  "unassigned_shards": 26,
  "delayed_unassigned_shards": 0,
  "number_of_pending_tasks": 0,
  "number_of_in_flight_fetch": 0,
  "task_max_waiting_in_queue_millis": 0,
  "active_shards_percent_as_number": 50
}
```
- 绿色：所有主分片和副本分片都正常运行
- 黄色：所有主分片都正常运行，但不是所有副本分片都正常运行
- 红色：有主分片没能正常运行

## 参考
[官网：Basic Concepts](https://www.elastic.co/guide/en/elasticsearch/reference/current/_basic_concepts.html)  

