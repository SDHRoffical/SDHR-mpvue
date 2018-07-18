# 上大海润ES6开发规范-初稿

## 一、类型

### `基本类型: 直接存取基本类型`

字符串、数值、布尔类型、null、 undefined

### `复制类型: 通过引用的方式存取复杂类型`
对象、数组、函数

## 二、引用

### `对所有的引用使用 const ；不要使用 var。`

这能确保你无法对引用重新赋值，也不会导致出现 bug 或难以理解

### `如果你一定需要可变动的引用，使用 let 代替 var。`

因为 let 是块级作用域，而 var 是函数作用域

### `注意 let 和 const 都是块级作用域。`

## 三、对象

1. `使用字面值创建对象`

```javascript
// bad
const item = new Object();
 
// good
const item = {};
```

2. `如果你的代码在浏览器环境下执行，别使用 保留字 作为键值。这样的话在 IE8 不会运行。 但在 ES6 模块和服务器端中使用没有问题。`

```javascript
// bad
const superman = {
    default: { clark: 'kent' },
    private: true,
};
 
// good
const superman = {
    defaults: { clark: 'kent' },
    hidden: true,
};
```

3. `使用同义词替换需要使用的保留字。`

```javascript
// bad
const superman = {
    klass: 'alien',
};
 
// good
const superman = {
    type: 'alien',
};
```

4. `创建有动态属性名的对象时，使用可被计算的属性名称。`
这样可以让你在一个地方定义所有的对象属性。
```javascript
function getKey(k) {
    return `a key named ${k}`;
}
 
// bad
const obj = {
    id: 5,
    name: 'San Francisco',
};
obj[getKey('enabled')] = true;
 
// good
const obj = {
    id: 5,
    name: 'San Francisco',
    [getKey('enabled')]: true,
};
```
5. `使用对象方法的简写`
```javascript
// bad
const atom = {
    value: 1,
 
    addValue: function (value) {
        return atom.value + value;
    },
};
 
// good
const atom = {
    value: 1,
 
    addValue(value) {
        return atom.value + value;
    },
};
```
6. `使用对象属性值的简写`
更短更有描述性。

```javascript
const lukeSkywalker = 'Luke Skywalker';

// bad
const obj = {
    lukeSkywalker: lukeSkywalker,
};
 
// good
const obj = {
    lukeSkywalker,
};
```

7. `在对象属性声明前把简写的属性分组`
能清楚地看出哪些属性使用了简写

```javascript
const anakinSkywalker = 'Anakin Skywalker';
const lukeSkywalker = 'Luke Skywalker';
 
// bad
const obj = {
    episodeOne: 1,
    twoJedisWalkIntoACantina: 2,
    lukeSkywalker,
    episodeThree: 3,
    mayTheFourth: 4,
    anakinSkywalker,
};
 
// good
const obj = {
    lukeSkywalker,
    anakinSkywalker,
    episodeOne: 1,
    twoJedisWalkIntoACantina: 2,
    episodeThree: 3,
    mayTheFourth: 4,
};
```

## 四、数组

1. `使用字面值创建数组`

```javascript
// bad
const items = new Array();
 
// good
const items = [];
```

2. `向数组添加元素时使用 Arrary#push 替代直接赋值`
```javascript
const someStack = [];
 
// bad
someStack[someStack.length] = 'abracadabra';
 
// good
someStack.push('abracadabra');
```

3. `使用拓展运算符 ... 复制数组`
```javascript
// bad
const len = items.length;
const itemsCopy = [];
let i;
 
for (i = 0; i < len; i++) {
    itemsCopy[i] = items[i];
}
 
// good
const itemsCopy = [...items];
```
4. `使用 Array#from 把一个类数组对象转换成数组`

```javascript
const foo = document.querySelectorAll('.foo');
const nodes = Array.from(foo);
```

## 五、解构

1. `使用解构存取和使用多属性对象`
解构能减少临时引用属性

```javascript
// bad
function getFullName(user) {
    const firstName = user.firstName;
    const lastName = user.lastName;
 
    return `${firstName} ${lastName}`;
}
 
// good
function getFullName(obj) {
    const { firstName, lastName } = obj;
    return `${firstName} ${lastName}`;
}
 
// best
function getFullName({ firstName, lastName }) {
    return `${firstName} ${lastName}`;
}
```
2. `对数组使用解构赋值`

```javascript
const arr = [1, 2, 3, 4];
 
// bad
const first = arr[0];
const second = arr[1];
 
// good
const [first, second] = arr;
```

3. `需要回传多个值时，使用对象解构，而不是数组解构`

```javascript
// bad
function processInput(input) {
    // then a miracle occurs
    return [left, right, top, bottom];
}
 
// 调用时需要考虑回调数据的顺序。
const [left, __, top] = processInput(input);
 
// good
function processInput(input) {
    // then a miracle occurs
    return { left, right, top, bottom };
}
 
// 调用时只选择需要的数据
const { left, right } = processInput(input);
```