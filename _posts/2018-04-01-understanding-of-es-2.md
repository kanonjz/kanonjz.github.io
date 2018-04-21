## 倒排索引
### 什么是倒排索引
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

### 持久性


### 索引更新机制

## 参考
[Inverted Index](https://www.elastic.co/guide/en/elasticsearch/guide/current/inverted-index.html)
