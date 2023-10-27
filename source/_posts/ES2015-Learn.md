---
title: ES2015-Learn
date: 2023-10-25 09:52:26
tags:
---

# [ES2015(ES6)](https://babeljs.io/docs/learn)



## ECMAScript 2015 Features

### Arrows and Lexical This

箭头函数是一个方法的缩写,使用`=>`这个符号.他跟C#,Java8等语言中的箭头函数特征是相似的.

跟`function`不同,箭头函数与其周围的代码共享同一个`this`.如果箭头函数是在另一个函数内部,他将共享其父函数的`arguments`变量.

```javascript
// Expression bodies
var odds = evens.map(v => v + 1);
var num = evens.map((v, i) => v + i);

// Statement bodies
nums.foreach(v => {
  if(v % 5 === 0)
    fives.push(v);
});

// Lexical this
var bob = {
  _name: "Bob",
  _friends: [],
  printFriends(){
    this._friends.foreach(f => 
    	console.log(this._name + " knows " + f));
  }
}

// Lexical arguments
function square(){
  let example = () => {
    let numbers = [];
    for(let number of arguments){
      numbers.push(number * number);
    }
    return numbers;
  }
  return example();
}
square(2, 4, 7.5, 8, 11.5, 21); // returns: [4, 16, 56.25, 64, 132.25, 441]
```

### Classes

ES2015的`class`是基于`prototype-based OO`模式上的语法糖,`class`支持`prototype-based`的`inheritance`,`super calls`,`instance`和`static methods`以及`constructors`.

```javascript
class SkinnedMesh extends THREE.Mesh {
  constructor(geometry, materials) {
    super(geometry, materials);

    this.idMatrix = SkinnedMesh.defaultMatrix();
    this.bones = [];
    this.boneMatrices = [];
    //...
  }
  update(camera) {
    //...
    super.update();
  }
  static defaultMatrix() {
    return new THREE.Matrix4();
  }
}
```

### 模板字符串

```JavaScript
// Basic literal string creation
`This is a pretty little template string.`

// Multiline strings
`In ES5 this is
 not legal.`

// Interpolate variable bindings
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`

// Unescaped template strings
String.raw`In ES5 "\n" is a line-feed.`

// Construct an HTTP request prefix is used to interpret the replacements and construction
GET`http://foo.org/bar?a=${a}&b=${b}
    Content-Type: application/json
    X-Credentials: ${credentials}
    { "foo": ${foo},
      "bar": ${bar}}`(myOnReadyStateChangeHandler);
```

### 解构赋值(Destructuring)

解构支持`arrays`和`objects`.

解构是`fail-soft`,类似于标准的对象查找`foo["bar"]`,当找不到时会返回一个`undefined`.

```JavaScript
// list matching
var[a, b] = [1, 2, 3];	// a=1, b=2
// object matching
var {op: a, lhs: {op: b}, rhs: c} = getASTNode();	// 赋值新名字op:a,a就是新名字(变量)
// object matching shorthand 
// binds `op`, `lhs` and `rhs` in scope
var {op, lhs, rhs} = getASTNode()
// Can be used in parameter position
function g({name: x}) {
  console.log(x);
}
g({name: 5})

// Fail-soft destructuring
var [a] = [];
a === undefined;

// Fail-soft destructuring with defaults
var [a = 1] = [];
a === 1;

// Destructuring + defaults arguments
function r({x, y, w = 10, h = 10}) {
  return x + y + w + h;
}
r({x:1, y:2}) === 23
```

### 参数默认值,剩余参数,展开数组

```javascript
function f(x, y = 12){
  // 如果没传第二个参数(或者传入的是undefined)y是12
  return x + y;
}
f(3)	// 15

function f(x, ...y){	// 剩余参数,只用用在形参的最后一位,并且只能使用一次
  // y是一个数组
  return x * y.length;
}
f(3, "hello", true) // 6

function f(x, y, z){
  return x+y+z;
}
// 将数组的每个元素作为参数传递
f(...[1,2,3])	// 6
```

### 对象字面量

**对象字面量是一种可以方便地按指定的规格创建新对象的表述法**,在js中创建一个自定义对象有两种方法,一种是使用new,另一种是使用对象字面量形式.

```js
var dynamicVar="dyna";
var person={
  [dynamicVar]:'test'
}
console.log(person[dynamicVar])// 跟person["dyna"]是等价的
```

```js
var dynamicVar="dyna";
var person={
  dynamicVar, //这是一个语法糖，js引擎会解释为dynamicVar:'dyna'
  age:15
}
console.log(person.dynamicVar)
```

```rust
const bar = "2"
const obj = {
    foo: 1,
    bar,
    fn() {
        console.log("fnfnfnfnfnfn")
    }
}
obj[1+1] = 3	// 给obj创建变量为2,值为3
obj.fn()
```

### 对象的扩展方法

`Object.assign()`静态方法将一个或者多个*源对象*中所有[可枚举](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/propertyIsEnumerable)的[自有属性](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwn)复制到*目标对象*，并返回修改后的目标对象。

```js
const obj1 = {
    a:1,
    b:2
}
const obj2 = {
    a:3,
    c:4
}
const obj = Object.assign(obj1, obj2)
console.log(obj)//{ a: 3, b: 2, c: 4 }
console.log(obj === obj1)//true
```

### [Proxy](https://juejin.cn/post/6975858843729264653)

Proxy是一个构造函数,接收两个参数:`target`和`handler`,两个参数必须是`object`类型.

`proxy`是代理的意思,表示由它来'代理'某些操作,网上还有另一种理解:

> 可以将`proxy`理解成"拦截",在目标对象之前架设一层"拦截",当外界对该对象的访问,都必须先通过这层拦截,正因为有了一种拦截机制,当外界访问时我们可以进行一些操作(过滤或改写)

在学习`proxy`之前我们先来回顾一下JavaScript是怎么操作对象的

#### JavaScript如何操作对象

```js
// 通过字面量方式创建对象
let obj = {
  name: 'luce',
  showName(){
    console.log(`my name is ${this.name}`);
  }
}
```

1. 获取对象属性

```javascript
console.log(obj.name);	// luce
```

2. 给对象添加属性

```js
obj.age = 17;	
```

3. 判断属性是否在对象中

```js
console.log('age' in obj);	// true
```

4. 删除对象属性

```js
delete obj.age
```

5. 通过各种方法遍历对象的所有属性

```js
console.log(Object.getOwnPropertyNames(obj));//["name", "showName"]
console.log(Object.getOwnPropertySymbols(obj));//[]
console.log(Object.keys(obj))//["name", "showName"]
for (let key in obj){
	console.log(key)
}//分别打印name showName

