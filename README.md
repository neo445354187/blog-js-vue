# ES2015 AND VUE

## 参考
- 入门：<http://www.jianshu.com/p/ebfeb687eb70>
- 书籍：<http://es6.ruanyifeng.com/#docs/let>
- 书籍：<https://cn.vuejs.org/v2/guide>
- 视频：<https://www.bilibili.com/video/BV1Kt411w7MP?p=2>

## todo
- vue-cli
- vue.config.js代理，以及配置，如果将配置有效？


## 功能需求
 
### js直接加入html
 
insertAdjacentHTML方法：在指定的地方插入html标签语句 
参数：
- swhere: 指定插入html标签语句的地方，有四种值可用：
    1. beforeBegin: 插入到标签开始前
    2. afterBegin:插入到标签开始标记之后
    3. beforeEnd:插入到标签结束标记前
    4. afterEnd:插入到标签结束标记后
- stext：要插入的内容

```javascript
var ul = document.querySelector('ul');
var obj = ["A", "B", "C"];
ul.insertAdjacentHTML("beforeend", `
    <li>${obj[0]}</li>
    <li>${obj[1]}</li>
    <li>${obj[2] + obj[1]}</li>
`);
```

## 阮一峰ES6书籍

### 基础

#### let 命令
说明：用来声明变量。它的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效。
说明2：let不进行变量提升，即使用必须在声明后
```javascript
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```
例2
```javascript
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);         // 变量i是var命令声明的，在全局范围内都有效，所以全局只有一个变量i。每一次循环，变量i的值都会发生改变
  };
}
a[6](); // 10
// 改为let
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);         // 变量i是let声明的，当前的i只在本轮循环有效，所以每一次循环的i其实都是一个新的变量，所以最后输出的是6。
  };
}
a[6](); // 6                // 骚操作


for (let i = 0; i < 3; i++) {   // 设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。
  let i = 'abc';
  console.log(i);
}
```

##### let 暂时性死区
说明：在块作用域中，let声明的变量之前，变量被使用将报错
```javascript
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;      // 存在全局变量tmp，但是块级作用域内let又声明了一个局部变量tmp，导致后者绑定这个块级作用域，所以在let声明变量前，对tmp赋值会报错。
}
```

#### const
说明：const声明一个只读的常量。一旦声明，常量的值就不能改变。
说明2：const的作用域与let命令相同：只在声明所在的块级作用域内有效。

```javascript
const PI = 3.1415;
PI // 3.1415

PI = 3;
// TypeError: Assignment to constant variable.

const foo = {}; // 因为{}为对象，等号赋值，只是将地址给了foo，const foo只能保证foo变量值不变，即地址不能变，但实际的对象是可以改变值的

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only

// 对于数组
const a = [];
a.push('Hello'); // 可执行
a.length = 0;    // 可执行
a = ['Dave'];    // 报错
```

#### 数组的解构赋值

```javascript
let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3

let [ , , third] = ["foo", "bar", "baz"];
third // "baz"

let [x, , y] = [1, 2, 3];
x // 1
y // 3

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []
```

#### 对象的解构赋值

```javascript
var {x = 3} = {};
x // 3

var {x, y = 5} = {x: 1};
x // 1
y // 5

var {x: y = 3} = {};
y // 3

var {x: y = 3} = {x: 5};
y // 5

var { message: msg = 'Something went wrong' } = {};
msg // "Something went wrong"
```
#### 字符串的解构赋值
```javascript
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
```

#### 数值和布尔值的解构赋值

```javascript
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true
```

#### 函数参数的解构赋值

#### 解构用途

（1）交换变量的值
```javascript
let x = 1;
let y = 2;

[x, y] = [y, x];
```
（2）从函数返回多个值

```javascript
// 返回一个数组

function example() {
  return [1, 2, 3];
}
let [a, b, c] = example();

// 返回一个对象

function example() {
  return {
    foo: 1,
    bar: 2
  };
}
let { foo, bar } = example();
```

（3）函数参数的定义

解构赋值可以方便地将一组参数与变量名对应起来。

```
// 参数是一组有次序的值
function f([x, y, z]) { ... }
f([1, 2, 3]);

// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
```

（4）提取 JSON 数据

解构赋值对提取 JSON 对象中的数据，尤其有用。

```javascript
let jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};

let { id, status, data: number } = jsonData;

console.log(id, status, number);
// 42, "OK", [867, 5309]
```
（6）遍历 Map 结构

```javascript
const map = new Map();
map.set('first', 'hello');
map.set('second', 'world');

for (let [key, value] of map) {
  console.log(key + " is " + value);
}
// first is hello
// second is world
// 获取键名
for (let [key] of map) {
  // ...
}

// 获取键值
for (let [,value] of map) {
  // ...
}
```
（7）输入模块的指定方法

```javascript
const { SourceMapConsumer, SourceNode } = require("source-map");
```

#### 模板字符串
说明：将字符串中嵌入变量和表达式

```javascript

var ul = document.querySelector('ul');
var obj = ["A", "B", "C"];
ul.insertAdjacentHTML("beforeend", `
    <li>${obj[0]}</li>
    <li>${obj[1]}</li>
    <li>${obj[2] + obj[1]}</li>
`);
```

