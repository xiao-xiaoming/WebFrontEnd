# JavaScript基础

 JavaScript 对大小写是敏感的，使用Unicode字符集。

## 使用JavaScript

HTML中的脚本必须位于 `<script>`与`</script>` 标签之间。

过去`<script>` 标签中必须使用type="text/javascript"，但现在已经不需要了，JavaScript 是所有现代浏览器以及 HTML5中的默认脚本语言。 

脚本可被放置在 HTML 页面的 `<body>`和`<head>`部分中。

JavaScript放在`<body>`：

```html
<!DOCTYPE html>
<html>
<body>
.
.
<script>
document.write("<h1>这是一个标题</h1>");
document.write("<p>这是一个段落</p>");
</script>
.
.
</body>
</html>
```

JavaScript放在`<head>`中：

```html
<!DOCTYPE html>
<html>
<head>
<script>
function myFunction()
{
    document.getElementById("demo").innerHTML="我的第一个 JavaScript 函数";
}
</script>
</head>
<body>
<h1>我的 Web 页面</h1>
<p id="demo">一个段落</p>
<button type="button" onclick="myFunction()">尝试一下</button>
</body>
</html>
```

引入外部 JavaScript 文件 ：

```javascript
<script src="myScript.js"></script>
```

分号用于分隔 JavaScript 语句:

```javascript
a = 5; b = 6; c = a + b;
```

在文本字符串中使用反斜杠对代码行进行换行：

```javascript
document.write("你好 \
世界!");
```

## JavaScript输出数据

JavaScript 可以通过不同的方式来输出数据：

- 使用 **window.alert()** 弹出警告框。
- 使用 **document.write()** 方法将内容写到 HTML 文档中。
- 使用 **innerHTML** 写入到 HTML 元素。
- 使用 **console.log()** 写入到浏览器的控制台。

```html
<p id="demo">我的第一个段落</p>

<script>
alert(5 + 6);
document.getElementById("demo").innerHTML = "段落已修改。";
document.write(Date());
console.log(5 + 6);
</script>
```

## JavaScript运算符

JavaScript支持的运算符：

| 类型                 | 实例                    |
| :------------------- | :---------------------- |
| 赋值，算术和位运算符 | `=`  `+`  `-`  `*`  `/` |
| 条件及比较运算符     | `==`  `!=`  `<`  `>`    |
| 逻辑运算符           | `&&`  `||`  `!`         |

算术运算符：

| 运算符 | 描述         | 例子(y=5) | x 运算结果 | y 运算结果 |
| :----- | :----------- | :-------- | :--------- | :--------- |
| +      | 加法         | x=y+2     | 7          | 5          |
| -      | 减法         | x=y-2     | 3          | 5          |
| *      | 乘法         | x=y*2     | 10         | 5          |
| /      | 除法         | x=y/2     | 2.5        | 5          |
| %      | 取模（余数） | x=y%2     | 1          | 5          |
| ++     | 自增         | x=++y     | 6          | 6          |
|        |              | x=y++     | 5          | 6          |
| --     | 自减         | x=--y     | 4          | 4          |
|        |              | x=y--     | 5          | 4          |

 赋值运算符(给定 **x=10** 和 **y=5**)：

| 运算符 | 例子 | 等同于 | 运算结果 |
| :----- | :--- | :----- | :------- |
| =      | x=y  | x=5    |          |
| +=     | x+=y | x=x+y  | x=15     |
| -=     | x-=y | x=x-y  | x=5      |
| *=     | x*=y | x=x*y  | x=50     |
| /=     | x/=y | x=x/y  | x=2      |
| %=     | x%=y | x=x%y  | x=0      |

比较运算符(x=5)：

| 运算符 | 描述                                               | 比较    | 返回值 |
| :----- | :------------------------------------------------- | :------ | :----- |
| ==     | 等于                                               | x==8    | false  |
|        | 在常规的比较中，数据类型是被忽略的                 | x=="5"  | true   |
| ===    | 绝对等于（值和类型均相等）                         | x==="5" | false  |
|        | 恒等计算符，同时检查表达式的值与类型               | x===5   | true   |
| !=     | 不等于                                             | x!=8    | true   |
| !==    | 不绝对等于（值和类型有一个不相等，或两个都不相等） | x!=="5" | true   |
|        |                                                    | x!==5   | false  |
| >      | 大于                                               | x>8     | false  |
| <      | 小于                                               | x<8     | true   |
| >=     | 大于或等于                                         | x>=8    | false  |
| <=     | 小于或等于                                         | x<=8    | true   |

## JavaScript注释

```javascript
// 单行注释
/*
多行注释
多行注释
多行注释
*/
```

## 自动添加分号的机制

如果把return语句拆成两行：

```javascript
function foo() {
    return
        { name: 'foo' };
}

foo(); // undefined
```

由于JavaScript引擎在行末自动添加分号的机制，上面的代码实际上变成了：

```javascript
function foo() {
    return; // 自动添加了分号，相当于return undefined;
        { name: 'foo' }; // 这行语句已经没法执行到了
}
```

所以正确的多行写法是：

```javascript
function foo() {
    return { // 这里不会自动加分号，因为{表示语句尚未结束
        name: 'foo'
    };
}
```

# JavaScript变量

JavaScript使用关键字 **var** 来定义变量， 使用等号来为变量赋值：

```javascript
var x, length;
x = 5;
length = 6;
```

在函数外声明的变量是全局变量， 全局变量在 JavaScript 程序的任何地方都可以访问。 

变量如果在函数内声明，变量是局部变量，只能在函数内部访问：

```javascript
// 这里不能使用 carName 变量
function myFunction() {
    var carName = "Volvo";
    // 函数内可调用 carName 变量
}
// 这里不能使用carName变量
```

## 严格模式

JavaScript在设计之初，如果一个变量没有通过`var`申明就被使用，那么该变量就自动被申明为全局变量：

```javascript
function test(){
	i = 10; // i现在是全局变量
}
test();
console.log(i);// 10
```

在同一个页面的不同的JavaScript文件中，如果都不用`var`申明，恰好都使用了变量`i`，将造成变量`i`互相影响，产生难以调试的错误结果。

使用`var`申明的变量则不是全局变量，它的范围被限制在该变量被申明的函数体内，同名变量在不同的函数体内互不冲突。

JavaScript 1.8.5 (ECMAScript5) 中新增strict模式，在strict模式下运行的JavaScript代码，强制通过`var`申明变量，未使用`var`申明变量就使用的，将导致运行错误。

启用strict模式的方法是在JavaScript代码的第一行写上：

```javascript
'use strict';
```

 在函数内部声明是局部作用域 (只在函数内使用严格模式)：

```JavaScript
x = 3.14;       // 不报错 
myFunction();

function myFunction() {
   "use strict";
    y = 3.14;   // 报错 (y 未定义)
}
```

严格模式的限制:

```JavaScript
//不允许使用未声明的变量：
"use strict";
x = 3.14;                // 报错 (x 未定义)

// 不允许删除变量或对象。
"use strict";
var x = 3.14;
delete x;                // 报错

//不允许删除函数。
"use strict";
function x(p1, p2) {}; 
delete x;                // 报错 

//不允许变量重名
"use strict";
function x(p1, p1) {};   // 报错

//不允许使用八进制:
"use strict";
var x = 010;             // 报错

//不允许使用转义字符:
"use strict";
var x = \010;            // 报错

//不允许对只读属性赋值:
"use strict";
var obj = {};
Object.defineProperty(obj, "x", {value:0, writable:false});
obj.x = 3.14;            // 报错

//不允许对一个使用getter方法读取的属性进行赋值
"use strict";
var obj = {get x() {return 0} };
obj.x = 3.14;            // 报错

//不允许删除一个不允许删除的属性：
"use strict";
delete Object.prototype; // 报错

//变量名不能使用 "eval" 字符串:
"use strict";
var eval = 3.14;         // 报错

//变量名不能使用 "arguments" 字符串:
"use strict";
var arguments = 3.14;    // 报错

//在作用域 eval() 创建的变量不能被调用：
"use strict";
eval ("var x = 2");
alert (x);               // 报错
```

