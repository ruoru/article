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

