## for in, Object.keys, Object.getOwnPropertyNames的区别

这三个方法都是用来遍历对象属性的。

首先通过Object.create方法创建一个父对象，父对象中有一个可枚举属性a。

``` javascript
const parent = Object.create(Object.prototype, {
	a: {
		value: 1000,
		configurable: true,
		writable: true,
		enumerable: true
	}
})
```

再创建继承自parent的字对象child，child中属性b不可枚举，属性c可枚举。

``` javascript
const child = Object.create(parent, {
	b: {
		value: 20000,
		configurable: true,
		writable: true,
		enumerable: false
	},
	c: {
		value: 30000,
		configurable: true,
		writable: true,
		enumerable: true
	}
})
```

### for in

``` javascript
for (const key in child) {
	console.log(key)
}

// c a
```

`for in`会输出自身及原型链上可枚举的属性。

如果仅需要输出自身可枚举属性，需要借助`hasOwnProperty`方法。

``` javascript
for (const key in child) {
	if (child.hasOwnProperty(key)) {
		console.log(key)
	}
}
// c
```

### Object.keys

`Object.keys`会以数组的形式返回自身可枚举的属性

``` javascript
console.log(Object.keys(child))
// ['c']
```

### Object.getOwnPropertyNames

`Object.getOwnPropertyNames`以数组形式返回自身所有属性。

``` javascript
console.log(Object.getOwnPropertyNames(child))
// ['b', 'c']
```