禁止this关键字指向全局对象:

```JavaScript
function f(){
    return !this;
} 
// 返回false，因为"this"指向全局对象，"!this"就是false

function f(){ 
    "use strict";
    return !this;
} 
// 返回true，因为严格模式下，this的值为undefined，所以"!this"为true。

```

使用构造函数时，如果忘了加new，this不再指向全局对象，而是报错：

```JavaScript
function f(){
    "use strict";
    this.a = 1;
};
f();// 报错，this未定义
```

## 变量作用域

由于JavaScript的函数可以嵌套，此时，内部函数可以访问外部函数定义的变量，反过来则不行：

```javascript
'use strict';

function foo() {
    var x = 1;
    function bar() {
        var y = x + 1; // bar可以访问foo的变量x!
    }
    var z = y + 1; // ReferenceError! foo不可以访问bar的变量y!
}
```

 JavaScript的函数在查找变量时从自身函数定义开始，从“内”向“外”查找。如果内部函数定义了与外部函数重名的变量，则内部函数的变量将“屏蔽”外部函数的变量。 

```javascript
function foo() {
    var x = 1;
    function bar() {
        var x = 'A';
        console.log('x in bar() = ' + x); // 'A'
    }
    bar();
    console.log('x in foo() = ' + x); // 1
}

foo();
```

## 全局对象window

不在任何函数内定义的变量就具有全局作用域。实际上，JavaScript默认有一个全局对象`window`，全局作用域的变量实际上被绑定到`window`的一个属性：

```javascript
'use strict';

var course = 'Learn JavaScript';
alert(course); // 'Learn JavaScript'
alert(window.course); // 'Learn JavaScript'
```

因此，直接访问全局变量`course`和访问`window.course`是完全一样的。

顶层函数的定义也被视为一个全局变量，并绑定到`window`对象：

```javascript
'use strict';

function foo() {
    alert('foo');
}
foo(); // 直接调用foo()
window.foo(); // 通过window.foo()调用
```

`alert()`函数其实也是`window`的一个变量：

```javascript
'use strict';

window.alert('调用window.alert()');
// 把alert保存到另一个变量:
var old_alert = window.alert;
// 给alert赋一个新函数:
window.alert = function () {}
alert('无法用alert()显示了!');
// 恢复alert:
window.alert = old_alert;
alert('又可以用alert()了!');
```

JavaScript只有一个全局作用域。任何变量（函数也视为变量），如果没有在当前函数作用域中找到，就会继续往上查找，最后如果在全局作用域中也没有找到，而且在严格模式下则报`ReferenceError`错误。 

## 局部作用域

由于JavaScript的变量作用域实际上是函数内部，在`for`循环等语句块中是无法定义具有局部作用域的变量的：

```javascript
'use strict';

function foo() {
    for (var i=0; i<100; i++) {
        //
    }
    i += 100; // 仍然可以引用变量i
}
```

为了解决块级作用域，ES6引入了新的关键字`let`，用`let`替代`var`可以申明一个块级作用域的变量：

```javascript
'use strict';

function foo() {
    var sum = 0;
    for (let i=0; i<100; i++) {
        sum += i;
    }
    // SyntaxError:
    i += 1;
}
```

在函数体外或代码块外使用 **var** 和 **let** 关键字声明的变量作用域都是 **全局的**。

但使用 **let** 关键字声明的全局作用域变量不属于 window 对象 。

## 常量

ES6标准引入了新的关键字`const`来定义常量，`const`与`let`都具有块级作用域：

```javascript
'use strict';

const PI = 3.14;
PI = 3; // 某些浏览器不报错，但是无效果！
PI; // 3.14
```

const 用于声明一个或多个常量，声明时必须进行初始化，且初始化后值不可再修改，不过使用 const 定义的对象或者数组，仍然可变:

```javascript
// 创建常量对象
const car = {type:"Fiat", model:"500", color:"white"};
// 修改属性:
car.color = "red";
// 添加属性
car.owner = "Johnson";
```

 

## JavaScript变量提升

JavaScript会先扫描整个作用域，把所有申明的变量“提升”到作用域顶部： 

```JavaScript
'use strict';

var x = 'Hello, ' + y;
console.log(x);//Hello, undefined
var y = 'Bob';
```

虽然是strict模式，但语句`var x = 'Hello, ' + y;`并不报错，原因是变量`y`在稍后申明了。但是`console.log`显示`Hello, undefined`，说明变量`y`的值为`undefined`。这正是因为JavaScript引擎自动提升了变量`y`的声明，但不会提升变量`y`的赋值。 

上述代码相当于： 

```JavaScript
'use strict';

var y; // 提升变量y的申明，此时y为undefined
var x = 'Hello, ' + y;
console.log(x);
y = 'Bob';
```

由于JavaScript的这个“特性”，在函数内部定义变量时，应当严格遵守“在函数内部首先申明所有变量”这一规则。最常见的做法是用一个`var`申明函数内部用到的所有变量：

```JavaScript
function foo() {
    var
        x = 1, // x初始化为1
        y = x + 1, // y初始化为2
        z, i; // z和i为undefined
    // 其他语句:
    for (i=0; i<100; i++) {
        ...
    }
}
```

## 解构赋值

从ES6开始，JavaScript引入了解构赋值，可以同时对一组变量进行赋值。

如何把一个数组的元素分别赋值给几个变量传统的做法：

```JavaScript
var array = ['hello', 'JavaScript', 'ES6'];
var x = array[0];
var y = array[1];
var z = array[2];
```

现在，在ES6中，可以使用解构赋值，直接对多个变量同时赋值：

```JavaScript
'use strict';
var [x, y, z] = ['hello', 'JavaScript', 'ES6'];
console.log('x = ' + x + ', y = ' + y + ', z = ' + z);
//x = hello, y = JavaScript, z = ES6
```

如果数组本身还有嵌套，也可以通过下面的形式进行解构赋值，注意嵌套层次和位置要保持一致：

```JavaScript
let [x, [y, z]] = ['hello', ['JavaScript', 'ES6']];
x; // 'hello'
y; // 'JavaScript'
z; // 'ES6'
```

解构赋值还可以忽略某些元素：

```JavaScript
let [, , z] = ['hello', 'JavaScript', 'ES6']; // 忽略前两个元素，只对z赋值第三个元素
z; // 'ES6'
```

如果需要从一个对象中取出若干属性，也可以使用解构赋值，便于快速获取对象的指定属性：

```JavaScript
'use strict';

var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school'
};
var {name, age, passport} = person;
console.log('name = ' + name + ', age = ' + age + ', passport = ' + passport);
//输出：name = 小明, age = 20, passport = G-12345678
var {gender, school} = person;
console.log(gender + ',' + school);
//输出：male,No.4 middle school
```

对嵌套的对象属性进行赋值，只要保证对应的层次是一致的：

```JavaScript
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school',
    address: {
        city: 'Beijing',
        street: 'No.1 Road',
        zipcode: '100001'
    }
};
var {name, address: {city, zip}} = person;
name; // '小明'
city; // 'Beijing'
zip; // undefined, 因为属性名是zipcode而不是zip
// 注意: address不是变量，而是为了让city和zip获得嵌套的address对象的属性:
address; // Uncaught ReferenceError: address is not defined
```

如果要使用的变量名和属性名不一致，可以用下面的语法获取：

```JavaScript
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678',
    school: 'No.4 middle school'
};

// 把passport属性赋值给变量id:
let {name, passport:id} = person;
name; // '小明'
id; // 'G-12345678'
// 注意: passport不是变量，而是为了让变量id获得passport属性:
passport; // Uncaught ReferenceError: passport is not defined
```