```

6. 获取对象的某个属性的描述对象

```js
let d = Object.getOwnPropertyDescriptor(obj,'name')
console.log(d)
//{value: "alice", writable: true, enumerable: true, configurable: true}

```

7. 使用Object身上的方法,为某个对象添加一个或多个属性

```js
Object.defineProperty(obj,'age',{			
	value:12,
	writable:true,
	enumerable:true,
	configurable:true
})
Object.defineProperties(obj,{
	showAge:{
		value:function(){console.log(`我今年${this.age}岁了`)},
		writable:true,
		enumerable:true,
		configurable:true,
	},
	showInfo:{
		value:function(){console.log(`我叫${this.name},我今年${this.age}岁了`)},
		writable:true,
		enumerable:true,
		configurable:true,
	}	
})

```

8. 获取一个对象的原型对象

```js
Object.getPrototypeOf(obj)		
console.log(Object.getPrototypeOf(obj) === obj.__proto__)//true
```

9. 设置某个对象的原型属性对象

```js
Object.setPrototypeOf(obj,null);
//表示设置对象的原型为null，也可以传入其他对象作为其原型
```

10. 让一个对象变得不可扩展，即不能添加新的属性

```js
Object.preventExtensions(obj)
```

11. 查看一个对象是不是可扩展的

    ```js
    console.log(Object.isExtensible(obj));//false，因为上面设置了该对象为不可扩展对象
    
    ```

12. 如果对象为function类型,可以执行`()`,`.call()`,`.apply()`

    ```js
    function fn(...args){
    	console.log(this,args) 
    }
    fn(1,2,3);
    fn.call(obj,1,2,3);
    fn.apply(obj,[1,2,3]);
    
    ```

13. 一切皆是对象.如果对象作为构造函数,则该对象可以用new生成出新的对象

    ```js
    function Person(){}
    let p1 = new Person();
    
    ```

再回到`Proxy`中来,在我们`new Proxy(target, handler)`中,在`handler`中可以写一些设置拦截的内容,当对象执行某个操作时,就会触发`handler`中定义的东西,而这些东西的本质是一个个函数.

下边随便写几个感受一下:

```js
var person = {
  name: "Alice"
};
var proxy = new Proxy(person, {
  get: function(target, propKey) {
    if (propKey in target) {
      return target[propKey];
    } else {
      throw new ReferenceError(`Prop name ${propKey} does not exist.`);
    }
  }
});
proxy.name // "Alice"
proxy.age // 抛出错误:Uncaught ReferenceError: Prop name age does not exist.

```

```js
var person = {
    name: "Alice"
};
var proxy = new Proxy(person, {
    ownKeys(target) {
        return Object.getOwnPropertyNames(target)//为了省事
    }
});
console.log(Object.getOwnPropertyNames(proxy))

```

```js
function f (x,y){ return x + y}
let proxy = new Proxy(f,{
    apply(target, object, args){
        console.log(`调用了f`);
        return f.call(object,...args)
    }
})
console.log(proxy(1,2));

```

几乎上面提到的对对象的操作都有相应的方法.

| handle                   | 触发条件                                                     |
| ------------------------ | ------------------------------------------------------------ |
| get                      | 读取某个属性                                                 |
| set                      | 写入某个属性                                                 |
| has                      | in 操作符                                                    |
| deleteProperty           | delete 操作符                                                |
| getProperty              | Object.getPropertypeOf()                                     |
| setProperty              | Object.setPrototypeOf()                                      |
| isExtensible             | Object.isExtensible()                                        |
| preventExtensions        | Object.preventExtensions()                                   |
| getOwnPropertyDescriptor | Object.getOwnPropertyDescriptor()                            |
| defineProperty           | Object.defineProperty()                                      |
| ownKeys                  | Object.keys() 、Object.getOwnPropertyNames()、Object.getOwnPropertySymbols() |
| apply                    | 调用一个函数                                                 |
| construct                | 用 new 调用一个函数                                          |

### Set数据结构

`Set`对象是值的合集,集合(set)中的元素只会出现一次,即集合中的元素是**唯一**的.

`set`有个常见的场景是用来数组去重

```js
const arr = [1,2,3,1,3,2]
const result = Array.from(new Set(arr))
// const result = [...new Set(arr)]
console.log(result)
```

















