# call、apply、bind

在读vue源码的时候，发现里面大量使用了这种方法来修正this指向，有时候会将自己绕湖涂了，这里单独开一章深入了解一下

## 来源

首先，在使用call、apply、bind方法时，我们有必要知道这三个方法来自哪里：

![this](https://github.com/cwzp990/vue-code/tree/master/vue/images/this.png)

call，apply，bind这三个方法其实都是继承自Function.prototype中的，属于实例方法。普通的对象、函数、数组都继承了Function.prototype对象的三个方法，所以这三个方法都可以在对象、数组、函数中使用。

## 使用

函数实例的call方法，可以指定该函数内部this的指向（即函数执行时所在的作用域），然后在所指定的作用域中，调用该函数。并且会立即执行该函数

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