解构赋值还可以使用默认值，这样就避免了不存在的属性返回`undefined`的问题：

```JavaScript
var person = {
    name: '小明',
    age: 20,
    gender: 'male',
    passport: 'G-12345678'
};

// 如果person对象没有single属性，默认赋值为true:
var {name, single=true} = person;
name; // '小明'
single; // true
```

有些时候，如果变量已经被声明了，再次赋值的时候，正确的写法也会报语法错误：

```JavaScript
// 声明变量:
var x, y;
// 解构赋值:
{x, y} = { name: '小明', x: 100, y: 200};
// 语法错误: Uncaught SyntaxError: Unexpected token =
```

这是因为JavaScript引擎把`{`开头的语句当作了块处理，于是`=`不再合法。解决方法是用小括号括起来：

```JavaScript
({x, y} = { name: '小明', x: 100, y: 200});
```

解构赋值在很多时候可以大大简化代码。例如，交换两个变量`x`和`y`的值，可以这么写，不再需要临时变量：

```JavaScript
var x=1, y=2;
[x, y] = [y, x]
```

快速获取当前页面的域名和路径：

```JavaScript
var {hostname:domain, pathname:path} = location;
```

## 补充

### undefined和null的区别

undefined和null的区别(值相等，但类型不等) :

```javascript
typeof undefined             // undefined
typeof null                  // object
null === undefined           // false
null == undefined            // true
```

### constructor属性

**constructor**属性返回所有JavaScript变量的构造函数。

```javascript
"John".constructor         // 返回函数 String() { [native code] }
(3.14).constructor         // 返回函数 Number() { [native code] }
false.constructor         // 返回函数 Boolean() { [native code] }
[1,2,3,4].constructor       // 返回函数 Array()  { [native code] }
{name:'John', age:34}.constructor // 返回函数 Object() { [native code] }
new Date().constructor       // 返回函数 Date()  { [native code] }
function () {}.constructor     // 返回函数 Function(){ [native code] }
```


可以使用 constructor 属性来查看对象是否为数组 (包含字符串 "Array"):

```
function isArray(myArray) {
  return myArray.constructor.toString().indexOf("Array") > -1;
}
```

可以使用 constructor 属性来查看对象是否为日期 (包含字符串 "Date"):

```
function isDate(myDate) {
  return myDate.constructor.toString().indexOf("Date") > -1;
}
```

### typeof操作符

typeof 操作符可以检测变量的数据类型 ：

```javascript
typeof "John"                 // 返回 string 
typeof 3.14                   // 返回 number
typeof NaN                    // 返回 number
typeof false                  // 返回 boolean
typeof [1,2,3,4]              // 返回 object
typeof {name:'John', age:34}  // 返回 object
typeof new Date()             // 返回 object
typeof function () {}         // 返回 function
typeof myCar                  // 返回 undefined (如果 myCar 没有声明)
typeof null                   // 返回 object
```



# JavaScript数据类型

**值类型(基本类型)**：字符串（String）、数字(Number)、布尔(Boolean)、对空（Null）、未定义（Undefined）、Symbol( ES6 引入了一种新的原始数据类型，表示独一无二的值)。

**引用数据类型**：对象(Object)、数组(Array)、函数(Function)。

示例：

```javascript
var length = 16;                  // Number
var points = x * 10;               // Number
var lastName = "Johnson";             // String
var cars = ["Saab", "Volvo", "BMW"];       // Array
var person = {firstName:"John", lastName:"Doe"}; // Object
// 定义一个函数：
function myFunction(a, b) { return a * b;}
```

JavaScript 拥有动态类型。这意味着相同的变量可用作不同的类型：

```javascript
var x;        // x 为 undefined
var x = 5;      // 现在 x 为数字
var x = "John";   // 现在 x 为字符串
x = null;  	// 设置为null可清空变量。
x = undefined;     // 也设置为undefined来清空变量。
```

声明新变量时，可以使用关键词 "new" 来声明其类型：

```javascript
var carname=new String;
var x=   new Number;
var y=   new Boolean;
var cars=  new Array;
var person= new Object;
```

## 布尔值

一个布尔值只有`true`、`false`两种值，要么是`true`，要么是`false`，

可以直接用`true`、`false`表示布尔值，也可以通过布尔运算计算出来：

```javascript
true; // 这是一个true值
false; // 这是一个false值
2 > 1; // 这是一个true值
2 >= 3; // 这是一个false值
```

`&&`运算是与运算，只有所有都为`true`，`&&`运算结果才是`true`：

```javascript
true && true; // 这个&&语句计算结果为true
true && false; // 这个&&语句计算结果为false
false && true && false; // 这个&&语句计算结果为false
```

`||`运算是或运算，只要其中有一个为`true`，`||`运算结果就是`true`：

```javascript
false || false; // 这个||语句计算结果为false
true || false; // 这个||语句计算结果为true
false || true || false; // 这个||语句计算结果为true
```

`!`运算是非运算，它是一个单目运算符，把`true`变成`false`，`false`变成`true`：

```javascript
! true; // 结果为false
! false; // 结果为true
! (2 > 5); // 结果为true
```

布尔值经常用在条件判断中，比如：

```javascript
var age = 15;
if (age >= 18) {
    alert('adult');
} else {
    alert('teenager');
}
```

## 数组

### 创建数组

javascript 的数组下标是从0开始的，创建数组：

```javascript
var cars=new Array();
cars[0]="Saab";
cars[1]="Volvo";
cars[2]="BMW";
// 或者 (condensed array):
var cars=new Array("Saab","Volvo","BMW");
// 或者 (literal array):
var cars=["Saab","Volvo","BMW"];
```

数组是一组按顺序排列的集合，集合的每个值称为元素。JavaScript的数组可以包括任意数据类型。例如：

```javascript
[1, 2, 3.14, 'Hello', null, true];
```

上述数组包含6个元素。数组用`[]`表示，元素之间用`,`分隔。

另一种创建数组的方法是通过`Array()`函数实现：

```javascript
new Array(1, 2, 3); // 创建了数组[1, 2, 3]
```

如果数组的某个元素又是一个`Array`，则可以形成多维数组，例如：

```javascript
var arr = [[1, 2, 3], [400, 500, 600], '-'];
```

上述`Array`包含3个元素，其中头两个元素本身也是`Array`。

### 访问数组

数组的元素可以通过索引来访问。请注意，索引的起始值为`0`：

```javascript
var arr = [1, 2, 3.14, 'Hello', null, true];
arr[0]; // 返回索引为0的元素，即1
arr[5]; // 返回索引为5的元素，即true
arr[6]; // 索引超出了范围，返回undefined
```

要取得`Array`的长度，直接访问`length`属性：

```javascript
var arr = [1, 2, 3.14, 'Hello', null, true];
arr.length; // 6
```

`Array`可以通过索引把对应的元素修改为新的值，因此，对`Array`的索引进行赋值会直接修改这个`Array`：

```javascript
var arr = ['A', 'B', 'C'];
arr[1] = 99;
arr; // arr现在变为['A', 99, 'C']
```

### JavaScript的数组可修改大小

大多数其他编程语言不允许直接改变数组的大小，越界访问索引会报错。然而，JavaScript的`Array`却不会有任何错误。在编写代码时，不建议直接修改`Array`的大小，访问索引时要确保索引不会越界。

直接给`Array`的`length`赋一个新的值会导致`Array`大小的变化：

```javascript
var arr = [1, 2, 3];
arr.length; // 3
arr.length = 6;
arr; // arr变为[1, 2, 3, undefined, undefined, undefined]
arr.length = 2;
arr; // arr变为[1, 2]
```

如果通过索引赋值时，索引超过了范围，同样会引起`Array`大小的变化：

```javascript
var arr = [1, 2, 3];
arr[5] = 'x';
arr; // arr变为[1, 2, 3, undefined, undefined, 'x']
```

### 数组常用方法

