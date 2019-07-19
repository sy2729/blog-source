---
title: MongoDB学习
tags:
  - MongoDB
categories:
  - 学习
toc: false
date: 2019-01-01 22:01:12
---

* mongod - mongo
* mongo dbs
* use xxxx
* show collections
* db

* db.[collection].find().pretty() 可以美化规整展示结果

* use db不仅可以进入，还可以新建
* 增： db.user.insert({“name”: “Shuai yuan”})    进行数据插入
*  查：db.user.find()      进行查找
* db.user.findOne()     自动返回首个数据
* 改：db.user.update(第一个参数为原始数据，第二个为新的数据)
* 删： db.user.remove() 删除
* db.user.drop()删除整个库 || db.dropDatabase()

* 在 JS中写mongoDB， var db = connect(database的名字)；然后 “mongo 文件名” 运行
* 在terminal中直接载入/运行文件： load(文件路径)

* update修改器


* ObjectId()可以创建一个随机ID并且这个id值有自带的方法getTimestamp()可以查询添加时间
* $gt - greater than
* $gte - greater than or equal
* $lt
* $lte
* $or
* $exists - 接受boolean值
* 如果嵌套很多层的数据，可以通过key.key.key…的方式来find得到值
* 查询的时候，如果想要查询的结果是数组中的第一个位置，可以这么申明： hobbies.0.name
* 使用变量名查询时，一般是{var: value}的形式,其实后面一般不仅可以跟值，还可以跟正则表达式: {$regx: “xiao”}
* 对搜索数组长度的限制: {hobbies: {$size: 2}}
* Find等方法的第二个参数：projection，指根据查找数据中的第二个参数部分声明的数据值来显示查找到的数据 - 按需取用，加快查询速度：  {hobbies: 0} - 显示查找到的数据中所有的数据，但是隐藏hobbies部分； {hobbies: 1} - 只显示查找到的数据中hobbies数据，但是隐藏其他数据
* findOneAndUpdate方法第三个参数{new: true} - 返回更新后的文档
* Update方法第三个参数{multi: true}更新多个
* findOneAndUpdate方法第三个参数{upsert: true} - 未找到就插入
* find({}).count() - 统计数量

* Aggregate
    * $match
    * $group
        * $sum
        * $avg
    * $unwind