#### 模板编译
```javascript
// 编译模板
function compile(template){
  const evalExpr = /<%=(.+?)%>/g;
  const expr = /<%([\s\S]+?)%>/g;

  template = template
    .replace(evalExpr, '`); \n  echo( $1 ); \n  echo(`')
    .replace(expr, '`); \n $1 \n  echo(`');

  template = 'echo(`' + template + '`);';

  let script =
  `(function parse(data){
    let output = "";

    function echo(html){
      output += html;
    }

    ${ template }

    return output;
  })`;

  return script;
}

let template = `
<ul>
  <% for(let i=0; i < data.supplies.length; i++) { %>
    <li><%= data.supplies[i] %></li>
  <% } %>
</ul>
`;

let fun_parse = eval(compile(template));
var res = fun_parse({ supplies: [ "broom", "mop", "cleaner" ] });
console.log(res);
```

#### 字符串实例方法

##### includes(), startsWith(), endsWith()
- indexOf() :相当于php的strpos()，不存在则返回-1
- includes()：返回布尔值，表示是否找到了参数字符串。
- startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。
- endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。

```javascript
let s = 'Hello world!';

s.indexOf("He")     // 0
s.startsWith('Hello') // true
s.endsWith('!') // true
s.includes('o') // true
```

##### repeat() 
```javascript
'x'.repeat(3) // "xxx"
'hello'.repeat(2) // "hellohello"
'na'.repeat(0) // ""
```

##### padStart()，padEnd()
```javascript
'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'

'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'
```

##### trimStart()，trimEnd()
```javascript
const s = '  abc  ';

s.trim() // "abc"
s.trimStart() // "abc  "
s.trimEnd() // "  abc"
```

#### 字符串的正则方法

字符串对象共有 4 个方法，可以使用正则表达式：match()、replace()、search()和split()。
```javascript
var str = "ab12ef11";
console.log(str.match(/\d+/g));         // 获取正则的匹配
console.log(str.replace(/\d+/g,'RE'));  // 替换字符串
console.log(str.search(/\d+/));         // 返回子字符串的起始位置
console.log(str.split(/\d+/));          // 返回分割的数组，有点像php的preg_split()
```

#### 正则方法

```javascript
var patt = /e/;
patt.test("The best things in life are free!");         // 判断，返回true或者false
/li(\w)+/.exec("The best things in life are free!");    // 执行匹配，返回匹配数组
```
String.prototype.matchAll()
```javascript
var regex = /t(e)(st(\d?))/g;
var string = 'test1test2test3';

var matches = [];
var match;
while (match = regex.exec(string)) {
  matches.push(match);
}

matches
// [
//   ["test1", "e", "st1", "1", index: 0, input: "test1test2test3"],
//   ["test2", "e", "st2", "2", index: 5, input: "test1test2test3"],
//   ["test3", "e", "st3", "3", index: 10, input: "test1test2test3"]
// ]
```
ES2020 String.prototype.matchAll()

```javascript
const string = 'test1test2test3';
const regex = /t(e)(st(\d?))/g;

for (const match of string.matchAll(regex)) {
  console.log(match);
}
// ["test1", "e", "st1", "1", index: 0, input: "test1test2test3"]
// ["test2", "e", "st2", "2", index: 5, input: "test1test2test3"]
// ["test3", "e", "st3", "3", index: 10, input: "test1test2test3"]
```

#### 数值扩展

##### Number.isFinite(), Number.isNaN()
ES6 在Number对象上，新提供了Number.isFinite()和Number.isNaN()两个方法。
注意：如果参数类型不是数值，Number.isFinite一律返回false。
注意：如果参数类型不是NaN，Number.isNaN一律返回false。
```javascript
Number.isFinite(15); // true
Number.isFinite(0.8); // true
Number.isFinite(NaN); // false
Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false
Number.isFinite('foo'); // false
Number.isFinite('15'); // false
Number.isFinite(true); // false
Number.isNaN(NaN) // true
Number.isNaN(15) // false
Number.isNaN('15') // false
Number.isNaN(true) // false
Number.isNaN(9/NaN) // true
Number.isNaN('true' / 0) // true
Number.isNaN('true' / 'true') // true
```
##### Number.parseInt(), Number.parseFloat()
```javascript
// ES5的写法
parseInt('12.34') // 12
parseFloat('123.45#') // 123.45

// ES6的写法
Number.parseInt('12.34') // 12
Number.parseFloat('123.45#') // 123.45
```

##### Number.isInteger() 
```javascript
Number.isInteger(25) // true
Number.isInteger(25.1) // false
Number.isInteger(25) // true
Number.isInteger(25.0) // true
Number.isInteger() // false
Number.isInteger(null) // false
Number.isInteger('15') // false
Number.isInteger(true) // false
```
##### Math.trunc()

Math.trunc方法用于去除一个数的小数部分，返回整数部分。
```javascript
Math.trunc(4.1) // 4
Math.trunc(4.9) // 4
Math.trunc(-4.1) // -4
Math.trunc(-4.9) // -4
Math.trunc(-0.1234) // -0
Math.trunc('123.456') // 123
Math.trunc(true) //1
Math.trunc(false) // 0
Math.trunc(null) // 0
```

#### 函数的 length 属性

说明：函数的没有默认值参数个数
```javascript
(function (a) {}).length // 1
(function (a = 5) {}).length // 0
(function (a, b, c = 5) {}).length // 2
(function(...args) {}).length // 0
```

#### 尾递归
```javascript
function factorial(n, total) {
  if (n === 1) return total;
  return factorial(n - 1, n * total);
}

factorial(5, 1) // 120
```

### 数组

#### 扩展运算符
扩展运算符（spread）是三个点（...）。它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列。
放在形参：就是将多个参数合并为一个数组
放在实参：就是将数组转化为多个参数
```javascript
console.log(...[1, 2, 3])
// 1 2 3

console.log(1, ...[2, 3, 4], 5)
function push(array, ...items) {
  array.push(...items);
}

function add(x, y) {
  return x + y;
}

const numbers = [4, 38];
add(...numbers) // 42
```
替代函数的 apply 方法
```javascript
// ES5 的写法
Math.max.apply(null, [14, 3, 77])

// ES6 的写法
Math.max(...[14, 3, 77])

// 等同于
Math.max(14, 3, 77);
```

#### 复制数组

```javascript
const a1 = [1, 2];
// 写法一
const a2 = [...a1]; // 浅拷贝（只拷贝1层），即如果数组元素是符合类型，那么只是拷贝指针，即会同步更改
// 写法二
const [...a2] = a1; // 浅拷贝（只拷贝1层），即如果数组元素是符合类型，那么只是拷贝指针，即会同步更改
```

#### 合并数组
```javascript
const arr1 = [[1,2], 'b'];
const arr2 = ['c'];
const arr3 = ['d', 'e'];

// ES5 的合并数组
//arr1.concat(arr2, arr3);
// [ 'a', 'b', 'c', 'd', 'e' ]

// ES6 的合并数组
let res = [...arr1, ...arr2, ...arr3]   // 浅拷贝（只拷贝1层），即如果数组元素是符合类型，那么只是拷贝指针，即会同步更改
// [ 'a', 'b', 'c', 'd', 'e' ]
res[0][0] = 100;
console.log(res, arr1)
```

#### 数组结构
```javascript
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest  // [2, 3, 4, 5]

const [first, ...rest] = [];
first // undefined
rest  // []

const [first, ...rest] = ["foo"];
first  // "foo"
rest   // []

const [...butLast, last] = [1, 2, 3, 4, 5];
// 报错

const [first, ...middle, last] = [1, 2, 3, 4, 5];
// 报错
```

#### 字符串转

扩展运算符还可以将字符串转为真正的数组。
```javascript
[...'hello']
// [ "h", "e", "l", "l", "o" ]
```

#### Map 和 Set 结构，Generator 函数
扩展运算符内部调用的是数据结构的 Iterator 接口，因此只要具有 Iterator 接口的对象，都可以使用扩展运算符，比如 Map 结构。

```javascript
let map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

let arr = [...map.keys()]; // [1, 2, 3]
```
Generator 函数运行后，返回一个遍历器对象，因此也可以使用扩展运算符。
```javascript
const go = function*(){
  yield 1;
  yield 2;
  yield 3;
};

[...go()] // [1, 2, 3]
```
上面代码中，变量go是一个 Generator 函数，执行后返回的是一个遍历器对象，对这个遍历器对象执行扩展运算符，就会将内部遍历得到的值，转为一个数组。

如果对没有 Iterator 接口的对象，使用扩展运算符，将会报错。
```javascript
const obj = {a: 1, b: 2};
let arr = [...obj]; // TypeError: Cannot spread non-iterable object
```
```javascript
var go = function* () {
    for (let i = 0; i < 5; i++) {
        yield  i;
    }
}
const g = go()  // 这一步是需要的，不能直接使用go.next()
console.log(g.next())
console.log(g.next())
```

#### Array.from()
Array.from方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象
（包括 ES6 新增的数据结构 Set 和 Map）。

下面是一个类似数组的对象，Array.from将它转为真正的数组。
```javascript
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

// ES5的写法
var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']

// ES6的写法
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
```
实际应用中，常见的类似数组的对象是 DOM 操作返回的 NodeList 集合，以及函数内部的arguments对象。
Array.from都可以将它们转为真正的数组。
```javascript
// NodeList对象
let ps = document.querySelectorAll('p');
Array.from(ps).filter(p => {                // 骚操作
  return p.textContent.length > 100;
});

// arguments对象
function foo() {
  var args = Array.from(arguments);
  // ...
}
```
上面代码中，querySelectorAll方法返回的是一个类似数组的对象，可以将这个对象转为真正的数组，再使用filter方法。

只要是部署了 Iterator 接口的数据结构，Array.from都能将其转为数组。

```javascript
Array.from('hello')
// ['h', 'e', 'l', 'l', 'o']

let namesSet = new Set(['a', 'b'])
Array.from(namesSet) // ['a', 'b']
```

#### Array.of()
Array.of方法用于将一组值，转换为数组。
```javascript
Array.of(3, 11, 8) // [3,11,8]
Array.of(3) // [3]
Array.of(3).length // 1
```
#### 数组实例的 find() 和 findIndex()
```javascript
[1, 4, -5, 10].find((n) => n < 0)       // 一次迭代元素，知道遇到返回值为true的元素，将其返回，如果没有，则为undefined
// -5
[1, 5, 10, 15].findIndex(function(value, index, arr) {// 与find类似，不过返回下标，如果没有，则返回-1
  return value > 9;
}) // 2
```
#### 数组实例的 fill()
fill方法使用给定值，填充一个数组。
```javascript
['a', 'b', 'c'].fill(7)
// [7, 7, 7]

new Array(3).fill(7)
// [7, 7, 7]
```
#### 数组实例的 entries()，keys() 和 values()

keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历。
```javascript
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
```
#### 数组实例的 includes() 
Array.prototype.includes方法返回一个布尔值，表示某个数组是否包含给定的值，**与字符串的includes方法类似**
```javascript
[1, 2, 3].includes(2)     // true
[1, 2, 3].includes(4)     // false
[1, 2, NaN].includes(NaN) // true
[1, 2, 3].includes(3, 3);  // false // 该方法的第二个参数表示搜索的起始位置，默认为0。
[1, 2, 3].includes(3, -1); // true
```

#### 数组实例的 flat()，flatMap()
- flat 数组的成员有时还是数组，Array.prototype.flat()用于将嵌套的数组“拉平”，变成一维的数组。该方法返回一个新数组，对原数据没有影响。
- flatMap 方法对原数组的每个成员执行一个函数（相当于执行Array.prototype.map()），然后对返回值组成的数组执行flat()方法。
          该方法返回一个新数组，不改变原数组。flatMap()只能展开一层数组。
```javascript
[1, 2, [3, [4, 5]]].flat()
// [1, 2, 3, [4, 5]]

[1, 2, [3, [4, 5]]].flat(2)         // 拉平2层
// [1, 2, 3, 4, 5]
[1, [2, [3]]].flat(Infinity)        // 无限拉平
// [1, 2, 3]

[1, 2, 3, 4].flatMap(x => [[x * 2]])
// [[2], [4], [6], [8]]
```

#### 数组的空位
```javascript
Array(3) // [, , ,]
```
注意，空位不是undefined，一个位置的值等于undefined，依然是有值的。空位是没有任何值，in运算符可以说明这一点。
说明：x in data;当data是数组时：x指的是数组的“索引”；当data为对象时，x指的是对象的“属性”。
```javascript
0 in [undefined, undefined, undefined] // true
0 in [, , ,] // false
```

#### 数组sort
说明：有点类似于php的sort和usort的结合
```javascript
var arr = [1, 5, 3, 4];
console.log(arr.sort());            // 默认升序
console.log(arr.sort(function (a1, a2) {
    return a1 > a2 ? -1 : 1;        // 这样是降序
}));
```

### 对象

#### 可枚举性
- for...in循环：只遍历对象自身的和继承的可枚举的属性。
- Object.keys()：返回对象自身的所有可枚举的属性的键名。
- JSON.stringify()：只串行化对象自身的可枚举的属性。
- Object.assign()： 忽略enumerable为false的属性，只拷贝对象自身的可枚举的属性。

#### super
说明：super，指向当前对象的原型对象。
注意与super()方法区分哦
```javascript
const proto = {
  foo: 'hello'
};

const obj = {
  foo: 'world',
  find() {
    return super.foo;
  }
};

Object.setPrototypeOf(obj, proto);//设置对象obj的__proto__为proto对象
obj.find() // "hello"
```

#### 对象解构赋值
```javascript
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
x // 1
y // 2
z // { a: 3, b: 4 }
```

#### 对象的拷贝
```javascript
const obj = {
    foo: 'world',
    second: [1, 2]
};

const res = {...obj};       // 浅拷贝
res.second[0] = 100;        // 复合类型值同步变化
console.log(res, obj);
```

### Object

#### Object.is()
说明：两个值是否相等
```javascript
Object.is('foo', 'foo')
// true
Object.is({}, {})
// false
+0 === -0 //true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

#### Object.assign()
Object.assign()方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。
类似于php的array_merge
```javascript
const target = { a: 1 };

const source1 = { b: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}

Object.assign([1, 2, 3], [4, 5])
// [4, 5, 3]
```

#### __proto__属性，Object.setPrototypeOf()，Object.getPrototypeOf()

__proto__属性
- Object.create()           // 生成操作
- Object.setPrototypeOf()   // 写操作
- Object.getPrototypeOf()   // 读操作

```javascript
// es5 的写法
const obj = {
  method: function() {  }
};
obj.__proto__ = someOtherObj;

// es6 的写法
var obj = Object.create(someOtherObj);
obj.method = function() { };

// 格式
Object.setPrototypeOf(object, prototype)

// 用法
const o = Object.setPrototypeOf({}, null);
```

#### Object.keys()，Object.values()，Object.entries()
```javascript
let {keys, values, entries} = Object;
let obj = { a: 1, b: 2, c: 3 };

for (let key of keys(obj)) {
  console.log(key); // 'a', 'b', 'c'
}

for (let value of values(obj)) {
  console.log(value); // 1, 2, 3
}

for (let [key, value] of entries(obj)) {
  console.log([key, value]); // ['a', 1], ['b', 2], ['c', 3]
}
```

#### Object.fromEntries()
Object.fromEntries()方法是Object.entries()的逆操作，用于将一个键值对数组转为对象。
```javascript
// 例一
const entries = new Map([
  ['foo', 'bar'],
  ['baz', 42]
]);

Object.fromEntries(entries)
// { foo: "bar", baz: 42 }

// 例二
const map = new Map().set('foo', true).set('bar', false);
Object.fromEntries(map)
// { foo: true, bar: false }
```

### Symbol
说明：一种新的数据格式，它的值除了与自身可以相等外，与其他值都不同
说明2：主要为了防止覆盖
```javascript
let s1 = Symbol('foo');
let s2 = Symbol('bar');

s1 // Symbol(foo)
s2 // Symbol(bar)

s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"

console.log(Symbol() === Symbol())  // false

const s3 = Symbol();

const obj = {
    [s3]: 'world',  // 注意用法，如果直接写s3那就是字符串了，[s3]才能解析
    second: [1, 2]
};

console.log(obj[s3]);
```


#### Symbol.for()，Symbol.keyFor()

- Symbol.for() 接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值。如果有，就返回这个 Symbol 值，
否则就新建一个以该字符串为名称的 Symbol 值，并将其注册到全局。
- Symbol.keyFor() 返回一个已登记的 Symbol 类型值的key。
```javascript
let s1 = Symbol.for('foo');
let s2 = Symbol.for('foo');

s1 === s2 // true

let s1 = Symbol.for("foo"); // Symbol.for有登记作用
Symbol.keyFor(s1) // "foo"

let s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```

#### 模块的 Singleton 模式
参考：<https://es6.ruanyifeng.com/#docs/symbol>


### Set
Set：类似于数组，但是成员的值都是唯一的，没有重复的值。
猜测：不能直接采用下标访问，不能像arr[i]
```javascript
const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4
// 例一
const set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]

