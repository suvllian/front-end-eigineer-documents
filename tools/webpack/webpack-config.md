## webpack配置

### mode
production | development | none

### context和entry

### output
包含一组选项，指示webpack如何输出

### module
决定如何处理项目中的不同类型的模块

### resolve
设置模块如何被解析，配置项说明
* modules：配置webpack去哪些目录下寻找第三方模块，默认情况下，只会去node_modules下寻找。如果你我们项目中某个文件夹下的模块经常被导入，不希望写很长的路径，那么就可以通过配置resolve.modules来简化。
* alias：通过别名把原导入路径映射成一个新的导入路径。
* extensions：适配多端的项目中，可能会出现.web.js, .wx.js，例如在转web的项目中，我们希望首先找.web.js，如果没有，再找.js。
* enforceExtension：配置了resolve.enforceExtension为true，那么导入语句不能缺省文件后缀。

### plugins
自定义webpack的构建过程

### devServer
webpack-dev-server用于快速开发应用程序

### devtool
控制是否生成，以及如何生成source map

### 构建目标
指定目标环境

### watch、watchOptions
监听文件变化，修改后重新编译

### externals
从输出的bundle中排除依赖，bundle依赖用户环境中的依赖

### performance
控制webpack通知“资源和入口起点超过文件限制”

### stats