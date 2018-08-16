---
title: Mongoose快速入门
categories:
  - 前端之巅
tags:
  - 前端之巅
  - JavaScript
date: 2018-08-16 11:24:49
---

Mongoose快速入门

Mongoose是在node.js异步环境下对mongodb进行便捷操作的对象模型工具。本文将详细介绍如何使用Mongoose来操作MongoDB
<!--more-->

# 基础介绍

## 安装 引用

启动数据库命令

```
mongod --dbpath=D:\Data\mongodb\_data
```

`–dbpath`：指定数据存储位置

连接本地的test数据库

```
var mongoose = require('mongoose');

var db = mongoose.connect("mongodb://127.0.0.1:27017/test");

db.connection.on("error", function (error) {
	console.log("数据库连接失败：" + error);
});

db.connection.on("open", function () {
	console.log("------数据库连接成功！------");
});
```

## Schema简介

Schema：一种以文件形式存储的数据库模型骨架，无法直接通往数据库端，也就是说它不具备对数据库的操作能力，仅仅只是数据库模型在程序片段中的一种表现，可以说是数据属性模型(传统意义的表结构)，又或着是"集合"的模型骨架。

那如何去定义一个Schema呢，请看示例：

```
var mongoose = require("mongoose");

var TestSchema = new mongoose.Schema({
	name : { type:String },//属性name,类型为String
	age  : { type:Number, default:0 },//属性age,类型为Number,默认为0
	time : { type:Date, default:Date.now },
	email: { type:String,default:''}
});
```

注：Schema定义集合结构（定义表的列）

## Model–操作数据库

```
var db = mongoose.connect("mongodb://127.0.0.1:27017/test");
// 创建Model
var TestModel = db.model("test1", TestSchema);
```

## Entity--给集合赋值

```
var TestEntity = new TestModel({
  	 name : "Lenka",
   	age  : 36,
   email: "lenka@qq.com"
});

console.log(TestEntity.name); // Lenka

console.log(TestEntity.age); // 36
```

## 创建集合

```
//引入数据库模块
var mongoose = require("mongoose");

//连接本地名为test的数据库，格式
//var db = mongoose.connect("mongodb://user:pass@localhost:port/database");
var db = mongoose.connect("mongodb://127.0.0.1:27017/test");

//用Schema定义集合结构
var TestSchema = new mongoose.Schema({
	name : { type:String },
	age  : { type:Number, default:0 },
	email: { type:String },
	time : { type:Date, default:Date.now }
});

//创建model,在内存中创建结构为TestSchema名为test1的集合
var TestModel = db.model("test1", TestSchema );

//插入数据到内存中的test1集合
var TestEntity = new TestModel({
	name : "helloworld",
	age  : 28,
	email: "helloworld@qq.com"	
});

//将test1写入到数据库中
TestEntity.save(function(error,doc){
if(error){
 	console.log("error :" + error);
}else{
 	console.log(doc);
}
});
```
	
## 小结

本章节学习了如何通过Mongoose去创建一个数据库"集合"，还有定义"集合"的基本组成结构并使其具有相应的操作数据库能力。

简单回顾：

- `Schema`：数据库集合的模型骨架，或者是数据属性模型传统意义的表结构。

- `Model`：通过Schema构造而成，除了具有Schema定义的数据库骨架以外，还可以具体的操作数据库。

- `Entity`：通过Model创建的实体，它也可以操作数据库。

# 增删改查

## 查询

`find查询：obj.find(查询条件,callback);`

```
Model.find({},function(error,docs){
//若没有向find传递参数，默认的是显示所有文档
});

Model.find({ "age": 28 }, function (error, docs) {
	if(error){
		console.log("error :" + error);
	}else{
		console.log(docs); //docs: age为28的所有文档
	}
});
```

## model保存方法

`Model.create(文档数据, callback))`

```
Model.create({ name:"model\_create", age:26}, function(error,doc){
	if(error) {
    	console.log(error);
	} else {
    	console.log(doc);
	}
});
```

## entity保存方法

`Entity.save(文档数据, callback))`

```
var Entity = new Model({name:"entity\_save",age: 27});

Entity.save(function(error,doc) {
  if(error) {
    	console.log(error);
	} else {
    	console.log(doc);
	}
});
```

model调用的是create方法，entity调用的是save方法

## 数据更新

`obj.update(查询条件,更新对象,callback);`

```
var conditions = {name : 'test\_update'};

var update = {$set : { age : 16 }};

TestModel.update(conditions, update, function(error){
if(error) {
	console.log(error);
} else {
	console.log('Update success!');
}
});
```
	
## 删除数据

`obj.remove(查询条件,callback);`

```
var conditions = { name: 'tom' };

TestModel.remove(conditions, function(error){
if(error) {
 	console.log(error);
} else {
	console.log('Delete success!');
}
});
```

## 小结

- 查询：find查询返回符合条件一个、多个或者空数组文档结果。

- 保存：model调用create方法，entity调用的save方法。

- 更新：obj.update(查询条件,更新对象,callback)，根据条件更新相关数据。

- 删除：obj.remove(查询条件,callback)，根据条件删除相关数据。

# 简单查询

## find过滤查询

`属性过滤 find(Conditions,field,callback);`

field省略或为Null，则返回所有属性。

```
//返回只包含一个键值name、age的所有记录
Model.find({},{name:1, age:1, \_id:0}，function(err,docs){
//docs 查询结果集
})
```