// 例二
const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
items.size // 5

// 例三
const set = new Set(document.querySelectorAll('div'));
set.size // 56

// 类似于
const set = new Set();
document
 .querySelectorAll('div')
 .forEach(div => set.add(div));
set.size // 56
```

```
// 去除数组的重复成员
[...new Set(array)];        // 骚操作
```

#### Set 实例的属性和方法
 Set 结构的实例有以下属性。
 - Set.prototype.constructor：构造函数，默认就是Set函数。
 - Set.prototype.size：返回Set实例的成员总数。
 
 Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。下面先介绍四个操作方法。
 - Set.prototype.add(value)：添加某个值，返回 Set 结构本身。
 - Set.prototype.delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
 - Set.prototype.has(value)：返回一个布尔值，表示该值是否为Set的成员。
 - Set.prototype.clear()：清除所有成员，没有返回值。
```javascript
s.add(1).add(2).add(2);
// 注意2被加入了两次

s.size // 2

s.has(1) // true
s.has(2) // true
s.has(3) // false

s.delete(2);
s.has(2) // false
```

Array.from方法可以将 Set 结构转为数组。
```javascript
const items = new Set([1, 2, 3, 4, 5]);
const array = Array.from(items);
```
这就提供了去除数组重复成员的另一种方法。
```javascript
function dedupe(array) {
  return Array.from(new Set(array));
}

dedupe([1, 1, 2, 3]) // [1, 2, 3]       // 骚操作去重
```

#### 遍历操作
Set 结构的实例有四个遍历方法，可以用于遍历成员。

- Set.prototype.keys()：返回键名的遍历器
- Set.prototype.values()：返回键值的遍历器
- Set.prototype.entries()：返回键值对的遍历器
- Set.prototype.forEach()：使用回调函数遍历每个成员

#### WeakSet
WeakSet 结构与 Set 类似，也是不重复的值的集合。但是，它与 Set 有两个区别。

首先，WeakSet 的成员只能是对象，而不能是其他类型的值。
其次，WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，
那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。
参考：<https://es6.ruanyifeng.com/#docs/set-map>

### Map
说明：类似于对象，但是键可以为复合类型

```javascript
const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);
const key = [1];
map.set(key, 'haha')
console.log(map.get(key))

map.size // 2
map.has('name') // true
map.get('name') // "张三"
map.has('title') // true
map.get('title') // "Author"
```
#### 遍历方法
Map 结构原生提供三个遍历器生成函数和一个遍历方法。

- Map.prototype.keys()：返回键名的遍历器。
- Map.prototype.values()：返回键值的遍历器。
- Map.prototype.entries()：返回所有成员的遍历器。
- Map.prototype.forEach()：遍历 Map 的所有成员。

```javascript
const map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);

for (let key of map.keys()) {
  console.log(key);
}
// "F"
// "T"

for (let value of map.values()) {
  console.log(value);
}
// "no"
// "yes"

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// "F" "no"
// "T" "yes"

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"

// 等同于使用map.entries()
for (let [key, value] of map) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"
```

#### Map与其他数据结构的互相转换
参考：<https://es6.ruanyifeng.com/#docs/set-map>

#### WeakMap
参考：<https://es6.ruanyifeng.com/#docs/set-map>

### Proxy

Proxy：作用类似于php的getter和setter

```javascript
var proxy = new Proxy({}, {
  get: function(target, propKey) {
    return 35;
  }
});

