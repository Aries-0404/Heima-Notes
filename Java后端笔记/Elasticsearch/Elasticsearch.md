# Elasticsearch

## 1、elasticsearch

### 是什么：

**Elasticsearch** 是一个分布式搜索引擎，主要用于存储、搜索和分析大量数据。Elasticsearch 提供了强大的全文搜索、实时分析和可扩展性，广泛应用于日志管理、数据分析、推荐系统、内容搜索等领域。

### 为什么：

#### （1）为什么要使用elasticsearch?

1. **强大的搜索功能**

   - **全文搜索**：Elasticsearch 提供高效的全文搜索能力，支持模糊查询、短语查询、分词搜索等高级查询功能。它能够根据文本的相关性排名返回搜索结果，这对于文本内容的搜索非常强大。

   - **高效的查询**：Elasticsearch 通过倒排索引优化了搜索性能，可以快速返回查询结果，尤其是对于大量文档的情况。

2. **高可扩展性和分布式架构**

   - **分布式架构**：Elasticsearch 是一个分布式系统，可以通过增加节点来扩展集群，以处理更大的数据量或更高的查询吞吐量。

   - **自动分片与副本**：Elasticsearch 支持将数据划分为多个分片，每个分片可以有多个副本，分布在不同的节点上。这不仅能提高查询性能，还能确保数据的高可用性和容错性。

### 怎么办：

#### （1）如何安装elasticsearch？

![image-20241207124900921](./Elasticsearch.assets/image-20241207124900921.png)



##### notes：将es的镜像压缩文件上传到虚拟机并加载成镜像的过程

- 将es.tar文件上传到Centos7_03这个虚拟机中

![image-20241203091820279](./Elasticsearch.assets/image-20241203091820279.png)



- 接着利用docker将这两个tar文件加载成镜像

![image-20241203092007168](./Elasticsearch.assets/image-20241203092007168.png)





## 2、倒排索引

### 是什么：

**倒排索引（Inverted Index）** 是一种常用于文本搜索引擎（如 Elasticsearch）的数据结构，可以提高大规模文档集合中的搜索效率，elasticsearch正是因为采用了这种搜索引擎的技术所以搜索效率才这么高的。

### 为什么：

#### （1）为什么要使用倒排索引？

![image-20241207164451412](./Elasticsearch.assets/image-20241207164451412.png)

### 怎么办：

#### （1）倒排索引与正排索引的对比？

![image-20241207164037603](./Elasticsearch.assets/image-20241207164037603.png)





## 3、es中的基本概念

### （1）文档和字段：相当于mysql中的一条数据和数据的字段

![image-20241207164741218](./Elasticsearch.assets/image-20241207164741218.png)



### （2）索引和映射：相当于mysql中的表和表结构关系

![image-20241216092159003](./Elasticsearch.assets/image-20241216092159003.png)

![image-20241207165003048](./Elasticsearch.assets/image-20241207165003048.png)





#### notes：mysql和es的结构区别

![image-20241207165251290](./Elasticsearch.assets/image-20241207165251290.png)





## 4、IK分词器

### 是什么：

是es中用于中文文本识别的一个插件

### 为什么：

#### （1）为什么要使用ik分词器？

前面我们说了es的倒排索引是通过将文本内容拆分成词条然后对词条建立索引的，这里就需要对文本内容很熟悉才能将文本拆分成一个个有意义的词条，而现在对于英文文本的识别很方便，对于中文文本的识别就很难，而IK分词器就是这样的一个用于中文文本识别的插件（它里面的词库配置了很多常用的中文词条，可以根据这些词条进行识别，并且对于新兴的文本还可以在词库中进行补充）

### 怎么办：

#### （1）IK分词器的安装与使用

![image-20241207170525174](./Elasticsearch.assets/image-20241207170525174.png)

![image-20241207170703188](./Elasticsearch.assets/image-20241207170703188.png)





## 5、索引库操作：对应mysql中数据表的增删改查（表的创建、删除等）

### `index` 索引：对应mysql中的数据表

### `mapping`映射：对应mysql中的表结构

### `properties` 字段：对应mysql中表的字段（对于es中的每一个字段都具备4种mapping属性，分别是：`type 字段数据类型`、`index 是否创建索引`、`analyzer 使用哪种分词器`、`properties 该字段的子字段`）

![image-20241207172242874](./Elasticsearch.assets/image-20241207172242874.png)



## 6、文档操作：对应mysql中表记录的增删改查

### （1）新增文档：相当于mysql中增加一条表记录

![image-20241207205315358](./Elasticsearch.assets/image-20241207205315358.png)



### （2）删除文档：相当于mysql中删除一条表记录

![image-20241207205520658](./Elasticsearch.assets/image-20241207205520658.png)



### （3）修改文档：相当于mysql中修改一条表记录

![image-20241207205744176](./Elasticsearch.assets/image-20241207205744176.png)



### （4）查询文档：相当于mysql中查询一条表记录

