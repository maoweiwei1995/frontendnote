

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

#### 常见js类型转换

##### 一、字符串转数字

js提供了parseInt()和parseFloat()两个转换函数。前者把值转换成整数，后者把值转换成浮点数。只有对String类型调用这些方法，这两个函数才能正确运行；对其他类型返回的都是NaN(Not a Number)。
 一些示例如下：

```js
parseInt("1234blue"); //returns 1234
parseInt("blue1234"); //returns NaN
parseInt("0xA"); //returns 10
parseInt("22.5"); //returns 22
parseInt("blue"); //returns NaN
parseInt("123");//123
parseInt("+123");//123
parseInt("-123");//-123
parseInt("-12.3元")//-12
parseInt("abc");//NaN
parseInt([1,2])//1
parseInt([1,2,4])//1
parseInt("  ");//NaN
```

parseInt()方法还有基模式，可以把二进制、八进制、十六进制或其他任何进制的字符串转换成整数。基是由parseInt()方法的第二个参数指定的，示例如下：

```js
parseInt("AF", 16); //returns 175
parseInt("10", 2); //returns 2
parseInt("10", 8); //returns 8
parseInt("10", 10); //returns 10
//如果十进制数包含前导0，那么最好采用基数10，这样才不会意外地得到八进制的值。例如：
parseInt("010"); //returns 8
parseInt("010", 8); //returns 8
parseInt("010", 10); //returns 10
```

parseFloat()方法与parseInt()方法的处理方式相似。
 使用parseFloat()方法的另一不同之处在于，字符串必须以十进制形式表示浮点数，parseFloat()没有基模式。

```js
parseFloat("1234blue"); //returns 1234.0
parseFloat("0xA"); //returns NaN
parseFloat("22.5"); //returns 22.5
parseFloat("22.34.5"); //returns 22.34
parseFloat("0908"); //returns 908
parseFloat("blue"); //returns NaN
parseFloat("123");//123
parseFloat("-123");//-123
parseFloat("+123");//123
parseFloat("12.34");//12.34
parseFloat("12.35元")//12.35
parseFloat("12.23.122");//12.23
parseFloat("av");//NaN
parseFloat("0xAA");//0
parseFloat("0110");//110
parseFloat([1]);//1
parseFloat([2,3]);//2
parseFloat([]);//NaN
parseFloat(null)//NaN
parseFloat(undefined)//NaN
```

强制类型转换Number(value)——把给定的值转换成数字（可以是整数或浮点数），与parseInt()和parseFloat()方法的处理方式相似，只是它转换的是整个值，而不是部分值。

```js
Number(false) 0
Number(true) 1
Number(undefined) NaN
Number(null) 0
Number( "5.5 ") 5.5
Number( "56 ") 56
Number( "5.6.7 ") 报错
Number(100) //100
Number("123");//123
Number("+123");//123
Number("12.3");//12.3
Number(" ");//0
Number("abc");//NaN
Number([]);//0
Number([1]);//1
Number([1,2]);//NaN
Number(new Object());//NaN
```

##### 二、 字符串、数组互转

1. ###### 字符串=>数组

    split() 方法用于把一个字符串分割成字符串数组。

```
var str="How,are,you,doing,today";
var n=str.split(",");//n=['How','are','you','doing','today']
```

1. ###### 数组=>字符串

    toString() 方法可把数组转换为字符串，并返回结果。

```
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.toString();//'Banana,Orange,Apple,Mango'
```

##### 三、js隐式类型转换

js中的不同的数据类型之间的比较转换规则如下：

###### 1. 对象和布尔值比较

**对象和布尔值进行比较时，对象先转换为字符串，然后再转换为数字，布尔值直接转换为数字**

```js
[] == true;  //false  []转换为字符串'',然后转换为数字0,true转换为数字1，所以为false
```

2. ###### 对象和字符串比较

**对象和字符串进行比较时，对象转换为字符串，然后两者进行比较。**

```js
[1,2,3] == '1,2,3' // true  [1,2,3]转化为'1,2,3'，然后和'1,2,3'， so结果为true;
```

###### 3. 对象和数字比较

**对象和数字进行比较时，对象先转换为字符串，然后转换为数字，再和数字进行比较。**

```js
[1] == 1;  // true  `对象先转换为字符串再转换为数字，二者再比较 [1] => '1' => 1 所以结果为true
```

###### 4. 字符串和数字比较

**字符串和数字进行比较时，字符串转换成数字，二者再比较。**

```js
'1' == 1 // true
```

5. ###### 字符串和布尔值比较

**字符串和布尔值进行比较时，二者全部转换成数值再比较。**

```js
'1' == true; // true 
```

6. ###### 布尔值和数字比较

**布尔值和数字进行比较时，布尔转换为数字，二者比较。**

```js
true == 1 // true
```


![数据转换](https://upload-images.jianshu.io/upload_images/2791152-ba592aa9b81fe174.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如图，任意两种类型比较时，如果不是同一个类型比较的话，则按如图方式进行相应类型转换，如对象和布尔比较的话，**对象 => 字符串 => 数值**    **布尔值 => 数值。**

**7.如果比较的双方中有一方为 Number，一方为Object时，则会调用 valueOf 方法将Object转换为数字，然后进行比较。**

```js
1 == { valueOf: function() {return1;} }    // true 
1 + { valueOf: function() {return1;} }    //2
```

需要强调的是，在 Javascript 中，只有 **空字符串、数字0、false、null、undefined 和 NaN** 这 6 个值为假之外，其他所有的值均为真值。

说到 NaN，就不得不提一下 isNaN() 方法，isNaN() 方法自带隐式类型转换，该方法在测试其参数之前，**会先调用Number() 方法将其转换为数字**。所以 isNaN('1') 这个语句中明明用一个字符串去测试，返回值仍然为 false 也就不足为怪了。

8.在 + 号运算中,**数字/字符串+对象**

当字符串和对象进行 + 运算的时候，Javascript 会通过对象的 toString() 方法将其自身转换为字符串，然后进行连接操作。		

```js
"1" + { toString: function() {return 1;} } // "11"
```

之所以说它特殊，是因为当一个对象同时包含 toString() 和 valueOf() 方法的时候，运算符 + 应该调用哪个方法并不明显(做字符串连接还是加法应该根据其参数类型，但是由于隐式类型转换的存在，类型并不显而易见。)，Javascript 会盲目的选择 valueOf() 方法而不是 toString() 来解决这个问题。这就意味着如果你打算对一个对象做字符串连接的操作，但结果却是......

```js
var obj = {
    toString:function(){return "Object CustomObj"; },
    valueOf:function(){return 1; }
}
console.log("Object: "+ obj);// "Object: 1"
```

隐式类型转换会给我们造成很多麻烦建议在所有使用条件判断的时候都使用全等运算符 **===** 来进行条件判断。全等运算符会先进行数据类型判断，并且不会发生隐式类型转换。

来看一个有趣的题

```js
[] == false; //true
![] == false;//true   空字符串(''),NaN,0，null,undefined**这几个外返回的都是true   []先转换为布尔值返回true
console.log( !'' && !NaN && !0 && !null && !undefined) //true
console.log(' ') //true
```

这两个的结果都是true,第一个是，对象 => 字符串 => 数值0 false转换为数字0,这个是true应该没问题，第二个前边多了个!，则直接转换为布尔值再取反，转换为布尔值时，**空字符串(''),NaN,0，null,undefined**，**false**这几个外返回的都是true, 所以! []这个[] => true 取反为false,所以[] == false为true。

### 还有一些需要记住的，像：

```js
undefined == null //true undefined和null 比较返回true，二者和其他值比较返回false
Number(null) //0
```

```js
//if中的条件会被自动转为Boolean类型
//会被转为false的数据
if(false) console.log(2333)   //false
if('') console.log(2333)      //false
if(null) console.log(2333)    //false
if(undefined) console.log(2333)  //false
if(NaN) console.log(2333)         //false
if(NaN) console.log(2333)         //false
//会被转为true的数据
if(true) console.log(2333) // true
if('test') console.log(2333) // true
if([]) console.log(2333) // true  ****
if({}) console.log(2333) // true

//参与+运算都会被隐式的转为字符串

//会被转为空字符串的数据
'str-' + '' // str-
'str-' + []  // str-
//会被转为字符串的数据
'str-' + '1' // "str-1"
'str-' + 1 // "str-1"
'str-' + false // "str-false"
'str-' + true // "str-true"
'str-' + null // "str-null"
'str-' + undefined // "str-undefined"
'str-' + NaN // "str-NaN"
//会被转为数据类型标记的数据
'str-' + {} // "str-[object Object]"
'str-' + {a:1} // "str-[object Object]"

//参与*运算都会被隐式的转为数字
//会被转为0的数据
2 * '' // 0
2 * '0' // 0
2 * [] // 0
2 * false // 0
//会被转为1的数据
2 * '1' // 2
2 * [1] // 2
2 * true // 2
//会被转为NaN的数据
2 * {} // NaN
2 * {a:1} // NaN
//== 运算符
//为true的时候
0 == false // true
0 == '' // true
0 == '0' // true
0 == [] // true *****
0 == [0] // true
1 == true // true
1 == '1' // true
1 == [1] // true
[1] == true // true
[] == false // true
//为false的时候
0 == {} // false ***
0 == null // false ***
0 == undefined // false
0 == NaN // false ***
1 == {} // false
1 == null // false
1 == undefined // false
1 == NaN // false
[] == [] // false *** 两个对象 比较的是地址 地址不一样
[1] == [1] // false  **
[1] == {} // false
[1] == {a:1} // false  **
[1] == false // false  ***
[1] == null // false
[1] == undefined // false
[1] == NaN // false
{} == {} // false  两个对象 比较的是地址 地址不一样
{a:1} == {a:1} // false 两个对象 比较的是地址 地址不一样
//注：空数组[]，在+运算符下是转为空字符串''，在*运算符下是转为数字0。但在if语句中，则转为true。
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

### ES6

#### 1.变量声明const和let

在ES6之前，我们都是用`var`关键字声明变量。无论声明在何处，都会被视为声明在**函数的最顶部**(不在函数内即在**全局作用域的最顶部**)。这就是函数**变量提升**例如:

```js
  function aa() {
    if(flag) {
        var test = 'hello man'
    } else {
        console.log(test)
    }
  }
```

以上的代码实际上是：

```js
  function aa() {
    var test // 变量提升，函数最顶部
    if(flag) {
        test = 'hello man'
    } else {
        //此处访问 test 值为 undefined
        console.log(test)
    }
    //此处访问 test 值为 undefined
  }
```

所以不用关心flag是否为 `true` or `false`。实际上，无论如何 test 都会被创建声明。

接下来ES6主角登场：
我们通常用 `let` 和 `const` 来声明，`let` 表示**变量**、`const` 表示**常量**。`let` 和 `const` 都是块级作用域。怎么理解这个块级作用域？

- 在一个函数内部
- 在一个代码块内部

> 说白了只要在**{}花括号内**的代码块即可以认为 `let` 和 `const` 的作用域。

看以下代码：

```js
  function aa() {
    if(flag) {
       let test = 'hello man'
    } else {
        //test 在此处访问不到
        console.log(test)
    }
  }
```

**let的作用域是在它所在当前代码块，但不会被提升到当前函数的最顶部**。

再来说说 `const`
`const` 声明的变量必须提供一个值，而且会被认为是常量，意思就是它的值被设置完成后就不能再修改了。

```js
    const name = 'lux'
    name = 'joe' // 再次赋值此时会报错
```

还有，如果 `const` 的是一个对象，对象所包含的值是可以被修改的。抽象一点儿说，就是对象所指向的地址不能改变，而变量成员是可以修改的。