proxy.time // 35
proxy.name // 35
proxy.title // 35
```

Proxy 支持的拦截操作一览，一共 13 种。

- get(target, propKey, receiver)：拦截对象属性的读取，比如proxy.foo和proxy['foo']。
- set(target, propKey, value, receiver)：拦截对象属性的设置，比如proxy.foo = v或proxy['foo'] = v，返回一个布尔值。
- has(target, propKey)：拦截propKey in proxy的操作，返回一个布尔值。
- deleteProperty(target, propKey)：拦截delete proxy[propKey]的操作，返回一个布尔值。
- ownKeys(target)：拦截Object.getOwnPropertyNames(proxy)、Object.getOwnPropertySymbols(proxy)、Object.keys(proxy)、for...in循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而Object.keys()的返回结果仅包括目标对象自身的可遍历属性。
- getOwnPropertyDescriptor(target, propKey)：拦截Object.getOwnPropertyDescriptor(proxy, propKey)，返回属性的描述对象。
- defineProperty(target, propKey, propDesc)：拦截Object.defineProperty(proxy, propKey, propDesc）、Object.defineProperties(proxy, propDescs)，返回一个布尔值。
- preventExtensions(target)：拦截Object.preventExtensions(proxy)，返回一个布尔值。
- getPrototypeOf(target)：拦截Object.getPrototypeOf(proxy)，返回一个对象。
- isExtensible(target)：拦截Object.isExtensible(proxy)，返回一个布尔值。
- setPrototypeOf(target, proto)：拦截Object.setPrototypeOf(proxy, proto)，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。
- apply(target, object, args)：拦截 Proxy 实例作为函数调用的操作，比如proxy(...args)、proxy.call(object, ...args)、proxy.apply(...)。
- construct(target, args)：拦截 Proxy 实例作为构造函数调用的操作，比如new proxy(...args)。
参考：<https://es6.ruanyifeng.com/#docs/proxy>

```javascript
var handler = {
  get: function(target, name) {
    if (name === 'prototype') {
      return Object.prototype;
    }
    return 'Hello, ' + name;
  },
  // 拦截函数调用
  apply: function(target, thisBinding, args) {
    return args[0];
  },

  construct: function(target, args) {
    return {value: args[1]};
  }
};

var fproxy = new Proxy(function(x, y) {
  return x + y;
}, handler);

fproxy(1, 2) // 1
new fproxy(1, 2) // {value: 2}
fproxy.prototype === Object.prototype // true
fproxy.foo === "Hello, foo" // true
```
示例2
```javascript
// 直接类似于函数调用`res()`，需要`new Proxy()`的第一个参数本身是函数，如果是对象，那么不能直接函数方式调用，但可以访问属性
const res = new Proxy(()=>{}, {
    apply:function(target, thisArg, argArray) {
        return 1;
    }
})
console.log(res())
```

### Reflect
说明：就是映射，类似于php中的reflection或者golang中的reflect

（1） 将Object对象的一些明显属于语言内部的方法（比如Object.defineProperty），放到Reflect对象上。
现阶段，某些方法同时在Object和Reflect对象上部署，未来的新方法将只部署在Reflect对象上。
也就是说，从Reflect对象上可以拿到语言内部的方法。

（2） 修改某些Object方法的返回结果，让其变得更合理。比如，Object.defineProperty(obj, name, desc)在无法定义属性时，
会抛出一个错误，而Reflect.defineProperty(obj, name, desc)则会返回false。
```javascript
// 老写法
try {
  Object.defineProperty(target, property, attributes);
  // success
} catch (e) {
  // failure
}

// 新写法
if (Reflect.defineProperty(target, property, attributes)) {
  // success
} else {
  // failure
}
```
（3） 让Object操作都变成函数行为。某些Object操作是命令式，
比如name in obj和delete obj[name]，而Reflect.has(obj, name)和Reflect.deleteProperty(obj, name)让它们变成了函数行为。
```javascript
// 老写法
'assign' in Object // true

// 新写法
Reflect.has(Object, 'assign') // true
```
（4）Reflect对象的方法与Proxy对象的方法一一对应，只要是Proxy对象的方法，就能在Reflect对象上找到对应的方法。
这就让Proxy对象可以方便地调用对应的Reflect方法，完成默认行为，作为修改行为的基础。也就是说，不管Proxy怎么修改默认行为，
你总可以在Reflect上获取默认行为。
```javascript
Proxy(target, {
  set: function(target, name, value, receiver) {
    var success = Reflect.set(target, name, value, receiver);
    if (success) {
      console.log('property ' + name + ' on ' + target + ' set to ' + value);
    }
    return success;
  }
});
```
上面代码中，Proxy方法拦截target对象的属性赋值行为。
它采用Reflect.set方法将值赋值给对象的属性，确保完成原有的行为，然后再部署额外的功能。

#### Reflect静态方法
Reflect对象一共有 13 个静态方法。

- Reflect.apply(target, thisArg, args)
- Reflect.construct(target, args)
- Reflect.get(target, name, receiver)
- Reflect.set(target, name, value, receiver)
- Reflect.defineProperty(target, name, desc)
- Reflect.deleteProperty(target, name)
- Reflect.has(target, name)
- Reflect.ownKeys(target)
- Reflect.isExtensible(target)
- Reflect.preventExtensions(target)
- Reflect.getOwnPropertyDescriptor(target, name)
- Reflect.getPrototypeOf(target)
- Reflect.setPrototypeOf(target, prototype)
上面这些方法的作用，大部分与Object对象的同名方法的作用都是相同的，而且它与Proxy对象的方法是一一对应的。下面是对它们的解释。


### Promise
参考：<https://es6.ruanyifeng.com/#docs/promise>
**一个容器**，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。

常见语法
```javascript
// resolve, reject是两个函数，由 JavaScript 引擎提供，不用自己部署。
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if ('/* 异步操作成功 */'){
    resolve(value); // 给then中的第一个参数函数传递参数，即下面的result
  } else {
    reject(error);  // 给them中的第二个参数函数传递参数（不推荐的写法），当然也可以传递给catch（catch才是推荐的写法），即下面的error
  }
});

promise
.then(result => {})
.catch(error => {})
.finally(() => {}); // 无论如何都要执行的代码
```
例1
```javascript
const getJSON = function(url) {
  const promise = new Promise(function(resolve, reject){
    const handler = function() {
      if (this.readyState !== 4) {
        return;
      }
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
    const client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();

  });

  return promise;
};

getJSON("/posts.json").then(function(json) {
  console.log('Contents: ' + json);
}, function(error) {
  console.error('出错了', error);
});
```

### Iterator 和 for...of 循环

原生具备 Iterator 接口的数据结构如下。

- Array
- Map
- Set
- String
- TypedArray
- 函数的 arguments 对象
- NodeList 对象

让对象可以使用for...of遍历，注意，对象本身是可以被for in迭代的
```javascript
class RangeIterator {
  constructor(start, stop) {
    this.value = start;
    this.stop = stop;
  }

  [Symbol.iterator]() { return this; }  // 必须实现[Symbol.iterator]为名字的方法，而且必须返回一个具有next()方法的对象

  next() {
    var value = this.value;
    if (value < this.stop) {
      this.value++;
      return {done: false, value: value};
    }
    return {done: true, value: undefined};
  }
}

function range(start, stop) {
  return new RangeIterator(start, stop);
}

for (var value of range(0, 3)) {    // 主要：这里说明for of后面的函数只会执行一次，但是next()方法却会执行多次
  console.log(value); // 0, 1, 2
}
```

### Generator 函数的语法

参考：<https://es6.ruanyifeng.com/#docs/generator>

Generator 函数是一个普通函数，但是有两个特征。
一是，function关键字与函数名之间有一个星号；
二是，函数体内部使用yield表达式，定义不同的内部状态
```javascript
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();
hw.next()
// { value: 'hello', done: false }

hw.next()
// { value: 'world', done: false }

hw.next()
// { value: 'ending', done: true }

hw.next()
// { value: undefined, done: true }
```

#### yield
注意：yield表达式只能用在 Generator 函数里面，用在其他地方都会报错。

遍历器对象的next方法的运行逻辑如下。

（1）遇到yield表达式，就暂停执行后面的操作，并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值。

（2）下一次调用next方法时，再继续往下执行，直到遇到下一个yield表达式。

（3）如果没有再遇到新的yield表达式，就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值，作为返回的对象的value属性值。

（4）如果该函数没有return语句，则返回的对象的value属性值为undefined。

#### 与 Iterator 接口的关系

```javascript
var myIterable = {};
myIterable[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};

[...myIterable] // [1, 2, 3]
```

#### for...of 循环
```javascript
function* foo() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  yield 5;
  return 6;
}

for (let v of foo()) {
  console.log(v);
}
// 1 2 3 4 5
```

#### Generator.prototype.throw() § ⇧
Generator 函数返回的遍历器对象，都有一个throw方法，可以在函数体外抛出错误，**然后在 Generator 函数体内捕获。**
```javascript
 var g = function* () {
   try {
     yield;
   } catch (e) {
     console.log('内部捕获', e);
   }
 };
 
 var i = g();
 i.next();
 
 try {
   i.throw('a');
   i.throw('b');
 } catch (e) {
   console.log('外部捕获', e);
 }
 // 内部捕获 a
 // 外部捕获 b
```
上面代码中，遍历器对象i连续抛出两个错误。第一个错误被 Generator 函数体内的catch语句捕获。
i第二次抛出错误，由于 Generator 函数内部的catch语句已经执行过了，不会再捕捉到这个错误了，
所以这个错误就被抛出了 Generator 函数体，被函数体外的catch语句捕获。

#### Generator.prototype.return()
Generator 函数返回的遍历器对象，还有一个return方法，可以返回给定的值，并且终结遍历 Generator 函数。
```javascript
function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

