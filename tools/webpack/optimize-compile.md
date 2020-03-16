## webpack优化打包

### 原则

优化打包通常从两方面入手：

* 优化编译打包的速度
* 优化打包后的静态资源

### 常见方法

* 减小打包的整体体积
  * 代码压缩
  * 移除未使用的模块
  * 按需引入模块
  * 公共依赖使用external
  * 选择可以替代的体积较小的模块（例如替换moment.js）
* Code Splitting: 按需加载，优化页面首次加载体积。如根据路由按需加载，根据是否可见按需加载
  * 使用 import() 动态加载模块
  * 使用 React.lazy() 动态加载组件
  * 使用 lodable-component 动态加载路由，组件或者模块
* Bundle Splitting：分包，根据模块更改频率分层次打包，充分利用缓存

### 参考
* [前端高级进阶：如何更好地优化打包资源](https://mp.weixin.qq.com/s/Gh4VJSk33OqHKM3FIIkERw)

## 优化方法

### 利用webpack external优化打包

**作用** ：防止将某些`import`的包(package)打包到 bundle 中，而是在运行时(runtime)再去从外部获取这些扩展依赖，从而减小打包的bundle体积。

#### 使用经验

通过加载CDN或全局JS中暴露全局变量使用常用的基础包，如react和react-dom

#### 参考

* [官方文档-externals](https://www.webpackjs.com/configuration/externals/)
* [webpack externals 深入理解](https://segmentfault.com/a/1190000012113011?utm_source=tag-newest)
* [webpack打包优化之外部扩展externals的实际应用](https://www.cnblogs.com/weiqinl/p/10020773.html)
