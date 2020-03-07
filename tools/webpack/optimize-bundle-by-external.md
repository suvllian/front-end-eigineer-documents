## 利用webpack external优化打包

**作用** ：防止将某些`import`的包(package)打包到 bundle 中，而是在运行时(runtime)再去从外部获取这些扩展依赖，从而减小打包的bundle体积。

### 使用经验

通过加载CDN或全局JS中暴露全局变量使用常用的基础包，如react和react-dom

### 参考

* [官方文档-externals](https://www.webpackjs.com/configuration/externals/)
* [webpack externals 深入理解](https://segmentfault.com/a/1190000012113011?utm_source=tag-newest)
* [webpack打包优化之外部扩展externals的实际应用](https://www.cnblogs.com/weiqinl/p/10020773.html)