| 方法          | 描述                                                 |
| :------------ | :--------------------------------------------------- |
| concat()      | 连接两个或更多的数组，并返回结果。                   |
| copyWithin()  | 从数组的指定位置拷贝元素到数组的另一个指定位置中。   |
| entries()     | 返回数组的可迭代对象。                               |
| every()       | 检测数值元素的每个元素是否都符合条件。               |
| fill()        | 使用一个固定值来填充数组。                           |
| filter()      | 检测数值元素，并返回符合条件所有元素的数组。         |
| find()        | 返回符合传入测试（函数）条件的数组元素。             |
| findIndex()   | 返回符合传入测试（函数）条件的数组元素索引。         |
| forEach()     | 数组每个元素都执行一次回调函数。                     |
| from()        | 通过给定的对象中创建一个数组。                       |
| includes()    | 判断一个数组是否包含一个指定的值。                   |
| indexOf()     | 搜索数组中的元素，并返回它所在的位置。               |
| isArray()     | 判断对象是否为数组。                                 |
| join()        | 把数组的所有元素放入一个字符串。                     |
| keys()        | 返回数组的可迭代对象，包含原始数组的键(key)。        |
| lastIndexOf() | 搜索数组中的元素，并返回它最后出现的位置。           |
| map()         | 通过指定函数处理数组的每个元素，并返回处理后的数组。 |
| pop()         | 删除数组的最后一个元素并返回删除的元素。             |
| push()        | 向数组的末尾添加一个或更多元素，并返回新的长度。     |
| reduce()      | 将数组元素计算为一个值（从左到右）。                 |
| reduceRight() | 将数组元素计算为一个值（从右到左）。                 |
| reverse()     | 反转数组的元素顺序。                                 |
| shift()       | 删除并返回数组的第一个元素。                         |
| slice()       | 选取数组的的一部分，并返回一个新数组。               |
| some()        | 检测数组元素中是否有元素符合指定条件。               |
| sort()        | 对数组的元素进行排序。                               |
| splice()      | 从数组中添加或删除元素。                             |
| unshift()     | 向数组的开头添加一个或更多元素，并返回新的长度。     |

**indexOf：**

与String类似，`Array`也可以通过`indexOf()`来搜索一个指定的元素的位置：

```javascript
var arr = [10, 20, '30', 'xyz'];
arr.indexOf(10); // 元素10的索引为0
arr.indexOf(20); // 元素20的索引为1
arr.indexOf(30); // 元素30没有找到，返回-1
arr.indexOf('30'); // 元素'30'的索引为2
```

注意了，数字`30`和字符串`'30'`是不同的元素。

**slice：**

`slice()`就是对应String的`substring()`版本，它截取`Array`的部分元素，然后返回一个新的`Array`：

```javascript
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
arr.slice(0, 3); // 从索引0开始，到索引3结束，但不包括索引3: ['A', 'B', 'C']
arr.slice(3); // 从索引3开始到结束: ['D', 'E', 'F', 'G']
```

注意到`slice()`的起止参数包括开始索引，不包括结束索引。

如果不给`slice()`传递任何参数，它就会从头到尾截取所有元素。利用这一点，我们可以很容易地复制一个`Array`：

```javascript
var arr = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];
var aCopy = arr.slice();
aCopy; // ['A', 'B', 'C', 'D', 'E', 'F', 'G']
aCopy === arr; // false
```

**push和pop：**

`push()`向`Array`的末尾添加若干元素，`pop()`则把`Array`的最后一个元素删除掉：

```javascript
var arr = [1, 2];
arr.push('A', 'B'); // 返回Array新的长度: 4
arr; // [1, 2, 'A', 'B']
arr.pop(); // pop()返回'B'
arr; // [1, 2, 'A']
arr.pop(); arr.pop(); arr.pop(); // 连续pop 3次
arr; // []
arr.pop(); // 空数组继续pop不会报错，而是返回undefined
arr; // []
```

**unshift和shift：**

如果要往`Array`的头部添加若干元素，使用`unshift()`方法，`shift()`方法则把`Array`的第一个元素删掉：

```javascript
var arr = [1, 2];
arr.unshift('A', 'B'); // 返回Array新的长度: 4
arr; // ['A', 'B', 1, 2]
arr.shift(); // 'A'
arr; // ['B', 1, 2]
arr.shift(); arr.shift(); arr.shift(); // 连续shift 3次
arr; // []
arr.shift(); // 空数组继续shift不会报错，而是返回undefined
arr; // []
```

**sort：**

`sort()`可以对当前`Array`进行排序，它会直接修改当前`Array`的元素位置，直接调用时，按照默认顺序排序：

```javascript
var arr = ['B', 'C', 'A'];
arr.sort();
arr; // ['A', 'B', 'C']
```

降序排序： 

```javascript
var points = [40,100,1,5,25,10];
points.sort(function(a,b){return b-a});
points;//100,40,25,10,5,1
```

**reverse：**

`reverse()`把整个`Array`反转：

```javascript
var arr = ['one', 'two', 'three'];
arr.reverse(); 
arr; // ['three', 'two', 'one']
```

**splice：**

`splice()`方法可以从指定的索引开始删除若干元素，然后再从该位置添加若干元素：

```javascript
var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
// 从索引2开始删除3个元素,然后再添加两个元素:
arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
// 只删除,不添加:
arr.splice(2, 2); // ['Google', 'Facebook']
arr; // ['Microsoft', 'Apple', 'Oracle']
// 只添加,不删除:
arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
```

**concat：**

`concat()`方法把当前的`Array`和另一个`Array`连接起来，并返回一个新的`Array`：

```javascript
var arr = ['A', 'B', 'C'];
var added = arr.concat([1, 2, 3]);
added; // ['A', 'B', 'C', 1, 2, 3]
arr; // ['A', 'B', 'C']
```

请注意，`concat()`方法并没有修改当前`Array`，而是返回了一个新的`Array`。

实际上，`concat()`方法可以接收任意个元素和`Array`，并且自动把`Array`拆开，然后全部添加到新的`Array`里：

```javascript
var arr = ['A', 'B', 'C'];
arr.concat(1, 2, [3, 4]); // ['A', 'B', 'C', 1, 2, 3, 4]
```

**join：**

`join()`方法把当前`Array`的每个元素都用指定的字符串连接起来，然后返回连接后的字符串：

```javascript
var arr = ['A', 'B', 'C', 1, 2, 3];
arr.join('-'); // 'A-B-C-1-2-3'
```

如果`Array`的元素不是字符串，将自动转换为字符串后再连接。

## 对象

 创建 JavaScript 对象: 

```javascript
var person = {firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"};
// 可横跨多行
var p1 = {
    firstName:"John",
    lastName:"Doe",
    age:50,
    eyeColor:"blue",
    fullName : function() { return this.firstName+" "+this.lastName; }
};
```

读取对象属性：

```javascript
name=person.lastname;
name=person["lastname"];
```

访问了person对象的fullName() 方法:

```javascript
name = person.fullName();
// 直接访问fullName属性，它将作为一个定义函数的字符串返回
name = person.fullName;
```

### 动态添加/删除属性

由于JavaScript的对象是动态类型，你可以自由地给一个对象添加或删除属性：

```javascript
var xiaoming = {
    name: '小明'
};
xiaoming.age; // undefined
xiaoming.age = 18; // 新增一个age属性
xiaoming.age; // 18
delete xiaoming.age; // 删除age属性
xiaoming.age; // undefined
delete xiaoming['name']; // 删除name属性
xiaoming.name; // undefined
delete xiaoming.school; // 删除一个不存在的school属性也不会报错
```

### 属性检测

如果我们要检测`xiaoming`是否拥有某一属性，可以用`in`操作符：

```javascript
var xiaoming = {
    name: '小明',
    birth: 1990,
    school: 'No.1 Middle School',
    height: 1.70,
    weight: 65,
    score: null
};
'name' in xiaoming; // true
'grade' in xiaoming; // false
```

