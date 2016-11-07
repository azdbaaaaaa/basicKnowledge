# basicKnowledge
some basic knowledge that QA should know

# lession 1 - 数据库

## 关系型数据库（SQL）

简单来说，关系模型指的就是二维表格模型，而一个关系型数据库就是由二维表及其之间的联系所组成的一个数据组织。

特点：
ACID是Atomic原子性，
Consistency一致性，
Isolation隔离性，
Durability持久性

瓶颈：

- 高并发读写需求  
网站的用户并发性非常高，往往达到每秒上万次读写请求，对于传统关系型数据库来说，硬盘I/O是一个很大的瓶颈

- 海量数据的高效率读写  
网站每天产生的数据量是巨大的，对于关系型数据库来说，在一张包含海量数据的表中查询，效率是非常低的

- 扩展性和可用性
在基于web的结构当中，数据库是最难进行横向扩展的，业务发展的同时，表的结构却是固定了的，所以需要对表的结构进行扩展需要停机维护和数据迁移

典型是：mqSql， Oracle

## 非关系型数据库（NoSQL）

- 面向高性能并发读写的key-value数据库  
key-value数据库的主要特点即使具有极高的并发读写性能，Redis就是这类的代表

- 面向海量数据访问的面向文档数据库  
这类数据库的特点是，可以在海量的数据中快速的查询数据，典型代表为MongoDB

- 面向可扩展性的分布式数据库  
这类数据库想解决的问题就是传统数据库存在可扩展性上的缺陷，这类数据库可以适应数据量的增加以及数据结构的变化

非关系型数据库严格上不是一种数据库，应该是一种数据结构化存储方法的集合。
必须强调的是，数据的持久存储，尤其是海量数据的持久存储，还是需要一种关系数据库这员老将

## MongoDB 常用命令

### 基本命令

`mongo` 进入mongodb命令行

`show dbs` 显示当前可用的database

`use 数据库名` 切换到对应的数据库 

`db.help()` 显示数据库操作帮助文档

`db.getCollectionNames()` 显示当前数据库中文档的列表

### 查询

`db.users.find()` 条件查询,查询users这个表中所有的数据（相当于select * from users）

`db.users.findOne()` 条件查询,查询users这个表中的一条数据（相当于select * from users limit 1）

`db.users.find({age: 25})` 条件查询,查询出users这个表中符合条件的数据

`db.users.find({age: {$gt: 22}})` 条件查询,使用操作符查询出users表中年龄大于22的数据（$lt, $gte, $lte）

`db.users.find({name: /mongo/})` 模糊查询，查询出name字段模糊匹配mongo的数据，（相当于select * from users where name like '%mongo%'）

`db.users.find({name: /^mongo/})` 模糊查询，（相当于select * from users where name like 'mongo%'）

`db.users.find({}, {name: 1, age: 1})` 显示指定列，1为显示，0为不显示

`db.users.find({}).sort({age: -1})` 排序显示，-1为逆序，1为正序

`db.users.find({}).limit(5)` 限制显示数量,查询前5条数据

`db.users.find({}).skip(10)` 限制显示数量,查询10条以后的数据

`db.users.find({}).limit(10).skip(5)` 限制显示数量,查询6-10条的数据，可用于分页显示

`db.users.find({$or: [{age: 22}, {age: 25}]})` 条件查询，使用操作符进行逻辑或条件查询

`db.users.find({age: {$gte: 25}}).count()` 只查询出符合条件的数量

### 索引，测试用的不多
`db.users.getIndexes()` 查询当前聚集集合所有索引

### 添加
`db.users.save({name: ‘zhangsan', age: 25, sex: true})` 往数据库中添加数据

### 删除
`db.users.remove({age: 132})` 删除符合条件的数据

### 修改
`db.users.update({age: 25}, {$set: {name: 'changeName'}}, false, true)` 修改age=25的数据，设置他们的name为changeName

总结来说：各种查询应该来说用的是最多的，所以列的最详细，其他的几种，列举的比较少，只列举了一些基本命令  
而UI界面的话与命令行差不多，掌握了命令行用法，各个不同的UI操作应该都类似的
