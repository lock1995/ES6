# ES6新语法概览

### 简介

ES6是JavaScript语言的新一代标准，加入了一些新的功能和语法，正式发布于2015年6月，亦称ES2015；该标准由ECMA（欧洲计算机制造联合会）的第39号技术专家委员会（TC39）制订，ES7正在制订中，据称会在2017年发布。



### 语法

#### 箭头函数、this

ES6中可以使用 => 作为函数表达形式，极简风格，参数＋ => ＋函数体。

```javascript
var foo = function(){return 1;};
//等价于
let foo = () => 1;
```

```javascript
let nums = [1,2,3,5,10];
let fives = [];

nums.forEach(v => {
  if (v % 5 === 0)
    fives.push(v);
});

console.log(fives); //[5,10]
```

箭头函数中的 this 指的不是window，是对象本身。

```javascript
function aa(){
  this.bb = 1;
  setTimeout(() => {
  	this.bb++; //this指向aa
    console.log(this.bb);
  },500);
}

aa(); //2
```

#### let、const

- ES6 推荐在函数中使用 let 定义变量


- const 用来声明一个常量，但也并非一成不变的
- let 和 const 只在最近的一个块中（花括号中）有效

```javascript
var a = 1;
{
  let a = 2;
  console.log(a); //2
}
console.log(a); //1
```

```javascript
const A = [1,2];
A.push = 3;
console.log(A); //[1,2,3]
A = 10; //Error
```

#### Classes

ES6中增加了类的概念，其实ES5中已经可以实现类的功能了，只不过使用Class实现可以更加清晰，更像面向对象的写法。

```javascript
class Animal {
  constructor(){
    console.log('我是一个动物');
  }
}

class Person extends Animal {
  constructor(){
    super();
    console.log('我是一个程序员');
  }
}

let aa = new Person();
//我是一个动物
//我是一个程序员

```

#### 解构

解构赋值是ES6中推出的一种高效、简洁的赋值方法

没啥说的，直接上代码:

```javascript
//通常情况下
var first = someArray[0];
var second = someArray[1];
var third = someArray[2];

//解构赋值
let [first, second, third] = someArray; //比上面简洁多了吧

//还有下面例子
let [,,third] = [1,2,3];
console.log(third); //3

let [first,...last] = [1,2,3];
console.log(last); //[2,3]

//对象解构
let {name,age} = {name: "lisi", age: "20"};
console.log(name); //lisi
console.log(age); //20

let {ept} = {};
console.log(ept); //undefined

```

#### Rest + Spread

"..."

```javascript
//Rest
function f(x, ...y) {
  return x * y.length;
}
f(3, "hello", true) == 6

//Spread
function f(x, y, z) {
  return x + y + z;
}
f(...[1,2,3]) == 6
```

#### 对象字面量扩展

- 可以在对象字面量里面定义原型
- 定义方法可以不用function关键字
- 直接调用父类方法

```javascript
//通过对象字面量创建对象
var human = {
    breathe() {
        console.log('breathing...');
    }
};
var worker = {
    __proto__: human, //设置此对象的原型为human,相当于继承human
    company: 'freelancer',
    work() {
        console.log('working...');
    }
};
human.breathe();//输出 ‘breathing...’
//调用继承来的breathe方法
worker.breathe();//输出 ‘breathing...’
```

#### 模版字符串

ES6中提供了用反引号｀来创建字符串，里面可包含${…}等

```
`This is a pretty little template string.`

`In ES5 this is
 not legal.`
 
let name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`
```

#### Iterators（迭代器）

ES6 中可以通过 Symbol.iterator 给对象设置默认的遍历器，直到状态为true退出。

```javascript
var arr = [11,12,13];
var itr = arr[Symbol.iterator]();

