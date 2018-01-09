---
layout: post
title: Mongoose介绍、入门
tags:
- node
categories: node
description: Mongoose介绍
---



<!-- more -->

## 简介
Mongoose是在node.js异步环境下对mongodb进行便捷操作的对象模型工具。<br />
那么要使用它，首先你得装上node.js和mongodb,关于mongodb的安装和操作介绍可以[参考](http://www.runoob.com/mongodb/mongodb-window-install.html)<br />
## Mongoose安装
```
npm install mongoose
```
![image](https://raw.githubusercontent.com/Agnee-he/agnee-he.github.com/master/assets/img/mongoose-install.png)

## 链接字符串
创建一个db.js
```
var mongoose = require('mongoose');
var DB_URL = 'mongodb://localhost:27017/mongoosesample';

/**
 * 连接
 */
mongoose.connect(DB_URL);

/**
  * 连接成功
  */
mongoose.connection.on('connected', function () {    
    console.log('Mongoose connection open to ' + DB_URL);  
});    

/**
 * 连接异常
 */
mongoose.connection.on('error',function (err) {    
    console.log('Mongoose connection error: ' + err);  
});    
 
/**
 * 连接断开
 */
mongoose.connection.on('disconnected', function () {    
    console.log('Mongoose connection disconnected');  
});
```
执行 node db.js<br />


从代码中可以看出，监听了几个事件，并且执行触发了connected事件，这表示连接成功。connection中不止有如上几个事件，关键看你想要监听哪个事件。


## Schema

schema是mongoose里会用到的一种数据模式，可以理解为表结构的定义；每个schema会映射到mongodb中的一个collection，它不具备操作数据库的能力<br />

我们先改造一下db.js，导出mongoose对象
```
var mongoose = require('mongoose'),
    DB_URL = 'mongodb://localhost:27017/mongoosesample';

/**
 * 连接
 */
mongoose.connect(DB_URL);

/**
  * 连接成功
  */
mongoose.connection.on('connected', function () {    
    console.log('Mongoose connection open to ' + DB_URL);  
});    

/**
 * 连接异常
 */
mongoose.connection.on('error',function (err) {    
    console.log('Mongoose connection error: ' + err);  
});    
 
/**
 * 连接断开
 */
mongoose.connection.on('disconnected', function () {    
    console.log('Mongoose connection disconnected');  
});    

module.exports = mongoose;
```

下面我们定义一个user的Schema，命名为user.js
```
/**
 * 用户信息
 */
var mongoose = require('./db.js'),
    Schema = mongoose.Schema;

var UserSchema = new Schema({          
    username : { type: String },                    //用户账号
    userpwd: {type: String},                        //密码
    userage: {type: Number},                        //年龄
    logindate : { type: Date}                       //最近登录时间
});
```

## Model
定义好了Schema，接下就是生成Model。<br />
model是由schema生成的模型，可以对数据库的操作<br />
我们对上面的定义的user的schema生成一个User的model并导出，修改后代码如下<br />
```
/**
 * 用户信息
 */
var mongoose = require('./db.js'),
    Schema = mongoose.Schema;

var UserSchema = new Schema({          
    username : { type: String },                    //用户账号
    userpwd: {type: String},                        //密码
    userage: {type: Number},                        //年龄
    logindate : { type: Date}                       //最近登录时间
});

module.exports = mongoose.model('User',UserSchema);
```

## 日常数据库操作

### 插入
```
var User = require("./user.js");

/**
 * 插入
 */
function insert() {
 
    var user = new User({
        username : 'Tracy McGrady',                 //用户账号
        userpwd: 'abcd',                            //密码
        userage: 37,                                //年龄
        logindate : new Date()                      //最近登录时间
    });

    user.save(function (err, res) {

        if (err) {
            console.log("Error:" + err);
        }
        else {
            console.log("Res:" + res);
        }

    });
}

insert();
```

### 更新
```
var User = require("./user.js");

function update(){
    var wherestr = {'username' : 'Tracy McGrady'};
    var updatestr = {'userpwd': 'zzzz'};
    
    User.update(wherestr, updatestr, function(err, res){
        if (err) {
            console.log("Error:" + err);
        }
        else {
            console.log("Res:" + res);
        }
    })
}

update();
```

### 删除
```
var User = require("./user.js");

function del(){
    var wherestr = {'username' : 'Tracy McGrady'};
    
    User.remove(wherestr, function(err, res){
        if (err) {
            console.log("Error:" + err);
        }
        else {
            console.log("Res:" + res);
        }
    })
}

del();
```

### 条件查询
```
var User = require("./user.js");

function getByConditions(){
    var wherestr = {'username' : 'Tracy McGrady'};
    
    User.find(wherestr, function(err, res){
        if (err) {
            console.log("Error:" + err);
        }
        else {
            console.log("Res:" + res);
        }
    })
}

getByConditions();
```

### 模糊查询
```
var User = require("./user.js");

function getByRegex(){
    var whereStr = {'username':{$regex:/m/i}};
    
    User.find(whereStr, function(err, res){
        if (err) {
            console.log("Error:" + err);
        }
        else {
            console.log("Res:" + res);
        }
    })
}

getByRegex();
```

### 分页查询
```
var User = require("./user.js");

function getByPager(){
    
    var pageSize = 5;                   //一页多少条
    var currentPage = 1;                //当前第几页
    var sort = {'logindate':-1};        //排序（按登录时间倒序）
    var condition = {};                 //条件
    var skipnum = (currentPage - 1) * pageSize;   //跳过数
    
    User.find(condition).skip(skipnum).limit(pageSize).sort(sort).exec(function (err, res) {
        if (err) {
            console.log("Error:" + err);
        }
        else {
            console.log("Res:" + res);
        }
    })
}

getByPager();
```












