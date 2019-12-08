# underscore

JavaScript可通过`Array`实现函数式编程，支持高阶函数和闭包。例如`Array`的`map()`和`filter()`方法：

```javascript
'use strict';
var a1 = [1, 4, 9, 16];
var a2 = a1.map(Math.sqrt); // [1, 2, 3, 4]
var a3 = a2.filter((x) => { return x % 2 === 0; }); // [2, 4]
```

而第三方库underscore提供了一套完善的函数式编程的接口，可以更方便地在JavaScript中实现函数式编程。

引入underscore：

```html
<script type="text/javascript" src="https://underscorejs.org/underscore-min.js"></script>
```

jQuery在加载时，会把自身绑定到唯一的全局变量`$`上，underscore与其类似，会把自身绑定到唯一的全局变量`_`上，这也是为啥它的名字叫underscore的原因。

用underscore实现`map()`操作如下：

```javascript
'use strict';
_.map([1, 2, 3], (x) => x * x); // [1, 4, 9]
```

可以作用于Object：

```javascript
'use strict';
_.map({ a: 1, b: 2, c: 3 }, (v, k) => k + '=' + v); // ['a=1', 'b=2', 'c=3']
```

## Collections

underscore为集合类Array和Object对象提供了一致的接口。暂不支持Map和Set。

underscore文档：http://underscorejs.org/#collections 

### map/filter

和`Array`的`map()`与`filter()`类似，但是underscore的`map()`和`filter()`可以作用于Object。当作用于Object时，传入的函数为`function (value, key)`，第一个参数接收value，第二个参数接收key：

```javascript
'use strict';

var obj = {
    name: 'bob',
    school: 'No.1 middle school',
    address: 'xueyuan road'
};
var upper_arr = _.map(obj, function (value, key) {
    return value.toUpperCase();
});
console.log(JSON.stringify(upper_arr));
//["BOB","NO.1 MIDDLE SCHOOL","XUEYUAN ROAD"]
var upper = _.mapObject(obj, function (value, key) {
    return value.toUpperCase();
});
console.log(JSON.stringify(upper));
//{"name":"BOB","school":"NO.1 MIDDLE SCHOOL","address":"XUEYUAN ROAD"}
```

 mapObject 可以返回对象。

### every/some

当集合的所有元素都满足条件时，`_.every()`函数返回`true`，当集合的至少一个元素满足条件时，`_.some()`函数返回`true`：

```javascript
'use strict';
// 所有元素都大于0？
_.every([1, 4, 7, -3, -9], (x) => x > 0); // false
// 至少一个元素大于0？
_.some([1, 4, 7, -3, -9], (x) => x > 0); // true
```

当集合是Object时，可以同时获得value和key：

```javascript
'use strict';
var obj = {
    name: 'bob',
    school: 'No.1 middle school',
    address: 'xueyuan road'
};
// 判断key和value是否全部是小写：
var r1 = _.every(obj, function (value, key) {
    return value===value.toLowerCase()&&key===key.toLowerCase();
});
var r2 = _.some(obj, function (value, key) {
    return value===value.toLowerCase()&&key===key.toLowerCase();
});
console.log('every key-value are lowercase: ' + r1 + '\nsome key-value are lowercase: ' + r2);
```

### max/min

这两个函数直接返回集合中最大和最小的数：

```javascript
'use strict';
var arr = [3, 5, 7, 9];
_.max(arr); // 9
_.min(arr); // 3

// 空集合会返回-Infinity和Infinity，所以要先判断集合不为空：
_.max([])
-Infinity
_.min([])
Infinity
```

注意，如果集合是Object，`max()`和`min()`只作用于value，忽略掉key：

```javascript
'use strict';
_.max({ a: 1, b: 2, c: 3 }); // 3
```

### groupBy

`groupBy()`把集合的元素按照key归类，key由传入的函数返回：

```javascript
'use strict';

var scores = [20, 81, 75, 40, 91, 59, 77, 66, 72, 88, 99];
var groups = _.groupBy(scores, function (x) {
    if (x < 60) {
        return 'C';
    } else if (x < 80) {
        return 'B';
    } else {
        return 'A';
    }
});
// 结果:
// {
//   A: [81, 91, 88, 99],
//   B: [75, 77, 66, 72],
//   C: [20, 40, 59]
// }
```

可见`groupBy()`用来分组是非常方便的。

### shuffle/sample

`shuffle()`用洗牌算法随机打乱一个集合：

```javascript
'use strict';
// 注意每次结果都不一样：
_.shuffle([1, 2, 3, 4, 5, 6]); // [3, 5, 4, 6, 2, 1]
```

`sample()`则是随机选择一个或多个元素：