```js
    const student = { name: 'cc' }
    // 没毛病
    student.name = 'yy'
    // 如果这样子就会报错了
    student  = { name: 'yy' }
```

说说TDZ(暂时性死区)，想必你早有耳闻。

```js
    {
        console.log(value) // 报错
        let value = 'lala'
    }
```

我们都知道，JS引擎扫描代码时，如果发现变量声明，用 `var` 声明变量时会将声明提升到函数或全局作用域的顶部。但是 `let` 或者 `const`，会将声明关进一个小黑屋也是TDZ(暂时性死区)，只有执行到变量声明这句语句时，变量才会从小黑屋被放出来，才能安全使用这个变量。

哦了，说一道面试题

```js
    var funcs = []
    for (var i = 0; i < 10; i++) {
        funcs.push(function() { console.log(i) })
    }
    funcs.forEach(function(func) {
        func()
    })
```

这样的面试题是大家很常见，很多同学一看就知道输出十次10
但是如果我们想依次输出0到9呢？
有两种解决方法，直接看一下代码：

```
    // ES5知识，我们可以利用“立即调用函数”解决这个问题
    var funcs = []
    for (var i = 0; i < 10; i++) {
        funcs.push(
          (function(value) {
            return function() {
                console.log(value)
            }
        })(i)
      )
    }
    funcs.forEach(function(func) {
        func()
    })
  // 再来看看es6怎么处理的
    const funcs = []
    for (let i = 0; i < 10; i++) {
        funcs.push(function() {
            console.log(i)
        })
    }
    funcs.forEach(func => func())
```

达到相同的效果，ES6 简洁的解决方案是不是更让你心动！！！

#### 2.字符串

ES6模板字符简直是开发者的福音啊，解决了 ES5 在字符串功能上的痛点。

**第一个用途**，**基本的字符串格式化**。将表达式嵌入字符串中进行拼接。用${}来界定。

```js
    //ES5 
    var name = 'lux'
    console.log('hello' + name)
    //es6
    const name = 'lux'
    console.log(`hello ${name}`) //hello lux
    
```

**第二个用途**，在ES5时我们通过反斜杠(\)来做多行字符串或者字符串一行行拼接。ES6反引号(``)直接搞定。

```js
    // ES5
    var msg = "Hi \
    man!
    "
    // ES6
    const template = `<div>
        <span>hello world</span>
    </div>`
```

对于字符串 ES6+ 当然也提供了很多厉害也很有意思的方法😊 说几个常用的。

```js
    // 1.includes：判断是否包含然后直接返回布尔值
    const str = 'hahay'
    console.log(str.includes('y')) // true

    // 2.repeat: 获取字符串重复n次
    const str = 'he'
    console.log(str.repeat(3)) // 'hehehe'
    //如果你带入小数, Math.floor(num) 来处理
    // s.repeat(3.1) 或者 s.repeat(3.9) 都当做成 s.repeat(3) 来处理

    // 3. startsWith 和 endsWith 判断是否以 给定文本 开始或者结束
    const str =  'hello world!'
    console.log(str.startsWith('hello')) // true
    console.log(str.endsWith('!')) // true
    
    // 4. padStart 和 padEnd 填充字符串，应用场景：时分秒
    setInterval(() => {
        const now = new Date()
        const hours = now.getHours().toString()
        const minutes = now.getMinutes().toString()
        const seconds = now.getSeconds().toString()
        console.log(`${hours.padStart(2, 0)}:${minutes.padStart(2, 0)}:${seconds.padStart(2, 0)}`)
    }, 1000)
```

关于模板字符串现在比较常出现的面试题有两道。同学们不妨写试试看？

- **模拟一个模板字符串的实现。**

```js
    let address = '北京海淀区'
    let name = 'lala'
    let str = `${name}在${address}上班...`
    // 模拟一个方法 myTemplate(str) 最终输出 'lala在北京海淀区上班...'
    function myTemplate(str) {
        // try it
    }
    console.log(myTemplate(str)) // lala在北京海淀区上班...
```

- **实现标签化模板(自定义模板规则)。**

```js
    const name = 'cc'
    const gender = 'male'
    const hobby = 'basketball'
    // 实现tag最终输出 '姓名：**cc**，性别：**male**，爱好：**basketball**'
    function tag(strings) {
        // do it
    }
    const str = tag`姓名：${name}，性别：${gender}，爱好：${hobby}`
    console.log(str) // '姓名：**cc**，性别：**male**，爱好：**basketball**'
```

#### 3.函数

**函数默认参数**

在ES5我们给函数定义参数默认值是怎么样？

```js
    function action(num) {
        num = num || 200
        //当传入num时，num为传入的值
        //当没传入参数时，num即有了默认值200
        return num
    }
```

但细心观察的同学们肯定会发现，num传入为0的时候就是false，但是我们实际的需求就是要拿到num = 0，此时num = 200 明显与我们的实际想要的效果明显不一样

ES6为参数提供了默认值。在定义函数时便初始化了这个参数，以便在参数没有被传递进去时使用。

```js
    function action(num = 200) {
        console.log(num)
    }
    action(0) // 0
    action() //200
    action(300) //300
```

**箭头函数**

ES6很有意思的一部分就是函数的快捷写法。也就是箭头函数。

箭头函数最直观的三个特点。

- 不需要 `function` 关键字来创建函数
- 省略 `return` 关键字
- 继承当前上下文的 `this` 关键字

```js
//例如：
    [1,2,3].map(x => x + 1)
    
//等同于：
    [1,2,3].map((function(x){
        return x + 1
    }).bind(this))
```

**说个小细节。**

当你的函数**有且仅有**一个参数的时候，是可以省略掉括号的。当你函数返回**有且仅有**一个表达式的时候可以省略{} 和 return；例如:

```js
    var people = name => 'hello' + name
    //参数name就没有括号
```

作为参考

```
    var people = (name, age) => {
        const fullName = 'hello' + name
        return fullName
    } 
    //如果缺少()或者{}就会报错
```

要不整一道笔试题？哈哈哈哈哈哈哈哈。我不管我先上代码了

```js
    // 请使用ES6重构以下代码
    
    var calculate = function(x, y, z) {
      if (typeof x != 'number') { x = 0 }
      if (typeof y != 'number') { y = 6 }

      var dwt = x % y
      var result

      if (dwt == z) { result = true }
      if (dwt != z) { result = false }
      
      return result
    }
    const calculate = (x, y, z) => {
      x = typeof x !== 'number' ? 0 : x
      y = typeof y !== 'number' ? 6 : y
      return x % y === z
    }
```

#### 4.拓展的对象功能

对象初始化简写

ES5我们对于对象都是以**键值对**的形式书写，是有可能出现键值对重名的。例如：

```js
    function people(name, age) {
        return {
            name: name,
            age: age
        };
    }
```

键值对重名，ES6可以简写如下：

```js
    function people(name, age) {
        return {
            name,
            age
        };
    }
```

ES6 同样改进了为对象字面量方法赋值的语法。ES5为对象添加方法：

```
    const people = {
        name: 'lux',
        getName: function() {
            console.log(this.name)
        }
    }
```

ES6通过省略冒号与 `function` 关键字，将这个语法变得更简洁

```js
    const people = {
        name: 'lux',
        getName () {
            console.log(this.name)
        }
    }
```

**ES6 对象提供了 `Object.assign()`这个方法来实现浅复制。**
`Object.assign()` 可以把任意多个源对象**自身可枚举**的属性拷贝给目标对象，然后返回目标对象。第一参数即为目标对象。在实际项目中，我们为了不改变源对象。一般会把目标对象传为{}

```js
    const objA = { name: 'cc', age: 18 }
    const objB = { address: 'beijing' }
    const objC = {} // 这个为目标对象
    const obj = Object.assign(objC, objA, objB)

    // 我们将 objA objB objC obj 分别输出看看
    console.log(objA)   // { name: 'cc', age: 18 }
    console.log(objB) // { address: 'beijing' }
    console.log(objC) // { name: 'cc', age: 18, address: 'beijing' }
    console.log(obj) // { name: 'cc', age: 18, address: 'beijing' }

    // 是的，目标对象ObjC的值被改变了。
    // so，如果objC也是你的一个源对象的话。请在objC前面填在一个目标对象{}
    Object.assign({}, objC, objA, objB)
```

#### 5.更方便的数据访问--解构

数组和对象是JS中最常用也是最重要表示形式。为了简化提取信息，ES6新增了**解构**，这是将一个数据结构分解为更小的部分的过程

ES5我们提取对象中的信息形式如下：

```js
    const people = {
        name: 'lux',
        age: 20
    }
    const name = people.name
    const age = people.age
    console.log(name + ' --- ' + age)
```

是不是觉得很熟悉，没错，在ES6之前我们就是这样获取对象信息的，一个一个获取。现在，解构能让我们从对象或者数组里取出数据存为变量，例如

```js
    //对象
    const people = {
        name: 'lux',
        age: 20
    }
    const { name, age } = people
    console.log(`${name} --- ${age}`)
    //数组
    const color = ['red', 'blue']
    const [first, second] = color
    console.log(first) //'red'
    console.log(second) //'blue'
```

要不来点儿面试题，看看自己的掌握情况？

```js
    // 请使用 ES6 重构一下代码

    // 第一题
    var jsonParse = require('body-parser').jsonParse

    // 第二题
    var body = request.body
    var username = body.username
    var password = body.password
    // 1.
    import { jsonParse } from 'body-parser'
    // 2. 
    const { body, body: { username, password } } = request
```

#### 6.Spread Operator 展开运算符

ES6中另外一个好玩的特性就是Spread Operator 也是三个点儿...接下来就展示一下它的用途。

组装对象或者数组

```js
    //数组
    const color = ['red', 'yellow']
    const colorful = [...color, 'green', 'pink']
    console.log(colorful) //[red, yellow, green, pink]
    
    //对象
    const alp = { fist: 'a', second: 'b'}
    const alphabets = { ...alp, third: 'c' }
    console.log(alphabets) //{ "fist": "a", "second": "b", "third": "c"
}
```

有时候我们想获取数组或者对象除了前几项或者除了某几项的其他项

```js
    //数组
    const number = [1,2,3,4,5]
    const [first, ...rest] = number
    console.log(rest) //2,3,4,5
    //对象
    const user = {
        username: 'lux',
        gender: 'female',
        age: 19,
        address: 'peking'
    }
    const { username, ...rest } = user
    console.log(rest) //{"address": "peking", "age": 19, "gender": "female"
}
```

对于 Object 而言，还可以用于组合成新的 Object 。(ES2017 stage-2 proposal) 当然如果有重复的属性名，右边覆盖左边

```js
    const first = {
        a: 1,
        b: 2,
        c: 6,
    }
    const second = {
        c: 3,
        d: 4
    }
    const total = { ...first, ...second }
    console.log(total) // { a: 1, b: 2, c: 3, d: 4 }
```

#### 7.import 和 export

import导入模块、export导出模块

```js
//全部导入
import people from './example'

//有一种特殊情况，即允许你将整个模块当作单一对象进行导入
//该模块的所有导出都会作为对象的属性存在
import * as example from "./example.js"
console.log(example.name)
console.log(example.age)
console.log(example.getName())

//导入部分
import {name, age} from './example'

// 导出默认, 有且只有一个默认
export default App

// 部分导出
export class App extend Component {};
```

以前有人问我，导入的时候有没有大括号的区别是什么。下面是我在工作中的总结：

```js
1.当用export default people导出时，就用 import people 导入（不带大括号）

2.一个文件里，有且只能有一个export default。但可以有多个export。

3.当用export name 时，就用import { name }导入（记得带上大括号）

4.当一个文件里，既有一个export default people, 又有多个export name 或者 export age时，导入就用 import people, { name, age } 

5.当一个文件里出现n多个 export 导出很多模块，导入时除了一个一个导入，也可以用import * as example
```

