### 模块化
AMD,CMD和CommonJs这个是之前存在的模块方案，其中比较有名模块加载器requirejs是AMD模式,阿里自己搞了一套叫sea.js是CMD模式的,CommonJS是服务端模块加载，比如node.js。

```
//依赖前置，jquery模块先声明，AMD
define(['jquery'], function ($) {
/***/
})

//同步加载  CommonJS
var $ = require('jquery');
/****/
module.exports = myFunc;

//CMD
define(function (require, exports, module) {
// 就近依赖
var $ = require('jquery');
/****/
})
```


现在es6有了自己的模块概念已被广泛应用。虽说现在还没有被浏览器支持，需要使用babel转换。
使用import导入，export导出。且es6的模块是静态的，即发生在编译阶段。对比的commonJS是动态加载的，即代码运行到那一行才会加载对应的模块。
我们的项目中关于模块的导入导出应用很多，随处可见。

### promise
promise可以解决回调函数多层嵌套，代码结构混乱的问题，这个概念有点类似jQuery中的deferred对象。

Promise本身是一个状态机，具有pending（等待），resolve（决议），reject（拒绝）这3个状态，当请求发送没有得到响应的时候会pending状态，并且一个Promise实例的状态只能从pending => resolve 或者从 pending => reject，即当一个Promise实例从pending状态改变后，就不会再改变了（不存在resolve => reject 或 reject => resolve）

```
promise.then((resolve, reject) => {

})
```
我们项目里面用的axios 就是全面使用的promise来实现 。

### 箭头函数
1.箭头函数没有arguments（建议使用更好的语法，剩余运算符替代）

2.箭头函数没有prototype属性，没有constructor，即不能用作与构造函数（不能用new关键字调用）


3.箭头函数没有自己this，它的this是词法的，引用的是上下文的this

这在数组一些操作上用的比较多，比如forEach、map等。

### 数组的扩展
findIndex 

find

of

from
...
### 对象的扩展
object.assign

### 解构赋值
消耗数组的迭代器，把生成对象的value属性的值赋值给对应的变量
{name, age} = person
name 
age

### 剩余/拓展运算符

```
let arr = [1,2,3]
let arr2 = [...arr, 6,7,8]

function func1(...rest){
    ...
}
func1(1,2,3);
```

### let const
let,const用于声明变量，用来替代老语法的var关键字，与var不同的是，let/const会创建一个块级作用域（通俗讲就是一个花括号内是一个新的作用域）

```
for (var i = 0;i<10, i++){
    console.log(i)
}
console.log(i);
```
把var 换成let试试

### 模板字符串

```
`hello, ${name}, nice to meet you`
``` 