不过要小心，如果`in`判断一个属性存在，这个属性不一定是`xiaoming`的，它可能是`xiaoming`继承得到的：

```javascript
'toString' in xiaoming; // true
```

因为`toString`定义在`object`对象中，而所有对象最终都会在原型链上指向`object`，所以`xiaoming`也拥有`toString`属性。

要判断一个属性是否是`xiaoming`自身拥有的，而不是继承得到的，可以用`hasOwnProperty()`方法：

```javascript
var xiaoming = {
    name: '小明'
};
xiaoming.hasOwnProperty('name'); // true
xiaoming.hasOwnProperty('toString'); // false
```

## Number

以下都是合法的Number类型：

```javascript
123; // 整数123
0.456; // 浮点数0.456
1.2345e3; // 科学计数法表示1.2345x1000，等同于1234.5
-99; // 负数
NaN; // NaN表示Not a Number，当无法计算结果时用NaN表示
Infinity; // Infinity表示无限大，当数值超过了JavaScript的Number所能表示的最大值时，就表示为Infinity
```

数字类型还可以通过科学（指数）计数法来书写：

```javascript
var x1=34.00;   //使用小数点来写
var x2=34;     //不使用小数点来写
var y=123e5;   // 12300000
var z=123e-5;   // 0.00123
```

计算机由于使用二进制，所以，有时候用十六进制表示整数比较方便，十六进制用0x前缀和0-9，a-f表示，例如：`0xff00`，`0xa5b4c3d2`，等等，它们和十进制表示的数值完全一样。 

四则运算：

```javascript
1 + 2; // 3
(1 + 2) * 5 / 2; // 7.5
2 / 0; // Infinity
0 / 0; // NaN
10 % 3; // 1
10.5 % 3; // 1.5
```

全局方法 **Number()** 可以将字符串转换为数字， 如果参数不能转换为一个数字将返回 NaN (非数字值)。 

```javascript
Number("3.14")  // 返回 3.14
Number(" ")    // 返回 0 
Number("")    // 返回 0
Number("99 88")  // 返回 NaN
```





### Number对象属性

| 属性              | 描述                       |
| :---------------- | :------------------------- |
| MAX_VALUE         | 可表示的最大的数。         |
| MIN_VALUE         | 可表示的最小的数。         |
| NEGATIVE_INFINITY | 负无穷大，溢出时返回该值。 |
| NaN               | 非数字值。                 |
| POSITIVE_INFINITY | 正无穷大，溢出时返回该值。 |

ES 6 增加了以下三个 Number 对象的属性：

- EPSILON: 表示 1 和比最接近 1 且大于 1 的最小 Number 之间的差别
- MIN_SAFE_INTEGER: 表示在 JavaScript中最小的安全的 integer 型数字 (`-(253 - 1)`)。
- MAX_SAFE_INTEGER: 表示在 JavaScript 中最大的安全整数（`253 - 1`）。

### Number对象方法

| 方法             | 描述                                              |
| :--------------- | :------------------------------------------------ |
| isFinite         | 检测指定参数是否为无穷大。                        |
| toExponential(x) | 把对象的值转换为指数计数法。                      |
| toFixed(x)       | 把数字转换为字符串，四舍五入为指定小数位数的数字. |
| toPrecision(x)   | 把数字格式化为指定的长度。                        |

ES 6 增加了以下两个 Number 对象的方法：

- Number.isInteger(): 用来判断给定的参数是否为整数。
- Number.isSafeInteger(): 判断传入的参数值是否是一个"安全整数"。 安全整数范围为 `-(253 - 1)到` `253 - 1 `之间的整数，包含 `-(253 - 1)和` `253 - 1`。 

Number.isFinite() 与全局的 isFinite() 函数不同，全局的 isFinite() 会先把检测值转换为 Number ，然后在检测。

Number.isFinite() 不会将检测值转换为 Number对象，如果检测值不是 Number 类型，则返回 false。

检测参数是否为无穷大：

```javascript
Number.isFinite(123) //true
Number.isFinite(-1.23) //true
Number.isFinite(5-2) //true
Number.isFinite(0) //true
Number.isFinite('123') //false
Number.isFinite('Hello') //false
Number.isFinite('2005/12/12') //false
Number.isFinite(Infinity) //false
Number.isFinite(-Infinity) //false
Number.isFinite(0 / 0) //false
```

把一个数字转换为指数计数法：

```javascript
var num = 5.56789;
var n=num.toExponential(3)//5.568e+0
```

 toFixed()示例：

```javascript
var num = 5.56789;
var n=num.toFixed(2);//5.57

var num = 5.56789;
var n=num.toFixed();//6

var num = 5.56789;
var n=num.toFixed(10);//5.5678900000
```

toPrecision()示例：

```javascript
var num = new Number(13.3714);
var a = num.toPrecision();//13.3714
var b = num.toPrecision(2);//13
var c = num.toPrecision(3);//13.4
var d = num.toPrecision(10);//13.37140000
```

### Math对象方法

| 方法             | 描述                                                         |
| :--------------- | :----------------------------------------------------------- |
| abs(x)           | 返回 x 的绝对值。                                            |
| acos(x)          | 返回 x 的反余弦值。                                          |
| asin(x)          | 返回 x 的反正弦值。                                          |
| atan(x)          | 以介于 -PI/2 与 PI/2 弧度之间的数值来返回 x 的反正切值。     |
| atan2(y,x)       | 返回从 x 轴到点 (x,y) 的角度（介于 -PI/2 与 PI/2 弧度之间）。 |
| ceil(x)          | 对数进行上舍入。                                             |
| cos(x)           | 返回数的余弦。                                               |
| exp(x)           | 返回 Ex 的指数。                                             |
| floor(x)         | 对 x 进行下舍入。                                            |
| log(x)           | 返回数的自然对数（底为e）。                                  |
| max(x,y,z,...,n) | 返回 x,y,z,...,n 中的最高值。                                |
| min(x,y,z,...,n) | 返回 x,y,z,...,n中的最低值。                                 |
| pow(x,y)         | 返回 x 的 y 次幂。                                           |
| random()         | 返回 0 ~ 1 之间的随机数。                                    |
| round(x)         | 四舍五入。                                                   |
| sin(x)           | 返回数的正弦。                                               |
| sqrt(x)          | 返回数的平方根。                                             |
| tan(x)           | 返回角的正切。                                               |

示例：

```javascript
Math.abs(-7.25);
Math.acos(0.5);
Math.atan2(8,4);
Math.log(2);
Math.max(5,10);
Math.pow(4,3);
Math.round(2.5);
Math.sin(3);
Math.tan(90);
```



## 字符串

字符串的索引从 0 开始，这意味着第一个字符索引值为 [0],第二个为 [1], 以此类推。

```javascript
var character = carname[7];
```

字符串可以是引号中的任意文本，可以使用单引号或双引号：

```javascript
var carname="Volvo XC60";
var carname='Volvo XC60';
```

可以在字符串中使用引号，字符串中的引号不要与字符串的引号相同：

```javascript
var answer = "It's alright";
var answer = "He is called 'Johnny'";
var answer = 'He is called "Johnny"';
```

可以在字符串添加转义字符来使用引号：

```javascript
var x = 'It\'s alright';
var y = "He is called \"Johnny\"";
```

可以使用内置属性 **length** 来计算字符串的长度：

```javascript
var txt = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
var sln = txt.length;
```

### 多行字符串

由于多行字符串用`\n`写起来比较费事，所以最新的ES6标准用反引号表示字符串：

```javascript
console.log(
`多行
字符串
测试`);
```

### 模板字符串

```javascript
var name = '小明';
var age = 20;
var message = `你好, ${name}, 你今年${age}岁了!`;
alert(message);
```

### 转义字符

可以使用转义字符转义的特殊字符：