```javascript
'use strict';
// 注意每次结果都不一样：
// 随机选1个：
_.sample([1, 2, 3, 4, 5, 6]); // 2
// 随机选3个：
_.sample([1, 2, 3, 4, 5, 6], 3); // [6, 1, 4]
```

## Arrays

underscore为`Array`提供了许多工具类方法，可以更方便快捷地操作`Array`。

参考underscore文档：http://underscorejs.org/#arrays

### first/last

顾名思义，这两个函数分别取第一个和最后一个元素：

```javascript
'use strict';
var arr = [2, 4, 6, 8];
_.first(arr); // 2
_.last(arr); // 8
```

### flatten

`flatten()`接收一个`Array`，无论这个`Array`里面嵌套了多少个`Array`，`flatten()`最后都把它们变成一个一维数组：

```javascript
'use strict';

_.flatten([1, [2], [3, [[4], [5]]]]); // [1, 2, 3, 4, 5]
```

### zip/unzip

`zip()`把两个或多个数组的所有元素按索引对齐，然后按索引合并成新数组。例如，你有一个`Array`保存了名字，另一个`Array`保存了分数，现在，要把名字和分数给对上，用`zip()`轻松实现：

```javascript
'use strict';

var names = ['Adam', 'Lisa', 'Bart'];
var scores = [85, 92, 59];
_.zip(names, scores);
// [['Adam', 85], ['Lisa', 92], ['Bart', 59]]
```

`unzip()`则是反过来：

```javascript
'use strict';
var namesAndScores = [['Adam', 85], ['Lisa', 92], ['Bart', 59]];
_.unzip(namesAndScores);
// [['Adam', 'Lisa', 'Bart'], [85, 92, 59]]
```

### object

```javascript
'use strict';

var names = ['Adam', 'Lisa', 'Bart'];
var scores = [85, 92, 59];
_.object(names, scores);
// {Adam: 85, Lisa: 92, Bart: 59}
```

注意`_.object()`是一个函数，不是JavaScript的`Object`对象。

### range

`range()`让你快速生成一个序列，不再需要用`for`循环实现了：

```javascript
'use strict';

// 从0开始小于10:
_.range(10); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

// 从1开始小于11：
_.range(1, 11); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

// 从0开始小于30，步长5:
_.range(0, 30, 5); // [0, 5, 10, 15, 20, 25]

// 从0开始大于-10，步长-1:
_.range(0, -10, -1); // [0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
```

### uniq

使用`uniq()`对数组元素进行**不区分大小写**去重： 

```javascript
'use strict';

var arr = ['Apple', 'orange', 'banana', 'ORANGE', 'apple', 'PEAR'];
var result = _.uniq(arr,x=>x.toLowerCase());
console.log(result);
```



## Functions

underscore为了充分发挥JavaScript的函数式编程特性，提供了大量JavaScript本身没有的高阶函数。

参考underscore文档：http://underscorejs.org/#functions

### bind

`bind()`可以把`s`对象直接绑定在`fn()`的`this`指针上，就可以直接调用`fn()`了：

```javascript
'use strict';

var s = ' Hello  ';
var fn = _.bind(s.trim, s);
fn();
// 输出Hello
```

结论：当用一个变量`fn`指向一个对象的方法时，直接调用`fn()`是不行的，因为丢失了`this`对象的引用。用`bind`可以修复这个问题。

### partial

`partial()`就是为一个函数创建偏函数。偏函数是固定原函数某些参数的函数。比如固定第一个参数：

```javascript
'use strict';

var pow2N = _.partial(Math.pow, 2);
pow2N(3); // 8
pow2N(5); // 32
pow2N(10); // 1024
```

固定第二个参数的偏函数可以用`_`作占位符：

```javascript
'use strict';

var cube = _.partial(Math.pow, _, 3);
cube(3); // 27
cube(5); // 125
cube(10); // 1000
```

创建偏函数的目的是将原函数的某些参数固定住，可以降低新函数调用的难度。

### memoize

用`memoize()`可以自动缓存函数计算的结果：

```javascript
'use strict';

var factorial = _.memoize(function(n) {
    console.log('start calculate ' + n + '!...');
    if (n <= 2) {
        return n;
    }
    return n * factorial(n - 1);
});

factorial(10); // 3628800
// 输出结果说明factorial(1)~factorial(10)都已经缓存了:
// start calculate 10!...
// start calculate 9!...
// start calculate 8!...
// start calculate 7!...
// start calculate 6!...
// start calculate 5!...
// start calculate 4!...
// start calculate 3!...
// start calculate 2!...
// start calculate 1!...

factorial(9); // 362880
// console无输出
```

### once