itr.next(); //{ value: 11, done: false }
itr.next(); //{ value: 12, done: false }
itr.next(); //{ value: 13, done: false }
itr.next(); //{ value: undefined, done: true }
```

#### Generators

ES6中非常受关注的的一个功能，能够在函数中间暂停，一次或者多次，并且之后恢复执行，在它暂停的期间允许其他代码执行，并可以用其实现异步。

Run-Stop-Run...

```javascript
function *foo(x) {
    var y = 2 * (yield (x + 1));
    var z = yield (y / 3);
    return (x + y + z);
}

var it = foo( 5 );

console.log( it.next() );       // { value:6, done:false }
console.log( it.next( 12 ) );   // { value:8, done:false }
console.log( it.next( 13 ) );   // { value:42, done:true }

```

generator能实现好多功能，如配合for...of使用，实现异步等等，我在这里就不多说了，[详见。](https://davidwalsh.name/es6-generators)

#### for…of && for…in

for…of 遍历（数组）

```javascript
let arr = [1,2,3];
for (let itr of arr) {
  console.log(itr); //1 2 3
}
```

for…in 遍历对象中的属性

```javascript
let arr = [1,2,3];
arr.aa = 'bb';
for (let itr in arr) {
  console.log(itr); //0 1 2 aa
}
```

#### Map + Set + WeakMap + WeakSet

- Set 对象是一组不重复的值，重复的值将被忽略，值类型可以是原始类型和引用类型


- WeakSet是一种弱引用，同理WeakMap

```javascript
// Sets
var s = new Set();
s.add("hello").add("goodbye").add("hello");
s.size === 2;
s.has("hello") === true;

// Maps
var m = new Map();
m.set("hello", 42);
m.set(s, 34);
m.get(s) == 34;

// Weak Maps
var wm = new WeakMap();
wm.set(s, { extra: 42 });
wm.size === undefined

// Weak Sets
var ws = new WeakSet();
ws.add({ data: 42 });

```

#### Proxies

Proxy可以监听对象身上发生了什么事情，并在这些事情发生后执行一些相应的操作。

```javascript
//定义被侦听的目标对象
var engineer = { name: 'Joe Sixpack', salary: 50 };
//定义处理程序
var interceptor = {
  set: function (receiver, property, value) {
    console.log(property, 'is changed to', value);
    receiver[property] = value;
  }
};
//创建代理以进行侦听
engineer = Proxy(engineer, interceptor);
//做一些改动来触发代理
engineer.salary = 60; 
//控制台输出：salary is changed to 60
```

#### Symbols

Symbol 是一种新的数据类型，它的值是唯一的，不可变的。ES6 中提出 symbol 的目的是为了生成一个唯一的标识符，不过你访问不到这个标识符.

```javascript
var sym = Symbol( "Symbol" );
console.log(typeof sym); // symbol
```

如果要获取对象 symbol 属性，需要使用`Object.getOwnPropertySymbols(o)`  

#### Promises

- ES6 对 Promise 有了原生的支持，一个 Promise 是一个等待被异步执行的对象，当它执行完成后，其状态会变成 resolved 或者 rejected


- Promises是处理异步操作的一种模式，之前在很多三方库中有实现，比如jQuery的 deferred 对象。当你发起一个异步请求，并绑定了.when(), .done()等事件处理程序时，其实就是在应用promise模式

```javascript
function timeout(duration = 0) {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, duration);
    })
}

var p = timeout(1000).then(() => {
    return timeout(2000);
}).then(() => {
    throw new Error("hmm");
}).catch(err => {
    return Promise.all([timeout(100), timeout(200)]);
})
```

想要了解promise实际应用等，[详见。](http://liubin.org/promises-book/)



### 小结

总之，ES6还是有很多棒棒的语法，有利于精简代码，高效开发；只不过一些低级别浏览器不支持，可以用Babel等工具把ES6转化成ES5，但是有些语法还是不够完善；但是在不久的将来，ES6一定会成为主流的。对了，还有ES7、8、9……



## REFERENCE

[Learn ES2015]: http://babeljs.cn/docs/learn-es2015/
[ES6-for-humans]: https://github.com/metagrover/ES6-for-humans
[es6features]: https://github.com/lukehoban/es6features



> by Lock


> Feel free to repost but keep the link to this page please!

