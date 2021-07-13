# JS
## 1.解释一下原型链（重点）
答：实例对象有__proto__,指向了其构造函数的prototype原型,__proto__ 将对象和原型连接起来组成了原型链

加分项：instanceof原理、原型继承、寄生组合继承

[开始学习](https://github.com/mqyqingfeng/Blog/issues/2)
## 2.instanceof原理（重点）
答：循环将左侧对象的__proto__和右侧构造函数的prototype原型对比，全等就返回true，否则返回false

加分项：原型链

[开始学习](https://juejin.cn/post/6844903613584654344)
## 3.apply和call的作用及区别（重点）
答：改变函数运行时上下文并执行，参数不同，call是序列，apply是数组或类数组

加分项：bind、this、new

[开始学习](https://segmentfault.com/a/1190000018017796)
## 4.实现 add(1)(2)(3)
答：
```ts
const curry = (fn, ...args) =>
    args.length >= fn.length ?
    fn(...args) :
    (..._args) => curry(fn, ...args, ..._args);
```
加分项：

[开始学习](https://juejin.cn/post/6844904093467541517)
## 5.es5实现继承
答：
```ts
// 完美继承
function Parent(name) {
  this.name = name;
}
Parent.prototype.getName = function() { 
  return this.name; 
}
function Child(name, age) {
  Parent.call(this, name);
  this.age = age;
}
// Child.prototype = new Parent();
// Child.prototype.constructor = Child;

function create(o) {
  function F() {}
  F.prototype = o;
  return new F();
}

function prototype(child, parent) {
  var prototype = create(parent.prototype);
  prototype.constructor = child;
  child.prototype = prototype;
}

prototype(Child, Parent);

var children = new Child('lilin', 25);
```
加分项：原型继承、经典继承、组合继承、寄生组合式继承优缺点

[开始学习](https://github.com/mqyqingfeng/Blog/issues/16)
## 6.实现一个promise
答：
```ts
const PENDDING = 'PENDDING';
const FULLFILLED = 'FULLFILLED';
const REJECTED = 'REJECTED';

function MyPromise(fn) {
  const that = this;
  that.state = PENDDING;
  that.value = null;
  that.resolvedCallbacks = [];
  that.rejectedCallbacks = [];

  function resolve(value) {
    if (value instanceof MyPromise) {
      return value.then(resolve, reject);
    }

    setTimeout(() => {
      that.state = FULLFILLED;
      that.value = value;
      that.resolvedCallbacks.forEach(cb => cb(that.value));
    }, 0);
  }

  function reject(value) {
    setTimeout(() => {
      that.state = REJECTED;
      that.value = value;
      that.rejectedCallbacks.forEach(cb => cb(that.value));
    }, 0);
  }

  try {
    fn(resolve, reject);
  } catch (e) {
    reject(e);
  }
}

MyPromise.prototype.then = function(onFullfilled, onRejected) {
    const that = this;
    onFullfilled = typeof onFullfilled === 'function' ? onFullfilled : v => v;
    onRejected = typeof onRejected === 'function' ? onRejected : v => {throw v};
    if (that.state === PENDDING) {
        return (promise2 = new MyPromise((resolve, reject) => {
            this.resolvedCallbacks.push(() => {
                try {
                    const x = onFullfilled(that.value);
                    resolutionProcedure(promise2, x, resolve, reject);
                } catch (r) {
                    reject(r);
                }
            });
            this.rejectedCallbacks.push(() => {
                try {
                    const x = onRejected(that.value);
                    resolutionProcedure(promise2, x, resolve, reject);
                } catch (r) {
                    reject(r);
                }
            });
        }));
    } else if (that.state === FULLFILLED) {
        return (promise2 = new MyPromise((resolve, reject) => {
            this.resolvedCallbacks.push(() => {
                try {
                    const x = onFullfilled(that.value);
                    resolutionProcedure(promise2, x, resolve, reject);
                } catch (r) {
                    reject(r);
                }
            });
        }));
    } else if (that.state === REJECTED) {
        return (promise2 = new MyPromise((resolve, reject) => {
            this.rejectedCallbacks.push(() => {
                try {
                    const x = onRejected(that.value);
                    resolutionProcedure(promise2, x, resolve, reject);
                } catch (r) {
                    reject(r);
                }
            });
        }));
    }
}    
function resolutionProcedure(promose2, x, resolve, reject) {
    if (x === promose2) {
        return reject(new TypeError('Error'));
    }
    let called = false;
    if (x !== null && (typeof x === 'object' || typeof x === 'function')) {
        try {
            const then = x.then;
            if (typeof then === 'function') {
                then.call(
                    x,
                    y => {
                        if (called) return;
                        called = true;
                        resolutionProcedure(promose2, y, resolve, reject);
                    },
                    e => {
                        if (called) return;
                        called = true;
                        reject(e);
                    }
                )
            } else {
                resolve(x);
            }
        } catch(r) {
            if (called) return;
            called = true;
            reject(r);
        }
    } else {
        resolve(x);
    }
}
```
加分项：别报错

[开始学习](https://juejin.cn/post/6844904063570542599)
## 7.闭包的作用和原理
答：
- 作用：
  - 1.读取函数内部变量 
  - 2.保持这些变量到内存，不随上下文销毁而释放
- 原理：
  
  函数执行时会初始化执行上下文，此时上下文中会创建变量对象、作用域链、this，创建的变量对象会保存在作用域链中，函数执行时访问的自由变量会从这个作用域链中从顶到底部查找，故而不会受创建该函数的上下文销毁的影响

加分项：函数执行的整个过程、执行上下文、作用域和作用域链、变量对象

[开始学习](https://github.com/mqyqingfeng/Blog/issues/9)
## 8.0.1+0.2为什么不等于0.3
答：IEEE754标准来表示整数和浮点数值，有4种表示方式：单精确度(32位)、双精确度(64位)、延伸单精确度、延伸双精确度。ECMAScript中用的是双精度度64位方式，其中0.1二进制表示为0011，用科学计数法表示为2^-4 * 1.10011(循环)，由于精度位数有限，所以数字就被裁剪了，最终转换成十进制的数值就是0.100000000000000002,同理0.2转换成0.200000000000000002，所以0.1 + 0.2 === 0.100000000000000002 + 0.200000000000000002 === 0.300000000000000004

加分项：

[开始学习](https://github.com/mqyqingfeng/Blog/issues/155)
## 8.bind的实现
答：
```ts
Function.prototype.bind2 = function(context) {
    var self = this;
    var args = Array.prototype.slice.call(arguments, 1);
    function fBound() {
        var fBoundArgs = Array.prototype.slice.call(arguments);
        self.apply(this instanceof fBound ? this : context, args.concat(fBoundArgs));
    }
    // fBound.prototype = this.prototype; // 前者修改影响后者
    function F() {}
    F.prototype = this.prototype;
    fBound.prototype = new F();
    return fBound;
}
```
加分项：

[开始学习](https://github.com/mqyqingfeng/Blog/issues/12)
## 9.实现一个发布订阅模式
答：
```ts
class Subjects {
    constructor() {
        this.subs = [];
        this.state = 0;
    }
    addSubs(sub) {
        var isExsit = this.subs.includes(sub);
        if (isExsit) {
            return console.log('sub existed');
        }
        this.subs.push(sub);
    }
    removeSubs(sub) {
        var index = this.subs.indexOf(sub);
        if (index === -1) {
            return console.log('sub not exist');
        }
        this.subs.splice(index, 1);
    }
    notify() {
        console.log('sub update');
        this.subs.forEach(sub => sub.update(this.state));
    }
    doSomeLogic() {
        console.log('doSomeLogic');
        this.state = Math.floor(Math.random() * 10);
        this.notify();
    }
}
class Observer {
    constructor(name) {
        this.name = name;
    }
    update(state) {
        console.log(this.name + ' Recived state' + state);
    }
}
var observerA = new Observer('A');
var observerB = new Observer('B');
var subject = new Subjects();
subject.addSubs(observerA);
subject.addSubs(observerB);
subject.doSomeLogic();
subject.doSomeLogic();
subject.removeSubs(observerB);
subject.doSomeLogic();
```
加分项：

[开始学习](https://refactoringguru.cn/design-patterns/observer/typescript/example)
## 10.谈谈变量提升
答：
- 是什么：虽然变量还没有声明，但我们仍然能访问该变量声明的值，这种情况叫做变量提升。
- 为什么：是为了解决函数互相调用
- 有几种提升：变量提升和函数提升，函数提升优先于变量提升
- var、let、const区别：var声明变量会导致变量提升，let和const不会，他们声明时会创建暂时性死区（块级作用域），声明的变量不会挂到window，无法重复声明，const声明的变量无法重新赋值。

加分项：

[开始学习](https://github.com/mqyqingfeng/Blog/issues/5)
## 11.new操作符具体做了什么
答：
- 1.创建对象：创建obj，__proto__ = fn.prototype
- 2.执行函数：执行函数并保存返回值
- 3.返回函数返回值或创建的对象：执行的函数的返回值是对象则返回这个对象，否则返回创建的对象obj

加分项：

[开始学习](https://github.com/mqyqingfeng/Blog/issues/13)

## 12.什么是立即执行函数
答：
是指函数表达式被立即调用，通过双括号包裹起来就能将内部的函数识别为函数表达式来调用。
双括号直接放函数声明末尾并不会直接调用，而是将括号内的代码单独运行，除非他是用var声明的函数表达式。

优点：
- 1.私有变量保存内部状态；
- 2.模块化基石：模块模式通过将参数视为模块依赖传入，通过window.module来暴露模块对象，

加分项：模块化

[开始学习](https://segmentfault.com/a/1190000003985390)
## 13.谈下事件循环机制
答：
- 是什么：JS执行代码有两种情况，执行栈中执行同步任务，任务队列放置异步任务的结果。主线程从"任务队列"中读取事件，这个过程是循环不断的，所以整个的这种运行机制又称为Event Loop（事件循环）。
- 1.JS引擎执行同步代码(全局代码，函数，eval)，压入执行栈中执行
- 2.同步代码执行完，执行栈为空
- 3.从微队列头部取出微任务压入执行栈执行，重复这个过程，执行微任务遇到微任务则把微任务压入微队列末尾
- 4.微任务执行完毕，微队列为空，执行栈为空
- 5.从宏队列头部取出宏任务压入执行栈中执行
- 6.宏队列为空，执行栈为空
- 7.重复以上步骤

加分项：nextTick

[开始学习](https://zhuanlan.zhihu.com/p/33058983)
## 14.说下事件模型
答：

浏览器的事件模型，就是通过监听函数（listener）对事件做出反应。
如何绑定监听函数：
- 1.HTML on-
- 2.元素节点的事件属性：onclick
- 3.EventTarget.addEventListener

事件传播：一个事件发生后，会在子元素和父元素之间传播
- 1.捕获阶段：window向目标节点传播
- 2.目标阶段：target
- 3.冒泡阶段：目标节点向window传播

事件代理：子元素事件会冒泡到父元素，所以可以在父元素上设置监听函数统一处理多个子元素的事件，这种方法叫事件代理。
- 优点：节省内存；不用注销子元素事件
- 加分项：如何绑定监听函数，事件传播、事件代理

[开始学习](https://javascript.ruanyifeng.com/dom/event.html)
## 15.实现一个trim方法
答：
```ts
String.prototype.trim = function () {
    var str = this;
    str = str.replace(/^\s*/, '');
    var ws = /\s/;
    var i = str.length;
    
    while(ws.test(str.charAt(--i))) {}
    return str.slice(0, i + 1);
}
```
加分项：

[开始学习](https://www.cnblogs.com/rubylouvre/archive/2009/09/18/1568794.html)
#### 16.谈谈你对作用域的理解
答：指的是程序源码中定义变量的区域；规定了如何查找变量；Js中的作用域是静态作用域，也就是说函数定义时就生成了，分为全局作用域和函数作用域。函数执行时如果使用了自由变量，那么就会依次从函数上级作用域中查找。

加分项：作用域链、执行上下文、变量对象

[开始学习](https://github.com/mqyqingfeng/Blog/issues/6)
## 17.实现节流函数
答：
```ts
function throttle(fn, wait) {
    var timeout, context, args;
    var previous = 0;
    var later = function() {
        previous = +new Date();
        timeout = null;
        fn.apply(context, args);
    }
    var throttled = function() {
        var now = +new Date();
        var remaining = wait - (now - previous);
        context = this;
        args = arguments;
        if (remaining <= 0 || remaining > wait) {
            if (timeout) {
                clearTimeout(timeout);
                timeout = null;
            }
            previous = now;
            fn.apply(context, args);
        } else if (!timeout) {
            timeout = setTimeout(later, remaining);
        }
    }
    throttled.cancel = function() {
        clearTimeout(timeout);
        timeout = null;
        previous = 0;
    }
}
```
加分项：

[开始学习](https://github.com/mqyqingfeng/Blog/issues/26)
## 18.实现防抖函数
答：
```ts
function debounce(fn, wait, immediate) {
    var timeout, result;
    var deboundced = function() {
        var context = this;
        if (immediate) {
            var callNow = !timeout;
            timeout = setTimeout(function() {
                timeout = null;
            });
            if (callNow) result = fn.apply(context, arguments);
        } else {
            timeout = setTimeout(function() {
                fn.apply(context, arguments);
            }, wait);
        }
        return result;
    }
    deboundced.cancle = function() {
        clearTimeout(timeout);
        timeout = null;
    }
}
```
加分项：

[开始学习](https://github.com/mqyqingfeng/Blog/issues/22)
## 19.找出数组中和为sum的n个数
答：
```ts
Array.prototype.findDups = function(arr, count) {
    arr.reduce((prev, num) => {
        var index = prev.findIndex(o => o === num);
        if (index >= 0) {
            prev[index].count++;
        } else {
            prev.push({ count: index, num });
        }
      
        return prev;
    }, []).filter(item => o.count >= count).map(o => o.num);
}
```
加分项：

[开始学习](https://wizardforcel.gitbooks.io/the-art-of-programming-by-july/content/02.03.html)
## 20.适配器和外观模式的区别
答：
- 适配器：让本不能一起工作的接口能混在一起工作
通过交接文档让实习生能接收正式工部分工作
- 外观：让一个父层类实现不同接口实现的逻辑
通过TL采用更多更好的办法处理实习生和正式工的工作

加分项：

[开始学习](https://refactoringguru.cn/design-patterns)
## 21.数组去重
答：const filter = (a) => [...new Set(a)];
加分项：多写几种

[开始学习](https://github.com/mqyqingfeng/Blog/issues/27)
## 22.v8垃圾回收机制
[开始学习](https://juejin.cn/post/6844904016325902344)