var g = gen();

g.next()        // { value: 1, done: false }
g.return('foo') // { value: "foo", done: true }
g.next()        // { value: undefined, done: true }
```

#### yield* 表达式
如果在 Generator 函数内部，调用另一个 Generator 函数。需要在前者的函数体内部，自己手动完成遍历。
```javascript
function* foo() {
  yield 'a';
  yield 'b';
}

function* bar() {
  yield 'x';
  // 手动遍历 foo()
  for (let i of foo()) {
    console.log(i);
  }
  yield 'y';
}

for (let v of bar()){
  console.log(v);
}
// x
// a
// b
// y
```
上面代码中，foo和bar都是 Generator 函数，在bar里面调用foo，就需要手动遍历foo。如果有多个 Generator 函数嵌套，写起来就非常麻烦。


#### Generator 函数的异步应用
```javascript
function* gen(x){
   // next(z)传入参数，是作为上一个yield的结果，z替换yield x + 2，给y赋值;注意，如果next()没有传递参数，那么y并不会被`yield x + 2;`的结果赋值，而是会被赋值undefined
  var y = yield x + 2; 
  return y;
}

var g = gen(1); // 除了将x赋值外，gen函数中的代码一句都没有执行，直到调用next()
g.next() // { value: 3, done: false }
g.next(2) // { value: 2, done: true }
```
上面代码中，第一个next方法的value属性，返回表达式x + 2的值3。第二个next方法带有参数2，
**这个参数可以传入 Generator 函数，作为上个阶段异步任务的返回结果，被函数体内的变量y接收。**
因此，这一步的value属性，返回的就是2（变量y的值）。


### async/await

1、首先需要理解async 和 await的基本含义

async 是一个修饰符，async 定义的函数（ps：需要执行撒）会默认的返回一个Promise对象，
因此对async函数可以直接进行then操作,返回的值即为then方法的传入函数
```javascript
// 0. async基础用法测试

async function fun0() {
    console.log(1)
    return 1
}

fun0().then( x => { console.log(x) })  //  输出结果 1， 1，


async function funa() {
    console.log('a')
    return 'a'
}

funa().then( x => { console.log(x) })  //  输出结果a， a，


async function funo() {
    console.log({})
    return {}
}

funo().then( x => { console.log(x) })   // 输出结果 {}  {}

async function funp() {
    console.log('Promise')
    return new Promise(function(resolve, reject){
        resolve('Promise')
    })
}

funp().then( x => { console.log(x) })   // 输出promise  promise
```

await 也是一个修饰符，

await 关键字 只能放在 async 函数内部， await关键字的作用 就是获取 Promise中返回的内容， 
获取的是Promise函数中resolve或者reject的值
// **如果await 后面并不是一个Promise的返回值，则会按照同步程序返回值处理**
当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。

```javascript
//  await 关键字 只能放在 async 函数内部， await关键字的作用 就是阻塞等待获取 Promise中返回的内容， 获取的是Promise函数中resolve或者reject的值
// 如果await 后面并不是一个Promise的返回值，则会按照同步程序返回值处理,为undefined
const bbb = function(){ return 'string'}

async function funAsy() {
   const a = await 1
   const b = await new Promise((resolve, reject)=>{
        setTimeout(function(){
           resolve('time')
        }, 3000)
   })
   const c = await bbb()
   console.log(a, b, c)
}

funAsy()  //  运行结果是 3秒钟之后 ，输出 1， time , string,
```
```javascript
var log =async function () {
    return new Promise(function (resolve,reject) {
        console.log('setTimeout')
        setTimeout(resolve,1000)
    })
}
// async函数内部await是阻塞的，只是相对于async函数外部代码来说，是异步的
async function say(){
    console.log('mark');
    let x = await log()
    let y = await log()
    let z = await log()
    console.log("say")
    console.log(x, y, z);
}
say();
console.log("start")
```
错误捕捉
```javascript
var sleep = function (time) {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            // 模拟出错了，返回 ‘error’
            reject('error');    // reject产生error
        }, time);
    })
};

var start = async function () {
    // 推荐写法
    try {
        console.log('start');
        await sleep(3000); // 这里得到了一个返回错误
        
        // 所以以下代码不会被执行了
        console.log('end');
    } catch (err) {
        console.log(err); // 这里捕捉到错误 `error`
    }
};
start();
```

### Class 的基本语法
参考：<https://es6.ruanyifeng.com/#docs/class>

### Module

#### 严格模式
ES6 的模块自动采用严格模式，不管你有没有在模块头部加上"use strict";。

严格模式主要有以下限制。

- 变量必须声明后再使用
- 函数的参数不能有同名属性，否则报错
- 不能使用with语句
- 不能对只读属性赋值，否则报错
- 不能使用前缀 0 表示八进制数，否则报错
- 不能删除不可删除的属性，否则报错
- 不能删除变量delete prop，会报错，只能删除属性delete global[prop]
- eval不会在它的外层作用域引入变量
- eval和arguments不能被重新赋值
- arguments不会自动反映函数参数的变化
- 不能使用arguments.callee
- 不能使用arguments.caller
- 禁止this指向全局对象
- 不能使用fn.caller和fn.arguments获取函数调用的堆栈
- 增加了保留字（比如protected、static和interface）

#### import和export

main.js
```javascript
import {name, fn, cls} from "./profile.js"  // 多个文件中同时引入"./profile.js"，profile.js中代码只会运行一次，里面的变量也是公共的，有点像golang
// import "./profile.js" // 直接加载模块，运行模块（如模块文件内有函数执行，会执行函数）
console.log(name);
fn();
(new cls()).doit('main');

function f() {
    console.log(this)
}
f();
```
profile.js
```javascript
export {name, fn, cls}; // export语句会提升，放在文件末尾功能一样

var name = "haha"
var fn = function () {
    console.log("lydia");
}
var cls = class {
    doit(x) {
        console.log("cls.doit",x);
    }
}

fn();// 被加载后会执行
```

index.html
```
<script type="module" src="./main.js"></script>
```













## 视频知识

### 类和对象

#### 01-利用构造函数创建对象
说明：
1. ES6中没有变量提升，所以必须先定义类，才能实例化对象
2. 类里面的属性和方法需要加this
3. 类里面的this的指向问题，constructor()方法中this指向实例对象，类普通方法里的this指向调用方法者

例1：
```javascript
class Star {
    // 没有function哦
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    sing(song) {
        console.log(this.name +"唱"+ song);
    }
}

var ldh = new Star("刘德华", 30);
var zxy = new Star("张学友", 20);
ldh.sing("爱情水");
zxy.sing("李香兰");
```
例2
```
<button>btn</button>
<script>
    var that
    class Father {
        // 没有function哦
        constructor(uname) {
            // 猜测：对象赋值，是浅拷贝
            that = this
            this.uname = uname;
            this.btn = document.querySelector("button");
            this.btn.onclick = this.say;
        }

        say() {
            console.log(this.uname);// undefined
            console.log(that.uname);// chao
        }

    }

    var f = new Father('haha');
</script>
```


#### 02-super
super：调用父类constructor方法，也可以调用普通方法
备注：super位置记得放在this之前哦
```javascript
class Father {
    // 没有function哦
    constructor(a, b) {
        this.a = a;
        this.b = b;
    }

    plus() {
        // 父类方法可以直接调用子类的属性
        return this.a + this.b;
    }
}

class Son extends Father {
    constructor(a, b) {
        // super一定要放在子类的this之前；简记：父亲永远第一位
        super(a, b);
        this.a = a;
        this.b = b;
    }

    reduce() {
        return this.a - this.b;
    }
}

var s = new Son(10, 2);
console.log(s.plus())
console.log(s.reduce())
```
示例2
```javascript
class Father {
    constructor(uname, age) {
        // this 指向父构造函数的对象实例
        this.uname = uname;
        this.age = age;
    }

    say(){
        console.log(this.uname,this.score)
    }
}

class Son extends Father{
    constructor(uname, age, score) {
        // this 指向子构造函数的对象实例
        super(uname, age);// 重点
        this.score = score;
    }
}

var son = new Son('刘德华', 18, 100);
son.say()
```

### 构造函数和对象
说明：ES6之前没有类，采用的其他方法来构造对象
创建对象可以通过以下三种方式：

1. 对象字面量
2. new Object()
3. 自定义构造函数

```javascript
// 1. 利用 new Object() 创建对象

var obj1 = new Object();

// 2. 利用 对象字面量创建对象

var obj2 = {};