| 代码 | 输出        |
| :--- | :---------- |
| \'   | 单引号      |
| \"   | 双引号      |
| \\   | 反斜杠      |
| \n   | 换行        |
| \r   | 回车        |
| \t   | tab(制表符) |
| \b   | 退格符      |
| \f   | 换页符      |

### 字符串对象

通常， JavaScript 字符串是原始值，可以使用字符创建： **var firstName = "John"**

但也可以使用 new 关键字将字符串定义为一个对象： **var firstName = new String("John")**

```javascript
var x = "John";
var y = new String("John");
typeof x // 返回 String
typeof y // 返回 Object
```

不要创建 String 对象，它会拖慢执行速度。

```
var x = "John";
var y = new String("John");
(x === y) // 结果为 false，因为 x 是字符串，y 是对象
```

=== 为绝对相等，即数据类型与值都必须相等。

### 操作字符串的常见方法

字符串的方法有：

| 方法                                            | 示例                                                         | 描述                                               |
| :---------------------------------------------- | ------------------------------------------------------------ | :------------------------------------------------- |
| charAt(index)                                   | "HELLO WORLD".charAt(2)                                      | 返回字符串中的第三个字符                           |
| charCodeAt(index)                               | "HELLO WORLD".charCodeAt(0)                                  | 返回字符串第一个字符的 Unicode 编码                |
| `string.concat(string1, string2, ..., stringX)` | "Hello ".concat("world!");                                   | 连接两个字符串                                     |
| `String.fromCharCode(n1, n2, ..., nX)`          | String.fromCharCode(72,69,76,76,79);                         | 将 Unicode 编码转换为一个字符串结果为HELLO         |
| indexOf(searchvalue,start)                      | "Hello world, welcome to the universe.".indexOf("welcome");  | 查找字符串 "welcome"在主串中的位置                 |
| lastIndexOf(searchvalue,start)                  | "I am from runoob，welcome to runoob site.".lastIndexOf("runoob", 20); | 从第20个字符开始查找字符串 "runoob" 最后出现的位置 |
| repeat(count)                                   | "Runoob".repeat(2);                                          | 复制字符串 "Runoob" 两次                           |
| match(regexp)                                   | "The rain in SPAIN stays mainly in the plain".match(/ain/gi); | 全局查找字符串 "ain"，且不区分大小写               |
| replace(searchvalue,newvalue)                   | "Mr Blue has a blue house and a blue car".replace(/blue/g,"red"); | 将所有的blue替换为red                              |
| search(searchvalue)                             | "Mr. Blue has a blue house".search(/blue/i);                 | 忽略大小写的检索blue第一次出现的位置               |
| slice(start,end)                                | "Hello world!".slice(3,8);                                   | 跟substring一样但支持负数角标                      |
| split(separator,limit)                          | "How are you doing today?".split(" ",3);                     | 把字符串分割为子字符串数组                         |
| substr(start,length)                            | "Hello world!".substr(2,3)                                   | 从角标2开始取3个字符                               |
| substring(from, to)                             | "Hello world!".substring(3,7)                                | 提取字符串中角标3到6的字符                         |
| toLowerCase()                                   | "Runoob".toLowerCase();                                      | 把字符串转换为小写                                 |
| toUpperCase()                                   | "Runoob".toUpperCase();                                      | 把字符串转换为大写                                 |
| trim()                                          | "       Runoob        ".trim())                              | 移除字符串首尾空白                                 |

`toUpperCase()`把一个字符串全部变为大写：

```javascript
var s = 'Hello';
s.toUpperCase(); // 返回'HELLO'
```

`toLowerCase()`把一个字符串全部变为小写：

```javascript
var s = 'Hello';
var lower = s.toLowerCase(); // 返回'hello'并赋值给变量lower
lower; // 'hello'
```

`indexOf()`会搜索指定字符串出现的位置：

```javascript
var s = 'hello, world';
s.indexOf('world'); // 返回7
s.indexOf('World'); // 没有找到指定的子串，返回-1
```

`substring()`返回指定索引区间的子串：

```javascript
var s = 'hello, world'
s.substring(0, 5); // 从索引0开始到5（不包括5），返回'hello'
s.substring(7); // 从索引7开始到结束，返回'world'
```

## Map

`Map`是一组键值对的结构，具有极快的查找速度。

根据数组创建Map：

```javascript
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Michael'); // 95
```

初始化`Map`需要一个二维数组，或者直接初始化一个空`Map`。`Map`具有以下方法：

```javascript
var m = new Map(); // 空Map
m.set('Adam', 67); // 添加新的key-value
m.set('Bob', 59);
m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
m.get('Adam'); // undefined
```

## Set

在`Set`中，没有重复的key。

要创建一个`Set`，需要提供一个`Array`作为输入，或者直接创建一个空`Set`：

```javascript
var s1 = new Set(); // 空Set
var s2 = new Set([1, 2, 3]); // 含1, 2, 3
```

重复元素在`Set`中自动被过滤：

```javascript
var s = new Set([1, 2, 3, 3, '3']);
s; // Set {1, 2, 3, "3"}
```

数字`3`和字符串`'3'`是不同的元素。

重复添加效果：

```javascript
s.add(4);
s; // Set {1, 2, 3, 4}
s.add(4);
s; // 仍然是 Set {1, 2, 3, 4}
```

通过`delete(key)`方法可以删除元素：

```javascript
var s = new Set([1, 2, 3]);
s; // Set {1, 2, 3}
s.delete(3);
s; // Set {1, 2}
```

# 流程控制语句

## 条件语句

if-else语法：

```javascript
if (condition1)
{
    当条件 1 为 true 时执行的代码
}
else if (condition2)
{
    当条件 2 为 true 时执行的代码
}
else
{
  当条件 1 和 条件 2 都不为 true 时执行的代码
}
三元表达式：
variablename=(condition)?value1:value2 
```

 如果变量 age 中的值小于 18，则向变量 voteable 赋值 "年龄太小"，否则赋值 "年龄已达到"。 

```javascript
 voteable=(age<18)?"年龄太小":"年龄已达到"; 
```

如果时间小于 10:00，则生成问候 "Good morning"，如果时间大于 10:00 小于 20:00，则生成问候 "Good day"，否则生成问候 "Good evening"： 

```javascript
if (time<10)
{
    document.write("<b>早上好</b>");
}
else if (time>=10 && time<16)
{
    document.write("<b>今天好</b>");
}
else
{
    document.write("<b>晚上好!</b>");
}
```

switch语法：

```javascript
switch(n)
{
    case 1:
        执行代码块 1
        break;
    case 2:
        执行代码块 2
        break;
    default:
        与 case 1 和 case 2 不同时执行的代码
}
```

 如果今天不是星期六或星期日，则会输出默认的消息： 

```javascript
var x;
var d=new Date().getDay();
switch (d)
{
    case 6:x="今天是星期六";
    break;
    case 0:x="今天是星期日";
    break;
    default:
    x="期待周末";
}
document.getElementById("demo").innerHTML=x;
```

## 循环语句

### for循环

语法： 

```javascript
for (语句 1; 语句 2; 语句 3)
{
    被执行的代码块
}
```

**语句 1** （代码块）开始前执行

**语句 2** 定义运行循环（代码块）的条件

**语句 3** 在循环（代码块）已被执行之后执行

```javascript
for (var i=0,len=cars.length; i<len; i++)
{ 
    document.write(cars[i] + "<br>");
}
或
var i=2,len=cars.length;
for (; i<len; i++)
{ 
    document.write(cars[i] + "<br>");
}
```

### for...in循环

for/in 语句循环遍历对象的属性： 

```javascript
var person={fname:"John",lname:"Doe",age:25}; 
for (x in person)  // x 为属性名
{
    txt=txt + person[x];
}
```

循环遍历`Array`的索引：

