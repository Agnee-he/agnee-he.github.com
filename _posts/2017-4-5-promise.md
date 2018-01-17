---
layout: post
title: promise
tags:
- ES6
categories: ES6
description: promise学习
---

<!-- more -->
之前在使用vue-resouse与axios的时候一直使用的是.then()、.catch(),但是不知道原来这就是promise，最近系统的学习了一遍，并记录下来。
### 语法
```
new Promise( function(resolve, reject) {...} /* executor */  );
```
executor是一个带有 resolve 和 reject 两个参数的函数 。executor 函数在Promise构造函数执行时同步执行，被传递 resolve 和 reject 函数（executor 函数在Promise构造函数返回新建对象前被调用）。resolve 和 reject 函数被调用时，分别将promise的状态改为fulfilled（完成）或rejected（失败）。executor 内部通常会执行一些异步操作，一旦完成，可以调用resolve函数来将promise状态改成fulfilled，或者在发生错误时将它的状态改为rejected。

### 描述
Promise 对象是一个代理对象（代理一个值），被代理的值在Promise对象创建时可能是未知的。它允许你为异步操作的成功和失败分别绑定相应的处理方法（handlers）。 这让异步方法可以像同步方法那样返回值，但并不是立即返回最终执行结果，而是一个能代表未来出现的结果的promise对象<br>
一个 Promise有以下几种状态:
- pending: 初始状态，成功或失败状态。
- fulfilled: 意味着操作成功完成。
- rejected: 意味着操作失败。
 注：一旦Promise的状态由pending——>fulfilled或者pending——>rejected，那么状态就不会再发生改变。
 
 
 ### Promise.all()
 #### 语法
 ```
Promise.all(iterable);
```
简单对的说，就是执行多个对象。当所有对象都resolve或者有一个被reject时，promise结束.
#### 示例
Promise.all 等待所有代码的完成（或第一个代码的失败）。
```
let p1 = Promise.resolve(3);
let p2 = 1337;
let p3 = new Promise((resolve, reject) => {
    setTimeout(resolve, 100, "foo");
}); 

Promise.all([p1, p2, p3]).then(values => { 
    console.log(values); 
    // [3, 1337, "foo"] 
});
```
如果iterable包含非promise值，则它们将被忽略，但仍然计入返回的promise数组值（如果promise已满足）：
```
// this will be counted as if the iterable passed is empty, so it gets fulfilled
var p = Promise.all([1,2,3]);
// this will be counted as if the iterable passed contains only the resolved promise with value "444", so it gets fulfilled
var p2 = Promise.all([1,2,3, Promise.resolve(444)]);
// this will be counted as if the iterable passed contains only the rejected promise with value "555", so it gets rejected
var p3 = Promise.all([1,2,3, Promise.reject(555)]);

// using setTimeout we can execute code after the stack is empty
setTimeout(function(){
    console.log(p);
    console.log(p2);
    console.log(p3);
});

// logs
// Promise { <state>: "fulfilled", <value>: Array[3] }
// Promise { <state>: "fulfilled", <value>: Array[4] }
// Promise { <state>: "rejected", <reason>: 555 }
```
Promise.all()是异步的，只有当传递的对象为空时是同步的。<br>
如果任意一个元素是rejected，那么Promise.all就是rejected。例如，如果你传递了五个参数，其中四个是经过一定时间间隔后执行resolve，另外一个立刻执行reject，那么Promise.all将会立刻执行reject。
```
var p1 = new Promise((resolve, reject) => { 
  setTimeout(resolve, 1000, 'one'); 
}); 
var p2 = new Promise((resolve, reject) => { 
  setTimeout(resolve, 2000, 'two'); 
});
var p3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 3000, 'three');
});
var p4 = new Promise((resolve, reject) => {
  setTimeout(resolve, 4000, 'four');
});
var p5 = new Promise((resolve, reject) => {
  reject('reject');
});

Promise.all([p1, p2, p3, p4, p5]).then(values => { 
  console.log(values);
}, reason => {
  console.log(reason)
});

//From console:
//"reject"

//You can also use .catch
Promise.all([p1, p2, p3, p4, p5]).then(values => { 
  console.log(values);
}).catch(reason => { 
  console.log(reason)
});

//From console: 
//"reject"
```
### Promise.prototype.catch()
#### 语法
```
p.catch(onRejected);

p.catch(function(reason) {
   // 拒绝
});

//onRejected 当Promise 被拒绝时,被调用的一个Function。 该函数拥有一个参数
//reason  拒绝的原因。
```
catch方法用来处理promise中出现的错误<br>

捕获抛出的错误
```
// 抛出一个错误，大多数时候将调用catch方法
var p1 = new Promise(function(resolve, reject) {
  throw 'Uh-oh!';
});

p1.catch(function(e) {
  console.log(e); // "Uh-oh!"
});

// 在异步函数中抛出的错误不会被catch捕获到
var p2 = new Promise(function(resolve, reject) {
  setTimeout(function() {
    throw 'Uncaught Exception!';
  }, 1000);
});

p2.catch(function(e) {
  console.log(e); // 不会执行
});

// 在resolve()后面抛出的错误会被忽略
var p3 = new Promise(function(resolve, reject) {
  resolve();
  throw 'Silenced Exception!';
});

p3.catch(function(e) {
   console.log(e); // 不会执行
});
```
如果已经决议（完成）
```
//创建一个新的 Promise ，且已决议
var p1 = Promise.resolve("calling next");

var p2 = p1.catch(function (reason) {
    //这个方法永远不会调用
    console.log("catch p1!");
    console.log(reason);
});

p2.then(function (value) {
    console.log("next promise's onFulfilled"); /* next promise's onFulfilled */
    console.log(value); /* calling next */
}, function (reason) {
    console.log("next promise's onRejected");
    console.log(reason);
});
```
### Promise.prototype.then()
#### 语法
```
p.then(onFulfilled, onRejected);

p.then(function(value) {
   // fulfillment
  }, function(reason) {
  // rejection
});
```
由于 then 和 Promise.prototype.catch() 方法都会返回 promise，它们可以被链式调用 — 一种称为复合（ composition） 的操作.
#### 示例
```
let p1 = new Promise(function(resolve, reject) {
  resolve("Success!");
  // or
  // reject ("Error!");
});

p1.then(function(value) {
  console.log(value); // Success!
}, function(reason) {
  console.log(reason); // Error!
});
```

