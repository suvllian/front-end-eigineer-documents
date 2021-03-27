## 写出更好的Javascript条件语句的小技巧

### 1.使用Array.includes来处理多重条件

``` javascript
function test(fruit) {
  if (fruit == 'apple' || fruit == 'strawberry') {
    console.log('red');
  }
}
```

用`Array.includes`重写

``` javascript
function test(fruit) {
  // 把条件提取到数组中
  const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];

  if (redFruits.includes(fruit)) {
    console.log('red');
  }
}
```

### 2.少写嵌套语句，尽早返回

``` javascript
function test(fruit, quantity) {
    const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];

    // 条件 1：fruit 必须有值
    if (fruit) {
        // 条件 2：必须为红色
        if (redFruits.includes(fruit)) {
          console.log('red');

          // 条件 3：必须是大量存在
          if (quantity > 10) {
            console.log('big quantity');
          }
        }
    } else {
        thrownewError('No fruit!');
    }
}

test(null); // 报错：No fruits
test('apple'); // 打印：red
test('apple', 20); // 打印：red，big quantity复制代码
```

将无效的条件尽早返回

``` javascript
function test(fruit, quantity) {
  const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];

  if (!fruit) thrownewError('No fruit!'); // 条件 1：尽早抛出错误
  if (!redFruits.includes(fruit)) return; // 条件 2：当 fruit 不是红色的时候，直接返回
  console.log('red');

  // 条件 3：必须是大量存在
  if (quantity > 10) {
    console.log('big quantity');
  }
}
```

### 3.使用函数默认参数和解构

``` javascript
function test(fruit, quantity) {
  if (!fruit) return;
  const q = quantity || 1; // 如果没有提供 quantity，默认为 1
  console.log(`We have ${q}${fruit}!`);
}

test('banana'); // We have 1 banana!
test('apple', 2); // We have 2 apple!
```

提供函数默认参数

``` javascript
function test(fruit, quantity = 1) { // 如果没有提供 quantity，默认为 1
    if (!fruit) return;
    console.log(`We have ${quantity}${fruit}!`);
}

test('banana'); // We have 1 banana!
test('apple', 2); // We have 2 apple!
```

如果fruit是一个对象

``` javascript
function test(fruit) { 
  // 如果有值，则打印出来
  if (fruit && fruit.name)  {
    console.log (fruit.name);
  } else {
    console.log('unknown');
  }
}

test(undefined); // unknown
test({ }); // unknown
test({ name: 'apple', color: 'red' }); // apple
```

添加默认参数，并进行解构赋值

``` javascript
function test({ name = 'unknown' } = {}) {
  console.log (name);
}

test(undefined); // unknown
test({ }); // unknown
test({ name: 'apple', color: 'red' }); // apple
```

### 4.使用Map/Object替换switch

``` javascript
function test(color) {
  switch (color) {
    case 'red':
      return ['apple', 'strawberry'];
    case 'yellow':
      return ['banana', 'pineapple'];
    case 'purple':
      return ['grape', 'plum'];
    default:
      return [];
  }
}

test(null); // []
test('yellow'); // ['banana', 'pineapple']
```

使用Object或者Map

``` javascript
// Object
const fruitColor = {
    red: ['apple', 'strawberry'],
    yellow: ['banana', 'pineapple'],
    purple: ['grape', 'plum']
  };
function test(color) {
  return fruitColor[color] || [];
}

// Map
const fruitColor = newMap()
    .set('red', ['apple', 'strawberry'])
    .set('yellow', ['banana', 'pineapple'])
    .set('purple', ['grape', 'plum']);
function test(color) {
  return fruitColor.get(color) || [];
}
```

### 5.使用Array.some和Array.every处理部分/全部满足条件

``` javascript
const fruits = [
    { name: 'apple', color: 'red' },
    { name: 'banana', color: 'yellow' },
    { name: 'grape', color: 'purple' }
  ];
function test() {
  let isAllRed = true;

  // 条件：所有的水果都必须是红色
  for (let f of fruits) {
    if (!isAllRed) break;
    isAllRed = (f.color == 'red');
  }

  console.log(isAllRed); // false
}
```

使用Array.every进行处理

``` javascript
const fruits = [
    { name: 'apple', color: 'red' },
    { name: 'banana', color: 'yellow' },
    { name: 'grape', color: 'purple' }
  ];
function test() {
  const isAllRed = fruits.every(f => f.color == 'red');

  console.log(isAllRed); // false
}
```