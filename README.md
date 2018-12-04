# 解决跨域

## 跨域如何发生？
-- 跨域是由于浏览器的限制而产生的，浏览器有《同源策略》的限制

同源策略（Same origin policy）是一种约定，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，则浏览器的正常功能可能都会受到影响。可以说 Web 是构建在同源策略基础之上的，浏览器只是针对同源策略的一种实现。

它的核心就在于它认为自任何站点装载的信赖内容是不安全的。当被浏览器半信半疑的脚本运行在沙箱时，它们应该只被允许访问来自同一站点的资源，而不是那些来自其它站点可能怀有恶意的资源。

## 什么是同源
https://www.baidu.com:80/aa
同源是指
* https:// 或者 http:// 协议
* www.baidu.com  域名
* 80  端口
- 3者相同就是同源，如果有其中一个不同就是跨域

另外，同源策略又分为以下两种：
* DOM 同源策略：禁止对不同源页面 DOM 进行操作。这里主要场景是 iframe 跨域的情况，不同域名的 iframe 是限制互相访问的。
* XMLHttpRequest 同源策略：禁止使用 XHR 对象向不同源的服务器地址发起 HTTP 请求。

## 为什么要有同源策略?
因为安全问题，为了让用户更加安全的上网

## 跨域是如何产生的那？
跨域的产生就是由于 `浏览器` 同源策略的限制，限制了不能跨域访问  `http请求`


## 如何解决跨域？
### jsonp
- jsop是利用资源不跨域的特性，同源策略只是限制了xhr对象发送的请求，并没有限制资源加载，所以jsonp利用了这个特性可以解决浏览器的跨域
```
/**
 * jsnop 请求利用了静态资源不跨域的特性，解决跨域问题，需要后端做配合
 * 1， 动态生成script标签,
 * 2， 动态添加script标签的src属性
 * 3， 动态生成回调函数的名称，并且动态生成回调函数必须是在Window上的
 * 4， 动态给src属性添加callback参数，告知后端是jsonp请求，并且需要后端配合返回一个js语法函数调用
 * 5， 返回格式callbackFn(参数)
 * 优点：解决了跨域， 兼容
 * 缺点：只能get请求， 不能捕获错误状态
 */
```

### cors 跨域资源共享
它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。
#### 简介
CORS需要浏览器和服务器同时支持。目前，所有浏览器都支持该功能，IE浏览器不能低于IE10。
整个CORS通信过程，都是浏览器自动完成，不需要用户参与。对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。
* node 实现
```
app.use('*', (req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");  // 允许的来源
  res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS"); // 允许的请求方法
  res.header("Content-Type", "application/json;charset=utf-8"); // 请求内容json格式，utf8类型
  res.header('Access-Control-Allow-Credentials', true); // 允许携带cookie
  next()
})
```
优点：
* 可以跨域
* 前端不需要做任何处理
* 正常ajax请求，不需要依赖其他特性

缺点：
兼容性问题

场景：
* 如果接口是共享接口，并且不需要关心ie低版本浏览器
* 适合移动端项目（接口域名和项目域名是分开）


### 代理
在开发项目的时候一般都是前后端分类，后端提供接口与，前端请求接口并且渲染页面，这个时候就会出现跨域问题。
在开发环境中前端项目是localhost， 而后端提供接口是 192.168.0.19:3000 
但是项目上线后都变成了  www.baidu.com 统一域名，不存在跨域了，也就是开发环境中存在跨域，而生产环境没有跨域问题了，这个时候我们就可以使用代理的形式解决跨域

## 解决方案
利用了服务器不存在跨域的特性进行解决，（通过我自己本地服务端去代理请求跨域的接口，在通过我本地的服务返回给我本地页面）


#### 正向代理
正向代理 是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。客户端必须要进行一些特别的设置才能使用正向代理。



#### 反向代理

#### 应用场景
开发环境下有跨域问题，部署到线上后就没有跨域







