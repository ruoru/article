# ECMA Script
![js发布流程](./t39-0.png)

* ES6
  * [ES6新特性](http://imweb.io/topic/55e330d6771670e207a16bbb)
* ES7
  * 新增两个特性
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

# 跟着9张思维导图学习Javascript
http://www.cnblogs.com/coco1s/p/3953653.html

## Function
### Function 值的问题
```javascript
const sum1 = function (a, b) { return a + b; };        //undefined   
sum1                                                   //[Function: sum]  
sum1(1, 3)                                             //4  
  
const sum2 = function (a, b) {};                       //undefined  
sum2                                                   //[Function: sum2]  
  
Math.floor                                             //[Function: floor]  
typeof Math.floor                                      //'function'  
```
可以理解为Function是一个特殊的Object对象，值可以看成String的赋值方式，值就是后面的整段代码，因为其不变，所以Function的值是固定的。显示为[Function: 你的function的名字] 


## JSON 和 字典
JSON ： http://json.org/
JSON是有非常严格的定义
- 复合类型的值只能是数组或对象，不能是函数、正则表达式对象、日期对象。
- 简单类型的值只有四种：字符串、数值（必须以十进制表示）、布尔值和null（不能使用NaN, Infinity, -Infinity和undefined）。
- 字符串必须使用双引号表示，不能使用单引号。
- 对象的键名必须放在双引号里面。
- 数组或对象最后一个成员的后面，不能加逗号。
  
  
字典（与JSON格式很像）：https://github.com/airbnb/javascript   参考19.2
建议最后一个数据加上逗号，便于阅读编程。Babel会自动将最后的逗号去掉。

