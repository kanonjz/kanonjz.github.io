## 倒排索引
### 什么是倒排索引
ES使用倒排索引来加速全文检索。一个倒排索引由两部分组成：在文档中出现的所有不同的词以及对每个词，它所出现的文档的列表。例如：如果我们有两个文档，每一个文档有一个content字段，包含的内容如下：
1.The quick brown fox jumped over the lazy dog
2.Quick brown foxes leap over lazy dogs in summer
要创建一个倒排索引，首先要将content分割成单独的词，创建一个所有terms的列表，然后罗列每一个term出现的文档。此过程称为tokenization。
现在，如果我们想要搜索 quick brown，我们仅仅只需要找每一个term出现的文档即可。
### 持久性

### 索引更新机制