说明：我们只需要把显示的属性设置为大于零的数就可以，当然1是最好理解的，_id是默认返回，如果不要显示加上("_id":0)，但是，对其他不需要显示的属性且不是_id，如果设置为0的话将会抛异常或查询无果

## 小结

- `find`过滤查询 ：find查询时我们可以过滤返回结果所显示的属性个数。

- `findOne`查询 ：只返回符合条件的首条文档数据。

- `findById`查询：根据文档_id来查询文档。

# 高级查询

## 大于、小于

`$gt(>)`、`$lt(<)`、`$lte(<=)`、`$gte(>=)`

示例：

```
Model.find({"age":{"$gt":18}},function(error,docs){
  //查询所有nage大于18的数据
});
```
        
## 不等于

`$ne(!=)`

示例：

```
Model.find({ age:{ $ne:24}},function(error,docs){
   //查询age不等于24的所有数据
});
```

`$ne`可以匹配单个值，也可以匹配不同类型的值。

## 匹配

`$in` 包含、等于

示例：

```
Model.find({ age:{ $in: 20}},function(error,docs){
  //查询age等于20的所有数据
});

Model.find({ age:{$in:[20,30]}},function(error,docs){
 //可以把多个值组织成一个数组
});
```
        
## 或者

`$or`

示例：

```
Model.find({"$or":[{"name":"yaya"},{"age":28}]},function(error,docs){
 //查询name为yaya或age为28的全部文档
});
```
        
## 存在

`$exists`

示例：

```
Model.find({name: {$exists: true}},function(error,docs){
 //查询所有存在name属性的文档
});

Model.find({telephone: {$exists: false}},function(error,docs){
 //查询所有不存在telephone属性的文档
});
```

## 小结

- `$gt(>)`,`$lt(<)`,`$lte(<=)`,`$gte(>=)`操作符：针对Number类型的查询具体超强的排除性。

- `$ne(!=)`操作符：相当于不等于、不包含，查询时可根据单个或多个属性进行结果排除。

- `$in`操作符：和$ne操作符用法相同，但意义相反。

- `$or`操作符：可查询多个条件，只要满足其中一个就可返回结果值。

- `$exists`操作符：主要用于判断某些属性是否存在。

# 游标

### 简介

数据库使用游标返回find的执行结果。客户端对游标的实现通常能够对最终结果进行有效的控制。可以限制结果的数量，略过部分结果，根据任意键按任意顺序的组合对结果进行各种排序，或者是执行其他一些强的操作。

## limit函数的基本用法

限制数量：`find(Conditions,fields,options,callback);`

```
Model.find({},null,{limit:20},function(err,docs){
    console.log(docs);
});
```

## skip函数的基本用法

skip函数和limit类似，都是对返回结果数量进行操作，不同的是skip函数的功能是略过指定数量的匹配结果，返回余下的查询结果。

示例：

1.跳过数量：`find(Conditions,fields,options,callback);`

```
Model.find({},null,{skip:4},function(err,docs){
    console.log(docs);
});
```

如果查询结果数量中少于4个的话，则不会返回任何结果。

## sort函数的基本用法

sort函数可以将查询结果数据进行排序操作，该函数的参数是一个或多个键/值对，键代表要排序的键名，值代表排序的方向，1是升序，-1是降序。

1.结果排序：`find(Conditions,fields,options,callback);`

```
Model.find({},null,{sort:{age:-1}},function(err,docs){
 //查询所有数据，并按照age降序顺序返回数据docs
});
```

## 小结

- `limit`函数：限制返回结果的数量。

- `skip`函数：略过指定的返回结果数量。

- `sort`函数：对返回结果进行有效排序。

# 属性方法

## 实例方法

有的时候，我们创造的Schema不仅要为后面的Model和Entity提供公共的属性，还要提供公共的方法.那怎么在Schema下创建一个实例方法呢，请看示例：

```
var mongoose = require('mongoose');

var saySchema = new mongoose.Schema({name : String});

saySchema.method('say', function () {

 console.log('Trouble Is A Friend');

})

var say = mongoose.model('say', saySchema);

var lenka = new say();

lenka.say(); //Trouble Is A Friend
```
  
## Schema静态方法

示例：

```
var mongoose = require("mongoose");
	
var db = mongoose.connect("mongodb://127.0.0.1:27017/test");
	
var TestSchema = new mongoose.Schema({
name : { type:String },
age  : { type:Number, default:0 },
email: { type:String, default:"" },
time : { type:Date, default:Date.now }
});

TestSchema.static('findByName', function (name, callback) {
   return this.find({ name: name }, callback);
});

var TestModel = db.model("test1", TestSchema );
TestModel.findByName('tom', function (err, docs) {
//docs所有名字叫tom的文档结果集
});
```

## Schema追加方法

为Schema模型追加speak方法

示例：

```
var mongoose = require("mongoose");

var db = mongoose.connect("mongodb://127.0.0.1:27017/test");

var TestSchema = new mongoose.Schema({
name : { type:String },
age  : { type:Number, default:0 },
email: { type:String, default:"" },
time : { type:Date, default:Date.now }
});

TestSchema.methods.speak = function(){
 console.log('我的名字叫'+this.name);
}

var TestModel = db.model('test1',TestSchema);

var TestEntity = new TestModel({name:'Lenka'});

TestEntity.speak();//我的名字叫Lenka
```

---

引用注释：

作者：18820227745

链接：https://cnodejs.org/topic/595d9ad5a4de5625080fe118

來源：网站

著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。