// 3. 利用构造函数创建对象
function Star(uname, age) {
    this.uname = uname;
    this.age = age;
    this.sing = function() {
        console.log('我会唱歌');
    }
}
// 注意有new哦
var ldh = new Star('刘德华', 18);
var zxy = new Star('张学友', 19);
console.log(ldh);
ldh.sing();
zxy.sing();
```

#### 构造函数原型 prototype（原型对象）
说明：构造函数通过原型分配的函数是所有对象所共享的。因为常规的将方法声明在构造函数内，每次实例化都会用新内存存储公共方法，造成内存浪费
JavaScript 规定，每一个构造函数都有一个 prototype 属性，指向另一个对象。注意这个 prototype 就是一个对象，
这个对象的所有属性和方法，都会被构造函数所拥有。

我们可以把那些不变的方法，直接定义在 prototype 对象上，这样所有对象的实例就可以共享这些方法。
```javascript
// 1. 构造函数的问题. 
function Star(uname, age) {
    this.uname = uname;
    this.age = age;
    // this.sing = function() {
    //     console.log('我会唱歌');

    // }
}
Star.prototype.sing = function() {
    console.log('我会唱歌');
}
var ldh = new Star('刘德华', 18);
var zxy = new Star('张学友', 19);
console.log(ldh.sing === zxy.sing);
// console.dir(Star);
ldh.sing();
zxy.sing();
// 2. 一般情况下,我们的公共属性定义到构造函数里面, 公共的方法我们放到原型对象身上
```

#### 对象原型
说明：对象都会有一个属性 `__proto__` 指向构造函数的 `prototype` 原型对象，之所以我们对象可以使用构造函数 `prototype` 原型对象的属性和方法，
就是因为对象有 `__proto__` 原型的存在。
- `__proto__`对象原型和原型对象 `prototype` 是等价的
- `__proto__`对象原型的意义就在于为对象的查找机制提供一个方向，或者说一条路线，但是它是一个非标准属性，因此实际开发中，不可以使用这个属性，
    它只是内部指向原型对象 `prototype`
```javascript
function Star(uname, age) {
    this.uname = uname;
    this.age = age;
}
Star.prototype.sing = function() {
    console.log('我会唱歌');
}
var ldh = new Star('刘德华', 18);
var zxy = new Star('张学友', 19);
ldh.sing();
console.log(ldh); // 对象身上系统自己添加一个 __proto__ 指向我们构造函数的原型对象 prototype
console.log(ldh.__proto__ === Star.prototype);
// 方法的查找规则: 首先先看ldh 对象身上是否有 sing 方法,如果有就执行这个对象上的sing
// 如果么有sing 这个方法,因为有__proto__ 的存在,就去构造函数原型对象prototype身上去查找sing这个方法
```

#### constructor  构造函数
说明
- 对象原型（ __proto__）和构造函数（prototype）原型对象里面都有一个属性 constructor 属性 ，
  constructor 我们称为构造函数，因为它指回构造函数本身。

- constructor 主要用于记录该对象引用于哪个构造函数，它可以让原型对象重新指向原来的构造函数。

- 一般情况下，对象的方法都在构造函数的原型对象中设置。如果有多个对象的方法，我们可以给原型对象采取对象形式赋值，
  但是这样就会覆盖构造函数原型对象原来的内容，这样修改后的原型对象 constructor  就不再指向当前构造函数了。
  此时，我们可以在修改后的原型对象中，添加一个 constructor 指向原来的构造函数。
```javascript
function Star(uname, age) {
    this.uname = uname;
    this.age = age;
}
// 很多情况下,我们需要手动的利用constructor 这个属性指回 原来的构造函数
// Star.prototype.sing = function() {
//     console.log('我会唱歌');
// };
// Star.prototype.movie = function() {
//     console.log('我会演电影');
// }
Star.prototype = {
    // 如果我们修改了原来的原型对象,给原型对象赋值的是一个对象,则必须手动的利用constructor指回原来的构造函数
    constructor: Star,
    sing: function() {
        console.log('我会唱歌');
    },
    movie: function() {
        console.log('我会演电影');
    }
}
var ldh = new Star('刘德华', 18);
var zxy = new Star('张学友', 19);
console.log(Star.prototype);
console.log(ldh.__proto__);
console.log(Star.prototype.constructor);
console.log(ldh.__proto__.constructor);
```

#### 原型链
说明：方法和属性按究竟原则查找
```javascript
function Star(uname, age) {
    this.uname = uname;
    this.age = age;
}
Star.prototype.sing = function() {
    console.log('我会唱歌');
}
var ldh = new Star('刘德华', 18);
// 1. 只要是对象就有__proto__ 原型, 指向原型对象
console.log(Star.prototype);
console.log(Star.prototype.__proto__ === Object.prototype);
// 2.我们Star原型对象里面的__proto__原型指向的是 Object.prototype
console.log(Object.prototype.__proto__);
// 3. 我们Object.prototype原型对象里面的__proto__原型  指向为 null
```

#### 原型对象中this指向
指向原则：除特殊情况外，一般谁调用方法，this就指向谁
特殊情况：
- 类中构造函数constructor里，this指向实例对象
- apply,bind,call函数可以改变this指向
```javascript
function Star(uname, age) {
    this.uname = uname;
    this.age = age;
}
var that;
Star.prototype.sing = function() {
    console.log('我会唱歌');
    that = this;
}
var ldh = new Star('刘德华', 18);
// 1. 在构造函数中,里面this指向的是对象实例 ldh
ldh.sing();//谁调用，指向谁
console.log(that === ldh);

// 2.原型对象函数里面的this 指向的是 实例对象 ldh
```

#### 扩展内置对象
说明：可以通过原型对象，对原来的内置对象进行扩展自定义的方法。比如给数组增加自定义求偶数和的功能。

注意：数组和字符串内置对象不能给原型对象覆盖操作 Array.prototype = {} ，只能是 Array.prototype.xxx = function(){} 的方式。
```javascript
// 原型对象的应用 扩展内置对象方法

Array.prototype.sum = function() {
    var sum = 0;
    for (var i = 0; i < this.length; i++) {
        sum += this[i];
    }
    return sum;
};
// Array.prototype = {
//     sum: function() {
//         var sum = 0;
//         for (var i = 0; i < this.length; i++) {
//             sum += this[i];
//         }
//         return sum;
//     }

