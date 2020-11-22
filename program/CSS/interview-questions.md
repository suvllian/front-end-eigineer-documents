# 常见面试题

## 1. 盒子模型是什么？标准盒子模型与怪异盒子模型是什么？
**盒子模型**：包括边距、边框和内容。  
margin：边框外的区域，外边距透明。  
border：在内边距和内容外的框。  
padding：内容外的区域。  
content：盒子的内容。

**标准盒子模型：**  
元素总高度 = 元素高度 + margin上下边距    
元素总宽度 = 元素宽度 + margin   
元素宽度 = content + padding左右边距 + border左右高度   
元素高度 = content + padding上下边距 + border上下高度   

**怪异盒子模型：**  
元素总高度 = 元素高度 + padding上下边距 + border上下高度 + margin上下边距   
元素总宽度 = 元素宽度 + padding左右边距 + border左右宽度 + margin左右边距

可以通过`box-sizing`属性设置盒子模型。  
``` css
box-sizing: content-box | border-box | inherit;
``` 
`content-box`：标准盒子模型  
`border-box`：怪异盒子模型

## 2. 如何清除浮动？

* **使用clear属性**：将浮动元素设为`clear:both;`或`clear: left;`活`clear:right;`
* **使用overflow属性**：将浮动元素设为`overflow:hidden;`，可以清除浮动。
* **给父元素设置BFC**

## 3. BFC
`BFC`：块级格式化上下文  

**BFC布局规则：**
* 内部的盒子会在垂直方向，一个接一个的放置。
* 垂直方向的距离由margin决定，同一个BFC内的两个盒子的margin会发生重叠。
* BFC是页面上一个隔离的独立容器，容器里的子元素不会影响到外面的元素。
* BFC的区域不会和float box重叠。
* 每个元素的margin box的左边与包含块的border box的左边相接触。

**会生成BFC的元素：**
* 根元素
* float不为node
* position为absolute或fixed
* display为inline-block、table-cell、table-caption、flex、inline-flex
* overflow不为visible

## 4. 浏览器如何匹配CSS
浏览器会先产生一个元素集合，这个集合往往由最后一个部分的索引产生。然后向左匹配，如果不匹配就把元素从集合中删除，直到选择器匹配完成。

**example:**
```  
.top .second p { font-size: 18px; }
```
浏览器会先匹配所有的p标签集合，然后再匹配所有class位second的p标签集合，最后匹配所有class为top的p标签集合。再将样式添加到匹配上的p标签集合上。

## 5. CSS选择器优先级
浏览器通过优先级来判断哪一些属性值与一个元素最为相关，从而在该元素上应用这些属性值。优先级是基于不同种类选择器组成的匹配规则。

选择器优先级规则：  
`内联 > ID选择器 > 类选择器 > 标签选择器`

最高优先级：`!important`

MDN文档：https://developer.mozilla.org/zh-CN/docs/Web/CSS/Specificity

## 6. 常见布局
### 左边定宽，右边自适应

### 自适应正方形

### 垂直水平居中

``` html
<div class="container">
	<div class="center"></div>
</div>
```

**方法一：position + margin**  
子元素设置为absolute定位，top、left、right、bottom属性全部设置为0，margin属性设置为auto即可进行垂直水平居中定位。

**方法二：position + transform**  
``` css
.container{
	width: 500px;
	height: 500px;
	background-color: #eee;
	position: relative;
}

.center{
	width: 100px;
	height: 100px;
	background-color: #aaa;
	left: 50%;
	top: 50%;
	transform: translate(-50%, -50%);
}
```

**方法三：position + margin**  
创建一个块级元素，给定宽度和高度。将其position属性设置为absolute，top或者left属性设置为50%，margin-top或margin-left属性设置为负的高的一半或者负的宽的一半，即可实现垂直或者水平方向上的居中定位。

**方法四：flex布局**
``` css
.container{
	width: 500px;
	height: 500px;
	background-color: #eee;
	display: flex;
	align-items: center;
	justify-content: center;
}

.center{
	width: 100px;
	height: 100px;
	background-color: #aaa;
}
```

### 两列布局

### 三列等宽显示