`once()`可以保证某个函数执行且仅执行一次。例如有一个方法叫`register()`，页面上点两个按钮的任何一个都可以执行的话，就可以用`once()`保证函数仅调用一次，无论用户点击按钮多少次：

```javascript
var register = _.once(function () {
    alert('Register ok!');
});

register();
register();
register();
```

### delay

`delay()`可以让一个函数延迟执行，效果和`setTimeout()`是一样的，但是代码明显简单了：

```javascript
'use strict';

// 2秒后调用alert():
_.delay(alert, 2000);
```

如果要延迟调用的函数有参数，把参数也传进去：

```javascript
'use strict';

var log = _.bind(console.log, console);
_.delay(log, 2000, 'Hello,', 'world!');
// 2秒后打印'Hello, world!':
```

## Objects

参考：https://underscorejs.org/#objects

### keys/allKeys

`keys()`可以非常方便地返回一个object自身所有的key，但不包含从原型链继承下来的：

```javascript
'use strict';

function Student(name, age) {
    this.name = name;
    this.age = age;
}

var xiaoming = new Student('小明', 20);
_.keys(xiaoming); // ['name', 'age']
```

`allKeys()`除了object自身的key，还包含从原型链继承下来的：

```javascript
'use strict';

function Student(name, age) {
    this.name = name;
    this.age = age;
}
Student.prototype.school = 'No.1 Middle School';
var xiaoming = new Student('小明', 20);
_.allKeys(xiaoming); // ['name', 'age', 'school']
```

### values

和`keys()`类似，`values()`返回object自身但不包含原型链继承的所有值：

```javascript
'use strict';

var obj = {
    name: '小明',
    age: 20
};
_.values(obj); // ['小明', 20]
```

但underscore并没有提供`allValues()`方法。

### mapObject

`mapObject()`就是针对object的map版本：

```javascript
'use strict';

var obj = { a: 1, b: 2, c: 3 };
// 注意传入的函数签名，value在前，key在后:
_.mapObject(obj, (v, k) => 100 + v); // { a: 101, b: 102, c: 103 }
```

### invert

`invert()`把object的每个key-value来个交换，key变成value，value变成key：

```javascript
'use strict';

var obj = {
    Adam: 90,
    Lisa: 85,
    Bart: 59
};
_.invert(obj); // {59: "Bart", 85: "Lisa", 90: "Adam"}
```

### extend/extendOwn

`extend()`把多个object的key-value合并到第一个object并返回：

```javascript
'use strict';

var a = {name: 'Bob', age: 20};
_.extend(a, {age: 15}, {age: 88, city: 'Beijing'}); // {name: 'Bob', age: 88, city: 'Beijing'}
// 变量a的内容也改变了：
a; // {name: 'Bob', age: 88, city: 'Beijing'}
```

注意：如果有相同的key，后面的object的value将覆盖前面的object的value。

`extendOwn()`和`extend()`类似，但获取属性时忽略从原型链继承下来的属性。

### clone

如果要复制一个object对象，就可以用`clone()`方法，它会把原有对象的所有属性都复制到新的对象中：

```javascript
'use strict';
var source = {
    name: '小明',
    age: 20,
    skills: ['JavaScript', 'CSS', 'HTML']
};
var copied = _.clone(source);
/*
{
  "name": "小明",
  "age": 20,
  "skills": [
    "JavaScript",
    "CSS",
    "HTML"
  ]
}
*/
```

注意，`clone()`是“浅复制”。所谓“浅复制”就是说，两个对象相同的key所引用的value其实是同一对象：

```javascript
source.skills === copied.skills; // true
```

也就是说，修改`source.skills`会影响`copied.skills`。

### isEqual

`isEqual()`对两个object进行深度比较，如果内容完全相同，则返回`true`：

```javascript
'use strict';

var o1 = { name: 'Bob', skills: { Java: 90, JavaScript: 99 }};
var o2 = { name: 'Bob', skills: { JavaScript: 99, Java: 90 }};

o1 === o2; // false
_.isEqual(o1, o2); // true
```

`isEqual()`其实对`Array`也可以比较：

```javascript
'use strict';

var o1 = ['Bob', { skills: ['Java', 'JavaScript'] }];
var o2 = ['Bob', { skills: ['Java', 'JavaScript'] }];

o1 === o2; // false
_.isEqual(o1, o2); // true
```

## Chaining

underscore提供了`chain()`函数把对象包装成能进行链式调用：

```javascript
var r = _.chain([1, 4, 9, 16, 25])
         .map(Math.sqrt)
         .filter(x => x % 2 === 1)
         .value();
console.log(r); // [1, 3, 5]
```

 因为每一步返回的都是包装对象，所以最后一步的结果需要调用`value()`获得最终结果。 