![image-20241207205430743](./Elasticsearch.assets/image-20241207205430743.png)





## 7、文档的批处理操作：对应mysql中多条表记录的批处理操作

### （1）文档批量新增操作

```bash
//首先创建es中的表
PUT /my_index
{
  "mappings": {
    "properties": {
      "name": { "type": "text" },
      "age": { "type": "integer" }
    }
  }
}

//接着进行批量新增文档的操作
POST /my_index/_bulk
{ "index": { "_id": 1 } }
{ "name": "Alice", "age": 25 }
{ "index": { "_id": 2 } }
{ "name": "Bob", "age": 30 }
{ "index": { "_id": 3 } }
{ "name": "Charlie", "age": 35 }
```

#### notes：文档批量新增操作与普通的文档新增操作的区别

![image-20241211090820185](./Elasticsearch.assets/image-20241211090820185.png)

#### notes：使用 Bulk API 的注意事项：

- 每个操作（如 `index`, `update`, `delete`）都必须单独一行，后面跟着要操作的文档或数据。
- 不能在一个批量请求中进行查询操作。
- 如果批量请求中的某个操作失败，Elasticsearch 会返回该操作的错误信息，而其他操作依然会执行成功。
- 每个请求的大小应注意控制，通常每个请求的操作数不应超过 1000，避免过大请求影响性能。



### （2）文档批量删除操作

```bash
//首先创建es中的表
PUT /my_index
{
  "mappings": {
    "properties": {
      "name": { "type": "text" },
      "age": { "type": "integer" }
    }
  }
}

//接着进行批量新增文档的操作
POST /my_index/_bulk
{ "index": { "_id": 1 } }
{ "name": "Alice", "age": 25 }
{ "index": { "_id": 2 } }
{ "name": "Bob", "age": 30 }
{ "index": { "_id": 3 } }
{ "name": "Charlie", "age": 35 }

//接着批量删除文档
POST /my_index/_bulk
{ "delete": { "_id": 1 } }
{ "delete": { "_id": 2 } }
```



### （3）文档批量修改操作

```bash
//首先创建es中的表
PUT /my_index
{
  "mappings": {
    "properties": {
      "name": { "type": "text" },
      "age": { "type": "integer" }
    }
  }
}

//接着批量新增文档
POST /my_index/_bulk
{ "index": { "_id": 1 } }
{ "name": "Alice", "age": 25 }
{ "index": { "_id": 2 } }
{ "name": "Bob", "age": 30 }
{ "index": { "_id": 3 } }
{ "name": "Charlie", "age": 35 }

//最后批量修改文档
POST /my_index/_bulk
{ "update": { "_id": 1 } }
{ "doc": { "age": 26 } }
{ "update": { "_id": 2 } }
{ "doc": { "age": 31 } }
```



### （4）文档批量查询操作

在es中并没有直接提供批量查询文档的方法



### （5）文档批量混合操作

批量操作不仅可以包含插入、更新和删除操作，还可以混合使用多种操作。例如，我们可以在同一个请求中同时进行文档的插入、更新和删除。

```bash
//首先创建es中的表
PUT /my_index
{
  "mappings": {
    "properties": {
      "name": { "type": "text" },
      "age": { "type": "integer" }
    }
  }
}

//接着批量新增文档
POST /my_index/_bulk
{ "index": { "_id": 1 } }
{ "name": "Alice", "age": 25 }
{ "index": { "_id": 2 } }
{ "name": "Bob", "age": 30 }
{ "index": { "_id": 3 } }
{ "name": "Charlie", "age": 35 }

//最后进行批量混合操作
POST /my_index/_bulk
{ "index": { "_id": 1 } }
{ "name": "Alice", "age": 25 }
{ "update": { "_id": 2 } }
{ "doc": { "age": 31 } }
{ "delete": { "_id": 3 } }
```

**解释：**

- 第一个操作是插入文档。
- 第二个操作是更新 ID 为 2 的文档。
- 第三个操作是删除 ID 为 3 的文档。





## 8、利用es的JavaAPI进行索引库的增删改查操作

### 是什么：

es中提供的供给Java语言使用的接口，Java语言可以调用这些接口从而实现操作es

### 为什么：

es是个很方便的搜索引擎，但是我们在编写Java代码的时候不可能直接通过es的图形化界面进行操作，所以需要一个Java语言来操作es的工具（这个工具里面封装了各种操作es的方法，我们可以通过调用这些方法实现操作es）

### 怎么办：

#### （1）es的常用Java API是什么？

![image-20241216232107114](./Elasticsearch.assets/image-20241216232107114.png)



#### （2）如何使用es的Java API？

![image-20241216232230290](./Elasticsearch.assets/image-20241216232230290.png)



#### （3）如何使用es的JavaAPI实现索引库的增删改查操作（对应mysql中数据表的增删改查）？

##### ①创建索引库

![image-20241216232757968](./Elasticsearch.assets/image-20241216232757968.png)

###### notes：对比上述es的JavaAPI创建索引库和在es界面中创建索引库操作的异同

