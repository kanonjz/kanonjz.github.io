## 将文档存储到某个shard（Routing a Document to a Shard）
当我们新建一个文档时，ES是如何知道将它保存在哪个primary shard里呢？  
这里有一个计算公式：
```
shard = hash(routing) % number_of_primary_shards
```
`routing`的值默认是文档的`_id`值，也可以手动设置。最终得到的shard编号介于0到`number_of_primary_shards - 1`之间。

**应用场景**：手动设置`routing`的值，将属于某个用户的文档都存放在同一个shard里面。

## 分布式搜索原理
