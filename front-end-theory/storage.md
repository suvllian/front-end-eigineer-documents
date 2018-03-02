## Web缓存机制

Web缓存可以分为浏览器缓存、服务器缓存和数据库缓存。

### 浏览器缓存

#### 一、浏览器缓存作用
> * 减少网络带宽
> * 降低服务器压力
> * 加快页面打开速度

#### 二、配置可缓存的站点
HTML控制：
Basic usage:
``` html
<meta name="pragma" content="no-cache" />
```
HTTP Header控制：  
 
##### 1、Expires
##### 2、Cache-control
| name | content |
| --- | --- |
| max-age | --- |
| private | --- |
| public | --- |
| no-cache | --- |
| no-store | --- |

##### 3、Last-modified/If-Modified-Since
##### 4、Etag/If-None-match

Etag解决last-modified的问题：
* 秒精度。
* 文件被修改，但是内容无变化。
* 服务器无法获取文件修改时间。
##### 5、Vary

方法：  
1、POST请求不可缓存，能使用GET请求地方就是用GET请求。  
2、静态资源CSS、image、javascript缓存，HTML文件不缓存。  
3、cookie不可缓存。

### 服务器缓存

#### CDN

### 数据库缓存
memcahce，从内存中读取数据。

