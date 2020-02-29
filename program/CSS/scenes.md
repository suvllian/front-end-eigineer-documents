## 常见场景及解法

### 1. 文字溢出显示省略号

**单行省略号**

``` html
<style>
.block {
	width: 200px;
	overflow: hidden;
	white-space: nowrap;
	text-overflow: ellipsis;
}
</style>
```

**多行省略号**

``` html
<style>
.block {
	width: 200px;
	display: -webkit-box;
	-webkit-box-orient: vertical;
	-webkit-line-clamp: 2;
	overflow: hidden;
	white-space: nowrap;
	text-overflow: ellipsis;
}
</style>
```

其他更复杂场景参见文章：[浅谈移动端过长文本溢出显示省略号的实现方案](https://mp.weixin.qq.com/s/39NCyZvm8EYiJ-pEEtjxGw)