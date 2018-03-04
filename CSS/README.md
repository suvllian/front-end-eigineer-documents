# HTML知识点整理

* [HTML相关文章收集](./articles.md)
* [HTML学习网站](./websites.md)
* [HTML书籍推荐](./books.md)

## 1、盒子模型是什么？标准盒子模型与怪异盒子模型是什么？
**盒子模型**  
包括边距、边框和内容。  
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

## 2、清除浮动

## 3、BFC

## 4、常见布局

### 4.1 左边定宽，右边自适应

### 4.2 自适应正方形

### 4.3 垂直水平居中

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

### 4.4 两列布局

### 4.5 三列等宽显示