```javascript
var a = ['A', 'B', 'C'];
for (var i in a) {
    console.log(i); // '0', '1', '2'
    console.log(a[i]); // 'A', 'B', 'C'
}
```

> 注意，`for ... in`对`Array`的循环得到的是`String`而不是`Number`。

### for...of循环

具有`iterable`类型的集合还可以通过`for ... of`循环来遍历，它是ES6引入的新的语法。

```javascript
var a = ['A', 'B', 'C'];
var s = new Set(['A', 'B', 'C']);
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
for (var x of a) { // 遍历Array
    console.log(x);
}
for (var x of s) { // 遍历Set
    console.log(x);
}
for (var x of m) { // 遍历Map
    console.log(x[0] + '=' + x[1]);
}
```

### `for of`循环和`for in`循环的区别

`for ... in`循环遍历的实际是对象的属性名称，手动给Array对象添加了额外的属性后：

当我们手动给`Array`对象添加了额外的属性后，`for ... in`循环将带来意想不到的意外效果：

```javascript
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x in a) {
    console.log(x); // '0', '1', '2', 'name'
}
```

`for ... of`循环则只循环集合本身的元素：

```javascript
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x of a) {
    console.log(x); // 'A', 'B', 'C'
}
```

###  `iterable`内置的`forEach`方法 

```javascript
var a = ['A', 'B', 'C'];
a.forEach(function (element, index, array) {
    // element: 指向当前元素的值
    // index: 指向当前索引
    // array: 指向Array对象本身
    console.log(element + ', index = ' + index);
});
var s = new Set(['A', 'B', 'C']);
//Set没有索引，因此回调函数的前两个参数都是元素本身
s.forEach(function (element, sameElement, set) {
    console.log(element);
});
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
//Map的回调函数参数依次为value、key和map本身
m.forEach(function (value, key, map) {
    console.log(value);
});
```

回调函数可以省略参数：

```javascript
var a = ['A', 'B', 'C'];
a.forEach(function (element) {
    console.log(element);
});
```

### while 循环

语法：

```javascript
while (条件)
{
    需要执行的代码
}
或
do
{
    需要执行的代码
}
while (条件);
```

例子：

```javascript
while (i<5)
{
    x=x + "The number is " + i + "<br>";
    i++;
}
do
{
    x=x + "The number is " + i + "<br>";
    i++;
}
while (i<5);
```



## JavaScript异常处理

**try** 语句测试代码块的错误。

**catch** 语句处理错误。

**throw** 语句创建自定义错误。

**finally** 语句在 try 和 catch 语句之后，无论是否有触发异常，该语句都会执行。

```javascript
try {
    ...    //异常的抛出
} catch(e) {
    ...    //异常的捕获与处理
} finally {
    ...    //结束处理
}
```

只有try ... catch，没有finally：

```javascript
try {
    ...
} catch (e) {
    ...
}
```

只有try ... finally，没有catch：

```javascript
try {
    ...
} finally {
    ...
}
```

**例子:**

```javascript
var txt=""; 
function message() 
{ 
    try { 
        adddlert("Welcome guest!"); 
    } catch(err) { 
        txt="本页有一个错误。\n\n"; 
        txt+="错误描述：" + err.message + "\n\n"; 
        txt+="点击确定继续。\n\n"; 
        alert(txt); 
    } 
}
```

finally 语句不论之前的 try 和 catch 中是否产生异常都会执行该代码块：

```javascript
function myFunction() {
  var message, x;
  message = document.getElementById("p01");
  message.innerHTML = "";
  x = document.getElementById("demo").value;
  try { 
    if(x == "") throw "值是空的";
    if(isNaN(x)) throw "值不是一个数字";
    x = Number(x);
    if(x > 10) throw "太大";
    if(x < 5) throw "太小";
  }
  catch(err) {
    message.innerHTML = "错误: " + err + ".";
  }
  finally {
    document.getElementById("demo").value = "";
  }
}
```

### 错误类型

JavaScript有一个标准的`Error`对象表示错误，还有从`Error`派生的`TypeError`、`ReferenceError`等错误对象。在处理错误时，可以通过`catch(e)`捕获的变量`e`访问错误对象：

```JavaScript
try {
    ...
} catch (e) {
    if (e instanceof TypeError) {
        alert('Type error!');
    } else if (e instanceof Error) {
        alert(e.message);
    } else {
        alert('Error: ' + e);
    }
}
```

使用变量`e`是一个习惯用法，也可以以其他变量名命名，如`catch(ex)`。

### 抛出错误

程序也可以主动抛出一个错误，让执行流程直接跳转到`catch`块。抛出错误使用`throw`语句。

例如，下面的代码让用户输入一个数字，程序接收到的实际上是一个字符串，然后用`parseInt()`转换为整数。当用户输入不合法的时候，我们就抛出错误：

```JavaScript
'use strict';
var r, n, s;
try {
    s = prompt('请输入一个数字');
    n = parseInt(s);
    if (isNaN(n)) {
        throw new Error('输入错误');
    }
    // 计算平方:
    r = n * n;
    console.log(n + ' * ' + n + ' = ' + r);
} catch (e) {
    console.log('出错了：' + e);
}
```

实际上，JavaScript允许抛出任意对象，包括数字、字符串。但是，最好还是抛出一个Error对象。

最后，当我们用catch捕获错误时，一定要编写错误处理语句：

```JavaScript
var n = 0, s;
try {
    n = s.length;
} catch (e) {
    console.log(e);
}
console.log(n);
```

哪怕仅仅把错误打印出来，也不要什么也不干。因为catch到错误却什么都不执行，就不知道程序执行过程中到底有没有发生错误。

### 错误传播

如果代码发生了错误，又没有被try ... catch捕获，那么，程序执行流程会跳转到哪呢？

```JavaScript
function getLength(s) {
    return s.length;
}
function printLength() {
    console.log(getLength('abc')); // 3
    console.log(getLength(null)); // Error!
}
printLength();
```

如果在一个函数内部发生了错误，它自身没有捕获，错误就会被抛到外层调用函数，如果外层函数也没有捕获，该错误会一直沿着函数调用链向上抛出，直到被JavaScript引擎捕获，代码终止执行。

所以，不必在每一个函数内部捕获错误，只需要在合适的地方来个统一捕获，一网打尽：

```JavaScript
function main(s) {
    console.log('BEGIN main()');
    try {
        foo(s);
    } catch (e) {
        console.log('出错了：' + e);
    }
    console.log('END main()');
}

function foo(s) {
    console.log('BEGIN foo()');
    bar(s);
    console.log('END foo()');
}

function bar(s) {
    console.log('BEGIN bar()');
    console.log('length = ' + s.length);
    console.log('END bar()');
}

main(null);
```

 当`bar()`函数传入参数`null`时，代码会报错，错误会向上抛给调用方`foo()`函数，`foo()`函数没有try ... catch语句，所以错误继续向上抛给调用方`main()`函数，`main()`函数有try ... catch语句，所以错误最终在`main()`函数被处理了。 



# 函数

语法：

```javascript
function myFunction()
{
  var x=5;
  return x;
}
```

函数表达式可以存储在变量中，之后变量也可作为一个函数使用： 

```javascript
var x = function (a, b) {return a * b};
var z = x(4, 3);
```

 在这种方式下，`function (a, b) {...}`是一个匿名函数，它没有函数名。但是，这个匿名函数赋值给了变量`x`，所以，通过变量`x`就可以调用该函数。 

## 调用函数

调用函数时，按顺序传入参数即可：

```javascript
abs(10); // 返回10
abs(-9); // 返回9
```

由于JavaScript允许传入任意个参数而不影响调用，因此传入的参数比定义的参数多也没有问题，虽然函数内部并不需要这些参数：

```javascript
abs(10, 'blablabla'); // 返回10
abs(-9, 'haha', 'hehe', null); // 返回9
```

传入的参数比定义的少也没有问题：

```javascript
abs(); // 返回NaN
```