#### 8. Promise

> 在promise之前代码过多的回调或者嵌套，可读性差、耦合度高、扩展性低。通过Promise机制，扁平化的代码机构，大大提高了代码可读性；用同步编程的方式来编写异步代码，保存线性的代码逻辑，极大的降低了代码耦合性而提高了程序的可扩展性。

**说白了就是用同步的方式去写异步代码。**

发起异步请求

```js
    fetch('/api/todos')
      .then(res => res.json())
      .then(data => ({ data }))
      .catch(err => ({ err }));
```

今天看到一篇关于面试题的很有意思。

```js
setTimeout(function() {
    console.log(1) // 5
}, 0);
new Promise(function executor(resolve) {
    console.log(2); // 1
    for( var i=0 ; i<10000 ; i++ ) {
        i == 9999 && resolve();
    }
    console.log(3); //2
})
.then(function() {
console.log(4); //4
})
console.log(5); //3

// 2 3 5 4 1
```

[Excuse me？这个前端面试在搞事！](https://zhuanlan.zhihu.com/p/25407758)  *********这道题不错

#### 9.Generators

生成器（ generator）是能返回一个**迭代器**的函数。**生成器函数也是一种函数**，最直观的表现就是比普通的function多了个星号*，在其函数体内可以使用yield关键字,有意思的是**函数会在每个yield后暂停**。

这里生活中有一个比较形象的例子。咱们到银行办理业务时候都得向大厅的机器取一张排队号。你拿到你的排队号，机器并不会自动为你再出下一张票。也就是说取票机“暂停”住了，直到下一个人再次唤起才会继续吐票。

OK。说说迭代器。**当你调用一个generator时，它将返回一个迭代器对象**。**这个迭代器对象拥有一个叫做next的方法来帮助你重启generator函数并得到下一个值**。next方法不仅返回值，它返回的对象具有两个属性：done和value。value是你获得的值，done用来表明你的generator是否已经停止提供值。继续用刚刚取票的例子，每张排队号就是这里的value，打印票的纸是否用完就这是这里的done。

```js
    // 生成器
    function *createIterator() {
        yield 1;
        yield 2;
        yield 3;
    }
    
    // 生成器能像正规函数那样被调用，但会返回一个迭代器
    let iterator = createIterator();
    
    console.log(iterator.next().value); // 1
    console.log(iterator.next().value); // 2
    console.log(iterator.next().value); // 3
```

那生成器和迭代器又有什么用处呢？

围绕着生成器的许多兴奋点都与**异步编程**直接相关。异步调用对于我们来说是很困难的事，我们的函数并不会等待异步调用完再执行，你可能会想到用回调函数，（当然还有其他方案比如Promise比如Async/await）。

**生成器可以让我们的代码进行等待。**就不用嵌套的回调函数。使用generator可以确保当异步调用在我们的generator函数运行一下行代码之前完成时暂停函数的执行。

那么问题来了，咱们也不能手动一直调用next()方法，你需要一个能够调用生成器并启动迭代器的方法。就像这样子的

```js
    function run(taskDef) { //taskDef即一个生成器函数

        // 创建迭代器，让它在别处可用
        let task = taskDef();

        // 启动任务
        let result = task.next();
    
        // 递归使用函数来保持对 next() 的调用
        function step() {
    
            // 如果还有更多要做的
            if (!result.done) {
                result = task.next();
                step();
            }
        }
    
        // 开始处理过程
        step();
    
    }
```

> 生成器与迭代器最有趣、最令人激动的方面，或许就是可创建外观清晰的异步操作代码。你不必到处使用回调函数，而是可以建立貌似同步的代码，但实际上却使用 yield 来等待异步操作结束。

#### 10.数组方法

##### 1.数组的创建

1. **使用`Array`构造函数的方式**

```js
new Array();  // 创建一个数组
new Array([size]);  // 创建一个数组并指定长度，注意不是上限，是长度
new Array(element0, element1, ..., elementn);  // 创建一个数组并赋值

const array = new Array();
array[0] = '1';
```

1. **采用字面量的方法**

```js
const array = []; //创建一个空数组
const array2 = [1, 2, 3]; //创建一个有三个元素的数组
```

在使用数组字面量表示法时，**不会调用`Array`构造函数**。

##### 2.数组自带属性

```js
constructor // 返回创建数组对象的原型函数
length // 返回数组对象的长度
prototype // 可以增加数组的原型方法和属性
```

**关于数组的length属性**

（1）关于数组的的`length`属性，**这个属性不是只读的，数组的该属性可读可写**；通过设置这个属性，可以从数组的末尾移除项或向数组中添加新项。

```js
// 将其 length 属性设置为 2 会移除最后一项结果再访问 colors[2]就会显示 undefined 了
var colors = ["red", "blue", "green"];     // 创建一个包含 3 个字符串的数组 
colors.length = 2; 
console.log(colors[2]);                 //undefined
```

（2）如果将其 `length` 属性设置为大于数组 项数的值，则新增的每一项都会取得`undefined` 值。

```js
var colors = ["red", "blue", "green"];    // 创建一个包含 3 个字符串的数组 
colors.length = 4; 
console.log(colors[3]);                 //undefined
```

（3）利用 `length` 属性可以方便地在数组末尾添加新项。

```js
var colors = ["red", "blue", "green"];   // 创建一个包含 3 个字符串的数组 
colors[colors.length] = "black";         //（在位置 3）添加一种颜色
 colors[colors.length] = "brown";         //（在位置 4）再添加一种颜色
```

##### 3.检测是否为数组

1. **使用`instanceof`方法**

`instanceof` 用于判断一个变量是否是某个对象的实例

```js
const array = new Array();
array instanceof Array; //true
```

2.**使用`constructor`属性**

`constructor` 属性返回对创建此对象的数组函数的引用，就是返回对象相对应的构造函数。

```js
const array = new Array();
array.constructor === Array; // true
```

**3.使用`isArray()`方法**

对支持`isArray`的浏览器,直接使用`isArray`方法。

```js
const array = new Array();
Array.isArray(array); //true
```

如果浏览器不支持`Array.isArray()`则需进行必要判断。

```js
/**
 * 判断一个对象是否是数组，参数不是对象或者不是数组，返回false
 *
 * @param {Object} arg 需要测试是否为数组的对象
 * @return {Boolean} 传入参数是数组返回true，否则返回false
 */
function isArray(arg) {
    if (typeof arg === 'object') {
        return Object.prototype.toString.call(arg) === '[object Array]';
    }
    return false;
}
```

##### 4.数组元素的增加与删除

1.  `array.push(e1, e2, ...eN)` 将一个或多个元素添加到数组的末尾，并返回新数组的长度。

```js
const array = [1, 2, 3];
const length = array.push(4, 5);
// array: [1, 2, 3, 4, 5]; length: 5
```

2.`array.unshift(e1, e2, ...eN)`将一个或多个元素添加到数组的开头，并返回新数组的长度。

```js
const array = [1, 2, 3];
const length = array.unshift(4, 5);
// array: [ 4, 5, 1, 2, 3]; length: 5
```

3. `array.pop()`从数组中删除最后一个元素，并返回最后一个元素的值，原数组的最后一个元素被删除。数组为空时返回`undefined`。

```js
const array = [1, 2, 3];
const poped = array.pop();  
// array: [1, 2]; poped: 3
```

4. `array.shift()`删除数组的第一个元素，并返回第一个元素，原数组的第一个元素被删除。数组为空时返回`undefined`。

```js
const array = [1, 2, 3];
const shifted = array.shift();  
// array: [2, 3]; shifted: 1
```

5.`array.splice(start[, deleteCount, item1, item2, ...])`从数组中添加/删除元素，**返回值是由被删除的元素组成的一个新的数组**，如果只删除了一个元素，则返回只包含一个元素的数组。如果没有删除元素，则返回空数组。

**会改变原数组**

> -  `start` 指定修改的开始位置（从0计数）。如果超出了数组的长度，则从数组末尾开始添加内容；如果是负值，则表示从数组末位开始的第几位（从1计数）。
> -  `deleteCount` (可选)，从`start`位置开始要删除的元素个数。如果 `deleteCount` 是 0，则不移除元素。这种情况下，至少应添加一个新元素。如果`deleteCount`大于`start`之后的元素的总数，则从`start`后面的元素都将被删除（含第 `start` 位）。
> -  `item1, item2, …`(可选)，要添加进数组的元素,从`start`位置开始。如果不指定，则 `splice()` 将只删除数组元素。

```js
const array = [1, 2, 3, 4, 5];

const deleted = array.splice(2, 0, 6); // 在索引为2的位置插入6
// array 变为 [1, 2, 6, 3, 4, 5]; deleted为[]
```

##### 5.数组与字符串的相互转化

1. **数组转字符串**

`array.join(separator=',')`将数组中的元素通过`separator`连接成字符串，并返回该字符串，separator默认为","。

**返回字符串，不会改变原数组**

```js
const array = [1, 2, 3];
let str = array.join(',');
// str: "1,2,3"
```

`toLocaleString()`、`toString()`、`valueOf()`：所有对象都具有这三个方法，数组继承的这个三个方法，可以看作是`join()`的特殊用法，不常用。

```js
var colors = ["red", "blue", "green"];   
// 调用数组的 toString()方法会返回由数组中每个值的字符串形式拼接而成的一个以逗号分隔的字符串
console.log(colors.toString());     // red,blue,green
// 调用 valueOf()返回的还是数组
console.log(colors.valueOf());      // ["red", "blue", "green"]
console.log(colors.toLocaleString()); //  red,blue,green
```

如果数组中的某一项的值是 `null` 或者 `undefined`，那么该值在`join()`、 `toLocaleString()`、`toString()`和 `valueOf()`方法返回的结果中以**空字符串**表示。

2.**字符串转数组**

> `string.split(separator,howmany)`用于把一个字符串分割成字符串数组。
>  `separator` (必需)，字符串或正则表达式，从该参数指定的地方对字符串进行分割。
>  `howmany` (可选)，该参数可指定返回的数组的最大长度。

```js
let str = "abc,abcd,aaa";
let array = str.split(",");// 在每个逗号(,)处进行分解。
// array: [abc,abcd,aaa]

const array1 = "helloworld";
let str1 = s1.split('');  
//["h", "e", "l", "l", "o", "w", "o", "r", "l", "d"]
```

##### 6.数组的截取和合并

1. 数组的截取 -  `array.slice(start, end)` 方法

`slice()`通过索引位置，从数组中返回`start`下标开始，直到`end`下标结束（**不包括**）的新数组，该方法**不会修改原数组，只是返回一个新的子数组**。

> -  `start` (必填)，设定新数组的起始位置（下标从0开始算起）；如果是负数，则表示从数组尾部开始算起（**-1 指最后一个元素**，-2 指倒数第二个元素，以此类推）。
> -  `end` (可选)，设定新数组的结束位置；如果不填写该参数，默认到数组结尾；如果是负数，则表示从数组尾部开始算起（-1 指最后一个元素，-2
>    指倒数第二个元素，以此类推）。

```js
// 获取仅包含最后一个元素的子数组
let array = [1,2,3,4,5];
array.slice(-1); // [5]

// 获取不包含最后一个元素的子数组
let array2 = [1,2,3,4,5];
array2.slice(0, -1); // [1,2,3,4]
```

**该方法并不会修改数组，而是返回一个子数组。如果想删除数组中的一段元素，应该使用方法 array.splice()。**

1. 数组的合并 - `array.concat([item1[, item2[, . . . [,itemN]]]])`方法

`conact()`是将多个数组（也可以是字符串，或者是数组和字符串的混合）连接为一个数组，返回连接好的新的数组。**不改变原数组**

```js
const array = [1,2].concat(['a', 'b'], ['name'],7);
// [1, 2, "a", "b", "name",7]
```

##### 7.数组元素的排序

1.  `array.sort()`方法

`sort()`方法用于对数组的元素进行排序，并**返回原数组**。如果不带参数，按照字符串`UniCode`码的顺序进行排序。

```js
const array = ['a', 'd', 'c', 'b'];
array.sort();  //['a', 'b', 'c', 'd']
```

为`sort()`中传入排序规则函数可实现自定义排序。

> 排序函数规则：(1)**传两个形参**；(2)**当返回值为正数时，交换传入两形参在数组中位置**。

**按照数值大小进行排序-升序**

```js
[1, 8, 5].sort((a, b) => {
  return a-b; // 从小到大排序
});
// [1, 5, 8]
```

**按照数值大小进行排序-降序**

```js
[1, 8, 5].sort((a, b) => {
  return b-a; // 从大到小排序
});
// [8, 5, 1]
```

1.  `array.reverse()`方法
    `reverse()` 方法将数组中**元素的位置颠倒**，第一个数组元素成为最后一个数组元素，最后一个数组元素成为第一个。**在原数组上操作，然后返回原数组**。

```js
let arr = [1,2,3,4,5]
console.log(arr.reverse())    // [5,4,3,2,1]
console.log(arr)    // [5,4,3,2,1]
```

**数组的sort()和reverse()方法都对原数组进行了修改，返回值是经过排序之后的数组。**

##### 8.元素在数组中的位置

1.  `indexOf()`与`lastIndexOf()` 

> -  `indexOf(searchElement[, fromIndex = 0])` 方法返回某个指定的字符串值在字符串中**首次出现的位置**。
> -  `lastIndexOf(searchElement[, fromIndex = 0])` 方法返回一个指定的字符串值**最后出现的位置**，在一个字符串中的指定位置从后向前搜索。
> - 这两个方法都接受两个参数：`searchElement`：要查找的元素；`fromIndex`：开始查找的索引位置。
> - 这两个方法**都返回查找的项在数组中的位置，或者在没找到的情况下返回-1**。

```js
[2, 9, 7, 8, 9].indexOf(9); // 1
[2, 9, 7, 8, 9].lastIndexOf(9); // 4
```

1.  `find()` 与 `findIndex()` 

(1) `find(callback[, thisArg])`方法，用于**找出第一个符合条件的数组元素**。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为true的成员，然后返回该成员。如果没有符合条件的成员，则返回`undefined`。

```js
[1, 4, -5, 10].find((n) => n < 0)
// -5
```

`find()`方法的回调函数可以接受三个参数，依次为当前的值、当前的位置和原数组。

```js
[1, 5, 10, 15].find(function(value, index, arr) {
  return value > 9;
}) // 10
```

(2) `findIndex(callback[, thisArg])`返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。

```js
[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2
```

这两个方法都可以接受第二个参数，用来绑定回调函数的`this`对象。

```js
function f(v){
  return v > this.age;
}
let person = {name: 'John', age: 20};
[10, 12, 26, 15].find(f, person);    // 26
```

1.  `includes(searchElement[, fromIndex = 0])`方法返回一个布尔值，表示某个数组是否包含给定的值。

这个方法都接受两个参数：`searchElement`：要查找的元素；`fromIndex`：开始查找的索引位置。

```js
[1, 2, 3].includes(2)     // true
[1, 2, 3].includes(3, 3);  // false
[1, 2, 3].includes(3, -1); // true   node里报错
```

##### 9.数组的遍历与迭代

1.  `array.filter(callback, thisArg)`方法使用指定的函数测试所有元素,并创建一个包含所有通过测试的元素的新数组。

参数说明：

> -  `callback` 用来测试数组的每个元素的函数，返回`true`表示保留该元素（通过测试），`false`则不保留。
> -  `thisArg` 可选。执行 `callback` 时的用于 `this` 的值。

```js
// callback定义如下，三个参数： element:当前元素值；index：当前元素下标； array:当前数组
function callback(element, index, array) {
  // callback函数必须返回true或者false，返回true保留该元素，false则不保留。
  return true || false;
}

const filtered = [1, 2, 3].filter(element => element > 1);
// filtered: [2, 3];
```

1.  `array.every(callback[, thisArg])`方法检测数组中的每一个元素是否都通过了`callback`测试，全部通过返回`true`，否则返回`false`。

```js
// callback定义如下： element:当前元素值；index：当前元素下标； array:当前数组
function callback(element, index, array) {
  // callback函数必须返回true或者false告知every是否通过测试
  return true || false;
}

let a = [1, 2, 3, 4, 5];
let b = a.every((item) => {
    return item > 0;
});
let c = a.every((item) => {
    return item > 1;
});
console.log(b); // true
console.log(c); // false
```

1.  `array.some(callback[, thisArg])`判断数组中是否包含可以通过`callback`测试的元素，与`every`不同的是，这里只要某一个元素通过测试，即返回`true`。`callback`定义同上。

```js
[2, 5, 8, 1, 4].some(item => item > 6);
// true
```

1.  `array.map(callback[, thisArg])`方法返回一个由原数组中的每个元素调用`callback`函数后的返回值组成的新数组。

```js
let a = [1, 2, 3, 4, 5];

let b = a.filter((item) => {
    return item > 3;
});
console.log(b); // [4 ,5]

let bb = [];
let str1 = a.map((item) => {
    if (item > 3) {
        bb.push(item);
        return true // 返回值是一个return后面的东西组成的数组 
    }
});
console.log(a);    // [1, 2, 3, 4, 5] 不改变原数组
console.log(bb);    // [4, 5]        
console.log(str1);    // [ undefined, undefined, undefined, true, true ]

let bbb = a.map((item) => {
    return item + 1;
});
console.log(bbb);   // [2, 3, 4, 5, 6]
```

1.  `array.forEach(callbak)`为数组的每个元素执行对应的方法。

```js
// callback定义如下： element:当前元素值；index：当前元素下标； array:当前数组

不改变原值 无返回值 没有return 
let a = [1, 2, 3, 4, 5];

let b = [];
let ss = a.forEach((item) => {
    b.push(item + 1);
});
console.log(b); // [2,3,4,5,6]
console.log(a)  // [1,2,3,4,5]
console.log(ss) // undefined
```

1. **遍历数组的方法：`entries()`、`values()`、`keys()**` 

这三个方法都是返回一个遍历器对象，可用`for...of`循环遍历，唯一区别：`keys()`是对键名的遍历、`values()`对键值的遍历、`entries()`是对键值对的遍历。

```js
for(let item of ['a','b'].keys()){
    console.log(item);
    //0
    //1
}
for(let item of ['a','b'].values()){
    console.log(item);
    //'a'
    //'b'
}
let arr4 = [0,1];
for(let item of arr4.entries()){
    console.log(item);  
    //  [0, 0]
    //  [1, 1]
}
for(let [index,item] of arr4.entries()){
    console.log(index+':'+item);
    //0:0
    //1:1
}
```

1.  `array.reduce(callback[, initialValue])`方法返回针对数组每项调用`callback`函数后产生的累积值。

```js
const total = [0, 1, 2, 3].reduce((sum, value) => {
  return sum + value;
}, 0);
// total is 6

const flattened = [[0, 1], [2, 3], [4, 5]].reduce((a, b) => {
  return a.concat(b);
}, []);
// flattened is [0, 1, 2, 3, 4, 5]
```

参数说明：`initialValue`：累加器初始值， `callback`函数定义如下：

```js
function callback(accumulator, currentValue, currentIndex, array) {
}
```

以上`callback`的参数中`accumulator`代表累加器的值，初始化时，如果`initialValue`有值，则`accumulator`初始化的值为`initialValue`，整个循环从第一个元素开始；`initialValue`无值，则`accumulator`初始化的值为数组第一个元素的值，`currentValue`为数组第二个元素的值，整个循环从第二个元素开始。`initialValue`的数据类型可以是任意类型，不需要跟原数组内的元素值类型一致。

```js
const newArray = [{ name: 'aa', age: 1 }, { name: 'bb', age: 2 }, { name: 'cc', age: 3 }].reduce((arr, element) => {
  if (element.age >= 2) {
    arr.push(element.name);
  }
  return arr; 
  // 必须有return，因为return的返回值会被赋给新的累加器，否则累加器的值会为undefined。
}, []);
// newArray is ["bb", "cc"];

// 上面代码的同等写法：
const newArray = [{ name: 'aa', age: 1 }, { name: 'bb', age: 2 }, { name: 'cc', age: 3 }].filter(element => element.age >= 2).map(item => item.name);
// newArray is ["bb", "cc"];
```

##### 10.其他方法

1.  `Array.from()`方法

`Array.from()`方法是用于将类似数组的对象（**即有length属性的对象**）和可遍历对象转为真正的数组。
 比如，使用·Array.from()·方法，可以轻松将·JSON·数组格式转为数组。

```js
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
```

1.  `Array.of()`方法

`Array.of()`方法是将一组值转变为数组。

```js
let arr0 = Array.of(1,2,33,5);
console.log(arr0);//[1,2,33,5]

let arr1 = Array.of('你好','hello');
console.log(arr1);//["你好", "hello"]
```

#### 11.class

#### 12.set--集合

```js
let arr = [1,2,2,2,NaN,NaN,undefined,undefined,null,null,1233,22,33,33]
let arr2 = new Set(arr)
arr2 = Array.from(arr2) // Set { 1, 2, NaN, undefined, null, 1233, 22, 33 }
let arr2 = [...new Set(arr)] // [ 1, 2, NaN, undefined, null, 1233, 22, 33 ]
console.log(arr2) // [ 1, 2, NaN, undefined, null, 1233, 22, 33 ]
```

ES6 提供了新的数据结构 Set。

**1.特性**

似于数组，但它的一大特性就是所有元素都是唯一的，没有重复。

我们可以利用这一唯一特性进行数组的去重工作。

单一数组的去重。

```js
let set6 = new Set([1, 2, 2, 3, 4, 3, 5])
console.log('distinct 1:', set6)
```

结果：

```js
distinct 1: Set { 1, 2, 3, 4, 5 }
```

多数组的合并去重

```js
let arr1 = [1, 2, 3, 4]
let arr2 = [2, 3, 4, 5, 6]
let set7 = new Set([...arr1, ...arr2])
console.log('distinct 2:', set7)
```

结果：

```js
distinct 2: Set { 1, 2, 3, 4, 5, 6 }
```

 

**2.操作**

1.向Set中添加元素。

```js
let set1 = new Set()
set1.add(1)
set1.add(2)
set1.add(3)
console.log('added:', set1)
```

结果：

```js
added: Set { 1, 2, 3 }
```

 

2.从Set中删除元素。

```js
let set1 = new Set()
set1.add(1)
set1.add(2)
set1.add(3)
set1.delete(1)
console.log('deleted:', set1)
```

结果：

```js
deleted: Set { 2, 3 }
```

3.判断某元素是否存在。

```js
let set1 = new Set()
set1.add(1)
set1.add(2)
set1.add(3)
set1.delete(1)
console.log('has(1):', set1.has(1))
console.log('has(2):', set1.has(2))
```

结果：

```js
has(1): false
has(2): true
```

4.清除所有元素。

```js
let set1 = new Set()
set1.add(1)
set1.add(2)
set1.add(3)
set1.clear()
console.log('cleared:', set1)
```

结果：

```js
cleared: Set {}
```

**3.Set和Array互转**

1.数组转Set

```js
let set2 = new Set([4,5,6])
console.log('array to set 1:', set2)

let set3 = new Set(new Array(7, 8, 9))
console.log('array to set 2:', set3)
```

结果：

```js
array to set 2: Set { 4, 5, 6 }
array to set 3: Set { 7, 8, 9 }
```

2.Set转数组

```js
let set4 = new Set([4, 5, 6])
console.log('set to array 1:', [...set4])
console.log('set to array 2:', Array.from(set4))
```

结果：

```js
set to array 1: [ 4, 5, 6 ]
set to array 2: [ 4, 5, 6 ]
```

**4.遍历**

可以使用Set实例对象的keys()，values()，entries()方法进行遍历。

由于**Set的键名和键值是同一个值**，它的每一个元素的key和value是相同的，所有keys()和values()的返回值是相同的，entries()返回的元素中的key和value是相同的。

```js
let set5 = new Set([4, 5, 'hello'])
console.log('iterate useing Set.keys()')
for(let item of set5.keys()) {
  console.log(item)
}

console.log('iterate useing Set.values()')
for(let item of set5.values()) {
  console.log(item)
}

console.log('iterate useing Set.entries()')
for(let item of set5.entries()) {
  console.log(item)
}
```

结果：

```js
iterate useing Set.keys()
4
5
hello

iterate useing Set.values()
4
5
hello

iterate useing Set.entries()
[ 4, 4 ]
[ 5, 5 ]
[ 'hello', 'hello' ]
```

**其他特性**

在向Set加入值时，Set不会转换数据类型，内部在判断元素是否存在时用的类似于精确等于(===)的方法，“2”和2是不同的，NaN等于其自身。

```js
let set8 = new Set()
set8.add(NaN)
set8.add(NaN)
console.log('NaN===Nan is true:', set8)
```

结果：

```js
NaN===Nan is true: Set { NaN }
```

> forEach()方法

可以使用`forEach`方法来遍历Set中的数据项，该方法传入一个回调函数`callback`，还可以传入一个`this`，用于回调函数之中：

回调函数callback中有三个参数：

1. 元素值；

2. 元素索引；

3. 将要遍历的对象；

   ```js
    let set = new Set([1,2,3,3,3,3]);
    set.forEach(function (value,key,ownerSet) {
        console.log(value);
        console.log(key);           
    })
   ```

Set中的value和key是相同的，这是为了让Set的forEach方法和数组以及Map的forEach方法保持一致，都具有三个参数。

在forEach方法中传入`this`，给回调函数使用：

```js
let set = new Set([1,2,3,3,3,3]);
let operation = {

    print(value){
        console.log(value);
    },
    iterate(set=[]){
        set.forEach(function(value,key,ownerSet){
            this.print(value);
            this.print(key);
        },this);
    }
}
operation.iterate(set);

// 输出：1 1 2 2 3 3
```

**如果回调函数使用箭头函数的话，就可以省略`this`的入参，这是因为箭头函数会通过作用域链找到当前this对象**，将上面的示例代码使用箭头函数来写：

```
let set = new Set([1,2,3,3,3,3]);
let operation ={

    print(value){
        console.log(value);
    },

    iterate(set=[]){
        set.forEach((value,key)=>{
            this.print(value);
            this.print(key);
        })
    }

}

operation.iterate(set);
```

> 将Set转换成数组

将数组转换成Set十分容易，可以将数组传入Set构造器即可；而将Set转换成数组，需要使用扩展运算符。扩展运算符能将数组中的数据项切分开，作为独立项传入到函数，如果将扩展运算符用于可迭代对象的话，就可以将可迭代对象转换成数组：

```js
let [...arr]=set;
console.log(arr); // [1,2,3]
```

> Weak Set

Set在存放对象时，实际上是存放的是对象的引用，即Set也被称之为Strong Set。如果所存储的对象被置为null，但是Set实例仍然存在的话，对象依然无法被垃圾回收器回收，从而无法释放内存：

```js
let set = new Set();
let key={};
let key2 = {};
set.add(key);
set.add(key2);
console.log(set.size); //2

key=null;
console.log(set.size); //2
```

可以看出就算对象key置为null，但是由于是强引用的方式，Set实例还存在，对象key依然不会被回收。

如果想让对象key正常释放的话，可以使用Weak Set，此时，**存放的是对象的弱引用，当对象只被Set弱引用的话，并不会阻止对象实例被回收**。**Weka Set同Set的用法几乎一致**。可以使用add()方法增加数据项，使用has()方法检查Weak Set中是否包含某项，以及使用delete()方法删除某一项。

```js
let set = new WeakSet();
let key = {};   
set.add(key);
console.log(set.has(key)); //true
set.delete(key);
console.log(set.has(key)); //false
```

但需要注意的是：**Weak Set构造器不接受基本类型数据，只接受对象**。同样的可以使用可迭代的对象如数组，来作为构造器参数，来创建Weak Set。

> Weak Set和Set之间的差异

对于Weak Set和Set之间的**重要差异**：

1. 对于Weak Set实例，若调用了add()方法时传入了非对象的参数，则会抛出错误。如果在has()或者delete()方法中传入了非对象的参数则会返回false；
2. Weak Set不可迭代，因此不能用于for-of循环；
3. Weak Set 无法暴露出任何迭代器（例如 keys() 与 values() 方法） ，因此没有任何编程手段可用于判断 Weak Set 的内容；
4. Weak Set没有forEach()方法；
5. Weak Set没有size属性；

#### 13.map--字典

**数据结构map基本概念**

字典map是用来储存不重复的key的hash结构。不同于集合set的是字典map使用的是[键，值]的形式储存数据的。**JavaScript的对象只能使用字符串当做key，这给它的使用带来了很大的限制**，为了解决这个问题，ES6提供了map数据结构。它类似于对象。是键值对的集合，但是键的返回不限制于字符串，各种类型的值都可以作为键，**也就是说Object提供了key---value的对应。map结构提供了value，value的对应关系，是一种更完善的hash结构。**

------

首先我们来回顾一下对象的知识：

```js
var data1={a:1},data2={b:2},obj={};
obj[data1] = 1;
obj[data2] = 2;
console.log(obj); //{[object Object]: 2}
console.log(data1.toString() == data2.toString()); // true
```

如何创建一个map；

```js
const map = new Map([['a',1],['b',2]]);
console.log(map);  //{"a" => 1, "b" => 2}
```

map自带的属性
 `console.log(map.size) // 2`
 map类的方法
 1 set(key, value) 设置键名key对应的键值为value，然后返回整个 Map 结构。如果key已经有值，则键值会被更新，否则就新生成该键。

```js
map.set('qwe', '123').set('new', 'fq').set('yu', 'li');
console.log(map); //{"a" => 1, "b" => 2, "qwe" => "123", "new" => "fq", "yu" => "li"}
```

2 get(key) get方法读取key对应的键值，如果找不到 key，返回undefined。

```js
console.log(map.get('new')); //true
console.log(map.get('x')); //false
```

3 delete(key) 删除某个键，返回true。如果删除失败，返回false。

```js
console.log(map.delete('a')); //true
console.log(map); //{ "b" => 2, "qwe" => "123", "new" => "fq", "yu" => "li"}
console.log(map.delete('a')); //false
```

4 has(key) 方法返回一个布尔值，表示某个键是否在当前Map对象之中。

```js
console.log(map.has('yu')); //true
console.log(map.has('a')); //false
```

5 clear() 清除所有数据，没有返回值。
 `map.clear(); console.log(map); // {}`
 6 keys() 返回键名的遍历器
 `console.log(map.keys());`
 7 values() 返回键值的遍历器
 `console.log(map.values());`
 8 entries() 返回键值对的遍历器
 `console.log(map.entries());`
 9 forEach() 使用回调函数遍历每个成员

```js
map.forEach(function (key, value, map){
   console.log(key + ':' + value);
 })
```

Map 在使用过程中的一些注意事项：

```js
map.set(NaN, 10).set(NaN, 100); 
console.log(map); //{NaN => 100}
map.set({}, 'x').set({}, 'y');
console.log(map); //{NaN => 100, Object {} => "x", Object {} => "y"}
console.log({} === {});//false
```

map对比的是引用地址，引用地址不同则视为不相同。

> Weak Map

Weak Map对Map而言，就像是Weak Set相对于Set一样：**Weak Map(或者Weak Set)都是存储对象弱引用的方式，在Weak Map（或者Weak Set）中，所有的键都必须是对象（尝试使用非对象的键会抛出错误），而且这些对象都是弱引用，不会干扰到垃圾回收。当Weak Map中的键在Weak Map之外不存在引用时，该键值对会被移除。**

> Weak Map的操作

1.Weak Map的初始化

Weak Map的键必须是对象，值可以是任意类型，初始化同Map一样，也可是使用数组来创建一个 Weak Map ：

```js
//使用数组来创建一个Weak Map
let key = {};
let key2 = {};
let map = new WeakMap([[key,'hello'],[key2,'world']]);
console.log(map.get(key)); //hello
console.log(map.get(key2)); //world
```

2.has方法以及delete方法

与Map一样，可以使用has()方法来检查Weak Map中是否存在某一个键值对，使用delete()方法可以删除一个键值对。clear() 方法不存在，这是因为没必要对键进行枚举，并且枚举 Weak Map 也是不可能的，这与 Weak Set 相同：

```js
let key = {};
let key2 = {};
let map = new WeakMap([[key,'hello'],[key2,'world']]);

map.delete(key);
console.log(map.has(key)); //false
```

> Weak Map 的用法与局限性

当决定是要使用 Weak Map 还是使用正规 Map 时，首要考虑因素在于你是否只想使用对象类型的键。如果你打算这么做，那么最好的选择就是 Weak Map 。因为它能确保额外数据在不再可用后被销毁，从而能优化内存使用并规避内存泄漏。

要记住 Weak Map 只为它们的内容提供了很小的可见度，因此你**不能使用 forEach() 方法、size 属性或 clear() 方法来管理其中的项**。如果你确实需要一些检测功能，那么正规 Map会是更好的选择，只是一定要确保留意内存的使用。

**4. 总结**

1. Set 是无重复值的有序列表。根据 `Object.is()`方法来判断其中的值不相等，以保证无重复。 Set 会自动移除重复的值，因此你可以使用它来过滤数组中的重复值并返回结果。 Set并不是数组的子类型，所以你无法随机访问其中的值。但你可以使用`has()` 方法来判断某个值是否存在于 Set 中，或通过 `size` 属性来查看其中有多少个值。 Set 类型还拥有`forEach()`方法，用于处理每个值。
2. Weak Set 是只能包含对象的特殊 Set 。其中的对象使用弱引用来存储，意味着当 Weak Set中的项是某个对象的仅存引用时，它不会屏蔽垃圾回收。由于内存管理的复杂性， Weak Set的内容不能被检查，因此最好将 Weak Set 仅用于追踪需要被归组在一起的对象。
3. Map 是有序的键值对，其中的键允许是任何类型。与 Set 相似，通过调用 `Object.is()`方法来判断重复的键，这意味着能将数值 5 与字符串 "5" 作为两个相对独立的键。使用`set()` 方法能将任何类型的值关联到某个键上，并且该值此后能用 `get()` 方法提取出来。Map 也拥有一个 `size` 属性与一个 `forEach()` 方法，让项目访问更容易。
4. Weak Map 是只能包含对象类型的键的特殊 Map 。与 Weak Set 相似，键的对象引用是弱引用，因此当它是某个对象的仅存引用时，也不会屏蔽垃圾回收。当键被回收之后，所关联的值也同时从 Weak Map 中被移除。

### ES6--Class

#### class的基本用法

##### 概述

JavaScript语言的传统方法是通过构造函数，定义并生成新对象。下面是一个例子:

```js
//声明一个函数 Point 
function Point(x,y){
    this.x = x;
    this.y = y;
}
//创建构造函数
Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);//p:{x:1,y:2}
```


 ES6提供了更接近传统语言的写法，引入了Class（类）这个概念，作为对象的模板。通过`class`关键字，可以定义类。基本上，ES6的`class`可以看作只是一个语法糖，它的绝大部分功能，ES5都可以做到，新的`class`写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。上面的代码用ES6的“类”改写。

```js
//定义类
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```

上面代码定义了一个“类”，可以看到里面有一个`constructor(构造器)`方法，这就是构造方法，而`this`关键字则代表实例对象。也就是说，ES5的构造函数`Point`，对应ES6的`Point`类的构造方法。
 `Point`类除了构造方法，还定义了一个`toString`方法。注意，定义“类”的方法的时候，**前面不需要加上`function`这个关键字**，直接把函数定义放进去了就可以了。另外，**方法之间不需要逗号分隔**，加了会报错。

```js
class Point {
  // ...
}

typeof Point // "function"
Point === Point.prototype.constructor // true
```

**上面代码表明，类的数据类型就是函数，类本身就指向构造函数。**
 *使用的时候，也是直接对类使用new命令，跟构造函数的用法完全一致。*

```js
class Bar {
  doStuff() {
    console.log('stuff');
  }
}

var b = new Bar();
b.doStuff() // "stuff"
```

构造函数的`prototype`属性，在ES6的“类”上面继续存在。事实上，***<u>类的所有方法都定义在类的`prototype`属性上面</u>***。

```js
class Point {
  constructor(){
    // ...
  }
  toString(){
    // ...
  }
  toValue(){
    // ...
  }
}
// 等同于
Point.prototype = {
  toString(){},
  toValue(){}
};
```

**在类的实例上面调用方法，其实就是调用原型上的方法。**

```js
class B {}
let b = new B();
b.constructor === B.prototype.constructor // true
b.__proto__.constructor === B.prototype.constructor // true
```

上面代码中，`b`是B类的实例，它的`constructor`方法就是B类原型的`constructor`方法。
 由于类的方法都定义在`prototype`对象上面，所以类的新方法可以添加在`prototype`对象上面。`Object.assign`方法可以很方便地一次向类添加多个方法。

```js
class Point {
  constructor(){
    // ...
  }
}

Object.assign(Point.prototype, {
  toString(){},
  toValue(){}
});
```

`prototype`对象的`constructor`属性，直接指向“类”的本身，这与ES5的行为是一致的。

```js
Point.prototype.constructor === Point // true
```

另外，**类的内部所有定义的方法，都是不可枚举的（non-enumerable）**。
 属性的枚举性会影响以下三个函数的结果：**for…in、Object.keys()、Object.values()、JSON.stringify**

```js
class Point {
  constructor(x, y) {
    // ...
  }

  toString() {
    // ...
  }
}

Object.keys(Point.prototype)
// []
Object.getOwnPropertyNames(Point.prototype)
// ["constructor","toString"]
```

上面代码中，`toString`方法是`Point`类内部定义的方法，它是不可枚举的。这一点与ES5的行为不一致。

```js
var Point = function (x, y) {
  // ...
};

Point.prototype.toString = function() {
  // ...
};

Object.keys(Point.prototype)
// ["toString"]
Object.getOwnPropertyNames(Point.prototype)
// ["constructor","toString"]
```

上面代码采用ES5的写法，`toString`方法就是可枚举的。

类的属性名，可以采用表达式。

```js
let methodName = "getArea";
class Square{
  constructor(length) {
    // ...
  }
  [methodName]() {
    // ...
  }
}
```

上面代码中，`Square`类的方法名`getArea`，是从表达式得到的。

#### constructor方法

`constructor`方法是类的默认方法，通过`new`命令生成对象实例时，自动调用该方法。一个类必须有`constructor`方法，如果没有显式定义，一个空的`constructor`方法会被默认添加。

```js
constructor() {}
```

`constructor`方法默认返回实例对象（即`this`），完全可以指定返回另外一个对象。

```js
class Foo {
  constructor() {
    return Object.create(null);
  }
}

new Foo() instanceof Foo
// false
```

上面代码中，`constructor`函数返回一个全新的对象，结果导致实例对象不是`Foo`类的实例。

类的构造函数，不使用`new`是没法调用的，会报错。这是它跟普通构造函数的一个主要区别，后者不用`new`也可以执行。

```js
class Foo {
  constructor() {
    return Object.create(null);
  }
}

Foo()
// TypeError: Class constructor Foo cannot be invoked without 'new'
```

#### 类的实例对象

生成类的实例对象的写法，与ES5完全一样，也是使用`new`命令。如果忘记加上`new`，像函数那样调用`Class`，将会报错。

```js
// 报错
var point = Point(2, 3);

// 正确
var point = new Point(2, 3);
```

与ES5一样，**实例的属性除非显式定义在其本身（即定义在`this`对象上），否则都是定义在原型上（即定义在`class`上）**。

```js
//定义类
class Point {

  constructor(x, y) {
  //实例属性
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }

}

var point = new Point(2, 3);

point.toString() // (2, 3)

point.hasOwnProperty('x') // true
point.hasOwnProperty('y') // true
point.hasOwnProperty('toString') // false
point.__proto__.hasOwnProperty('toString') // true
```

上面代码中，`x`和`y`都是实例对象`point`自身的属性（因为定义在`this`变量上），所以`hasOwnProperty`方法返回`true`，而`toString`是原型对象的属性（因为定义在`Point`类上），所以`hasOwnProperty`方法返回`false`。这些都与ES5的行为保持一致。

与ES5一样，类的所有实例共享一个原型对象。

```js
var p1 = new Point(2,3);
var p2 = new Point(3,2);

p1.__proto__ === p2.__proto__
//true
```

上面代码中，`p1`和`p2`都是Point的实例，它们的原型都是Point.prototype，所以`__proto__`属性是相等的。

这也意味着，可以通过实例的`__proto__`属性为Class添加方法。

```js
var p1 = new Point(2,3);
var p2 = new Point(3,2);

p1.__proto__.printName = function () { return 'Oops' };

p1.printName() // "Oops"
p2.printName() // "Oops"

var p3 = new Point(4,2);
p3.printName() // "Oops"
```

上面代码在`p1`的原型上添加了一个`printName`方法，由于`p1`的原型就是`p2`的原型，因此`p2`也可以调用这个方法。而且，此后新建的实例`p3`也可以调用这个方法。这意味着，**使用实例的`__proto__`属性改写原型，必须相当谨慎，不推荐使用，因为这会改变Class的原始定义，影响到所有实例**。

#### 不存在变量提升

Class不存在变量提升（hoist），这一点与ES5完全不同。

```js
new Foo(); // ReferenceError
class Foo {}
```

上面代码中，`Foo`类使用在前，定义在后，这样会报错，因为ES6不会把类的声明提升到代码头部。这种规定的原因与下文要提到的继承有关，必须保证子类在父类之后定义。

```js
{
  let Foo = class {};
  class Bar extends Foo {
  }
}
```

上面的代码不会报错，因为`Bar`继承`Foo`的时候，`Foo`已经有定义了。但是，如果存在`class`的提升，上面代码就会报错，因为`class`会被提升到代码头部，而`let`命令是不提升的，所以导致`Bar`继承`Foo`的时候，`Foo`还没有定义。

Class表达式

与函数一样，类也可以使用表达式的形式定义。

```js
const MyClass = class Me {
  getClassName() {
    return Me.name;
  }
};
```

上面代码使用表达式定义了一个类。需要注意的是，这个类的名字是`MyClass`而不是`Me`，`Me`只在Class的内部代码可用，指代当前类。

```js
let inst = new MyClass();
inst.getClassName() // Me
Me.name // ReferenceError: Me is not defined
```

上面代码表示，`Me`只在Class内部有定义。

如果类的内部没用到的话，可以省略`Me`，也就是可以写成下面的形式。

```js
const MyClass = class { /* ... */ };
```

采用Class表达式，可以写出立即执行的Class。

```js
let person = new class {
  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
}('张三');

person.sayName(); // "张三"
```

上面代码中，`person`是一个立即执行的类的实例。

私有方法

私有方法是常见需求，但ES6不提供，只能通过变通方法模拟实现。

一种做法是在命名上加以区别。

```js
class Widget {

  // 公有方法
  foo (baz) {
    this._bar(baz);
  }

  // 私有方法
  _bar(baz) {
    return this.snaf = baz;
  }

  // ...
}
```

上面代码中，`_bar`方法前面的下划线，表示这是一个只限于内部使用的私有方法。但是，这种命名是不保险的，在类的外部，还是可以调用到这个方法。

另一种方法就是索性将私有方法移出模块，因为模块内部的所有方法都是对外可见的。

```js
class Widget {
  foo (baz) {
    bar.call(this, baz);
  }

  // ...
}

function bar(baz) {
  return this.snaf = baz;
}
```

上面代码中，`foo`是公有方法，内部调用了`bar.call(this, baz)`。这使得`bar`实际上成为了当前模块的私有方法。

还有一种方法是利用`Symbol`值的唯一性，将私有方法的名字命名为一个`Symbol`值。

```js
const bar = Symbol('bar');
const snaf = Symbol('snaf');

export default class myClass{

  // 公有方法
  foo(baz) {
    this[bar](baz);
  }

  // 私有方法
  [bar](baz) {
    return this[snaf] = baz;
  }

  // ...
};
```

上面代码中，`bar`和`snaf`都是`Symbol`值，导致第三方无法获取到它们，因此达到了私有方法和私有属性的效果。

#### this的指向

类的方法内部如果含有`this`，它默认指向类的实例。但是，必须非常小心，一旦单独使用该方法，很可能报错。

```js
class Logger {
  printName(name = 'there') {
    this.print(`Hello ${name}`);
  }

  print(text) {
    console.log(text);
  }
}

const logger = new Logger();
const { printName } = logger;
printName(); // TypeError: Cannot read property 'print' of undefined
```

上面代码中，`printName`方法中的`this`，默认指向`Logger`类的实例。但是，如果将这个方法提取出来单独使用，`this`**会指向该方法运行时所在的环境**，因为找不到`print`方法而导致报错。

一个比较简单的解决方法是，在构造方法中绑定`this`，这样就不会找不到`print`方法了。

```js
class Logger {
  constructor() {
    this.printName = this.printName.bind(this);
  }

  // ...
}  ???
```

另一种解决方法是使用箭头函数。

```js
class Logger {
  constructor() {
    this.printName = (name = 'there') => {
      this.print(`Hello ${name}`);
    };
  }

  // ...
}
```

还有一种解决方法是使用`Proxy`，获取方法的时候，自动绑定`this`。

```js
function selfish (target) {
  const cache = new WeakMap();
  const handler = {
    get (target, key) {
      const value = Reflect.get(target, key);
      if (typeof value !== 'function') {
        return value;
      }
      if (!cache.has(value)) {
        cache.set(value, value.bind(target));
      }
      return cache.get(value);
    }
  };
  const proxy = new Proxy(target, handler);
  return proxy;
}

const logger = selfish(new Logger());
```

严格模式

类和模块的内部，默认就是严格模式，所以不需要使用`use strict`指定运行模式。只要你的代码写在类或模块之中，就只有严格模式可用。

考虑到未来所有的代码，其实都是运行在模块之中，所以ES6实际上把整个语言升级到了严格模式。

#### name属性

**由于本质上，ES6的类只是ES5的构造函数的一层包装，所以函数的许多特性都被`Class`继承，包括`name`属性。**

```js
class Point {}
Point.name // "Point"
```

`name`属性总是返回紧跟在`class`关键字后面的类名。

#### Class的继承

##### 基本用法

Class之间可以通过`extends`关键字实现继承，这比ES5的通过修改原型链实现继承，要清晰和方便很多。

```js
class ColorPoint extends Point {}
```

上面代码定义了一个`ColorPoint`类，该类通过`extends`关键字，继承了`Point`类的所有属性和方法。但是由于没有部署任何代码，所以这两个类完全一样，等于复制了一个`Point`类。下面，我们在`ColorPoint`内部加上代码。

```js
class ColorPoint extends Point {
  constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}
```

上面代码中，`constructor`方法和`toString`方法之中，都出现了`super`关键字，它在这里表示父类的构造函数，用来新建父类的`this`对象。

**子类必须在`constructor`方法中调用`super`方法，否则新建实例时会报错**。这是因为子类没有自己的`this`对象，而是继承父类的`this`对象，然后对其进行加工。如果不调用`super`方法，子类就得不到`this`对象。

```js
class Point { /* ... */ }

class ColorPoint extends Point {
  constructor() {
  }
}

let cp = new ColorPoint(); // ReferenceError
```

上面代码中，`ColorPoint`继承了父类`Point`，但是它的构造函数没有调用`super`方法，导致新建实例时报错。

ES5的继承，实质是先创造子类的实例对象`this`，然后再将父类的方法添加到`this`上面（`Parent.apply(this)`）。**ES6的继承机制完全不同，实质是先创造父类的实例对象`this`（所以必须先调用`super`方法），然后再用子类的构造函数修改`this`。**

如果子类没有定义`constructor`方法，这个方法会被默认添加，代码如下。也就是说，不管有没有显式定义，任何一个子类都有`constructor`方法。

```js
constructor(...args) {
  super(...args);
}
```

另一个需要注意的地方是，在子类的构造函数中，只有调用`super`之后，才可以使用`this`关键字，否则会报错。这是因为子类实例的构建，是基于对父类实例加工，**只有`super`方法才能返回父类实例**。

```js
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
}

class ColorPoint extends Point {
  constructor(x, y, color) {
    this.color = color; // ReferenceError
    super(x, y);
    this.color = color; // 正确
  }
}
```

上面代码中，子类的`constructor`方法没有调用`super`之前，就使用`this`关键字，结果报错，而放在`super`方法之后就是正确的。

下面是生成子类实例的代码。

```js
let cp = new ColorPoint(25, 8, 'green');

cp instanceof ColorPoint // true
cp instanceof Point // true
```

上面代码中，实例对象`cp`同时是`ColorPoint`和`Point`两个类的实例，这与ES5的行为完全一致。

##### 类的prototype属性和__proto__属性

大多数浏览器的ES5实现之中，**每一个对象都有`__proto__`属性，指向对应的构造函数的prototype属性**。Class作为构造函数的语法糖，同时有prototype属性和`__proto__`属性，因此同时存在两条继承链。

（1）子类的`__proto__`属性，表示构造函数的继承，总是指向父类。

（2）子类`prototype`属性的`__proto__`属性，表示方法的继承，总是指向父类的`prototype`属性。

```js
class A {
}

class B extends A {
}

B.__proto__ === A // true
B.prototype.__proto__ === A.prototype // true
```

上面代码中，子类`B`的`__proto__`属性指向父类`A`，子类`B`的`prototype`属性的`__proto__`属性指向父类`A`的`prototype`属性。

这样的结果是因为，类的继承是按照下面的模式实现的。

```js
class A {
}

class B {
}

// B的实例继承A的实例
Object.setPrototypeOf(B.prototype, A.prototype);
const b = new B();

// B的实例继承A的静态属性
Object.setPrototypeOf(B, A);
const b = new B();
```

《对象的扩展》一章给出过`Object.setPrototypeOf`方法的实现。

```js
Object.setPrototypeOf = function (obj, proto) {
  obj.__proto__ = proto;
  return obj;
}
```

因此，就得到了上面的结果。

```js
Object.setPrototypeOf(B.prototype, A.prototype);
// 等同于
B.prototype.__proto__ = A.prototype;

Object.setPrototypeOf(B, A);
// 等同于
B.__proto__ = A;
```

这两条继承链，可以这样理解：作为一个对象，子类（`B`）的原型（`__proto__`属性）是父类（`A`）；作为一个构造函数，子类（`B`）的原型（`prototype`属性）是父类的实例。

```js
Object.create(A.prototype);
// 等同于
B.prototype.__proto__ = A.prototype;
```

Extends 的继承目标

`extends`关键字后面可以跟多种类型的值。

```js
class B extends A {
}
```

上面代码的`A`，只要是一个有`prototype`属性的函数，就能被`B`继承。由于函数都有`prototype`属性（除了`Function.prototype`函数），因此`A`可以是任意函数。

下面，讨论三种特殊情况。

第一种特殊情况，子类继承Object类。

```js
class A extends Object {
}

A.__proto__ === Object // true
A.prototype.__proto__ === Object.prototype // true
```

这种情况下，`A`其实就是构造函数`Object`的复制，`A`的实例就是`Object`的实例。

第二种特殊情况，不存在任何继承。

```js
class A {
}

A.__proto__ === Function.prototype // true
A.prototype.__proto__ === Object.prototype // true
```

这种情况下，A作为一个基类（即不存在任何继承），就是一个普通函数，所以直接继承`Funciton.prototype`。但是，`A`调用后返回一个空对象（即`Object`实例），所以`A.prototype.__proto__`指向构造函数（`Object`）的`prototype`属性。

第三种特殊情况，子类继承`null`。

```js
class A extends null {
}

A.__proto__ === Function.prototype // true Function
A.prototype.__proto__ === undefined // true
```

这种情况与第二种情况非常像。`A`也是一个普通函数，所以直接继承`Funciton.prototype`。但是，A调用后返回的对象不继承任何方法，所以它的`__proto__`指向`Function.prototype`，即实质上执行了下面的代码。

```js
class A extends null {
  constructor() { return Object.create(null); }
}
```

##### Object.getPrototypeOf()

`Object.getPrototypeOf`方法可以用来从子类上获取父类。

```js
Object.getPrototypeOf(ColorPoint) === Point
// true
```

因此，可以使用这个方法判断，一个类是否继承了另一个类。

**super 关键字**

`super`这个关键字，既可以当作函数使用，也可以当作对象使用。在这两种情况下，它的用法完全不同。

第一种情况，`super`作为函数调用时，代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次`super`函数。

```js
class A {}

class B extends A {
  constructor() {
    super();
  }
}
```

上面代码中，子类`B`的构造函数之中的`super()`，代表调用父类的构造函数。这是必须的，否则 JavaScript 引擎会报错。

注意，`**super`虽然代表了父类`A`的构造函数，但是返回的是子类`B`的实例**，即`super`内部的`this`指的是`B`，因此`super()`在这里相当于`A.prototype.constructor.call(this)`。

```js
class A {
  constructor() {
    console.log(new.target.name);
  }
}
class B extends A {
  constructor() {
    super();
  }
}
new A() // A
new B() // B
```

上面代码中，`new.target`指向当前正在执行的函数。可以看到，在`super()`执行时，它指向的是子类`B`的构造函数，而不是父类`A`的构造函数。也就是说，`super()`内部的`this`指向的是`B`。

作为函数时，`super()`只能用在子类的构造函数之中，用在其他地方就会报错。

```js
class A {}

class B extends A {
  m() {
    super(); // 报错
  }
}
```

上面代码中，`super()`用在`B`类的`m`方法之中，就会造成句法错误。

第二种情况，`super`作为对象时，指向父类的原型对象。

```js
class A {
  p() {
    return 2;
  }
}

class B extends A {
  constructor() {
    super();
    console.log(super.p()); // 2
  }
}

let b = new B();
```

上面代码中，子类`B`当中的`super.p()`，就是将`super`当作一个对象使用。这时，`super`指向`A.prototype`，所以`super.p()`就相当于`A.prototype.p()`。

这里需要注意，**由于`super`指向父类的原型对象，所以定义在父类实例上的方法或属性，是无法通过`super`调用的**。

```js
class A {
  constructor() {
    this.p = 2;
  }
}

class B extends A {
  get m() {
    return super.p;
  }
}

let b = new B();
b.m // undefined
```

上面代码中，`p`是父类`A`实例的属性，`super.p`就引用不到它。

如果属性定义在父类的原型对象上，`super`就可以取到。

```js
class A {}
A.prototype.x = 2;

class B extends A {
  constructor() {
    super();
    console.log(super.x) // 2
  }
}

let b = new B();
```

上面代码中，属性`x`是定义在`A.prototype`上面的，所以`super.x`可以取到它的值。

**ES6 规定，通过`super`调用父类的方法时，`super`会绑定子类的`this**`。

```js
class A {
  constructor() {
    this.x = 1;
  }
  print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  m() {
    super.print();
  }
}

let b = new B();
b.m() // 2
```

上面代码中，`super.print()`虽然调用的是`A.prototype.print()`，但是`A.prototype.print()`会绑定子类`B`的`this`，导致输出的是`2`，而不是`1`。也就是说，实际上执行的是`super.print.call(this)`。

由于绑定子类的`this`，所以如果通过`super`对某个属性赋值，这时`super`就是`this`，赋值的属性会变成子类实例的属性。

```js
class A {
  constructor() {
    this.x = 1;
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
    super.x = 3;
    console.log(super.x); // undefined
    console.log(this.x); // 3
  }
}

let b = new B();
```

上面代码中，`super.x`赋值为`3`，这时等同于对`this.x`赋值为`3`。而当读取`super.x`的时候，读的是`A.prototype.x`，所以返回`undefined`。

注意，使用`super`的时候，必须显式指定是作为函数、还是作为对象使用，否则会报错。

```js
class A {}

class B extends A {
  constructor() {
    super();
    console.log(super); // 报错
  }
}
```

上面代码中，`console.log(super)`当中的`super`，无法看出是作为函数使用，还是作为对象使用，所以 JavaScript 引擎解析代码的时候就会报错。这时，如果能清晰地表明`super`的数据类型，就不会报错。

```js
class A {}

class B extends A {
  constructor() {
    super();
    console.log(super.valueOf() instanceof B); // true
  }
}

let b = new B();
```

上面代码中，`super.valueOf()`表明`super`是一个对象，因此就不会报错。同时，由于`super`绑定`B`的`this`，所以`super.valueOf()`返回的是一个`B`的实例。

最后，由于对象总是继承其他对象的，所以可以在任意一个对象中，使用`super`关键字。

```js
var obj = {
  toString() {
    return "MyObject: " + super.toString();
  }
};

obj.toString(); // MyObject: [object Object]
```

##### 实例的__proto__属性

子类实例的__proto__属性的__proto__属性，指向父类实例的__proto__属性。也就是说，子类的原型的原型，是父类的原型。

```js
var p1 = new Point(2, 3);
var p2 = new ColorPoint(2, 3, 'red');

p2.__proto__ === p1.__proto__ // false
p2.__proto__.__proto__ === p1.__proto__ // true
```

上面代码中，`ColorPoint`继承了`Point`，导致前者原型的原型是后者的原型。

因此，通过子类实例的`__proto__.__proto__`属性，可以修改父类实例的行为。

```js
p2.__proto__.__proto__.printName = function () {
  console.log('Ha');
};

p1.printName() // "Ha"
```

上面代码在`ColorPoint`的实例`p2`上向`Point`类添加方法，结果影响到了`Point`的实例`p1`。

##### 原生构造函数的继承

原生构造函数是指语言内置的构造函数，通常用来生成数据结构。ECMAScript的原生构造函数大致有下面这些。

- Boolean()
- Number()
- String()
- Array()
- Date()
- Function()
- RegExp()
- Error()
- Object()

以前，这些原生构造函数是无法继承的，比如，不能自己定义一个`Array`的子类。

```js
function MyArray() {
  Array.apply(this, arguments);
}

MyArray.prototype = Object.create(Array.prototype, {
  constructor: {
    value: MyArray,
    writable: true,
    configurable: true,
    enumerable: true
  }
});
```

上面代码定义了一个继承Array的`MyArray`类。但是，这个类的行为与`Array`完全不一致。

```
var colors = new MyArray();
colors[0] = "red";
colors.length  // 0

colors.length = 0;
colors[0]  // "red"
```

之所以会发生这种情况，是因为子类无法获得原生构造函数的内部属性，通过`Array.apply()`或者分配给原型对象都不行。原生构造函数会忽略`apply`方法传入的`this`，也就是说，原生构造函数的`this`无法绑定，导致拿不到内部属性。

ES5是先新建子类的实例对象`this`，再将父类的属性添加到子类上，由于父类的内部属性无法获取，导致无法继承原生的构造函数。比如，Array构造函数有一个内部属性`[[DefineOwnProperty]]`，用来定义新属性时，更新`length`属性，这个内部属性无法在子类获取，导致子类的`length`属性行为不正常。

下面的例子中，我们想让一个普通对象继承`Error`对象。

```js
var e = {};

Object.getOwnPropertyNames(Error.call(e))
// [ 'stack' ]

Object.getOwnPropertyNames(e)
// []
```

上面代码中，我们想通过`Error.call(e)`这种写法，让普通对象`e`具有`Error`对象的实例属性。但是，`Error.call()`完全忽略传入的第一个参数，而是返回一个新对象，`e`本身没有任何变化。这证明了`Error.call(e)`这种写法，无法继承原生构造函数。

ES6允许继承原生构造函数定义子类，因为ES6是先新建父类的实例对象`this`，然后再用子类的构造函数修饰`this`，使得父类的所有行为都可以继承。下面是一个继承`Array`的例子。

```
class MyArray extends Array {
  constructor(...args) {
    super(...args);
  }
}

var arr = new MyArray();
arr[0] = 12;
arr.length // 1

arr.length = 0;
arr[0] // undefined
```

上面代码定义了一个`MyArray`类，继承了`Array`构造函数，因此就可以从`MyArray`生成数组的实例。这意味着，ES6可以自定义原生数据结构（比如Array、String等）的子类，这是ES5无法做到的。

上面这个例子也说明，`extends`关键字不仅可以用来继承类，还可以用来继承原生的构造函数。因此可以在原生数据结构的基础上，定义自己的数据结构。下面就是定义了一个带版本功能的数组。

```
class VersionedArray extends Array {
  constructor() {
    super();
    this.history = [[]];
  }
  commit() {
    this.history.push(this.slice());
  }
  revert() {
    this.splice(0, this.length, ...this.history[this.history.length - 1]);
  }
}

var x = new VersionedArray();

x.push(1);
x.push(2);
x // [1, 2]
x.history // [[]]

x.commit();
x.history // [[], [1, 2]]
x.push(3);
x // [1, 2, 3]

x.revert();
x // [1, 2]
```

上面代码中，`VersionedArray`结构会通过`commit`方法，将自己的当前状态存入`history`属性，然后通过`revert`方法，可以撤销当前版本，回到上一个版本。除此之外，`VersionedArray`依然是一个数组，所有原生的数组方法都可以在它上面调用。

下面是一个自定义`Error`子类的例子。

```
class ExtendableError extends Error {
  constructor(message) {
    super();
    this.message = message;
    this.stack = (new Error()).stack;
    this.name = this.constructor.name;
  }
}

class MyError extends ExtendableError {
  constructor(m) {
    super(m);
  }
}

var myerror = new MyError('ll');
myerror.message // "ll"
myerror instanceof Error // true
myerror.name // "MyError"
myerror.stack
// Error
//     at MyError.ExtendableError
//     ...
```

注意，继承`Object`的子类，有一个[行为差异](https://link.jianshu.com?t=http://stackoverflow.com/questions/36203614/super-does-not-pass-arguments-when-instantiating-a-class-extended-from-object)。

```
class NewObj extends Object{
  constructor(){
    super(...arguments);
  }
}
var o = new NewObj({attr: true});
console.log(o.attr === true);  // false
```

上面代码中，`NewObj`继承了`Object`，但是无法通过`super`方法向父类`Object`传参。这是因为ES6改变了`Object`构造函数的行为，一旦发现`Object`方法不是通过`new Object()`这种形式调用，ES6规定`Object`构造函数会忽略参数。

Class的取值函数（getter）和存值函数（setter）

与ES5一样，在Class内部可以使用`get`和`set`关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。

```
class MyClass {
  constructor() {
    // ...
  }
  get prop() {
    return 'getter';
  }
  set prop(value) {
    console.log('setter: '+value);
  }
}

let inst = new MyClass();

inst.prop = 123;
// setter: 123

inst.prop
// 'getter'
```

上面代码中，`prop`属性有对应的存值函数和取值函数，因此赋值和读取行为都被自定义了。

存值函数和取值函数是设置在属性的descriptor对象上的。

```
class CustomHTMLElement {
  constructor(element) {
    this.element = element;
  }

  get html() {
    return this.element.innerHTML;
  }

  set html(value) {
    this.element.innerHTML = value;
  }
}

var descriptor = Object.getOwnPropertyDescriptor(
  CustomHTMLElement.prototype, "html");
"get" in descriptor  // true
"set" in descriptor  // true
```

上面代码中，存值函数和取值函数是定义在`html`属性的描述对象上面，这与ES5完全一致。

##### Class的Generator方法

如果某个方法之前加上星号（`*`），就表示该方法是一个Generator函数。

```js
class Foo {
  constructor(...args) {
    this.args = args;
  }
  * [Symbol.iterator]() {
    for (let arg of this.args) {
      yield arg;
    }
  }
}

for (let x of new Foo('hello', 'world')) {
  console.log(x);
}
// hello
// world
```

上面代码中，Foo类的Symbol.iterator方法前有一个星号，表示该方法是一个Generator函数。Symbol.iterator方法返回一个Foo类的默认遍历器，for...of循环会自动调用这个遍历器。

Class的静态方法

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上`static`关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod() // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```

上面代码中，`Foo`类的`classMethod`方法前有`static`关键字，表明该方法是一个静态方法，可以直接在`Foo`类上调用（`Foo.classMethod()`），而不是在`Foo`类的实例上调用。如果在实例上调用静态方法，会抛出一个错误，表示不存在该方法。

父类的静态方法，可以被子类继承。

```
class Foo {
  static classMethod() {
    return 'hello';
  }
}

class Bar extends Foo {
}

Bar.classMethod(); // 'hello'
```

上面代码中，父类`Foo`有一个静态方法，子类`Bar`可以调用这个方法。

静态方法也是可以从`super`对象上调用的。

```
class Foo {
  static classMethod() {
    return 'hello';
  }
}

class Bar extends Foo {
  static classMethod() {
    return super.classMethod() + ', too';
  }
}

Bar.classMethod();
```

Class的静态属性和实例属性

静态属性指的是Class本身的属性，即`Class.propname`，而不是定义在实例对象（`this`）上的属性。

```
class Foo {
}

Foo.prop = 1;
Foo.prop // 1
```

上面的写法为`Foo`类定义了一个静态属性`prop`。

目前，只有这种写法可行，因为ES6明确规定，Class内部只有静态方法，没有静态属性。

```
// 以下两种写法都无效
class Foo {
  // 写法一
  prop: 2

  // 写法二
  static prop: 2
}

Foo.prop // undefined
```

ES7有一个静态属性的[提案](https://link.jianshu.com?t=https://github.com/jeffmo/es-class-properties)，目前Babel转码器支持。

这个提案对实例属性和静态属性，都规定了新的写法。

（1）类的实例属性

类的实例属性可以用等式，写入类的定义之中。

```
class MyClass {
  myProp = 42;

  constructor() {
    console.log(this.myProp); // 42
  }
}
```

上面代码中，`myProp`就是`MyClass`的实例属性。在`MyClass`的实例上，可以读取这个属性。

以前，我们定义实例属性，只能写在类的`constructor`方法里面。

```
class ReactCounter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }
}
```

上面代码中，构造方法`constructor`里面，定义了`this.state`属性。

有了新的写法以后，可以不在`constructor`方法里面定义。

```
class ReactCounter extends React.Component {
  state = {
    count: 0
  };
}
```

这种写法比以前更清晰。

为了可读性的目的，对于那些在`constructor`里面已经定义的实例属性，新写法允许直接列出。

```
class ReactCounter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }
  state;
}
```

（2）类的静态属性

类的静态属性只要在上面的实例属性写法前面，加上`static`关键字就可以了。

```
class MyClass {
  static myStaticProp = 42;

  constructor() {
    console.log(MyClass.myProp); // 42
  }
}
```

同样的，这个新写法大大方便了静态属性的表达。

```
// 老写法
class Foo {
}
Foo.prop = 1;

// 新写法
class Foo {
  static prop = 1;
}
```

上面代码中，老写法的静态属性定义在类的外部。整个类生成以后，再生成静态属性。这样让人很容易忽略这个静态属性，也不符合相关代码应该放在一起的代码组织原则。另外，新写法是显式声明（declarative），而不是赋值处理，语义更好。

new.target属性

`new`是从构造函数生成实例的命令。ES6为`new`命令引入了一个`new.target`属性，（在构造函数中）返回`new`命令作用于的那个构造函数。如果构造函数不是通过`new`命令调用的，`new.target`会返回`undefined`，因此这个属性可以用来确定构造函数是怎么调用的。

```
function Person(name) {
  if (new.target !== undefined) {
    this.name = name;
  } else {
    throw new Error('必须使用new生成实例');
  }
}

// 另一种写法
function Person(name) {
  if (new.target === Person) {
    this.name = name;
  } else {
    throw new Error('必须使用new生成实例');
  }
}

var person = new Person('张三'); // 正确
var notAPerson = Person.call(person, '张三');  // 报错
```

上面代码确保构造函数只能通过`new`命令调用。

Class内部调用`new.target`，返回当前Class。

```
class Rectangle {
  constructor(length, width) {
    console.log(new.target === Rectangle);
    this.length = length;
    this.width = width;
  }
}

var obj = new Rectangle(3, 4); // 输出 true
```

需要注意的是，子类继承父类时，`new.target`会返回子类。

```
class Rectangle {
  constructor(length, width) {
    console.log(new.target === Rectangle);
    // ...
  }
}

class Square extends Rectangle {
  constructor(length) {
    super(length, length);
  }
}

var obj = new Square(3); // 输出 false
```

上面代码中，`new.target`会返回子类。

利用这个特点，可以写出不能独立使用、必须继承后才能使用的类。

```
class Shape {
  constructor() {
    if (new.target === Shape) {
      throw new Error('本类不能实例化');
    }
  }
}

class Rectangle extends Shape {
  constructor(length, width) {
    super();
    // ...
  }
}

var x = new Shape();  // 报错
var y = new Rectangle(3, 4);  // 正确
```

上面代码中，`Shape`类不能被实例化，只能用于继承。

注意，在函数外部，使用`new.target`会报错。

Mixin模式的实现

Mixin模式指的是，将多个类的接口“混入”（mix in）另一个类。它在ES6的实现如下。

```
function mix(...mixins) {
  class Mix {}

  for (let mixin of mixins) {
    copyProperties(Mix, mixin);
    copyProperties(Mix.prototype, mixin.prototype);
  }

  return Mix;
}

function copyProperties(target, source) {
  for (let key of Reflect.ownKeys(source)) {
    if ( key !== "constructor"
      && key !== "prototype"
      && key !== "name"
    ) {
      let desc = Object.getOwnPropertyDescriptor(source, key);
      Object.defineProperty(target, key, desc);
    }
  }
}
```

上面代码的`mix`函数，可以将多个对象合成为一个类。使用的时候，只要继承这个类即可。

```
class DistributedEdit extends mix(Loggable, Serializable) {
  // ...
}
```

