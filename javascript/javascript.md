# Javascript

[TOC]

## ECMA Script
![es发布流程](./t39-0.png)

* ES6
  
  * [ES6新特性](http://imweb.io/topic/55e330d6771670e207a16bbb)
* ES7
  1. Array.prototype.includes (Domenic Denicola, Rick Waldron)

    数组的 includes 方法有如下签名：

    Array.prototype.includes（value：任意值）： boolean
    如果传入的值在当前数组（this）中则返回 true，否则返回 false：
    ```js
    ['a', 'b', 'c'].includes('a')
    true
    ['a', 'b', 'c'].includes('d')
    false
    ```
    includes 方法与 indexOf 方法很相似——下面两个表达式是等价的：

    arr.includes(x)
    arr.indexOf(x) >= 0
    唯一的区别是 includes() 方法能找到 NaN，而 indexOf() 不行：
    ```js
    [NaN].includes(NaN)
    true
    [NaN].indexOf(NaN)
    -1
    ```
    includes 不会区分 +0 和 -0 （这也与其他 JavaScript 特性表现一致）：

    ```js
    [-0].includes(+0)
    true
    ```
    类型数组也有 includes() 方法：

    Typed Arrays will also have a method includes():
    ```js
    let tarr = Uint8Array.of(12, 5, 3);
    console.log(tarr.includes(5)); // true
    ```
    参考：https://www.w3ctech.com/topic/1614

  2. `**` is an infix operator for exponentiation:
    `x ** y` produces the same result as `Math.pow(x, y)`
    Examples:
    ```
    let squared = 3 ** 2; // 9

    let num = 3;
    num **= 2;
    console.log(num); // 9
    ```
* ES8
  
  * [ES8新增特性](https://zhuanlan.zhihu.com/p/27844393)

## 跟着9张思维导图学习Javascript
http://www.cnblogs.com/coco1s/p/3953653.html

### Function

#### Function 值的问题

```js
const sum1 = function (a, b) { return a + b; };        //undefined   
sum1                                                   //[Function: sum]  
sum1(1, 3)                                             //4  
  
const sum2 = function (a, b) {};                       //undefined  
sum2                                                   //[Function: sum2]  
  
Math.floor                                             //[Function: floor]  
typeof Math.floor                                      //'function'  
```

可以理解为Function是一个特殊的Object对象，值可以看成String的赋值方式，值就是后面的整段代码，因为其不变，所以Function的值是固定的。显示为[Function: 你的function的名字] 


### JSON 和 字典

JSON ： http://json.org/

JSON是有非常严格的定义

* 复合类型的值只能是数组或对象，不能是函数、正则表达式对象、日期对象。
* 简单类型的值只有四种：字符串、数值（必须以十进制表示）、布尔值和null（不能使用NaN, Infinity, -Infinity和undefined）。
* 字符串必须使用双引号表示，不能使用单引号。
* 对象的键名必须放在双引号里面。
* 数组或对象最后一个成员的后面，不能加逗号。

字典（与JSON格式很像）：https://github.com/airbnb/javascript   参考19.2
建议最后一个数据加上逗号，便于阅读编程。Babel会自动将最后的逗号去掉。

## 零散知识点

### 值判断问题

```javascript
function getEquals() {
  const arr = [{}, !{}, 0, !0, [], ![], true, false, undefined, !undefined, null, !null];
  const arr1 = [{}, !{}, 0, !0, [], ![], true, false, undefined, !undefined, null, !null];

  for (let i = 0; i < arr.length; i++) {
    const item1 = arr[i];
    for (let j = i; j < arr.length; j++) {
      const item2 = arr1[j];
      if (item1 == item2 && i !== j) {
        console.log(`${i} == ${j}`, item1 == item2);
      } else if (item1 != item2 && i === j) {
        console.log(`${i} != ${j}`, item1 == item2);
      }
    }
  }
}

// case ===:
// false === !{} === ![]
// true === !0 === !undefined === !null

// case !==:
// [] !== [], {} !== {}

// case ==:
// false == !{} == ![] == 0 == []
// true == !0 == !undefined == !null
// undefined == null

// case !=:
// [] !== [], {} !== {}
```


### var 与 let const的区别

* let命令

let命令用来定义变量,其定义的变量只在代码块内有效

而var定义的变量在全局范围内都有效

```js
{
  var p=2;
  let a = 1;
}
p // 2
a // ReferenceError
```

1. let命令没有变量提升

若在定义一个变量之前使用该变量则会抛出 ReferenceError 错误

而var则会将该变量视为 undefined

```js
p = 3;
let p;  // ReferenceError
console.log(a); // 输出undefined
var a = 2;
```

2. let不允许重复声明同一变量

```js
{
  var a;
  let a;    // error
}

{
  let a = 1;
  let a;  //error
}
```

* const命令

1. const用来声明一个只读的常量, 一旦声明，常量的值不可改变
声明时必须初始化,否则会报错

```js
const a = 1;
a = 5;  //TypeError: Assignment to constant variable.

const p;  //SyntaxError: Missing initializer in const declaration
```
2. const 只在声明其所在的块级作用域内有效
3. const 命令声明的常量也没有变量提升，只能在声明的位置后面使用
同样不可重复声明同一变量

注意:

对于复合类型的变量，变量名不指向数据，而是指向数据所在的地址。const命令只是保证变量名指向的地址不变，并不保证该地址的数据不变

```js
const a = [];
a.push('Java'); // 可运行 此时a为['Java']
a = ['script'];    // error
```

* 块级作用域的优点

1. 避免内层变量覆盖外层变量
2. 避免用来计数的循环变量泄露为全局变量
3. 引入了块级作用域，允许在块级作用域之中声明函数

[1]: [2017面试分享（js面试题记录）](https://segmentfault.com/a/1190000013827826?utm_source=weekly&utm_medium=email&utm_campaign=email_weekly)

## npm

### package lock 问题

1. npm 5.0.x 版本，不管 package.json 怎么变，npm install 时都会根据 package-lock.json 文件下载。这个 [issue #16866 · npm/npm](https://github.com/npm/npm/issues/16866) 控诉了这个问题，明明手动改了 package.json，为啥不给我升级包！然后就导致了5.1.0的问题。

2. 5.1.0版本后，npm install 会无视 package-lock.json 文件去下载最新的npm包，然后有人提了这个[issue #17979 · npm/npm](https://github.com/npm/npm/issues/17979) 控诉这个问题，最后演变成5.4.2版本后的规则。

3. 5.4.2版本后 [issue #17979 · npm/npm](https://github.com/npm/npm/issues/17979) 大致意思是，如果改了package.json，且 package.json 和 package-lock.json 文件不同，那么执行 npm install 时 npm 会根据 package 中的版本号以及语义含义去下载最新的包，并更新至 package-lock.json。如果两者是同一状态，那么执行 npm install 都会根据 lock 下载，不会理会 package 实际包的版本是否有新。
