# call、apply、bind

在读vue源码的时候，发现里面大量使用了这种方法来修正this指向，有时候会将自己绕湖涂了，这里单独开一章深入了解一下

## 来源

首先，在使用call、apply、bind方法时，我们有必要知道这三个方法来自哪里：

![this](https://github.com/cwzp990/vue-code/tree/master/vue/images/this.png)

call，apply，bind这三个方法其实都是继承自Function.prototype中的，属于实例方法。普通的对象、函数、数组都继承了Function.prototype对象的三个方法，所以这三个方法都可以在对象、数组、函数中使用。

## 使用

+ 函数实例的call方法，可以指定该函数内部this的指向（即函数执行时所在的作用域），然后在所指定的作用域中，调用该函数。并且会立即执行该函数

```js

var tom = {name: 'tom'}

var name = 'jack'

function say() {
  console.log(this.name)
}

say();                  // this指向window，输出 jack
say.call();             // 不传值this依旧指向window，输出 jack
say.call(null);         // 同上
say.call(undefined);    // 同上
say.call(tom);          // this指向了tom，输出 tom

```

+ call()方法可以传递两个参数。第一个参数是指定函数内部中this的指向（也就是函数执行时所在的作用域），第二个参数是函数调用时需要传递的参数

```js

function add(a,b) {
  console.log(a+b)
}

add.call(null, 1,2)       // null表示add处于全局作用域 第二个参数必须一个个添加，而在apply里必须以数组的形式

```

+ call方法的一个应用是调用对象的原生方法。也可以用于将类数组对象转换为数组

```js

var obj = {};

console.log(obj.hasOwnProperty('toString'));        // false

obj.hasOwnProperty = function() {
  return true;
}

console.log(obj.hasOwnProperty('toString'));        // true
console.log(Object.prototype.hasOwnProperty.call(obj, 'toString'))      // false

// 伪数组--->真正的数组
console.log(Array.prototype.slice.apply({0:1,length:2}));    //[1,undefined]

```

+ bind

bind方法用于指定函数内部的this指向（执行时所在的作用域），然后返回一个新函数。bind方法并非立即执行一个函数

```js

var num = {
  a: 1,
  count: function(){
    console.log(this.a++)
  } 
}

var f = num.count;
f();              // 这里的this指向window，window.a是undefined，所以得到的是NAN

var f = num.count.bind(num)

f();              // bind修改了this指向，但没有执行，我们需要执行f()

```