// }
var arr = [1, 2, 3];
console.log(arr.sum());
console.log(Array.prototype);
var arr1 = new Array(11, 22, 33);
console.log(arr1.sum());
```

#### call
说明：调用这个函数, 并且修改函数运行时的 this 指向   
`fun.call(thisArg, arg1, arg2, ...)`
- thisArg ：当前调用函数 this 的指向对象
- arg1，arg2：传递的其他参数

```javascript
// call 方法
function fn(x, y) {
    console.log('我想喝手磨咖啡');
    console.log(this);
    console.log(x + y);
}
var o = {
    name: 'andy'
};
// fn();
// 1. call() 可以调用函数
// fn.call();
// 2. call() 可以改变这个函数的this指向 此时这个函数的this 就指向了o这个对象
fn.call(o, 1, 2);
```

#### 继承
ES6之前并没有给我们提供 extends 继承（ps：现在有了）。我们可以通过**构造函数+原型对象**模拟实现继承，被称为组合继承。
- 构造函数：继承属性
- 原型对象：继承方法；
    + 子函数的`prototype`被**父函数的实例**赋值
    + 子函数的`prototype`的`constructor`再被子函数赋值（主要是将`constructor`指回来）
备注：直接采用`extends`+`super()`继承更好

```javascript
// 借用父构造函数继承属性
// 1. 父构造函数
function Father(uname, age) {
    // this 指向父构造函数的对象实例
    this.uname = uname;
    this.age = age;
}
// 2 .子构造函数 
function Son(uname, age, score) {
    // this 指向子构造函数的对象实例
    // 重点，这里就算继承属性了；当然可以采用声明`extend Father`配合构造函数中`super()`继承属性
    Father.call(this, uname, age);
    this.score = score;
}
var son = new Son('刘德华', 18, 100);
console.log(son);
```
例2
```javascript
// 借用父构造函数继承属性
// 1. 父构造函数
function Father(uname, age) {
    // this 指向父构造函数的对象实例
    this.uname = uname;
    this.age = age;
}
Father.prototype.money = function() {
    console.log(100000);

};
// 2 .子构造函数 
function Son(uname, age, score) {
    // this 指向子构造函数的对象实例
    Father.call(this, uname, age);
    this.score = score;
}
// Son.prototype = Father.prototype;  这样直接赋值会有问题,如果修改了子原型对象,父原型对象也会跟着一起变化
Son.prototype = new Father();       // 重点
// 如果利用对象的形式修改了原型对象,别忘了利用constructor 指回原来的构造函数
Son.prototype.constructor = Son;    // 重点
// 这个是子构造函数专门的方法
Son.prototype.exam = function() {
    console.log('孩子要考试');

}
var son = new Son('刘德华', 18, 100);
console.log(son);
console.log(Father.prototype);
console.log(Son.prototype.constructor);
```

#### 类的本质

1. class本质还是function.
2. 类的所有方法都定义在类的prototype属性上
3. 类创建的实例,里面也有__proto__ 指向类的prototype原型对象
4. 所以ES6的类它的绝大部分功能，ES5都可以做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。
5. 所以ES6的类其实就是语法糖.
6. 语法糖:语法糖就是一种便捷写法.   简单理解, 有两种方法可以实现同样的功能, 但是一种写法更加清晰、方便,那么这个方法就是语法糖
```javascript
// ES6 之前通过 构造函数+ 原型实现面向对象 编程
// (1) 构造函数有原型对象prototype 
// (2) 构造函数原型对象prototype 里面有constructor 指向构造函数本身
// (3) 构造函数可以通过原型对象添加方法
// (4) 构造函数创建的实例对象有__proto__ 原型指向 构造函数的原型对象
// ES6 通过 类 实现面向对象编程 
class Star {

}
console.log(typeof Star);
// 1. 类的本质其实还是一个函数 我们也可以简单的认为 类就是 构造函数的另外一种写法
// (1) 类有原型对象prototype 
console.log(Star.prototype);
// (2) 类原型对象prototype 里面有constructor 指向类本身
console.log(Star.prototype.constructor);
// (3)类可以通过原型对象添加方法
Star.prototype.sing = function() {
    console.log('冰雨');

}
var ldh = new Star();
console.dir(ldh);
// (4) 类创建的实例对象有__proto__ 原型指向 类的原型对象
console.log(ldh.__proto__ === Star.prototype);
i = i + 1;
i++
```

### ES5 新增方法

#### 数组     

##### forEach

```javascript
// forEach 迭代(遍历) 数组
var arr = [1, 2, 3];
var sum = 0;
arr.forEach(function(value, index, array) {
    console.log('每个数组元素' + value);
    console.log('每个数组元素的索引号' + index);
    console.log('数组本身' + array);
    sum += value;
})
console.log(sum);
```

##### filter
- filter() 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素,主要用于筛选数组
    注意它直接返回一个新数组
- currentValue: 数组当前项的值
- index：数组当前项的索引
- arr：数组对象本身

```javascript
// filter 筛选数组
var arr = [12, 66, 4, 88, 3, 7];
var newArr = arr.filter(function(value, index) {
    // return value >= 20;
    return value % 2 === 0;
});
console.log(newArr);
```

##### some
- some() 方法用于检测数组中的元素是否满足指定条件.   通俗点 查找数组中是否有满足条件的元素 
- 注意它返回值是布尔值, 如果查找到这个元素, 就返回true ,  如果查找不到就返回false.
- 如果找到第一个满足条件的元素,则终止循环. 不在继续查找.
- currentValue: 数组当前项的值
- index：数组当前项的索引
- arr：数组对象本身

```javascript
// some 查找数组中是否有满足条件的元素 
// var arr = [10, 30, 4];
// var flag = arr.some(function(value) {
//     // return value >= 20;
//     return value < 3;
// });
// console.log(flag);
var arr1 = ['red', 'pink', 'blue'];
var flag1 = arr1.some(function(value) {
    return value == 'pink';
});
console.log(flag1);
// 1. filter 也是查找满足条件的元素 返回的是一个数组 而且是把所有满足条件的元素返回回来
// 2. some 也是查找满足条件的元素是否存在  返回的是一个布尔值 如果查找到第一个满足条件的元素就终止循环
```

##### map
说明：对每个元素进行操作，返回新数组
```javascript
var numbers = [4, 9, 16, 25];
// 操作每个元素
var res = numbers.map(Math.sqrt);
console.log(res)
```
##### every
说明：与some类似，但是这个是全部元素是否满足
```javascript
var numbers = [4, 9, 16, 25];

var res = numbers.every(function (val) {
    return val > 0;
});
console.log(res)
```

##### forEach,filter,some区别
说明：前两者遇到`return`不会终止迭代，some则会终止

```javascript
var arr = ['red', 'green', 'blue', 'pink'];
// 1. forEach迭代 遍历
// arr.forEach(function(value) {
//     if (value == 'green') {
//         console.log('找到了该元素');
//         return true; // 在forEach 里面 return 不会终止迭代
//     }
//     console.log(11);

// })
// 如果查询数组中唯一的元素, 用some方法更合适,
arr.some(function(value) {
    if (value == 'green') {
        console.log('找到了该元素');
        return true; //  在some 里面 遇到 return true 就是终止遍历 迭代效率更高
    }
    console.log(11);

});
// arr.filter(function(value) {
//     if (value == 'green') {
//         console.log('找到了该元素');
//         return true; //  // filter 里面 return 不会终止迭代
//     }
//     console.log(11);

// });
```

#### 字符串

##### trim
```javascript
// trim 方法去除字符串两侧空格
var str = '   an  dy   ';
console.log(str);
var str1 = str.trim();
console.log(str1);
var input = document.querySelector('input');
var btn = document.querySelector('button');
var div = document.querySelector('div');
btn.onclick = function() {
    var str = input.value.trim();
    if (str === '') {
        alert('请输入内容');
    } else {
        console.log(str);
        console.log(str.length);
        div.innerHTML = str;
    }
}
```

#### 对象方法

##### Object.keys() 
说明：用于获取对象自身所有的属性名
- 效果类似 for…in
- 返回一个由属性名组成的数组

```javascript
// 用于获取对象自身所有的属性
var obj = {
    id: 1,
    pname: '小米',
    price: 1999,
    num: 2000
};
var arr = Object.keys(obj);
console.log(arr);
arr.forEach(function(value) {
    console.log(value);

})
```


##### Object.defineProperty() 
说明：定义对象中新属性或修改原有的属性。
用法：`Object.defineProperty(obj, prop, descriptor)`
- obj：必需。目标对象 
- prop：必需。需定义或修改的属性的名字
- descriptor：必需。目标属性所拥有的特性
    Object.defineProperty()   第三个参数 descriptor 说明： 以对象形式 { } 书写
     + value: 设置属性的值  默认为undefined
     + writable: 值是否可以重写。true | false  默认为false
     + enumerable: 目标属性是否可以被枚举。true | false 默认为 false
     + configurable: 目标属性是否可以被删除或是否可以再次修改特性 true | false  默认为false

```javascript
// Object.defineProperty() 定义新属性或修改原有的属性
var obj = {
    id: 1,
    pname: '小米',
    price: 1999
};
// 1. 以前的对象添加和修改属性的方式
// obj.num = 1000;
// obj.price = 99;
// console.log(obj);
// 2. Object.defineProperty() 定义新属性或修改原有的属性
Object.defineProperty(obj, 'num', {
    value: 1000,
    enumerable: true
});
console.log(obj);
Object.defineProperty(obj, 'price', {
    value: 9.9
});
console.log(obj);
Object.defineProperty(obj, 'id', {
    // 如果值为false 不允许修改这个属性值 默认值也是false
    writable: false,
});
obj.id = 2;
console.log(obj);
Object.defineProperty(obj, 'address', {
    value: '中国山东蓝翔技校xx单元',
    // 如果只为false 不允许修改这个属性值 默认值也是false
    writable: false,
    // enumerable 如果值为false 则不允许遍历, 默认的值是 false
    enumerable: false,
    // configurable 如果为false 则不允许删除这个属性 不允许在修改第三个参数里面的特性 默认为false
    configurable: false
});
console.log(obj);
console.log(Object.keys(obj));
delete obj.address;
console.log(obj);
delete obj.pname;
console.log(obj);
Object.defineProperty(obj, 'address', {
    value: '中国山东蓝翔技校xx单元',
    // 如果值为false 不允许修改这个属性值 默认值也是false
    writable: true,
    // enumerable 如果值为false 则不允许遍历, 默认的值是 false
    enumerable: true,
    // configurable 如果为false 则不允许删除这个属性 默认为false
    configurable: true
});
console.log(obj.address);
```



### 严格模式
严格模式对正常的 JavaScript 语义做了一些更改： 
- 消除了 Javascript 语法的一些不合理、不严谨之处，减少了一些怪异行为。
- 消除代码运行的一些不安全之处，保证代码运行的安全。
- 提高编译器效率，增加运行速度。
- 禁用了在 ECMAScript 的未来版本中可能会定义的一些语法，为未来新版本的 Javascript 做好铺垫。比如一些保留字如：class, enum, export, extends, import, super 不能做变量名

语法：`为整个脚本文件开启严格模式，需要在所有语句之前放一个特定语句“use strict”;（或‘use strict’;）。`

```javascript
　　"use strict";
　　console.log("这是严格模式。");

  (function (){
　　　　"use strict";
       var num = 10;
　　　　function fn() {}
　  })();

function fn(){
　　"use strict";
　　return "这是严格模式。";
}