- 相同点：
  - 二者目的相同，都是要在es中创建索引库
- 不同点：
  - 执行位置不同：一个是在Java代码中创建，一个是在es的图形化界面中通过命令创建
  - 二者的语法有些区别但大体类似，都是通过发送http请求，不同的是javaAPI中还包括单独的创建请求对象、发送请求对象的过程





##### ②删除索引库

![image-20241216234001050](./Elasticsearch.assets/image-20241216234001050.png)



##### ③修改索引库

es的JavaAPI中没法修改索引库



##### ④查询索引库

![image-20241216234402230](./Elasticsearch.assets/image-20241216234402230.png)



## 9、利用es的JavaAPI进行文档的增删改查操作

### （1）新增文档

![image-20241217091407300](./Elasticsearch.assets/image-20241217091407300.png)



### （2）删除文档

![image-20241217091725376](./Elasticsearch.assets/image-20241217091725376.png)

### （3）修改文档

![image-20241217092039706](./Elasticsearch.assets/image-20241217092039706.png)



### （4）查询文档

![image-20241217092252788](./Elasticsearch.assets/image-20241217092252788.png)



### （5）文档批处理操作

#### 是什么：

前面我们介绍的文档的增删改查操作每次都是针对单个文档，如果需要操作多个文档的话一次一次的进行太慢了，所以就可以对文档进行批处理操作，一次性操作多个文档

#### 怎么办：

##### ①如何对文档进行批处理操作呢？

![image-20241217093130714](./Elasticsearch.assets/image-20241217093130714.png)





## 10、DSL查询

### 是什么：

DSL是一种查询语言，类似于数据库查询中的sql语言，在es中使用 DSL 查询可以灵活地组合多种查询条件，支持丰富的查询操作。

### 为什么：

#### （1）为什么要使用DSL查询？

前面我们进行的文档的增删改查操作中的查询都是根据文档id进行的精确查询，并不涉及查询条件以及关键字的模糊查询，但是这样的话就没法发挥es倒排索引的优势，所以要使用DSL查询来构建查询条件

### 怎么办：

#### （1）DSL查询的分类？

##### ①叶子查询：对文档中某个字段的匹配查询

![image-20241222112655826](./Elasticsearch.assets/image-20241222112655826.png)

##### ②复合查询：将文档中的多个叶子查询组合在一起形成一个新的查询

![image-20241222112915116](./Elasticsearch.assets/image-20241222112915116.png)



#### （2）如何进行DSL查询？

![image-20241222114101074](./Elasticsearch.assets/image-20241222114101074.png)



##### notes：DSL查询语句中的`_search`是什么？

![image-20241222120255425](./Elasticsearch.assets/image-20241222120255425.png)

`_search` 是 Elasticsearch 提供的一个通用搜索接口，其内部封装了包括==搜索、排序、分页==在内的多种方法，我们正是通过调用该接口中的方法实现对文档的模糊搜索、排序、分页的功能。





#### （3）对查询结果进行排序操作

![image-20241222115555223](./Elasticsearch.assets/image-20241222115555223.png)



#### （4）对查询结果进行分页操作

![image-20241222120047166](./Elasticsearch.assets/image-20241222120047166.png)



#### （5）对查询结果进行高亮显示

![image-20241222143944879](./Elasticsearch.assets/image-20241222143944879.png)



## 11、利用es的JavaAPI实现DSL查询

### （1）基本示例

![image-20241222155402375](./Elasticsearch.assets/image-20241222155402375.png)



### （2）利用es的JavaAPI实现叶子查询

![image-20241222150038693](./Elasticsearch.assets/image-20241222150038693.png)

### （3）利用es的JavaAPI实现复合查询

![image-20241222150219992](./Elasticsearch.assets/image-20241222150219992.png)



### （4）利用es的JavaAPI实现排序和分页

![image-20241222150326319](./Elasticsearch.assets/image-20241222150326319.png)

### （5）利用es的JavaAPI实现高亮

![image-20241222150549683](./Elasticsearch.assets/image-20241222150549683.png)





## 12、数据聚合

### 是什么：

数据聚合指的是将一堆数据按照某一个字段进行分组，在每一个分组内计算数据的平均值、最大值、最小值等统计数据

### 怎么办：

#### （1）如何利用DSL实现聚合？

##### ①利用DSL实现Bucket聚合

![image-20241222153928399](./Elasticsearch.assets/image-20241222153928399.png)



##### ②利用DSL实现带条件聚合：就是在条件查询结果的基础上进行聚合

![image-20241222154125201](./Elasticsearch.assets/image-20241222154125201.png)

##### ③利用DSL实现Metric聚合：就是在Bucket聚合的结果内再统计平均值、最大值、最小值等信息

![image-20241222154343405](./Elasticsearch.assets/image-20241222154343405.png)

#### （2）利用es的JavaAPI实现聚合

![image-20241222154648122](./Elasticsearch.assets/image-20241222154648122.png)