此时`abs(x)`函数的参数`x`将收到`undefined`，计算结果为`NaN`。

要避免收到`undefined`，可以对参数进行检查：

```javascript
function abs(x) {
    if (typeof x !== 'number') {
        throw 'Not a number';
    }
    if (x >= 0) {
        return x;
    } else {
        return -x;
    }
}
```

## 创建函数

### 自调用函数

会自动调用的函数，表达式：

```javascript
(function () {
  var x = "Hello!!";   // 我将调用自己
})();
```

以上函数实际上是一个 **匿名自我调用的函数** (没有函数名)。

理论上讲，创建一个匿名函数并立刻执行可以这么写：

```javascript
function (x) { return x * x } (3);
```

但是由于JavaScript语法解析的问题，会报SyntaxError错误，因此需要用括号把整个函数定义括起来：

```javascript
(function (x) { return x * x }) (3);
```

通常，一个立即执行的匿名函数可以把函数体拆开，一般这么写：

```javascript
(function (x) {
    return x * x;
})(3);
```

### 箭头函数

ES6 新增了箭头函数，语法 ：

```javascript
(参数1, 参数2, …, 参数N) => { 函数声明 }
单一参数 => {函数声明}
() => {函数声明}
```

例子：

```javascript
const x = (x, y) => x * y;
const x = (x, y) => { return x * y };
```

箭头函数简化了函数定义，如果只包含一个表达式，可以省略`{ ... }`和`return`。

如果要返回一个对象，单表达式会报错：

```javascript
// SyntaxError:
x => { foo: x }
```

因为和函数体的`{ ... }`有语法冲突，所以要改为：

```javascript
// ok:
x => ({ foo: x })
```

箭头函数看上去是匿名函数的一种简写，但有个明显的区别：箭头函数内部的`this`是词法作用域，由上下文确定。 

箭头函数的`this`总是指向词法作用域，也就是外层调用者`obj`：

```javascript
var obj = {
    birth: 1990,
    getAge: function () {
        var b = this.birth; // 1990
        var fn = () => new Date().getFullYear() - this.birth; // this指向obj对象
        return fn();
    }
};
obj.getAge(); // 25
```

由于`this`在箭头函数中已经按照词法作用域绑定了，所以，用`call()`或者`apply()`调用箭头函数时，无法对`this`进行绑定，即传入的第一个参数被忽略：

```javascript
var obj = {
    birth: 2000,
    getAge: function (year) {
        var b = this.birth; // 1990
        var fn = (y) => y - this.birth; // this.birth仍是2000
        return fn.call({birth:2010}, year);
    }
};
obj.getAge(2015); // 15
```

### 闭包

```javascript
var add = (function () {
    var counter = 0;
    return function () {return counter += 1;}
})();
 
add();
add();
add();
```

## 对象中绑定函数

在一个对象中绑定函数，称为这个对象的方法。

```javascript
var xiaoming = {
    name: '小明',
    birth: 2000,
    age: function () {
        var y = new Date().getFullYear();
        return y - this.birth;
    }
};

xiaoming.age; // function xiaoming.age()
xiaoming.age(); // 今年调用是20,明年调用就变成21了
```

在一个方法内部，`this`是一个特殊变量，它始终指向当前对象，也就是`xiaoming`这个变量。所以，`this.birth`可以拿到`xiaoming`的`birth`属性。

如果以对象的方法形式调用，比如`xiaoming.age()`，该函数的`this`指向被调用的对象，也就是`xiaoming`。

如果单独调用函数，比如`getAge()`，此时，该函数的`this`指向全局对象，也就是`window`。

```javascript
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};
xiaoming.age(); // 25, 正常结果
getAge(); // NaN
```

在strict模式下函数的`this`指向`undefined`，因此，在strict模式下会得到一个错误：

```javascript
'use strict';

var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var y = new Date().getFullYear();
        return y - this.birth;
    }
};
var fn = xiaoming.age;
fn(); // Uncaught TypeError: Cannot read property 'birth' of undefined
```

strict模式只是让错误及时暴露出来，并没有解决`this`应该指向的正确位置。

### apply

在一个独立的函数调用中，根据是否是strict模式，`this`指向`undefined`或`window`。

要指定函数的`this`指向哪个对象，可以用函数本身的`apply`方法，它接收两个参数，第一个参数就是需要绑定的`this`变量，第二个参数是`Array`，表示函数本身的参数。

用`apply`修复`getAge()`调用：

```javascript
function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
}
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
};
xiaoming.age(); // 25
getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空
```

利用`apply()`可实现装饰器，比如想统计一下代码一共调用了多少次：

```javascript
'use strict';

var count = 0;
var oldParseInt = parseInt; // 保存原函数

window.parseInt = function () {
    count += 1;
    return oldParseInt.apply(null, arguments); // 调用原函数
};
parseInt('10');
parseInt('20');
parseInt('30');
console.log('count = ' + count); // 3
```

### call

`call()`与`apply()`类似，唯一区别是：

- `apply()`把参数打包成`Array`再传入；
- `call()`把参数按顺序传入。

比如调用`Math.max(3, 5, 4)`，分别用`apply()`和`call()`实现如下：

```javascript
Math.max.apply(null, [3, 5, 4]); // 5
Math.max.call(null, 3, 5, 4); // 5
```

对普通函数调用，通常把`this`绑定为`null`。

## 参数

### 默认参数

```javascript
function myFunction(x, y = 10) {
    // y is 10 if not passed or undefined
    return x + y;
}
//相当于
function myFunction(x, y) {
    y = y || 10;
}
```

### 内置对象argument

内置对象argument包含了函数调用的参数数组。

通过这种方式你可以很方便的找到最大的一个参数的值：

```javascript
function findMax() {
    var max = arguments[0];
    if(arguments.length <= 1) return max;
    for (var i = 1; i < arguments.length; i++) {
        if (arguments[i] > max) {
            max = arguments[i];
        }
    }
    return max;
}
document.getElementById("demo").innerHTML = findMax(1,233,53,34,23);
```

 创建一个函数用来统计所有数值的和： 

```javascript
x = sumAll(1, 123, 500, 115, 44, 88);
 
function sumAll() {
    var i, sum = 0;
    for (i = 0; i < arguments.length; i++) {
        sum += arguments[i];
    }
    return sum;
}
```

### rest参数

接收任意个参数，除了使用`arguments`外还可以使用`rest`参数，示例：

```javascript
function foo(a, b, ...rest) {
    console.log('a = ' + a);
    console.log('b = ' + b);
    console.log(rest);
}

foo(1, 2, 3, 4, 5);
// 结果:
// a = 1
// b = 2
// Array [ 3, 4, 5 ]

foo(1);
// 结果:
// a = 1
// b = undefined
// Array []
```

rest参数只能写在最后，前面用`...`标识，从运行结果可知，传入的参数先绑定`a`、`b`，多余的参数以数组形式交给变量`rest`，所以，不再需要`arguments`就获取了全部参数。

如果传入的参数连正常定义的参数都没填满，也不要紧，rest参数会接收一个空数组（注意不是`undefined`）。

rest参数编写一个`sum()`函数，接收任意个参数并返回它们的和： 

```javascript
function sum(...rest) {
    var s=0;
    for (var i of rest)
    	s += i;
    return s;
}
```

##  void操作符

 void操作符指定要计算一个表达式但是不返回值。 

```javascript
void func()
javascript:void func()
或者
void(func())
javascript:void(func())
```

示例：

```html
//点击以后不会发生任何事
<a href="javascript:void(0)">单击此处什么也不会发生</a>
//在用户点击链接后显示警告信息
<a href="javascript:void(alert('Warning!!!'))">点我!</a>
var a,b,c;
//参数 a 将返回 undefined :
<script type="text/javascript">
   var a,b,c;
   a = void ( b = 5, c = 7 );
   document.write('a = ' + a + ' b = ' + b +' c = ' + c );
</script>
```

