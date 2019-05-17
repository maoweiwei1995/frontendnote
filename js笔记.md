

# js笔记

### 类型检测

![img](https://mmbiz.qpic.cn/mmbiz_png/12mPmHVcSunnsWOSYTn5x8M9Vm5VfdbarhuEZMTO0DzviaSJs2RicXW95H9eHzFc8UQCBSq9ulibf0tarib7WCzBMQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

##### **JavaScript 的数据类型**

**Javascript 有两种数据类型，分别是基本数据类型和引用数据类型**。**其中基本数据类型包括 Undefined、Null、Boolean、Number、String、Symbol (ES6 新增，表示独一无二的值)，而引用数据类型统称为 Object 对象，主要包括对象、数组和函数。接下来我们分别看下两者的特点。**

###### **基本数据类型**

**1.值是不可变的**

```js
var name = 'java';
name.toUpperCase(); // 输出 'JAVA'
console.log(name); // 输出  'java'
```

由此可得，基本数据类型的**值是不可改变的**

**2.存放在栈区**

原始数据类型直接存储在栈(stack)中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储。

**3.值的比较**

```
var a = 1;
var b = true;
console.log(a == b);    // true
console.log(a === b);   // false
```

== : 只进行值的比较,会进行数据类型的转换。

=== : 不仅进行值得比较，还要进行数据类型的比较。

###### **引用数据类型**

**1.值是可变的**

```js
var a={age:20}；
a.age=21；
console.log(a.age)//21
```

上面代码说明引用类型可以拥有属性和方法，并且是可以动态改变的。

**2.同时保存在栈内存和堆内存**

引用数据类型存储在堆(heap)中的对象,占据空间大、大小不固定,如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。

![img](https://mmbiz.qpic.cn/mmbiz_png/12mPmHVcSunnsWOSYTn5x8M9Vm5VfdbarrED0jzOaGibKyicE8r68gtHcD19vrYIPbzrQmQpaqV9Dxg5n4KxRFww/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**3.比较是引用的比较**

当从一个变量向另一个变量赋引用类型的值时，同样也会将存储在变量中的对象的值复制一份放到为新变量分配的空间中。

```js
var a={age:20};
var b=a;
b.age=21;
console.log(a.age == b.age)//true
```

上面我们讲到基本类型和引用类型存储于内存的位置不同，引用类型存储在堆中的对象，与此同时，在栈中存储了指针，而这个指针指向正是堆中实体的起始位置。变量 a 初始化时，a 指针指向对象{age:20}的地址，a 赋值给 b 后,b 又指向该对象{age:20}的地址，这两个变量指向了同一个对象。因此，改变其中任何一个变量，都会相互影响。

![img](https://mmbiz.qpic.cn/mmbiz_png/12mPmHVcSunnsWOSYTn5x8M9Vm5VfdbatNianVSvleALACNlWoWmXS1ibe6gDVib9VUwSiaDNsLN3ibBYxcAic9JbhBg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

此时，如果取消某一个变量对于原对象的引用，不会影响到另一个变量。

```js
var a={age:20};
var b=a;
a = 1;
b // {age:20}
```

上面代码中，a 和 b 指向同一个对象，然后 a 的值变为 1，这时不会对 b 产生影响，b 还是指向原来的那个对象。

###### **检验数据类型**

**1.typeof**

typeof 返回一个表示数据类型的字符串，返回结果包括：number、boolean、string、symbol、object、undefined、function 等 7 种数据类型，但不能判断 null、array 等

```js
typeof Symbol(); // symbol 有效
typeof ''; // string 有效
typeof 1; // number 有效
typeof true; //boolean 有效
typeof undefined; //undefined 有效
typeof new Function(); // function 有效
typeof Object // function ????
typeof null; //object 无效
typeof [] ; //object 无效
typeof new Date(); //object 无效
typeof new RegExp(); //object 无效
```

数组和对象返回的都是 object，这时就需要使用 instanceof 来判断

**2.instanceof**

instanceof 是用来判断 A 是否为 B 的实例，表达式为：A instanceof B，如果 A 是 B 的实例，则返回 true,否则返回 false。instanceof 运算符用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。

**1 . 原始类型有哪几种？null 是对象吗？原始数据类型和复杂数据类型存储有什么区别？**

- 原始类型有 6 种，分别是 undefined,null,bool,string,number,symbol(ES6 新增)。
- 虽然 typeof null 返回的值是 object, 但是 null 不是对象，而是基本数据类型的一种。
- 原始数据类型存储在栈内存，存储的是值。
- 复杂数据类型的地址存储在栈内存，指向存储在堆内存的值。当我们把对象赋值给另外一个变量的时候，复制的是地址，指向同一块内存空间，当其中一个对象改变时，另一个对象也会变化。

 **2.typeof 是否正确判断类型? instanceof 呢？ instanceof 的实现原理是什么？**

1首先 typeof 能够正确的判断基本数据类型，但是除了 null, typeof null 输出的是object。

2但是对象来说，typeof 不能正确的判断其类型， typeof 一个函数可以输出 'function', 而除此之外，输出的全是 object, 这种情况下，我们无法准确的知道对象的类型。

思路：1先typeof判断是否是基本数据类型2不是基本数据类型，在用instanceof判断。

instanceof 是通过原型链判断的，A instanceof B, 在 A 的原型链中层层查找，是否有原型等于 B.prototype，如果一直找到 A 的原型链的顶端 (null; 即`Object.prototype.__proto__`), 仍然不等于 B.prototype，那么返回 false，否则返回 true。

instanceof 的实现代码:

```js
// L instanceof R
function instance_of(L, R) {//L 表示左表达式，R 表示右表达式
    var O = R.prototype;// 取 R 的显式原型
    L = L.__proto__;    // 取 L 的隐式原型
    while (true) {
        if (L === null) // 已经找到顶层
            return false;
        if (O === L)   // 当 O 严格等于 L 时，返回 true
            return true;
        L = L.__proto__;  // 继续向上一层原型链查找
    }
}
```

```js
[] instanceof Array; //true
{} instanceof Object;//true
new Date() instanceof Date;//true
new RegExp() instanceof RegExp//true
```

关于数组的类型判断，还可以用 ES6 新增Array.isArray()

```js
Array.isArray([]);   // true
```

instanceof 三大弊端：

对于**基本数据类型**来说，字面量方式创建出来的结果和实例方式创建的是有一定的区别的

```js
console.log(1 instanceof Number)//false
console.log(new Number(1) instanceof Number)//true
```

**从严格意义上来讲，只有实例创建出来的结果才是标准的对象数据类型值**，也是标准的 Number 这个类的一个实例；对于字面量方式创建出来的结果是基本的数据类型值，不是严谨的实例，但是由于 JS 的松散特点，导致了可以使用 Number.prototype 上提供的方法。

只要在当前实例的原型链上，我们用其检测出来的结果都是 true。在类的原型继承中，我们最后检测出来的结果未必准确。

```js
var arr = [1, 2, 3];
console.log(arr instanceof Array) // true
console.log(arr instanceof Object);  // true
function fn(){}
console.log(fn instanceof Function)// true
console.log(fn instanceof Object)// true
```

**不能检测 null 和 undefined**

对于特殊的数据类型 null 和 undefined，他们的所属类是 Null 和 Undefined，但是浏览器把这两个类保护起来了，不允许我们在外面访问使用。

![img](https://mmbiz.qpic.cn/mmbiz_png/12mPmHVcSunnsWOSYTn5x8M9Vm5Vfdba4kvaibmKPQZvZhbWYbIZTJaLiaQIN1uvG5SESfqj1MSaAUCxlWTZwZQQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**3.严格运算符===**

只能用于判断 null 和 undefined，因为这两种类型的值都是唯一的。

```js
var a = null
typeof a // "object"
a === null // true
```

undefined 还可以用 typeof 来判断

```js
var b = undefined;
typeof b === "undefined" // true typeof后是一个字符串 必须加引号
b === undefined // true
```

**4.constructor**

constructor 作用和 instanceof 非常相似。但 constructor 检测 Object 与 instanceof 不一样，还可以处理基本数据类型的检测。

```js
var aa=[1,2];
console.log(aa.constructor===Array);//true
console.log(aa.constructor===RegExp);//false
console.log((1).constructor===Number);//true
var reg=/^$/;
console.log(reg.constructor===RegExp);//true
console.log(reg.constructor===Object);//false
```

constructor 两大弊端：

null 和 undefined 是无效的对象，因此是不会有 constructor 存在的，这两种类型的数据需要通过其他方式来判断。

函数的 constructor 是不稳定的，这个主要体现在把类的原型进行重写，在重写的过程中很有可能出现把之前的 constructor 给覆盖了，这样检测出来的结果就是不准确的

```js
function Fn(){}
Fn.prototype = new Array()
var f = new Fn
console.log(f.constructor)//Array
```

**5.Object.prototype.toString.call()**

Object.prototype.toString.call() 最准确最常用的方式。首先获取 Object 原型上的 toString 方法，让方法执行，让 toString 方法中的 this 指向第一个参数的值。

关于 toString 重要补充说明：

- 本意是转换为字符串，但是某些 toString 方法不仅仅是转换为字符串
- 对于 Number、String，Boolean，Array，RegExp、Date、Function 原型上的 toString 方法都是把当前的数据类型转换为字符串的类型（它们的作用仅仅是用来转换为字符串的）
- **Object 上的 toString 并不是用来转换为字符串的**。

**Object 上的 toString 它的作用是返回当前方法执行的主体（方法中的 this）所属类的详细信息即"[object Object]",**其中第一个 object 代表当前实例是对象数据类型的(这个是固定死的)，第二个 Object 代表的是 this 所属的类是 Object。

```js
Object.prototype.toString.call(Object); //[object Function]
Object.prototype.toString.call('') ;   // [object String]
Object.prototype.toString.call(1) ;    // [object Number]
Object.prototype.toString.call(true) ; // [object Boolean]
Object.prototype.toString.call(undefined) ; // [object Undefined]
Object.prototype.toString.call(null) ; // [object Null]
Object.prototype.toString.call(new Function()) ; // [object Function]
Object.prototype.toString.call(new Date()) ; // [object Date]
Object.prototype.toString.call([]) ; // [object Array]
Object.prototype.toString.call(new RegExp()) ; // [object RegExp]
Object.prototype.toString.call(new Error()) ; // [object Error]
Object.prototype.toString.call(document) ; // [object HTMLDocument]
Object.prototype.toString.call(window) ; //[object global] window是全局对象global的引用
```

### 原型

###### 首先要搞明白几个概念：

1. 函数（function）
2. 函数对象(function object)
3. 本地对象(native object)
4. 内置对象(build-in object)
5. 宿主对象(host object)

###### 函数

```js
function** foo(){

}

var foo = **function**(){

}
```

前者为函数声明，后者为函数表达式。typeof foo的结果都是function。

###### 函数对象

函数就是对象,代表函数的对象就是函数对象

> 官方定义， 在Javascript中,每一个函数实际上都是一个函数对象.JavaScript代码中定义函数，或者调用Function创建函数时，最终都会以类似这样的形式调用Function函数:var newFun = new Function(funArgs, funBody)

其实也就是说，我们定义的函数，语法上，都称为**函数对象**，看我们如何去使用。如果我们单纯的把它当成一个函数使用，那么它就是函数，如果我们通过他来实例化出对象来使用，那么它就可以当成一个函数对象来使用，在面向对象的范畴里面，函数对象类似于类的概念。

```javascript
var foo = new function(){

}
typeof foo // object
 
或者
 
function Foo (){
    
}
var foo = new Foo();
 
typeof foo // object
```



###### 本地对象

>ECMA-262 把本地对象（native object）定义为“独立于宿主环境的 ECMAScript 实现提供的对象”。简单来说，本地对象就是 ECMA-262 定义的类（引用类型）。它们包括：
>Object,Function,Array,String,Boolean,Number
>Date,RegExp,Error,EvalError,RangeError,ReferenceError,SyntaxError,TypeError,URIError.

我们不能被他们起的名字是本地对象，就把他们理解成对象（虽然是事实上，它就是一个对象，因为JS中万物皆为对象），通过

```js
typeof(Object)
typeof(Array)
typeof(Date)
typeof(RegExp)
typeof(Math)
```

返回的结果都是function,也就是说其实这些本地对象（类）是通过function建立起来的。

```js
function Object（）{
    
}
function Array（）{
    
}
```

可以看出Object原本就是一个函数，通过new Object()之后实例化后，创建对象。类似于JAVA中的类。

###### 内置对象

> ECMA-262 把内置对象（built-in object）定义为“由 ECMAScript 实现提供的、独立于宿主环境的所有对象，在 ECMAScript 程序开始执行时出现”。这意味着开发者不必明确实例化内置对象，它已被实例化了。ECMA-262 只定义了两个内置对象，即 **Global** 和 **Math** （它们也是本地对象，根据定义，每个内置对象都是本地对象）。

理清楚了这几个概念，有助于理解我们下面要讲述的原型和原型链。

###### prototype

prototype属性是每一个函数都具有的属性，但是不是一个对象都具有的属性。比如

```js
function Foo(){
    
}
 
var foo = new Foo()；
```

其中Foo中有prototype属性，而foo没有。但是foo中的隐含的`__proto__`属性指向Foo.prototype。

```js
foo.__proto__ === Foo.prototype
```

为什么会存在prototype属性？

Javascript里面所有的数据类型都是对象，为了使JavaScript实现面向对象的思想，就必须要能够实现‘继承’使所有的对象连接起来。而如何实现继承呢？JavaScript采用了类似C++，java的方式，通过new的方式来实现实例。

举个例子，child1,child2都是Mother的孩子，且是双胞胎。（虽然不是很好，但是还是很能说明问题的）

```js
function Mother(name){
    this.name = name;
    this.father = 'baba';
}
var child1 = new Mother('huahua');
var child2 = new Mother('huihui');
```

如果有一天，发现孩子的父亲其实是Baba，那么就要对孩子每一个孩子的father属性。

```js
child1.father ='Baba';
console.log(child2.father) // baba
```

也就是说修改了其中一个孩子的father属性不会影响到下一个，属性的值无法共享。

正是这个原因才提出来prototype属性，把需要共享的属性放到构造函数也就是父类的实例中去。

###### `__proto__`

__proto__属性是每一个对象以及函数都隐含的一个属性。对于每一个含有`__proto__`属性，他所指向的是创建他的构造函数的prototype。原型链就是通过这个属性构件的。

想像一下，如果一个函数对象（也成为构造函数）a的prototype是另一个函数对象b构件出的实例，a的实例就可以通过`__proto__`与b的原型链起来。而b的原型其实就是Object的实例，所以a的实例对象，就可以通过原型链和object的prototype链接起来。

```js
function a(){
    
}
function b(){
    
}
var b1 = new b();
a.prototype = b1;
var a1 = new a();
console.log(a1.__proto__===b1);//true
console.log(a1.__proto__.__proto__===b.prototype) //true
console.log(a1.__proto__.__proto__.__proto__===Object.prototype) //true
```

###### 核心

如果要理清原型和原型链的关系，首先要明确一下几个概念：

1.JS中的所有东西都是对象，**函数也是对象**, 而且是一种特殊的对象

2.JS中所有的东西都由Object衍生而来, 即所有东西**原型链的终点**指向Object.prototype

3.JS对象都有一个隐藏的`__proto__`属性，他指向创建它的构造函数的原型，但是有一个例外，Object.prototype.`__proto__`指向的是null。

4.JS中构造函数和实例(对象)之间的微妙关系

构造函数通过定义prototype来约定其实例的规格, 再通过 new 来构造出实例,他们的作用就是生产对象.

```js
function Foo(){
    
}
var foo = new Foo();
foo其实是通过Foo.prototype来生成实例的。
```

构造函数本身又是方法(Function)的实例, 因此也可以查到它的`__proto__`(原型链)

```js
function Foo(){
    
}
等价于
var Foo= new Function（）；
```

而Function实际上是

```js
function Function(){
    Native Code
}
也就是等价于
var Function= new Function()；

```

所以说Function是**<u>通过自己</u>**创建出来的。正常情况下对象的`__proto__`是指向创建它的构造函数的prototype的.所以Function的`__proto__`指向的Function.prototype

Object 实际上也是通过Function创建出来的

```js
typeof(Object)//function
所以，
function Object(){
    Native Code
}
等价于
var Object = new Function();
```

那么Object的__proto__指向的是Function.prototype，也即是

```js
Object.__proto__ === Function.prototype //true
```

下面再来看Function.prototype的__proto__指向哪里

因为JS中所有的东西都是对象，那么，Function.prototype 也是对象，既然是对象，那么Function.prototype肯定是通过Object创建出来的，所以，

```js
Function.prototype.__proto__ === Object.prototype //true
```

综上所述，Function和Object的原型以及原型链的关系可以归纳为下图。

![1557329088340](C:\Users\MWW\AppData\Roaming\Typora\typora-user-images\1557329088340.png)

对于单个的对象实例，如果通过Object创建，

```js
var obj = new Object();
```

那么它的原型和原型链的关系如下图。

![1557329237844](C:\Users\MWW\AppData\Roaming\Typora\typora-user-images\1557329237844.png)

如果通过函数对象来创建，

```js
function Foo(){
    
}
var foo = new Foo();
```

那么它的原型和原型链的关系如下图

![1557329297542](C:\Users\MWW\AppData\Roaming\Typora\typora-user-images\1557329297542.png)

那JavaScript的整体的原型和原型链中的关系就很清晰了，如下图所示

![1557329320464](C:\Users\MWW\AppData\Roaming\Typora\typora-user-images\1557329320464.png)

### 继承

##### JS实现继承的几种方式

参考：<https://www.cnblogs.com/humin/p/4556820.html>

既然要实现继承，那么首先我们得有一个父类，代码如下：

```js
//定义一个动物类
function Animal (name) {
    //实例属性
    this.name = name || 'Animal';
    //实例方法
    this.sleep = function(){
        console.log(this.name + '正在睡觉');
    }
}
//原型方法
Animal.prototype.eat = function(food) {
    console.log(this.name + '正在吃：'+ food)
}
```

###### 1、原型链继承

核心：把父类的实例作为子类的原型

```js
function Cat() {
}
Cat.prototype = new Animal();
Cat.prototype.name = 'Cat';
//test code
var cat = new Cat();
console.log(cat.name);
console.log(cat.eat('fish'));
console.log(cat.sleep());
console.log(cat instanceof Animal); //true 
console.log(cat instanceof Cat); //true
```

特点：

1. 非常纯粹的继承关系，实例是子类的实例，也是父类的实例
2. 父类新增原型方法/原型属性，子类都能访问到
3. 简单，易于实现

缺点：

1. 要想为子类新增属性和方法，必须要在`new Animal()`这样的语句之后执行，不能放到构造器中。

2. 无法实现多继承。

3. 来自原型对象的所有属性被所有实例共享。

4. 创建子类实例时，无法向父类构造函数传参。

   

###### 2、构造继承

**核心：**使用父类的构造函数来增强子类实例，等于是复制父类的实例属性给子类（没用到原型）

```js
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}

// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // false 
console.log(cat instanceof Cat); // true
```

特点：

1. 解决了1中，子类实例共享父类引用属性的问题
2. 创建子类实例时，可以向父类传递参数
3. 可以实现多继承（call多个父类对象）

缺点：

1. 实例并**不是父类的实例**，只是子类的实例
2. 只能继承父类的实例属性和方法，**不能继承原型属性/方法**，
3. **无法实现函数复用**，每个子类都有父类实例函数的副本，影响性能

###### 3、实例继承

**核心：**为父类实例添加新特性，作为子类实例返回

```js
function Cat(name){
  var instance = new Animal();
  instance.name = name || 'Tom';
  return instance;
}

// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); // false
```

特点：

1. 不限制调用方式，不管是`new 子类()`还是`子类()`,返回的对象具有相同的效果

缺点：

1. 实例是父类的实例，不是子类的实例
2. 不支持多继承

###### 4、拷贝继承

```js
function Cat(name){
  var animal = new Animal();
  for(var p in animal){
    Cat.prototype[p] = animal[p];
  }
  Cat.prototype.name = name || 'Tom';
}

// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // false
console.log(cat instanceof Cat); // true
```

特点：

1. 支持多继承

缺点：

1. **效率较低，内存占用高**（因为要拷贝父类的属性）
2. **无法获取父类不可枚举的方法**（不可枚举方法，不能使用for in 访问到）

###### 5、组合继承

**核心：**通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用

```js
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
Cat.prototype = new Animal(); //这里Animal构造函数产生的属性和方法被屏蔽了

// 组合继承也是需要修复构造函数指向的。

Cat.prototype.constructor = Cat;

// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); // true
```

特点：

1. 弥补了方式2的缺陷，可以继承实例属性/方法，也可以继承原型属性/方法
2. 既是子类的实例，也是父类的实例
3. 不存在引用属性共享问题
4. 可传参
5. 函数可复用

缺点：

1. 调用了两次父类构造函数，生成了两份实例（子类实例将子类原型上的那份屏蔽了）

推荐指数：★★★★（仅仅多消耗了一点内存）

###### 6、寄生组合继承

**核心：**通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性，避免了组合继承的缺点

```js
function Cat(name){
  Animal.call(this);
  this.name = name || 'Tom';
}
(function(){
  // 创建一个没有实例方法的类
  var Super = function(){};
  Super.prototype = Animal.prototype;
  //将实例作为子类的原型
  Cat.prototype = new Super();
})();
Cat.prototype.constructor = Cat; // 需要修复下构造函数
// Test Code
var cat = new Cat();
console.log(cat.name);
console.log(cat.sleep());
console.log(cat instanceof Animal); // true
console.log(cat instanceof Cat); //true
```

特点：

1. 堪称完美

缺点：

1. 实现较为复杂

推荐指数：★★★★★

###### 附录

```js
function Animal (name) {
  // 属性
  this.name = name || 'Animal';
  // 实例方法
  this.sleep = function(){
    console.log(this.name + '正在睡觉！');
  }
  //实例引用属性
  this.features = [];
}
function Cat(name){
}
Cat.prototype = new Animal();

var tom = new Cat('Tom');
var kissy = new Cat('Kissy');

console.log(tom.name); // "Animal"
console.log(kissy.name); // "Animal"
console.log(tom.features); // []
console.log(kissy.features); // []

tom.name = 'Tom-New Name';
tom.features.push('eat');

//针对父类实例值类型成员的更改，不影响
console.log(tom.name); // "Tom-New Name"
console.log(kissy.name); // "Animal"
//针对父类实例引用类型成员的更改，会通过影响其他子类实例
console.log(tom.features); // ['eat']
console.log(kissy.features); // ['eat']
```

```
原因分析：

关键点：属性查找过程

执行tom.features.push，首先找tom对象的实例属性（找不到），
那么去原型对象中找，也就是Animal的实例。发现有，那么就直接在这个对象的
features属性中插入值。
在console.log(kissy.features); 的时候。同上，kissy实例上没有，那么去原型上找。
刚好原型上有，就直接返回，但是注意，这个原型对象中features属性值已经变化了。
```

### 闭包

##### 闭包的概念    

参考：<https://www.cnblogs.com/onepixel/p/5062456.html>

```js
function outer() {
     var  a = '变量1'
     var  inner = function () {
            console.info(a)
     }
    return inner    // inner 就是一个闭包函数，因为他能够访问到outer函数的作用域
}
var c = outer()
c()
```

这是最简单的闭包。

有了初步认识后，我们简单分析一下它和普通函数有什么不同，上面代码翻译成自然语言如下：

- 定义外部函数 outer
- 在 outer 中定义内部函数 inner
- 在 outer 中返回 inner
- 执行 outer，并把 outer 的返回结果赋值给外部变量 C
- 执行 C 

把这5步操作总结成一句话就是：

**函数A的内部函数B被函数A外的一个变量 c 引用。**

把这句话再加工一下就变成了闭包的定义：

**当一个内部函数被其外部函数之外的变量引用时，就形成了一个闭包。**

###### **闭包的定义**

**能够访问另一个函数作用域的变量的函数**。

清晰的讲：闭包就是一个函数，这个函数能够访问其他函数的作用域中的变量。

###### **为什么闭包函数能够访问其他函数的作用域** 

从堆栈的角度看待js函数
　　基本变量的值一般都是存在栈内存中，而对象类型的变量的值存储在堆内存中，栈内存存储对应空间地址。基本的数据类型: Number 、Boolean、Undefined、String、Null。

```js
var  a = 1   //a是一个基本类型
var  b = {m: 20 }   //b是一个对象
```

对应内存存储：

![img](https://upload-images.jianshu.io/upload_images/7155532-841a10bf5a08a51b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/421/format/webp)

当我们执行 b={m:30}时，堆内存就有新的对象{m：30}，栈内存的b指向新的空间地址( 指向{m：30} )，而堆内存中原来的{m：20}就会被程序引擎垃圾回收掉，节约内存空间。我们知道js函数也是对象，它也是在堆与栈内存中存储的，我们来看一下转化：

```js
var a = 1;
function fn(){
    var b = 2;
    function fn1(){
        console.log(b);
    }
    fn1();
}
fn();
```

![img](https://upload-images.jianshu.io/upload_images/7155532-ee4142a5b829d016.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/501/format/webp)

栈是一种先进后出的数据结构：
 1 在执行fn前，此时我们在全局执行环境(浏览器就是window作用域)，全局作用域里有个变量a；
 2 进入fn，此时栈内存就会push一个fn的执行环境，这个环境里有变量b和函数对象fn1，这里可以访问自身执行环境和全局执行环境所定义的变量
 3 进入fn1，此时栈内存就会push 一个fn1的执行环境，这里面没有定义其他变量，但是我们可以访问到fn和全局执行环境里面的变量，因为程序在访问变量时，是向底层栈一个个找，如果找到全局执行环境里都没有对应变量，则程序抛出underfined的错误。

4 随着fn1()执行完毕，fn1的执行环境被杯销毁，接着执行完fn()，fn的执行环境也会被销毁，只剩全局的执行环境下，现在没有b变量，和fn1函数对象了，只有a 和 fn(函数声明作用域是window下)

在函数内访问某个变量是根据函数作用域链来判断变量是否存在的，而函数作用域链是程序根据函数所在的执行环境栈来初始化的，所以上面的例子，我们在fn1里面打印变量b，根据fn1的作用域链的找到对应fn执行环境下的变量b。所以**当程序在调用某个函数时，做了一下的工作：准备执行环境，初始函数作用域链和arguments参数对象**

我们现在看回最初的例子outer与inner

```js
function outer() {
     var  a = '变量1'
     var  inner = function () {
            console.info(a)
     }
    return inner    // inner 就是一个闭包函数，因为他能够访问到outer函数的作用域
}
var  inner = outer()   // 获得inner闭包函数
inner()   //"变量1"
```

当程序执行完var inner = outer()，其实outer的执行环境并没有被销毁，因为他里面的变量a仍然被被inner的函数作用域链所引用，当程序执行完inner(), 这时候，inner和outer的执行环境才会被销毁调；《JavaScript高级编程》书中建议：**由于闭包会携带包含它的函数的作用域，因为会比其他函数占用更多内容，过度使用闭包，会导致内存占用过多**。

##### 闭包会带来的问题

###### **1： 引用的变量发生变化**

```js
function outer() {
      var result = [];
      for （var i = 0； i<10; i++）{
        result.[i] = function () {
            console.info(i)
        }
     }
     return result
}
```

看样子result每个闭包函数对打印对应数字，1,2,3,4,...,10, 实际不是，因为每个闭包函数访问变量i是outer执行环境下的变量i，随着循环的结束，i已经变成10了，所以执行每个闭包函数，结果打印10个10
怎么解决这个问题呢？

```js
function outer() {
      var result = [];
      for （var i = 0； i<10; i++）{
        result.[i] = function (num) {
             return function() {
                   console.info(num);    // 此时访问的num，是上层函数执行环境的num，数组有10个函数对象，每个对象的执行环境下的number都不一样
             }
        }(i)
     }
     return result
}
```

###### **2: this指向**

```js
var object = {
     name: 'object',
     getName： function() {
        return function() {
             return this.name
        }
    }
}
console.log(object.getName()())    // underfined 	
// 因为里面的闭包函数是在window作用域下执行的，也就是说，this指向windows
```

解决办法：1.保存原this

```js
var object = {
     name: 'object',
     getName： function() {
        var that = this
        return function() {
             return that.name
        }
    }
}
console.log(object.getName()())    // object
```

解决办法：2.用call强制改变this的指向

```js
var object = {
     name: 'object',
     getName： function() {
        return function() {
             return this.name
        }
    }
}
console.log(object.getName().call(object))    // object
```



###### **3:内存泄露**

```js
function  showId() {
    var el = document.getElementById("app")
    el.onclick = function(){
      console.log(el.id)   // 这样会导致闭包引用外层的el，当执行完showId后，el无法释放
    }
}

// 改成下面
function  showId() {
    var el = document.getElementById("app")
    var id  = el.id
    el.onclick = function(){
      alert(id)   // 这样会导致闭包引用外层的el，当执行完showId后，el无法释放
    }
    el = null    // 主动释放el
}
```

##### 闭包的用途

###### **1:解决递归调用**

```js
function  factorial(num) {
   if(num<= 1) {
       return 1;
   } else {
      return num * factorial(num-1)
   }
}
var anotherFactorial = factorial
factorial = null
anotherFactorial(4)   // 报错 ，因为最好是return num* arguments.callee（num-1），arguments.callee指向当前执行函数，但是在严格模式下不能使用该属性也会报错，所以借助闭包来实现


// 使用闭包实现递归
var newFactorial = （function f(num){
    if(num<1) {return 1}
    else {
       return num* f(num-1)
    }
}） //这样就没有问题了，实际上起作用的是闭包函数f，而不是外面的函数newFactorial
```

######  2:模仿块级作用域

 es6没出来之前，用var定义变量存在变量提升问题

```js
for(var i=0; i<10; i++){
    console.info(i)
}
alert(i)  // 变量提升，弹出10

//为了避免i的提升可以这样做
(function () {
    for(var i=0; i<10; i++){
         console.info(i)
    }
})()
alert(i)   // underfined   因为i随着闭包函数的退出，执行环境销毁，变量回收
```

当然现在大多用es6的let 和const 定义。

在了解闭包的作用之前，我们先了解一下 Javascript 中的 GC 机制:

**在 Javascript 中，如果一个对象不再被引用，那么这个对象就会被 GC 回收，否则这个对象一直会保存在内存中。**

在上述例子中，B 定义在 A 中，因此 B 依赖于 A ,而外部变量 C 又引用了 B , 所以A间接的被 C 引用。

也就是说，A 不会被 GC 回收，会一直保存在内存中。为了证明我们的推理，上面的例子稍作改进：

```js
function A() {
    var count = 0;
    function B() {
       count ++;
       console.log(count);
    }
    return B;
}
var C = A();
C();// 1
C();// 2
C();// 3
```

count 是函数A 中的一个变量，它的值在函数B 中被改变，函数 B 每执行一次，count 的值就在原来的基础上累加 1 。因此，函数A中的 count 变量会一直保存在内存中。

当我们需要在模块中定义一些变量，并希望这些变量一直保存在内存中但又不会 “污染” 全局的变量时，就可以用闭包来定义这个模块。

##### **闭包的高级写法**

```js
(function (document) {
    var viewport;
    var obj = {
        init: function(id) {
           viewport = document.querySelector('#' + id);
        },
        addChild: function(child) {
            viewport.appendChild(child);
        },
        removeChild: function(child) {
            viewport.removeChild(child);
        }
    }
    window.jView = obj;
})(document);
```

这个组件的作用是：初始化一个容器，然后可以给这个容器添加子容器，也可以移除一个容器。

功能很简单，但这里涉及到了另外一个概念：立即执行函数。 简单了解一下就行，需要重点理解的是这种写法是如何实现闭包功能的。

可以将上面的代码拆分成两部分：**(function(){})** 和 **()** 。

第1个**()** 是一个表达式，而这个表达式本身是一个匿名函数，所以在这个表达式后面加 **()** 就表示执行这个匿名函数。

因此这段代码执行执行过程可以分解如下：

```js
var f = function(document) {
    var viewport;
    var obj = {
        init: function(id) {
            viewport = document.querySelector('#' + id);
        },
        addChild: function(child) {
            viewport.appendChild(child);
        },
        removeChild: function(child) {
            viewport.removeChild(child);
        }
    }
    window.jView = obj;
};
f(document);
```

在这段代码中似乎看到了闭包的影子，但 f 中没有任何返回值，似乎不具备闭包的条件，注意这句代码：

```js
`window.jView = obj;`
```

obj 是在函数 f 中定义的一个对象，这个对象中定义了一系列方法， 执行window.jView = obj 就是在 window 全局对象定义了一个变量 jView，并将这个变量指向 obj 对象，即全局变量 jView 引用了 obj . 而 obj 对象中的函数又引用了函数 f 中的变量 viewport ,因此函数 f 中的 viewport 不会被 GC 回收，viewport 会一直保存到内存中，所以这种写法满足了闭包的条件。

##### 闭包典型例题

###### 1.经典打印问题

```js
function fn(){
    var num = 3
    return function(){
        var n = 0
        console.log(++n)
        console.log(++num)
    }
}
var fn1 = fn()
fn1() //1 4
fn1()// 1 5
```

###### 2.定时器问题

```js
for (var i = 1; i <= 5; ++i){
    setTimeout(function(){
        console.log(i)
    },1000)
}
//一秒后一次性输出5个6
for (var i = 1; i <= 5; ++i){
    setTimeout(function(){
        console.log(i)
    },i*1000)
}// 瞬间输出一个6 然后每隔1s输出一个6 一共5个6
```

解决：

1、

```js
for(var i = 1; i <= 5; ++i){
    (function(num){
        setTimeout(function(){
            console.log(num)
        },1000)
    })(i)
}
// 1秒后 一次性直接输出 1 2 3 4 5
```

2、完美解决

```js
for(var i = 1; i <= 5; ++i){
    (function(num){
        setTimeout(function(){
            console.log(num)
        },i*1000)
    })(i)
}
//每隔一秒输出1 2 3 4 5

//或者
for (var i=1; i<=5; i++) { 
    setTimeout( (function(i) {
        return function() {
            console.log(i);
        }
    })(i), i*1000 );
}
// 每隔1s 输出 1 2 3 4 5
```

如果我们直接这样写，根据setTimeout定义的操作在函数调用栈清空之后才会执行的特点，for循环里定义了5个setTimeout操作。而当这些操作开始执行时，for循环的i值，已经先一步变成了6。因此输出结果总为6。而我们想要让输出结果依次执行，我们就必须借助闭包的特性，**每次循环时，将i值保存在一个闭包中，当setTimeout中定义的操作执行时，则访问对应闭包保存的i值即可。**

而我们知道在函数中闭包判定的准则，即执行时是否在内部定义的函数中访问了上层作用域的变量。因此我们需要包裹一层自执行函数为闭包的形成提供条件。

因此，我们只需要2个操作就可以完成题目需求，**一是使用自执行函数提供闭包条件，二是传入i值并保存在闭包中。**

在这段代码中，相当于同时启动4个定时器，i*1000是为4个定时器分别设置了不同的时间，同时启动，但是执行时间不同，每个定时器间隔都是1000毫秒，实现了每隔1000毫秒就执行一次打印的效果。

###### 3、闭包作为参数传递

```js
var num = 15
var fn1 = function(x){
    if(x>num){
        console.log(x)
    }
}
void function sss(fn2){
    var num = 100
    fn2(30)
}(fn1)

//30  num应用的是fn1的num 也就是window.num
// void 是对表达式求值 void(expression) 意思就是执行expression 返回值恒为undefined
```

###### 4、闭包是独立的

一个闭包机制可以创建多个闭包函数出来，它们彼此没有联系，都是独立的。

并且每个闭包函数可以保存自己个性化的信息。

看下代码，理解下三个闭包彼此独立、没有联系

```js
function f1(num){
        function f2(){
            alert('数字:'+num);
        }
        return f2
    }
    var fa = f1(10);
    var fb = f1(20);
    var fc = f1(30);
    fa();   //数字:10
    fb();   //数字:20
    fc();   //数字:30
```

以及：

```js
//创建数组元素
    var num = new Array();
    for(var i=0; i<4; i++){
        //num[i] = 闭包;//闭包被调用了4次，就会生成4个独立的函数
        //每个函数内部有自己可以访问的个性化(差异)的信息
        num[i] = f1(i);
    }
    function f1(n){
        function f2(){
            console.log(n);
        }
        return f2;
    }
    num[2]();  //2
    num[1]();  //1
    num[0]();  //0
    num[3]();  //3
```

###### 5、一道繁琐的题

```js
function fun(n,o) {
    console.log(o)
    return {
      fun:function(m){
        return fun(m,n);
      }
    };
  }
  var a = fun(0);  a.fun(1);  a.fun(2);  a.fun(3);//undefined,?,?,?
  var b = fun(0).fun(1).fun(2).fun(3);//undefined,?,?,?
  var c = fun(0).fun(1);  c.fun(2);  c.fun(3);//undefined,?,?,?
//a 
//1先输出 undefined 因为只传了一个参数进去
然后a = {
    fun:function(m){
        return fun(m,0)
    }
}
再执行a.fun(1)//
//等价于
(function(m){
  return fun(m,0)
})(1)
返回值是 fun(1,0)
fun(1,0)是啥 递归了
1输出 0
2返回 
{
  fun:function(m){
    return fun(m,n);
  }
}
第一轮执行后 a不变 所以后面输出是2个0
总体就是 undefined 0 0 0 
//b
//1先输出 undefined 因为只传了一个参数进去
然后b = {
    fun:function(m){
        return fun(m,0)
    }
}
即 fun(1,0)
//2此时输出0
返回值:
{
    fun:function(m){
        return fun(m,1)
    }
}
以此类推 输出1
所以输出 undefined 0 1 2 
//c
//1先输出 undefined 因为只传了一个参数进去
然后c = {
    fun:function(m){
        return fun(m,0)
    }
}
即 fun(1,0)
//2此时输出0
返回值:
c =  {
    fun:function(m){
        return fun(m,1)
    }
}
c.fun(2) 以此类推 输出1
由于c没有重新赋值 因此c.fun(2)还是输出1  
即undefined 0 1 1
```



##### 理解setTimeout

###### 一道例题

```js
setTimeout(function() {
    console.log(a);
}, 0);
 
var a = 10;
 
console.log(b);
console.log(fn);
 
var b = 20;
 
function fn() {
    setTimeout(function() {
        console.log('setTImeout 10ms.');
    }, 10);
}
 
fn.toString = function() {
    return 30;
}
 
console.log(fn);
 
setTimeout(function() {
    console.log('setTimeout 20ms.');
}, 20);
 
fn();
```

变量提升等价于:

```js
var a
var b
function fn
a = 10
console.log(b);
console.log(fn);
b = 20
fn = function () {
    setTimeout(function() {
        console.log('setTImeout 10ms.');
    }, 10);
}
fn.toString = function() {
    return 30;
}
console.log(fn);
setTimeout(function() {
    console.log(a);
}, 0);
setTimeout(function() {
    console.log('setTimeout 20ms.');
}, 20); 
fn();
```

故输出：

```js
undefined    // console.log(b)
[Function: fn]   // console.log(fn) 变量提升了
{ [Function: fn] toString: [Function] } //console.log(fn)
10 //setTimeout(function() { console.log(a);}, 0);
setTImeout 10ms. //
setTImeout 20ms. //：？？？？？？？？

```

### Promise

#### 1.概述

参考：<https://www.jianshu.com/p/a99030ac7a8a>

Promise是ES6中新增的一种异步编程的方式，用于解决回调的方式的各种问题，提供了更多的可能性。

Promise表示的是一个计算结果或网络请求的占位符。由于当前计算或网络请求尚未完成，所以结果暂时无法取得。

Promise对象一共有3种状态，**pending`，`fullfilled`（又称为`resolved`）和`rejected**：

- pending——任务仍在进行中。

- resolved——任务已完成。

- reject——任务出错。

  

romise对象初始时处于pending状态，其生命周期内只可能发生以下一种状态转换：

- 任务完成，状态由pending转换为resolved。
- 任务出错返回，状态由pending转换为rejected。

**Promise对象的状态转换一旦发生，就不可再次更改**。这或许就是Promise之“承诺”的含义吧。

#### 2.基本用法

##### 2.1 创建Promise

`Javascript`提供了`Promise`构造函数用于创建`Promise`对象。格式如下：

```js
let p = new Promise(executor(resolve, reject));
```

代码中`executor`是用户自定义的函数，用于实现具体异步操作流程。**该函数有两个参数`resolve`和`reject`，它们是Javascript引擎提供的函数，不需要用户实现。**在`executor`函数中，如果异步操作成功，则调用`resolve`将`Promise`的状态转换为`resolved`，`resolve`函数以结果数据作为参数。如果异步操作失败，则调用`reject`将`Promise`的状态转换为`rejected`，`reject`函数以具体错误对象作为参数。

##### 2.2 `then`方法

`Promise`对象创建完成之后，我们需要调用`then(succ_handler, fail_handler)`方法指定成功和/或失败的回调处理。例如：

```js
let p = new Promise(function(resolve, reject) {
    resolve("finished");
});

p.then(function (data) {
    console.log(data); // 输出finished
}, function (err) {
    console.log("oh no, ", err.message);
});
```

在上面的代码中，我们创建了一个`Promise`对象，在`executor`函数中调用`resolve`将该对象状态转换为`resolved`。

进而`then`指定的成功回调函数被调用，输出`finished`。

```js
let p = new Promise(function(resolve, reject) {
    reject(new Error("something be wrong"));
});

p.then(function (data) {
    console.log(data);
}, function (err) {
    console.log("oh no, ", err); // 输出oh no,  something be wrong
});
```

以上代码中，在`executor`函数中调用`reject`将`Promise`对象状态转换为`rejected`。

进而`then`指定的失败回调函数被调用，输出`oh no, something be wrong`。

这就是最基本的使用`Promise`编写异步处理的方式了。但是，有几点需要注意：

（1） `then`方法**可以只传入成功或失败回调**。

（2）`executor`函数是立即执行的，而成功或失败的回调函数会到当前`EventLoop`的最后再执行。下面的代码可以验证这一点：

```js
let p = new Promise(function(resolve, reject) {
    console.log("promise constructor");
    resolve("finished");
});

p.then(function (data) {
    console.log(data);
});

console.log("end");
```

输出结果为：

```js
promise constructor
end
finished
```

（3） `then`方法返回的是一个新的`Promise`对象，所以可以链式调用：

```js
let p = new Promise(function(resolve) {
    resolve(5);
});

p.then(function (data) {
    return data * 2;
})
 .then(function (data) {
    console.log(data); // 输出10
});
```

（4）`Promise`对象的`then`方法可以被调用多次，而且可以被重复调用（不同于事件，同一个事件的回调只会被调用一次。）。

```js
let p = new Promise(function(resolve) {
    resolve("repeat");
});

p.then(function (data) {
    console.log(data);
});

p.then(function (data) {
    console.log(data);
});

p.then(function (data) {
    console.log(data);
});
```

输出：

```js
repeat
repeat
repeat
```

##### 2.3 `catch`方法

由前面的介绍，我们知道，可以由`then`方法指定错误处理。但是ES6提供了一个更好用的方法`catch`。直观上理解可以认为`catch(handler)`等同于`then(null, handler)`。

```js
let p = new Promise(function(resolve, reject) {
    reject(new Error("something be wrong"));
});

p.catch(function (err) {
    console.log("oh no, ", err.message); // 输出oh no, something be wrong
});
```

**通常不建议在`then`方法中指定错误处理，而是在调用链的最后增加一个`catch`方法用于处理前面的步骤中出现的错误。**

使用时注意一下几点：

- `then`方法指定两个处理函数，**调用成功处理函数抛出异常时，失败处理函数不会被调用**。
- `Promise`中未被处理的异常不会终止当前的执行流程，也就是说`Promise`会**“吞掉异常”**。

```js
let p = new Promise(function (resolve, reject) {
    throw new Error("something be wrong");
});

p.then(function (data) {
    console.log(data);
});

console.log("end");
// 程序正常结束，输出end
```

##### 2.4 其他创建Promise对象的方式

除了`Promise`构造函数，ES6还提供了两个简单易用的创建`Promise`对象的方式，即`Promise.resolve`和`Promise.reject`。

##### `Promise.resolve`

顾名思义，`Promise.resolve`创建一个`resolved`状态的`Promise`对象：

```js
let p = Promise.resolve("hello");

p.then(function (data) {
    console.log(data); // 输出hello
});
```

`Promise.resolve`的参数分为以下几种类型：

（1）参数是一个`Promise`对象，那么直接返回该对象。

（2） 参数是一个`thenable`对象，即拥有`then`函数的对象。这时`Promise.resolve`会将该对象转换为一个`Promise`对象，并且立即执行其`then`函数。

```js
let thenable = {
    then: function (resolve, reject) {
        resolve(25);
    };
};
let p = Promise.resolve(thenable);
p.then(function (data) {
    console.log(data); // 输出25
});
```

（3）其他参数（无参数相当于有一个undefined参数），创建一个状态为`resolved`的`Promise`对象，参数作为操作结果会传递给后续回调处理。

##### `Promise.reject`

`Promise.reject`不管参数为何种类型，都是创建一个状态为`rejected`的`Promise`对象。

#### 3.高级用法

##### 3.1 Flatten Promise

`then`方法的成功回调函数可以返回一个新的`Promise`对象，这时旧的`Promise`对象将会被冻结，其状态取决于新`Promise`对象的状态。

```js
let p1 = new Promise(function (resolve) {
    setTimeout(function () {
        resolve("promise1");
    }, 3000);
});

let p2 = new Promise(function (resolve) {
    resolve("promise2");
});

p2.then(function (data) {
    return p1;  // (A)
})
  .then(function (data) { // (B)
    console.log(data); // 输出promise2
});
```

我们在(A)行直接返回了另一个`Promise`对象。后面的`then`方法执行取决于该对象的状态，所以在3s后输出`promise1`，不会输出`promise2`。

##### 3.2 Promise.all 方法

很多时候，我们想要等待多个异步操作完成后再进行一些处理。如果使用回调的方式，会出现前面提到过的回调地狱。例如：

```js
let fs = require("fs");

fs.readFile("file1", "utf8", function (data1, err1) {
    if (err1 != nil) {
        console.log(err1);
        return;
    }
    fs.readFile("file2", "utf8", function (data2, err2) {
        if (err2 != nil) {
            console.log(err2);
            return;
        }
        fs.readFile("file3", "utf8", function (data3, err3) {
            if (err3 != nil) {
                console.log(err3);
                return;
            }
            console.log(data1);
            console.log(data2);
            console.log(data3);
        });
    });
});
```

假设文件`file1`，`file2`，`file3`中的内容分别是"in file1"，"in file2"，"in file3"。那么输出如下：

```js
in file1
in file2
in file3
```

这种情况下，`Promise.all`就派上大用场了。`Promise.all`接受一个可迭代对象（即ES6中的Iterable对象），每个元素通过调用`Promise.resolve`转换为`Promise`对象。`Promise.all`方法返回一个新的`Promise`对象。该对象在所有`Promise`对象状态变为`resolved`时，其状态才会转换为`resolved`，参数为各个`Promise`的结果组成的数组。只要有一个对象的状态变为`rejected`，新对象的状态就会转换为`rejected`。使用`Promise.all`我们可以很优雅的实现上面的功能：

```js
let fs = require("fs");

let promise1 = new Promise(function (resolve, reject) {
    fs.readFile("file1", "utf8", function (err, data) {
        if (err != null) {
            reject(err);
        } else {
            resolve(data);
        }
    });
});

let promise2 = new Promise(function (resolve, reject) {
    fs.readFile("file2", "utf8", function (err, data) {
        if (err != null) {
            reject(err);
        } else {
            resolve(data);
        }
    });
});

let promise3 = new Promise(function (resolve, reject) {
    fs.readFile("file3", "utf8", function (err, data) {
        if (err != null) {
            reject(err);
        } else {
            resolve(data);
        }
    });
});

let p = Promise.all([promise1, promise2, promise3]);
p.then(function (datas) {
    console.log(datas);
})
 .catch(function (err) {
    console.log(err);
});
```

输出如下：

```
['in file1', 'in file2', 'in file3']
```

第二段代码我们可以进一步简化为：

```js
let fs = require("fs");

let myReadFile = function (filename) {
    return new Promise(function (resolve, reject) {
        fs.readFile(filename, "utf8", function (err, data) {
            if (err != null) {
                reject(err);
            } else {
                resolve(data);
            }
        });
    });
}

let promise1 = myReadFile("file1");
let promise2 = myReadFile("file2");
let promise3 = myReadFile("file3");

let p = Promise.all([promise1, promise2, promise3]);
p.then(function (datas) {
    console.log(datas);
})
 .catch(function (err) {
    console.log(err);
});
```

#### 3.3 Promise.race 方法

`Promise.race`与`Promise.all`一样，接受一个可迭代对象作为参数，返回一个新的`Promise`对象。不同的是，只要参数中有一个`Promise`对象状态发生变化，新对象的状态就会变化。也就是说哪个操作快，就用哪个结果（或出错）。利用这种特性，我们可以实现超时处理：

```js
let p1 = new Promise(function (resolve, reject) {
    setTimeout(function () {
        reject(new Error("time out"));
    }, 1000);
});

let p2 = new Promise(function (resolve, reject) {
    // 模拟耗时操作
    setTimeout(function () {
        resolve("get result");
    }, 2000);
});

let p = Promise.race([p1, p2]);

p.then(function (data) {
    console.log(data);
})
 .catch(function (err) {
    console.log(err);
});
```

对象`p1`在1s之后状态转换为`rejected`，`p2`在2s后转换为`resolved`。所以1s后，`p1`状态转换时，`p`的状态紧接着就转为`rejected`了。从而，输出为：

```js
time out
```

如果将对象`p2`的延迟改为0.5s，那么在0.5s后`p2`状态改变时，`p`紧随其后状态转换为`resolved`。从而输出为：

```js
get result
```

#### 4.使用案例

前面我们提到过，`then`方法会返回一个新的`Promise`对象。所以`then`方法可以链式调用，前一个成功回调的返回值会作为下一个成功回调的参数。例如：

```js
let p = new Promise(function (resolve, reject) {
    resolve(25);
});

p.then(function (num) { // (A)
    return num + 1;
})
 .then(function (num) { // (B)
    return num * 2;
})
 .then(function (num) { // (C)
    console.log(num);
});
```

对象`p`状态变为`resolved`时，结果为`25`。行(A)处函数最先被调用，参数`num`的值为`25`，返回值为`26`。`26`又作为行(B)处函数的参数，函数返回`52`。`52`作为行(C)处函数的参数，被输出。

下面给出结合AJAX的一个案例。

```js
let getJSON = function (url) {
    return new Promise(function (resolve, reject) {
        let xhr = new XMLHttpRequest();
        xhr.open('GET', url);
        xhr.onreadystatechange = function () {
            if (xhr.readyState !== 4) {
                return;
            }

            if (xhr.status === 200) {
                resolve(xhr.response);
            } else {
                reject(new Error(xhr.statusText));
            }
        }
        xhr.send();
    });
}

getJSON("http://api.icndb.com/jokes/random")
 .then(function (responseText) {
    return JSON.parse(responseText);
})
 .then(function (obj) {
    console.log(obj.value.joke);
})
 .catch(function (err) {
    console.log(err.message);
});
```

`getJSON`函数接受一个`url`地址，请求json数据。但是请求到的数据是文本格式，所以在第一个`then`方法的回调中使用`JSON.parse`将其转为对象，第二个`then`方法回调再进行具体处理。

### ajax

#### 1.1概要

ajax的核心是**XMLHttpRequest**对象。

一个最基本的ajax请求

```js
var xhr = new XMLHttpRequest()
xhr.onreadystatechange = function(){
    if(xhr.readyState == 4) {
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
            console.log('响应数据：',xhr.responseText)
        } else {
            console.log('request is not successful:',xhr.status)
        }
    }
}
xhr.open('get', 'example.php',false)
xhr.send(null)
```

这里的send()方法接收一个参数，即要作为请求主体发送的数据。如果不需要通过请求主体发送数据，则必须传入null，因为这个参数对有些浏览器来说是必需的。调用send()之后，请求就会被分派到服务器。
由于这次请求是同步的，JavaScript 代码会等到服务器响应之后再继续执行。在收到响应后，响应的数据会自动填充XHR 对象的属性，相关的属性简介如下。
**responseText**：作为响应主体被返回的文本。
**responseXML**：如果响应的内容类型是"text/xml"或"application/xml"，这个属性中将保存包含着响应数据的XML DOM 文档。
**status**：响应的HTTP 状态。
**statusText**：HTTP 状态的说明。

一般来说，可以将HTTP状态代码为200 作为成功的标志,。此外，状态代码为304 表示请求的资源并没有被修改，可
以直接使用浏览器中缓存的版本。

无论内容类型是什么，响应主体的内容都会保存到responseText 属性中；而对于非XML 数据而言，responseXML 属性的值将为null。

我们还是要发送异步请求，才能让
JavaScript 继续执行而不必等待响应。此时，可以检测XHR 对象的readyState 属性，该属性表示请求响应过程的当前活动阶段。这个属性可取的值如下。
	0：未初始化。尚未调用open()方法。
	1：启动。已经调用open()方法，但尚未调用send()方法。
	2：发送。已经调用send()方法，但尚未接收到响应。
	3：接收。已经接收到部分响应数据。
	4：完成。已经接收到全部响应数据，而且已经可以在客户端使用了。
只要readyState 属性的值由一个值变成另一个值，都会触发一次readystatechange 事件。可
以利用这个事件来检测每次状态变化后readyState 的值。通常，我们只对readyState 值为4 的阶
段感兴趣，因为这时所有数据都已经就绪。不过，必须在调用open()之前指定onreadystatechange
事件处理程序才能确保跨浏览器兼容性。下面来看一个例子。



```js
var xhr = window.XMLHttpRequst? new XMLHttpRequest() : new ActiveXObject('Microsoft.XMLHTTP')
xhr.onreadystatechange = function(){
    if (xhr.readyState == 4){
    	if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
            alert(xhr.responseText);
         } else {
            alert("Request was unsuccessful: " + xhr.status);
     	}
	}
};
xhr.open("get", "example.txt", true);
xhr.setRequestHeader("MyHeader", "MyValue");
xhr.send(null);
```

另外，在接收到响应之前还可以调用abort()方法来取消异步请求，如下所示：
xhr.abort();

#### 1.2HTTP头部信息

默认情况下，在发送XHR 请求的同时，还会发送下列头部信息。
 Accept：浏览器能够处理的内容类型。
 Accept-Charset：浏览器能够显示的字符集。
 Accept-Encoding：浏览器能够处理的压缩编码。
 Accept-Language：浏览器当前设置的语言。
 Connection：浏览器与服务器之间连接的类型。
 Cookie：当前页面设置的任何Cookie。
 Host：发出请求的页面所在的域 。
 Referer：发出请求的页面的URI。注意，HTTP 规范将这个头部字段拼写错了，而为保证与规
范一致，也只能将错就错了。（这个英文单词的正确拼法应该是referrer。）
 User-Agent：浏览器的用户代理字符串。

调用XHR 对象的getResponseHeader()方法并传入头部字段名称，可以取得相应的响应头部信
息。而调用getAllResponseHeaders()方法则可以取得一个包含所有头部信息的长字符串。来看下
面的例子。
var myHeader = xhr.getResponseHeader("MyHeader");
var allHeaders = xhr.getAllResponseHeaders();

#### 1.3 GET请求

GET 是最常见的请求类型，最常用于向服务器查询某些信息。

使用GET 请求经常会发生的一个错误，就是查询字符串的格式有问题。查询字符串中每个参数的名
称和值都必须使用encodeURIComponent()进行编码

```js
xhr.open("get", "example.php?name1=value1&name2=value2", true);

```

```js
function addURLParam(url, name, value) {
url += (url.indexOf("?") == -1 ? "?" : "&");
url += encodeURIComponent(name) + "=" + encodeURIComponent(value);
return url;
}
```

上面这个函数可以辅助向现有URL 的末尾添加查询字符串参数。

下面是使用这个函数来构建请求URL 的示例。

```js
var url = "example.php";
//添加参数
url = addURLParam(url, "name", "Nicholas");
url = addURLParam(url, "book", "Professional JavaScript");
//初始化请求
xhr.open("get", url, false);
```

在这里使用addURLParam()函数可以确保查询字符串的格式良好，并可靠地用于XHR 对象。

#### 1.4 POST请求

POST 请求，通常用于向服务器发送应该被保存的数据。POST 请求应该把数据作为请求的主体提交，而GET 请求传统上不是这样。

```js
xhr.open("post", "example.php", true);
```

默认情况下，服务器对POST 请求和提交Web 表单的请求并不会一视同仁。因此，服务器端必须有
程序来读取发送过来的原始数据，并从中解析出有用的部分。不过，我们可以使用XHR 来模仿表单提
交：首先将Content-Type 头部信息设置为application/x-www-form-urlencoded，也就是表单
提交时的内容类型，其次是以适当的格式创建一个字符串。第14 章曾经讨论过，POST 数据的格式与查
询字符串格式相同。如果需要将页面中表单的数据进行序列化，然后再通过XHR 发送到服务器，那么
就可以使用第14 章介绍的serialize()函数来创建这个字符串：

```js
function submitData(){
var xhr = createXHR();
xhr.onreadystatechange = function(){
if (xhr.readyState == 4){
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
        	alert(xhr.responseText);
        } else {
        	alert("Request was unsuccessful: " + xhr.status);
        }
    }
};
xhr.open("post", "postexample.php", true);
//POST必须添加内容类型为 application/x-www-form-urlencoded
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
var form = document.getElementById("user-info");
xhr.send(serialize(form));
}
```

这个函数可以将ID 为"user-info"的表单中的数据序列化之后发送给服务器。而下面的示例PHP
文件postexample.php 就可以通过$_POST 取得提交的数据了：

```js
<?php
header("Content-Type: text/plain");
echo <<<EOF
Name: {$_POST[‘user-name’]}
Email: {$_POST[‘user-email’]}
EOF;
?>
```

如果不设置Content-Type 头部信息，那么发送给服务器的数据就不会出现在$_POST 超级全局变量中。这时候，要访问同样的数据，就必须借助$HTTP_RAW_POST_DATA。

#### 1.5封装一个ajax请求

```js
//opt.data = {key:value,key:value}
//opt.url = 'www.baidu.com'
//opt.type = 'get'
//opt.success = function(data){
//opt.error = function(data){}
function formsParams(data){
        var arr = [];
        for(var prop in data){
            arr.push(prop + "=" + data[prop]);
        }
        return arr.join("&");
}
var $.ajax = function(opts) {
    // 创建xhr对象
    var xhr = null
    if (window.XMLHttpRequest) {
        xhr = new window.XMLHttpRequest()
    } else {
        xhr = new ActiveXObject('Microsoft.XMLHTTP')
    }
    xhr.onreadystatechange = function(){
        if(xhr.readyState == 4){
            if((xhr.status >= 200 && xhr.status < 300) && xhr.status == 304){
                opts.success(JSON.parse(xhr.responseText))
            } else{
                opts.error()
            }
        }
    }
    // 获取参数
    var url = opts.url
    var params = formsParams(opts.data)
    // 根据 type发送请求
    if (opts.type.toLowerCase() == 'get') {
        xhr.open('get',url+'?'+params,true)
        xhr.send(null)
    } else {
        xhr.open('post',url,true)
        xhr.setRequestHeader('Content-type','application/x-www-form-urlencoded')
        xhr.send(params)
    }
}
```

### 深拷贝与浅拷贝

#### **一、JS的基本数据类型**

- 基本数据类型：String，Boolean，Number，Undefined，Null；

- 引用数据类型：Object(Array，Date，RegExp，Function)；

- 基本数据类型和引用数据类型的区别：
  1、保存位置不同：**基本数据类型保存在栈内存中，引用数据类型保存在堆内存中**，**然后在栈内存中保存了一个对堆内存中实际对象的引用，即数据在堆内存中的地址**，JS对引用数据类型的操作都是操作对象的引用而不是实际的对象，如果obj1拷贝了obj2，那么这两个引用数据类型就指向了同一个堆内存对象，具体操作是obj1将栈内存的引用地址复制了一份给obj2，因而它们共同指向了一个堆内存对象；

  ###### 为什么基本数据类型保存在栈中，而引用数据类型保存在堆中？

   1）堆比栈大，栈比堆速度快；
   2）基本数据类型比较稳定，而且相对来说占用的内存小；
   3）引用数据类型大小是动态的，而且是无限的，引用值的大小会改变，不能把它放在栈中，否则会降低变量查找的速度，因此放在变量栈空间的值是该对象存储在堆中的地址，地址的大小是固定的，所以把它存储在栈中对变量性能无任何负面影响；
   4）堆内存是无序存储，可以根据引用直接获取；

按引用访问：js不允许直接访问保存在堆内存中的对象，所以在访问一个对象时，首先得到的是这个对象在堆内存中的地址，然后再按照这个地址去获得这个对象中的值；

ECMAScript中所有函数的参数都是按值来传递的，对于原始值，只是把变量里的值传递给参数，之后参数和这个变量互不影响，对于引用值，对象变量里面的值是这个对象在堆内存中的内存地址，因此它传递的值也就是这个内存地址，这也就是为什么函数内部对这个参数的修改会体现在外部的原因，因为它们都指向同一个对象；
 2、基本数据类型使用typeof可以返回其基本数据类型，但是NULL类型会返回object，因此null值表示一个空对象指针；引用数据类型使用typeof会返回object，此时需要使用instanceof来检测引用数据类型；
 3、定义引用数据类型需要使用new操作符，后面再跟一个构造函数来创建；
 1）使用new操作符创建对象；

```
var obj1 = new Object();
obj1.a = 1;
```

2）使用对象字面量表示法创建对象；

```
var obj1 = {
  a: 1,
  b: 2
}
```

3）可以通过点表示法访问对象的属性，也可以使用方括号表示法来访问对象的属性；

- ES6新增数据类型：Map，Set，Generator，Symbol
- 本地对象：ECMA-262 把本地对象（native object）定义为“独立于宿主环境的 ECMAScript 实现提供的对象”，即本地对象就是 ECMA-262 定义的类（引用类型）；
- 宿主对象：宿主”就是我们网页的运行环境，即“操作系统”和“浏览器”，所有非本地对象都是宿主对象（host object），即由 ECMAScript 实现的宿主环境提供的对象，所有的BOM和DOM对象都是宿主对象，因为其对于不同的“宿主”环境所展示的内容不同，即ECMAScript官方未定义的对象都属于宿主对象，因为其未定义的对象大多数是自己通过ECMAScript程序创建的对象；
- JS内置对象：是指JS语言自带的一些对象，供开发者使用，这些对象提供了一些常用的或是最基本而必要的功能；
  1、Arguments：函数参数集合；
  2、Array对象：length,instanceof,isArray(),toString()返回字符串,valueOf()返回数组的值,join()可以将数组转为字符串,push(),pop(),shift(),unshift(),reverse(),sort(),
  slice(),splice(),indexOf(),lastIndexOf(),迭every(),filter(),forEach(),map(),some(),
  归并方法reduce(),reduceRight()；
  3、Boolean：布尔对象；
  4、Error：异常对象；
  5、Number：数值对象；
  6、String对象：length,charAt()返回指定位置的字符,concat(),slice(),subString(),
  subStr(),indexOf(),lastIndexOf(),trim(),toLowerCase(),toUpperCase(),split(),
  text.match(),text.splice()；
  7、Date对象：toUTCstring(),getTime()；
  8、RegExp对象：test()；
  9、Function对象：arguments,this,apply(this,arguments),call(this,num1,num2)；
  10、Math对象：min(),max(),ceil(),floor(),round(),random()；
  11、Global对象：encodeURI,encodeURIComponent，parseInt(),eval()；
  12、Object对象：prototype,constructor；
- 基本包装类型：Boolean,Number,String
  1、转换为数值：parseInt()专门用于把字符串转换成数值,Number()用于任何类型；
  2、非字符转换成字符：toString()；
  3、数组转成字符：join()；
  4、字符串转换成数组：split()；

```js
// 基本数据类型的复制，基本数据类型是按值传递的
var a = 1;
var b = a;
b = 2;
console.log(a);  // 1
console.log(b);  // 2
// 引用数据类型的复制，引用数据类型按引用传值
var obj1 = {
  a: 1,
  b: 2
}
var obj2 = obj1;
obj2.a = 3;
console.log(obj1.a); // 3
console.log(obj2.a); // 3
```

**二、JS浅拷贝**

![img](https:////upload-images.jianshu.io/upload_images/13263206-c651dc07788bf561.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

#### 二、深拷贝和浅拷贝

- 深拷贝和浅拷贝简单解释
      浅拷贝和深拷贝都只针对于引用数据类型，浅**拷贝只复制指向某个对象的指针，而不复制对象本身，新旧对象还是共享同一块内存；但深拷贝会另外创造一个一模一样的对象，新对象跟原对象不共享内存，修改新对象不会改到原对象；**
      区别：浅拷贝只复制对象的第一层属性、深拷贝可以对对象的属性进行递归复制；

```js
// 只复制第一层的浅拷贝
function simpleCopy(obj1) {
   var obj2 = Array.isArray(obj1) ? [] : {};
   for (let i in obj1) {
   obj2[i] = obj1[i];
  }
   return obj2;
}
var obj1 = {
   a: 1,
   b: 2,
   c: {
   d: 3
  }
}
var obj2 = simpleCopy(obj1);
obj2.a = 3;
obj2.c.d = 4;
alert(obj1.a); // 1
alert(obj2.a); // 3
alert(obj1.c.d); // 4
alert(obj2.c.d); // 4
```

- Object.assign()实现浅拷贝及一层的深拷贝

```js
let obj1 = {
   a: {
     b: 1
   },
   c: 2
}
let obj2 = Object.assign({},obj1)
obj2.a.b = 3;
obj2.c = 3
console.log(obj1.a.b); // 3
console.log(obj2.a.b); // 3
console.log(obj1.c); // 2
console.log(obj2.c); // 3
```

**二、JS深拷贝**

- 手动实现深拷贝

```js
let obj1 = {
   a: 1,
   b: 2
}
let obj2 = {
   a: obj1.a,
   b: obj1.b
}
obj2.a = 3;
alert(obj1.a); // 1
alert(obj2.a); // 3
let obj1 = {
   a: {
     b: 2
   }
}
let obj2 = {
   a: obj1.a
}
obj2.a.b = 3;
console.log(obj1.a.b); // 3
console.log(obj2.a.b); // 3
```

- **递归实现深拷贝**

```js
function deepCopy(obj1) {
      var obj2 = Array.isArray(obj1) ? [] : {};
      if (obj1 && typeof obj1 === "object") {
        for (var i in obj1) {
          if (obj1.hasOwnProperty(i)) {
            // 如果子属性为引用数据类型，递归复制
            if (obj1[i] && typeof obj1[i] === "object") {
              obj2[i] = deepCopy(obj1[i]);
            } else {
              // 如果是基本数据类型，只是简单的复制
              obj2[i] = obj1[i];
            }
          }
        }
      }
      return obj2;
    }
    var obj1 = {
      a: 1,
      b: 2,
      c: {
        d: 3
      }
    }
    var obj2 = deepCopy(obj1);
    obj2.a = 3;
    obj2.c.d = 4;
    alert(obj1.a); // 1
    alert(obj2.a); // 3
    alert(obj1.c.d); // 3
    alert(obj2.c.d); // 4
```

缺陷：当遇到两个互相引用的对象，会出现死循环的情况，为了避免相互引用的对象导致死循环的情况，则应该在遍历的时候判断是否相互引用对象，如果是则退出循环；

```js
function deepCopy(obj1) {
	let obj2 = Array.isArray(obj1)? []:{}
    for ( let key in obj1) {
        if (obj1[key] == obj1){
            continue
        } 
        if (obj1.hasOwnProperty(key)){
            if (obj1[key] && typeof obj1[key] === 'object') {
                obj2[key] = deepCopy(obj1[key])
            } else {
                obj2[key] = obj1[key]
            }
        }
    }
    return obj2
}
    var obj1 = {
      a: 1,
      b: 2,
      c: {
        d: 3
      }
    }
    var obj2 = deepCopy(obj1);
```



```js
// Object.create实现深拷贝1，但也只能拷贝一层
function deepCopy(obj1) {
      var obj2 = Array.isArray(obj1) ? [] : {};
      if (obj1 && typeof obj1 === "object") {
        for (var i in obj1) {
          var prop = obj1[i]; // 避免相互引用造成死循环，如obj1.a=obj
          if (prop == obj1) {
            continue;
          }
          if (obj1.hasOwnProperty(i)) {
            // 如果子属性为引用数据类型，递归复制
            if (prop && typeof prop === "object") {
              obj2[i] = (prop.constructor === Array) ? [] : Object.create(prop);
            } else {
              // 如果是基本数据类型，只是简单的复制
              obj2[i] = prop;
            }
          }
        }
      }
      return obj2;
    }
    var obj1 = {
      a: 1,
      b: 2,
      c: {
        d: 3
      }
    }
    var obj2 = deepCopy(obj1);
    obj2.a = 3;
    obj2.c.d = 4;
    alert(obj1.a); // 1
    alert(obj2.a); // 3
    alert(obj1.c.d); // 3
    alert(obj2.c.d); // 4
// Object实现拷贝2，浅拷贝
var obj1 = {
      a: 1,
      b: 2,
      c: {
        d: 3
      }
    }
    var obj2 = Object.create(obj1);
    obj2.a = 3;
    obj2.c.d = 4;
    alert(obj1.a); // 1
    alert(obj2.a); // 3
    alert(obj1.c.d); // 4
    alert(obj2.c.d); // 4

```

- 使用JSON.stringify和JSON.parse实现深拷贝：JSON.stringify把对象转成字符串，再用JSON.parse把字符串转成新的对象；

```js
function deepCopy(obj1){
    let _obj = JSON.stringify(obj1);
    let obj2 = JSON.parse(_obj);
    return obj2;
  }
    var a = [1, [1, 2], 3, 4];
    var b = deepCopy(a);
    b[1][0] = 2;
    alert(a); // 1,1,2,3,4
    alert(b); // 2,2,2,3,4

```

缺陷：它会抛弃对象的constructor，深拷贝之后，不管这个对象原来的构造函数是什么，在深拷贝之后都会变成Object；这种方法能正确处理的对象只有 Number, String, Boolean, Array, 扁平对象，也就是说，只有可以转成JSON格式的对象才可以这样用，像function没办法转成JSON；

```
let obj1 = {
   fun:function(){
      alert(123);
   }
}
let obj2 = JSON.parse(JSON.stringify(obj1));
console.log(typeof obj1.fun); // function
console.log(typeof obj2.fun); // undefined

```

- 热门的函数库lodash，也有提供_.cloneDeep用来做深拷贝；

```js
var _ = require('lodash');
var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
};
var obj2 = _.cloneDeep(obj1);
console.log(obj1.b.f === obj2.b.f);
// false

```

- jquery实现深拷贝
  jquery 提供一个`$.extend`可以用来做深拷贝；

```js
var $ = require('jquery');
var obj1 = {
   a: 1,
   b: {
     f: {
       g: 1
     }
   },
   c: [1, 2, 3]
};
var obj2 = $.extend(true, {}, obj1);
console.log(obj1.b.f === obj2.b.f);  // false

```

- slice是否为深拷贝

```
// 对只有一级属性值的数组对象使用slice
var a = [1,2,3,4];
var b = a.slice();
b[0] = 2;
alert(a); // 1,2,3,4
alert(b); // 2,2,3,4
// 对有多层属性的数组对象使用slice
var a = [1,[1,2],3,4];
var b = a.slice();
b[1][0] = 2;
alert(a); // 1,2,2,3,4
alert(b); // 1,2,2,3,4

```

结论：slice()和concat()都并非深拷贝；

