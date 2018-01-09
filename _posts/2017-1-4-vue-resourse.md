---
layout: post
title: vue-resourse
tags:
- vue
categories: vue
description: vue-resourse入门
---

<!-- more -->
### vue-resourse介绍
vue-resource插件具有以下特点：
- 体积小<br>

vue-resource非常小巧，在压缩以后只有大约12KB，服务端启用gzip压缩后只有4.5KB大小，这远比jQuery的体积要小得多。

- 支持主流的浏览器<br>

和Vue.js一样，vue-resource除了不支持IE 9以下的浏览器，其他主流的浏览器都支持。

- 支持Promise API和URI Templates<br>

Promise是ES6的特性，Promise的中文含义为“先知”，Promise对象用于异步计算。
URI Templates表示URI模板，有些类似于ASP.NET MVC的路由模板。

- 支持拦截器<br>

拦截器是全局的，拦截器可以在请求发送前和发送请求后做一些处理。
拦截器在一些场景下会非常有用，比如请求发送前在headers中设置access_token，或者在请求失败时，提供共通的处理方式。

### vue-resourse安装
npm
```
npm install vue-resource
```
cnd
```
<script src="https://cdn.jsdelivr.net/npm/vue-resource@1.3.5"></script>
```

使用vue-cli的需要在main.js里配置就可以全局使用
```
import VueResource from 'vue-resource';

Vue.use(VueResource);
```

### vue-resourse使用
引入vue-resource后，可以基于全局的Vue对象使用http，也可以基于某个Vue实例使用http。
```
// 基于全局Vue对象使用http
Vue.http.get('/someUrl', [options]).then(successCallback, errorCallback);
Vue.http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);

// 在一个Vue实例内使用$http
this.$http.get('/someUrl', [options]).then(successCallback, errorCallback);
this.$http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);
```
这是我项目中使用到的一个例子,使用的是es6的Lambda写法
```
this.$http.get('/api/goods').then((response) => {
		 	response = response.body;
		 	if (response.errno === ERR_OK) {
		 		this.goods = response.data;
		 		this.$nextTick(() => {
		 			this._initScroll();
		 			this._calculateHeight();
		 		});
		 	}
		 });
```

### 支持的HTTP方法
vue-resource的请求API是按照REST风格设计的，它提供了7种请求API：
- get(url, [options])
- head(url, [options])
- delete(url, [options])
- jsonp(url, [options])
- post(url, [body], [options])
- put(url, [body], [options])
- patch(url, [body], [options])
除了jsonp以外，另外6种的API名称是标准的HTTP方法。当服务端使用REST API时，客户端的编码风格和服务端的编码风格近乎一致，这可以减少前端和后端开发人员的沟通成本。

### emulateHTTP的作用
如果Web服务器无法处理PUT, PATCH和DELETE这种REST风格的请求，你可以启用enulateHTTP现象。启用该选项后，请求会以普通的POST方法发出，并且HTTP头信息的X-HTTP-Method-Override属性会设置为实际的HTTP方法。
```
Vue.http.options.emulateHTTP = true;
```

### emulateJSON的作用
如果Web服务器无法处理编码为application/json的请求，你可以启用emulateJSON选项。启用该选项后，请求会以application/x-www-form-urlencoded作为MIME type，就像普通的HTML表单一样。
```
Vue.http.options.emulateJSON = true;
```











