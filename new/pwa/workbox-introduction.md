## workbox

workbox是PWA相关的工具集合，它是Google官方的PWA框架，它解决的就是用底层API写PWA太过复杂的问题。  
围绕它的还有一些列工具，如 workbox-cli、gulp-workbox、webpack-workbox-plagin等。

### 缓存策略
#### workbox缓存策略 
**Stale-While-Revalidate**
![Stale-While-Revalidate](./img/Stale-While-Revalidate.png)

**Cache First**
![Cache First](./img/Cache%20First.png)

**Network First**
![Network First](./img/Network%20First.png)

**Network Only**
![Network Only](./img/Network%20Only.png)

**Cache Only**
![Cache Only](./img/Cache%20Only.png)

#### 业务中的缓存策略
* HTML：
  * 如果页面需要离线可以访问，使用NetworkFirst；
  * 如果不需要离线访问，使用 NetworkOnly，其他策略均不建议对HTML使用。
* JS、CSS：
  * 使用Stale-While-Revalidate策略，保证页面速度，即便失败，用户刷新一下即可更新。SW无法办法判断从CDN上请求下来的资源是否正确（HTTP 200），如果缓存了失败的资源，导致页面无法正常访问。
  *  CSS、JS与站点在同一个域下，并且文件名中带了Hash版本号，可以使用Cache First策略。
* 图片：
  * Cache First，并设置一定的失效事件，请求一次就不会再变动
* 其他：
  * 对于不在同一域下的任何资源，绝对不能使用Cache only和Cache first

