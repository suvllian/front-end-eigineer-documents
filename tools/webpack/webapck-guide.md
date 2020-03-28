## webpack指南

### 管理资源
* css
* 图片
* 字体
* 数据

### 管理输出
output配置输出，可配置hash，使用plugin-HtmlWebpackPlugin

### 开发
设置source-map

选择开发工具  
* webpack's Watch Mode
* webpack-dev-server
* webpack-dev-middleware

### 模块热替换HMR

### tree shaking
移除未使用的代码。
* 使用 ES2015 模块语法（即 import 和 export）。
* 在项目 package.json 文件中，添加一个 "sideEffects" 入口。
* 引入一个能够删除未引用代码(dead code)的压缩工具(minifier)（例如 UglifyJSPlugin）。

### 生产环境构建
关注更小的bundle，更轻量的source map，以及更优化的资源，以改善加载时间。

### 代码分离
把代码分离到不同的bundle中，然后可以按需加载或并行加载这些文件。代码分离可以用于获取更小的 bundle，以及控制资源加载优先级，如果使用合理，会极大影响加载时间。
* 入口起点：使用 entry 配置手动地分离代码。
* 防止重复：使用 CommonsChunkPlugin 去重和分离 chunk。
* 动态导入：通过模块的内联函数调用来分离代码。

### 缓存
文件名加上hash便于客户端缓存

### shimming

### 公共路径
为项目中的所有资源指定一个基础路径