```

#### 严格模式下 this 指向问题
- 以前在全局作用域函数中的 this 指向 window 对象。
- 严格模式下全局作用域中函数中的 this 是 undefined。
- 以前构造函数时不加 new也可以 调用,当普通函数，this 指向全局对象
- 严格模式下,如果 构造函数不加new调用, this 指向的是undefined 如果给他赋值则 会报错
- new 实例化的构造函数指向创建的对象实例。
- 定时器 this 还是指向 window 。
- 事件、对象还是指向调用者。

```javascript
'use strict';
    // 1. 我们的变量名必须先声明再使用
    // num = 10;
    // console.log(num);
    var num = 10;
    console.log(num);
    // 2.我们不能随意删除已经声明好的变量
    // delete num;
    // 3. 严格模式下全局作用域中函数中的 this 是 undefined。
    // function fn() {
    //     console.log(this); // undefined。

    // }
    // fn();
    // 4. 严格模式下,如果 构造函数不加new调用, this 指向的是undefined 如果给他赋值则 会报错.
    // function Star() {
    //     this.sex = '男';
    // }
    // // Star();
    // var ldh = new Star();
    // console.log(ldh.sex);
    // 5. 定时器 this 还是指向 window 
    // setTimeout(function() {
    //     console.log(this);

    // }, 2000);
    // a = 1;
    // a = 2;
    // 6. 严格模式下函数里面的参数不允许有重名
    // function fn(a, a) {
    //     console.log(a + a);

    // };
    // fn(1, 2);
    function fn() {}
```

#### 函数变化
函数不能有重名的参数。
函数必须声明在顶层.新版本的 JavaScript 会引入“块级作用域”（ ES6 中已引入）。为了与新版本接轨，不允许在非函数的代码块内声明函数。 

更多严格模式要求参考：<https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode>

### 高阶函数
说明：高阶函数是对其他函数进行操作的函数，它接收函数作为参数或将函数作为返回值输出。

```javascript
function fn(callback){
  callback&&callback();
}
fn(function(){alert('hi')})

//或者
function fn(){
    return function() {}
}
 fn();

```

### 闭包
变量根据作用域的不同分为两种：全局变量和局部变量。
1. 函数内部可以使用全局变量。
2. 函数外部不可以使用局部变量。
3. 当函数执行完毕，本作用域内的局部变量会销毁。

概念：闭包（closure）指有权访问另一个函数作用域中变量的函数。  -----  JavaScript 高级程序设计
     简单理解就是 ，一个作用域可以访问另外一个函数内部的局部变量。
闭包产生条件：
    - 另一个函数中必须有局部变量
    - 并且内部的函数引用了外面函数（即另一个函数）的局部变量
作用：延伸了变量作用范围
说明：闭包中的this指向闭包调用者，如果没有调用者，则指向window；
     如果对象A的方法fn()里return一个闭包函数，那么`A.fn()`并不是调用哦，而只是返回一个闭包；`(A.fn())()`执行闭包，调用者为window
```javascript
function fn1(){    // fn1 就是闭包函数
　　　　var num = 10;
　　　　function fn2(){
　　　　　　console.log(num); // 10
　　　　}
       fn2()
}
fn1();

```
应用
```javascript
// 闭包应用-点击li输出当前li的索引号
// 1. 我们可以利用动态添加属性的方式
var lis = document.querySelector('.nav').querySelectorAll('li');
for (var i = 0; i < lis.length; i++) {
    lis[i].index = i;                   // 骚操作
    lis[i].onclick = function() {
        // console.log(i);
        console.log(this.index);

    }
}
// 2. 利用闭包的方式得到当前小li 的索引号
for (var i = 0; i < lis.length; i++) {
    // 利用for循环创建了4个立即执行函数
    // 立即执行函数也成为小闭包因为立即执行函数里面的任何一个函数都可以使用它的i这变量
    (function(i) {
        // console.log(i);
        lis[i].onclick = function() {
            console.log(i);
        }
    })(i);                              // 骚操作
}
```

应用2
```javascript
// 闭包应用-3秒钟之后,打印所有li元素的内容
var lis = document.querySelector('.nav').querySelectorAll('li');
for (var i = 0; i < lis.length; i++) {
    (function(i) {
        setTimeout(function() {
            console.log(lis[i].innerHTML);
        }, 3000)
    })(i);
}
```

#### 闭包思考题
```javascript
// 思考题 1：

var name = "The Window";
var object = {
    name: "My Object",
    getNameFunc: function() {
        return function() {
            return this.name;
        };
    }
};

console.log(object.getNameFunc()()); // 闭包中的this指向闭包调用者，如果没有调用者，则指向window；
var f = object.getNameFunc();
// 类似于
var f = function() {
    return this.name;
}
f();

// 思考题 2：

// var name = "The Window";　　
// var object = {　　　　
//     name: "My Object",
//     getNameFunc: function() {
//         var that = this;
//         return function() {
//             return that.name;
//         };
//     }
// };
// console.log(object.getNameFunc()())
```

### 递归
说明：如果一个函数在内部可以调用其本身，那么这个函数就是递归函数。

```javascript
var data = [{
    id: 1,
    name: '家电',
    goods: [{
        id: 11,
        gname: '冰箱',
        goods: [{
            id: 111,
            gname: '海尔'
        }, {
            id: 112,
            gname: '美的'
        }, ]
    }, {
        id: 12,
        gname: '洗衣机'
    }]
}, {
    id: 2,
    name: '服饰'
}];
// 我们想要做输入id号,就可以返回的数据对象
// 1. 利用 forEach 去遍历里面的每一个对象
function getID(json, id) {
    var o = {};
    json.forEach(function(item) {
        // console.log(item); // 2个数组元素
        if (item.id == id) {
            // console.log(item);
            o = item;
            // 2. 我们想要得里层的数据 11 12 可以利用递归函数
            // 里面应该有goods这个数组并且数组的长度不为 0 
        } else if (item.goods && item.goods.length > 0) {
            o = getID(item.goods, id);
        }

    });
    return o;
}
console.log(getID(data, 1));
console.log(getID(data, 2));
console.log(getID(data, 11));
console.log(getID(data, 12));
console.log(getID(data, 111));
```

### 浅拷贝和深拷贝
- 直接采用`=`，标量（str,int,bool）直接拷贝值（即真拷贝），矢量（数组，对象）只拷贝了地址，相当于0层拷贝
- 浅拷贝只是拷贝一层, 更深层次对象级别的只拷贝引用
- 深拷贝拷贝多层, 每一级别的数据都会拷贝
- Object.assign(target, ...sources)    es6 新增方法可以浅拷贝，拷贝1层

```javascript
// 浅拷贝只是拷贝一层, 更深层次对象级别的只拷贝引用.
// 深拷贝拷贝多层, 每一级别的数据都会拷贝.
var obj = {
    id: 1,
    name: 'andy',
    msg: {
        age: 18
    }
};
var o = {};
// for (var k in obj) {
//     // k 是属性名   obj[k] 属性值
//     o[k] = obj[k];
// }
// console.log(o);
// o.msg.age = 20;
// console.log(obj);

console.log('--------------');
Object.assign(o, obj);
console.log(o);
o.msg.age = 20;
console.log(obj);
```

```javascript
// 深拷贝拷贝多层, 每一级别的数据都会拷贝.
var obj = {
    id: 1,
    name: 'andy',
    msg: {
        age: 18
    },
    color: ['pink', 'red']
};
var o = {};
// 封装函数 
function deepCopy(newobj, oldobj) {
    for (var k in oldobj) {
        // 判断我们的属性值属于那种数据类型
        // 1. 获取属性值  oldobj[k]
        var item = oldobj[k];
        // 2. 判断这个值是否是数组
        if (item instanceof Array) {
            newobj[k] = [];
            deepCopy(newobj[k], item)
        } else if (item instanceof Object) {
            // 3. 判断这个值是否是对象
            newobj[k] = {};
            deepCopy(newobj[k], item)
        } else {
            // 4. 属于简单数据类型
            newobj[k] = item;
        }

    }
}
deepCopy(o, obj);
console.log(o);

var arr = [];
console.log(arr instanceof Object);
o.msg.age = 20;
console.log(obj);
```

### 正则表达式

#### 创建正则表达式
1. 通过调用 RegExp 对象的构造函数创建
`var 变量名 = new RegExp(/表达式/); `
2. 通过字面量创建
`var 变量名 = /表达式/; `

```javascript
// 正则表达式在js中的使用

// 1. 利用 RegExp对象来创建 正则表达式
var regexp = new RegExp(/123/);
console.log(regexp);

// 2. 利用字面量创建 正则表达式
var rg = /123/;
// 3.test 方法用来检测字符串是否符合正则表达式要求的规范
console.log(rg.test(123));
console.log(rg.test('abc'));
```

#### 正则表达式替换
`stringObject.replace(regexp/substr,replacement)`
- 第一个参数:   被替换的字符串 或者  正则表达式
- 第二个参数:   替换为的字符串
- 返回值是一个替换完毕的新字符串

```javascript
var str = "abc123cd";
var res = str.replace(/\d+/, 'C')
console.log(res)
```
