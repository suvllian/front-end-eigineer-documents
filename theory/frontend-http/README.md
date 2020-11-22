## 前端必知的HTTP知识

### 1. HTTP状态码

[HTTP状态码](https://www.runoob.com/http/http-status-codes.html)

常见的HTTP状态码：
* 200
* 301：Moved Permanently
* 304：Not Modified
* 400：Bad Request
* 403
* 404
* 500：Internal Server Error

### 2. 浏览器可以并行下载多少个资源？
同一时间针对同一域名下的请求有一定数量限制，超过限制数目的请求会被阻止，通常是6个。

### 3. cookie httponly
被设置为httponly的cookie只能被服务端修改