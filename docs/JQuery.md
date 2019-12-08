# JQuery

jQuery能做的事情：

- 消除浏览器差异：不需要自己写冗长的代码来针对不同的浏览器来绑定事件，编写AJAX等代码；
- 简洁的操作DOM的方法：写`$('#test')`肯定比`document.getElementById('test')`来得简洁；
- 轻松实现动画、修改CSS等各种操作。

jQuery的理念是“Write Less, Do More“

## jQuery版本

目前jQuery有1.x和2.x两个主要版本，区别在于2.x移除了对古老的IE 6、7、8的支持，因此2.x的代码更精简。选择哪个版本主要取决于你是否想支持IE 6~8。

从[jQuery官网](http://jquery.com/download/)可以下载最新版本。

## 使用jQuery

使用jQuery只需要在页面的``引入jQuery文件即可：

```javascript
<script src="//code.jquery.com/jquery-1.11.3.min.js"></script>
```

## $符号

`$`是著名的jQuery符号。实际上，jQuery把所有功能全部封装在一个全局变量`jQuery`中，而`$`也是一个合法的变量名，它是变量`jQuery`的别名：

```javascript
window.jQuery; // jQuery(selector, context)
window.$; // jQuery(selector, context)
$ === jQuery; // true
typeof($); // 'function'
```

`$`本质上就是一个函数，但是函数也是对象，于是`$`除了可以直接调用外，也可以有很多其他属性。

由于很多JavaScript压缩工具可以对函数名和参数改名，所以压缩过的jQuery源码`$`函数可能变成`a(b, c)`而不是`jQuery(selector, context)`。

绝大多数时候，我们都直接用`$`（因为写起来更简单嘛）。但是，如果`$`这个变量不幸地被占用了，而且还不能改，那我们就只能让`jQuery`把`$`变量交出来，然后就只能使用`jQuery`这个变量：

```javascript
$; // jQuery(selector, context)
jQuery.noConflict();
$; // undefined
jQuery; // jQuery(selector, context)
```

这种黑魔法的原理是jQuery在占用`$`之前，先在内部保存了原来的`$`,调用`jQuery.noConflict()`时会把原来保存的变量还原。

# 选择器

## 基本选择器

### 按ID查找

如果某个DOM节点有`id`属性，利用jQuery查找如下：

```javascript
// 查找<div id="abc">:
var div = $('#abc');
```

*注意*，`#abc`以`#`开头。返回的对象是jQuery对象。

什么是jQuery对象？jQuery对象类似数组，它的每个元素都是一个引用了DOM节点的对象。

以上面的查找为例，如果`id`为`abc`的`<div>`存在，返回的jQuery对象如下：

```html
[<div id="abc">...</div>]
```

如果`id`为`abc`的`  <div>  `不存在，返回的jQuery对象如下：

```
[]
```

总之jQuery的选择器不会返回`undefined`或者`null`，这样的好处是你不必在下一行判断`if (div === undefined)`。

jQuery对象和DOM对象之间可以互相转化：

```javascript
var div = $('#abc'); // jQuery对象
var divDom = div.get(0); // 假设存在div，获取第1个DOM元素
var another = $(divDom); // 重新把DOM包装为jQuery对象
```

通常情况下你不需要获取DOM对象，直接使用jQuery对象更加方便。如果你拿到了一个DOM对象，那可以简单地调用`$(aDomObject)`把它变成jQuery对象，这样就可以方便地使用jQuery的API了。

### 按tag查找

按tag查找只需要写上tag名称就可以了：

```javascript
var ps = $('p'); // 返回所有<p>节点
ps.length; // 数一数页面有多少个<p>节点
```

### 按class查找

按class查找注意在class名称前加一个`.`：

```javascript
var a = $('.red'); // 所有节点包含`class="red"`都将返回
// 例如:
// <div class="red">...</div>
// <p class="green red">...</p>
```

通常很多节点有多个class，可以查找同时包含`red`和`green`的节点：

```javascript
var a = $('.red.green'); // 注意没有空格！
// 符合条件的节点：
// <div class="red green">...</div>
// <div class="blue green red">...</div>
```

### 按属性查找

一个DOM节点除了`id`和`class`外还可以有很多属性，很多时候按属性查找会非常方便，比如在一个表单中按属性来查找：

```javascript
var email = $('[name=email]'); // 找出<??? name="email">
var passwordInput = $('[type=password]'); // 找出<??? type="password">
var a = $('[items="A B"]'); // 找出<??? items="A B">
```

当属性的值包含空格等特殊字符时，需要用双引号括起来。

按属性查找还可以使用前缀查找或者后缀查找：

```javascript
var icons = $('[name^=icon]'); // 找出所有name属性值以icon开头的DOM
// 例如: name="icon-1", name="icon-2"
var names = $('[name$=with]'); // 找出所有name属性值以with结尾的DOM
// 例如: name="startswith", name="endswith"
```

这个方法尤其适合通过class属性查找，且不受class包含多个名称的影响：

```javascript
var icons = $('[class^="icon-"]'); // 找出所有class包含至少一个以`icon-`开头的DOM
// 例如: class="icon-clock", class="abc icon-home"
```

### 组合查找

组合查找就是把上述简单选择器组合起来使用。如果我们查找`$('[name=email]')`，很可能把表单外的``也找出来，但我们只希望查找``，就可以这么写：

```javascript
var emailInput = $('input[name=email]'); // 不会找出<div name="email">
```

同样的，根据tag和class来组合查找也很常见：

```javascript
var tr = $('tr.red'); // 找出<tr class="red ...">...</tr>
```

### 多项选择器

多项选择器就是把多个选择器用`,`组合起来一块选：

```javascript
$('p,div'); // 把<p>和<div>都选出来
$('p.red,p.green'); // 把<p class="red">和<p class="green">都选出来
```

要注意的是，选出来的元素是按照它们在HTML中出现的顺序排列的，而且不会有重复元素。例如，``不会被上面的`$('p.red,p.green')`选择两次。

## 层级选择器（Descendant Selector）

如果两个DOM元素具有层级关系，就可以用`$('ancestor descendant')`来选择，层级之间用空格隔开。例如：

```html
<!-- HTML结构 -->
<div class="testing">
    <ul class="lang">
        <li class="lang-javascript">JavaScript</li>
        <li class="lang-python">Python</li>
        <li class="lang-lua">Lua</li>
    </ul>
</div>
```

要选出JavaScript，可以用层级选择器：

```JavaScript
$('ul.lang li.lang-javascript'); // [<li class="lang-javascript">JavaScript</li>]
$('div.testing li.lang-javascript'); // [<li class="lang-javascript">JavaScript</li>]
```

因为`div`和`ul`都是`li`的祖先节点，所以上面两种方式都可以选出相应的`li`节点。

要选择所有的`li`节点，用：

```javascript
$('ul.lang li');
```

这种层级选择器相比单个的选择器好处在于，它缩小了选择范围，因为首先要定位父节点，才能选择相应的子节点，这样避免了页面其他不相关的元素。

例如：

```javascript
$('form[name=upload] input');
```

就把选择范围限定在`name`属性为`upload`的表单里。如果页面有很多表单，其他表单的`<input>`不会被选择。

多层选择也是允许的：

```javascript
$('form.test p input'); // 在form表单选择被<p>包含的<input>
```

### 子选择器（Child Selector）

子选择器`$('parent>child')`类似层级选择器，但是限定了层级关系必须是父子关系，就是``节点必须是``节点的直属子节点。还是以上面的例子：

```javascript
$('ul.lang>li.lang-javascript'); // 可以选出[<li class="lang-javascript">JavaScript</li>]
$('div.testing>li.lang-javascript'); // [], 无法选出，因为<div>和<li>不构成父子关系
```

### 过滤器（Filter）

过滤器一般不单独使用，它通常附加在选择器上，帮助我们更精确地定位元素。观察过滤器的效果：

```javascript
$('ul.lang li'); // 选出JavaScript、Python和Lua 3个节点

$('ul.lang li:first-child'); // 仅选出JavaScript
$('ul.lang li:last-child'); // 仅选出Lua
$('ul.lang li:nth-child(2)'); // 选出第N个元素，N从1开始
$('ul.lang li:nth-child(even)'); // 选出序号为偶数的元素
$('ul.lang li:nth-child(odd)'); // 选出序号为奇数的元素
```

### 表单相关

针对表单元素，jQuery还有一组特殊的选择器：

- `:input`：可以选择 ` <input> `，` <textarea> `，` <select> `和` <button> `； 
- `:file`：可以选择` <input type="file"> `，和`input[type=file]`一样；
- `:checkbox`：可以选择复选框，和`input[type=checkbox]`一样；
- `:radio`：可以选择单选框，和`input[type=radio]`一样；
- `:focus`：可以选择当前输入焦点的元素，例如把光标放到一个` <input> `上，用`$('input:focus')`就可以选出；
- `:checked`：选择当前勾上的单选框和复选框，用这个选择器可以立刻获得用户选择的项目，如`$('input[type=radio]:checked')`；
- `:enabled`：可以选择可以正常输入的` <input> `、` <select> ` 等，也就是没有灰掉的输入；
- `:disabled`：和`:enabled`正好相反，选择那些不能输入的。

此外，jQuery还有很多有用的选择器，例如，选出可见的或隐藏的元素：

```javascript
$('div:visible'); // 所有可见的div
$('div:hidden'); // 所有隐藏的div
```

## 查找和过滤

### 查找

如下HTML结构：

```html
<!-- HTML结构 -->
<ul class="lang">
    <li class="js dy">JavaScript</li>
    <li class="dy">Python</li>
    <li id="swift">Swift</li>
    <li class="dy">Scheme</li>
    <li name="haskell">Haskell</li>
</ul>
```

用`find()`查找：

```javascript
var ul = $('ul.lang'); // 获得<ul>
var dy = ul.find('.dy'); // 获得JavaScript, Python, Scheme
var swf = ul.find('#swift'); // 获得Swift
var hsk = ul.find('[name=haskell]'); // 获得Haskell
```

如果要从当前节点开始向上查找，使用`parent()`方法：

```javascript
var swf = $('#swift'); // 获得Swift
var parent = swf.parent(); // 获得Swift的上层节点<ul>
var a = swf.parent('.red'); // 获得Swift的上层节点<ul>，同时传入过滤条件。如果ul不符合条件，返回空jQuery对象
```

对于位于同一层级的节点，可以通过`next()`和`prev()`方法，例如：

当我们已经拿到`Swift`节点后：

```javascript
var swift = $('#swift');

swift.next(); // Scheme
swift.next('[name=haskell]'); // 空的jQuery对象，因为Swift的下一个元素Scheme不符合条件[name=haskell]

swift.prev(); // Python
swift.prev('.dy'); // Python，因为Python同时符合过滤器条件.dy
```

### 过滤

和函数式编程的map、filter类似，jQuery对象也有类似的方法。

`filter()`方法可以过滤出符合选择器条件的节点：

```javascript
var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell
var a = langs.filter('.dy'); // 拿到JavaScript, Python, Scheme
```

或者传入一个函数，要特别注意函数内部的`this`被绑定为DOM对象，不是jQuery对象：

```javascript
var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell
langs.filter(function () {
    return this.innerHTML.indexOf('S') === 0; // 返回S开头的节点
}); // 拿到Swift, Scheme
```

`map()`方法把一个jQuery对象包含的若干DOM节点转化为其他对象：

```javascript
var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell
var arr = langs.map(function () {
    return this.innerHTML;
}).get(); // 用get()拿到包含string的Array：['JavaScript', 'Python', 'Swift', 'Scheme', 'Haskell']
```

此外，一个jQuery对象如果包含了不止一个DOM节点，`first()`、`last()`和`slice()`方法可以返回一个新的jQuery对象，把不需要的DOM节点去掉：

```javascript
var langs = $('ul.lang li'); // 拿到JavaScript, Python, Swift, Scheme和Haskell
var js = langs.first(); // JavaScript，相当于$('ul.lang li:first-child')
var haskell = langs.last(); // Haskell, 相当于$('ul.lang li:last-child')
var sub = langs.slice(2, 4); // Swift, Scheme, 参数和数组的slice()方法一致
```

### 练习

对于下面的表单：

```html
<form id="test-form" action="#0" onsubmit="return false;">
    <p><label>Name: <input name="name"></label></p>
    <p><label>Email: <input name="email"></label></p>
    <p><label>Password: <input name="password" type="password"></label></p>
    <p>Gender: <label><input name="gender" type="radio" value="m" checked> Male</label> <label><input name="gender" type="radio" value="f"> Female</label></p>
    <p><label>City: <select name="city">
    	<option value="BJ" selected>Beijing</option>
    	<option value="SH">Shanghai</option>
    	<option value="CD">Chengdu</option>
    	<option value="XM">Xiamen</option>
    </select></label></p>
    <p><button type="submit">Submit</button></p>
</form>
```

输入值后，用jQuery获取表单的JSON字符串，key和value分别对应每个输入的name和相应的value，例如：`{"name":"Michael","email":...}`

原始办法：

```javascript
'use strict';
json = {
  Name: $('#test-form input[name=name]')[0].value,
  Email: $('#test-form input[name=email]')[0].value,
  Password: $('#test-form input[name=password]')[0].value,
  Gender: $('#test-form input[name=gender]:checked')[0].value,
  City: $('#test-form select[name=city]')[0].value
};
json = JSON.stringify(json,null,'  ');
```

函数处理：

```javascript
'use strict';
var json = {};
$("#test-form input,select").filter(function() {
    return this.name!="gender" || this.checked;
}).map(function () {
    json[this.name] = this.value
})
json = JSON.stringify(json,null,'  ');
```

# 操作DOM

## 修改Text和HTML

jQuery对象的`text()`和`html()`方法分别获取节点的文本和原始HTML文本，例如，如下的HTML结构：

```html
<!-- HTML结构 -->
<ul id="test-ul">
    <li class="js">JavaScript</li>
    <li name="book">Java &amp; JavaScript</li>
</ul>
```

分别获取文本和HTML：

```javascript
$('#test-ul li[name=book]').text(); // 'Java & JavaScript'
$('#test-ul li[name=book]').html(); // 'Java &amp; JavaScript'
```

设置文本：

```javascript
'use strict';
var j1 = $('#test-ul li.js');
var j2 = $('#test-ul li[name=book]');
j1.html('<span style="color: red">JavaScript</span>');
j2.text('JavaScript & ECMAScript');
```

一个jQuery对象可以包含0个或任意个DOM对象，它的方法实际上会作用在对应的每个DOM节点上:

```javascript
$('#test-ul li').text('JS'); // 两个节点都变成了JS
```

jQuery对象可以执行一个操作作用在对应的一组DOM节点上，即使选择器没有返回任何DOM节点，调用jQuery对象的方法仍然不会报错：

```javascript
// 如果不存在id为not-exist的节点：
$('#not-exist').text('Hello'); // 代码不报错，没有节点被设置为'Hello'
```

这样可以免去许多`if`语句。

## 修改CSS

下面的HTML结构：

```html
<!-- HTML结构 -->
<ul id="test-css">
    <li class="lang dy"><span>JavaScript</span></li>
    <li class="lang"><span>Java</span></li>
    <li class="lang dy"><span>Python</span></li>
    <li class="lang"><span>Swift</span></li>
    <li class="lang dy"><span>Scheme</span></li>
</ul>
```

要高亮显示动态语言，调用jQuery对象的`css('name', 'value')`方法：

```javascript
$('#test-css li.dy>span').css('background-color', '#ffd351').css('color', 'red');
```

> jQuery对象的所有方法都返回一个jQuery对象，这样就可以进行链式调用。

jQuery对象的`css()`方法也可以这么用：

```javascript
var div = $('#test-div');
div.css('color'); // '#000033', 获取CSS属性
div.css('color', '#336699'); // 设置CSS属性
div.css('color', ''); // 清除CSS属性
```

jQuery对象的CSS属性可以用`'background-color'`和`'backgroundColor'`两种格式。

`css()`方法将作用于DOM节点的`style`属性，具有最高优先级。如果要修改`class`属性，可以用jQuery提供的下列方法：

```javascript
var div = $('#test-div');
div.hasClass('highlight'); // false， class是否包含highlight
div.addClass('highlight'); // 添加highlight这个class
div.removeClass('highlight'); // 删除highlight这个class
```

## 获取DOM信息

利用jQuery对象的若干方法，可以直接获取DOM的高宽等信息：

```javascript
// 浏览器可视窗口大小:
$(window).width(); // 800
$(window).height(); // 600

// HTML文档大小:
$(document).width(); // 800
$(document).height(); // 3500

// 某个div的大小:
var div = $('#test-div');
div.width(); // 600
div.height(); // 300
div.width(400); // 设置CSS属性 width: 400px，是否生效要看CSS是否有效
div.height('200px'); // 设置CSS属性 height: 200px，是否生效要看CSS是否有效
```

`attr()`和`removeAttr()`方法用于操作DOM节点的属性：

```javascript
// <div id="test-div" name="Test" start="1">...</div>
var div = $('#test-div');
div.attr('data'); // undefined, 属性不存在
div.attr('name'); // 'Test'
div.attr('name', 'Hello'); // div的name属性变为'Hello'
div.removeAttr('name'); // 删除name属性
div.attr('name'); // undefined
```

`prop()`方法和`attr()`类似，但是

HTML5中规定有一种属性在DOM节点中可以没有值，只有出现与不出现两种，例如：

```html
<input id="test-radio" type="radio" name="test" checked value="1" />
等价于：
<input id="test-radio" type="radio" name="test" checked="checked" value="1" />
```

`prop()`可以读取这种属于的boolean值，不过用`is()`方法判断更好：

```javascript
var radio = $('#test-radio');
radio.attr('checked'); // 'checked'
radio.prop('checked'); // true
radio.is(':checked'); // true
```

类似的属性还有`selected`，处理时最好用`is(':selected')`。

## 操作表单

对于表单元素，jQuery对象统一提供`val()`方法获取和设置对应的`value`属性：

```javascript
/*
    <input id="test-input" name="email" value="">
    <select id="test-select" name="city">
        <option value="BJ" selected>Beijing</option>
        <option value="SH">Shanghai</option>
        <option value="SZ">Shenzhen</option>
    </select>
    <textarea id="test-textarea">Hello</textarea>
*/
var
    input = $('#test-input'),
    select = $('#test-select'),
    textarea = $('#test-textarea');

input.val(); // 'test'
input.val('abc@example.com'); // 文本框的内容已变为abc@example.com

select.val(); // 'BJ'
select.val('SH'); // 选择框已变为Shanghai

textarea.val(); // 'Hello'
textarea.val('Hi'); // 文本区域已更新为'Hi'
```

可见，一个`val()`就统一了各种输入框的取值和赋值的问题。

## 修改DOM结构

要添加新的DOM节点，除了通过jQuery的`html()`这种暴力方法外，还可以用`append()`方法，例如：

```html
<div id="test-div">
    <ul>
        <li><span>JavaScript</span></li>
        <li><span>Python</span></li>
        <li><span>Swift</span></li>
    </ul>
</div>
```

`append()`可以接受字符串，传入原始的DOM对象，jQuery对象和函数对象：

```javascript
var ul = $('#test-div>ul');
ul.append('<li><span>Haskell</span></li>');
// 创建DOM对象:
var ps = document.createElement('li');
ps.innerHTML = '<span>Pascal</span>';
// 添加DOM对象:
ul.append(ps);
// 添加jQuery对象:
ul.append($('#scheme'));
// 添加函数对象:
ul.append(function (index, html) {
    return '<li><span>Language - ' + index + '</span></li>';
});
```

`append()`把DOM添加到最后，`prepend()`则把DOM添加到最前。

如果要添加的DOM节点已经存在于HTML文档中，它会首先从文档移除，然后再添加。

如果要把新节点插入到指定位置，可以先定位到插入点，然后用`after()`或`before()`方法：

```javascript
var js = $('#test-div>ul>li:first-child');
js.after('<li><span>Lua</span></li>');
js.before('<li><span>Mysql</span></li>');
```

要删除节点，调用节点`remove()`方法即可：

```javascript
var li = $('#test-div>ul>li');
li.remove(); // 所有<li>全被删除
```

添加Pascal、Lua和Ruby后按字母顺序排序节点：

```html
<!-- HTML结构 -->
<div id="test-div">
    <ul>
        <li><span>JavaScript</span></li>
        <li><span>Python</span></li>
        <li><span>Swift</span></li>
    </ul>
</div>
```

脚本：

```javascript
'use strict';
var str = ['Pascal','Lua','Ruby'];
var ul = $('#test-div>ul');
str.map((s)=>{
    ul.append(`<li><span>${s}</span></li>`);
});
var list = $('#test-div>ul>li').sort((l1,l2)=>{
    return $(l1).text()> $(l2).text() ? 1 : -1; 
});
ul.append(list);
```

# 事件

假设要在用户点击了超链接时弹出提示框，可以用on绑定一个`click`事件：

```javascript
/* HTML:
 * <a id="test-link" href="#0">点我试试</a>
 */

// 获取超链接的jQuery对象:
var a = $('#test-link');
a.on('click', function () {
    alert('Hello!');
});
```

更简化的写法是直接调用`click()`方法：

```javascript
a.click(function () {
    alert('Hello!');
});
```

## jQuery绑定事件

jQuery能够绑定的事件主要包括：

### 鼠标事件

- click: 鼠标单击时触发； 
- dblclick：鼠标双击时触发； 
- mouseenter：鼠标进入时触发； 
- mouseleave：鼠标移出时触发； 
- mousemove：鼠标在DOM内部移动时触发； 
- hover：鼠标进入和退出时触发两个函数，相当于mouseenter加上mouseleave。

### 键盘事件

键盘事件仅作用在当前焦点的DOM上，通常是` <input> `和` <textarea> `。

- keydown：键盘按下时触发；
- keyup：键盘松开时触发；
- keypress：按一次键后触发。

### 其他事件

- focus：当DOM获得焦点时触发； 
- blur：当DOM失去焦点时触发； 
- change：当` <input> `、` <select> `或` <textarea> `的内容改变时触发； 
- submit：当`<form>`提交时触发；
- ready：当页面被载入并且DOM树完成初始化后触发。

## ready初始化加载

其中，`ready`仅作用于`document`对象。由于`ready`事件在DOM完成初始化后触发，且只触发一次，所以非常适合用来写其他的初始化代码。

初始化绑定submit函数：

```html
<html>
<head>
    <script>
        $(document).on('ready', function () {
            $('#testForm).on('submit', function () {
                alert('submit!');
            });
        });
    </script>
</head>
<body>
    <form id="testForm">
        ...
    </form>
</body>
```

由于`ready`事件使用非常普遍，所以可以这样简化：

```javascript
$(document).ready(function () {
    $('#testForm).submit(function () {
        alert('submit!');
    });
});
```

还可以再简化为：

```javascript
$(function () {
    // init...
});
```

`$(function() {...})`就是`document`对象的`ready`事件处理函数。

可以反复绑定事件处理函数，它们会依次执行：

```javascript
$(function () {
    console.log('init A...');
});
$(function () {
    console.log('init B...');
});
$(function () {
    console.log('init C...');
});
```

## 事件参数

有些事件，如`mousemove`和`keypress`需要获取鼠标位置和按键的值，可以从`Event`对象上获取到更多的信息：

```javascript
$(function () {
    $('#testMouseMoveDiv').mousemove(function (e) {
        $('#testMouseMoveSpan').text('pageX = ' + e.pageX + ', pageY = ' + e.pageY);
    });
});
```

## 取消绑定

一个已被绑定的事件可以解除绑定，通过`off('click', function)`实现：

```javascript
function hello() {
    alert('hello!');
}
a.click(hello); // 绑定事件
// 10秒钟后解除绑定:
setTimeout(function () {
    a.off('click', hello);
}, 10000);
```

由于两个匿名函数是两个不同的函数对象，所以下面这种写法是无效的：

```javascript
// 绑定事件:
a.click(function () {
    alert('hello!');
});
// 解除绑定:
a.off('click', function () {
    alert('hello!');
});
```

为了实现移除效果，使用`off('click')`可以一次性移除已绑定的`click`事件的所有处理函数。

同理，无参数调用`off()`一次性移除已绑定的所有类型的事件处理函数。

```javascript
// 一次性移除已绑定的click事件的所有处理函数
a.off('click');
// 一次性移除已绑定的所有类型的事件处理函数
a.off();
```

## 事件触发条件

监控文本框的内容改动：

```javascript
var input = $('#test-input');
input.change(function () {
    console.log('changed...');
});
```

当用户在文本框中输入时，就会触发`change`事件。但是，如果用JavaScript代码去改动文本框的值，将**不会**触发`change`事件：

```javascript
var input = $('#test-input');
input.val('change it!'); // 无法触发change事件
```

希望用代码触发`change`事件，可以直接调用无参数的`change()`方法：

```javascript
var input = $('#test-input');
input.val('change it!');
input.change(); // 触发change事件
```

 `input.change()`相当于`input.trigger('change')`，它是`trigger()`方法的简写。 

## 浏览器安全限制

在浏览器中，有些JavaScript代码只有在用户触发下才能执行，例如，`window.open()`函数：

```javascript
// 无法弹出新窗口，将被浏览器屏蔽:
$(function () {
    window.open('/');
});
```

这些“敏感代码”只能由用户操作来触发：

```javascript
var button1 = $('#testPopupButton1');
var button2 = $('#testPopupButton2');

function popupTestWindow() {
    window.open('/');
}

button1.click(function () {
    popupTestWindow();
});

button2.click(function () {
    // 不立刻执行popupTestWindow()，100毫秒后执行:
    setTimeout(popupTestWindow, 100);
});
```

当用户点击`button1`时，`click`事件被触发，由于`popupTestWindow()`在`click`事件处理函数内执行，这是浏览器允许的，而`button2`的`click`事件并未立刻执行`popupTestWindow()`，延迟执行的`popupTestWindow()`将被浏览器拦截。

## 练习

对如下的Form表单：

```javascript
<form id="test-form" action="test">
    <legend>请选择想要学习的编程语言：</legend>
    <fieldset>
        <p>
			<label class="selectAll">
				<input type="checkbox">
				<span class="selectAll">全选</span>
				<span class="deselectAll">全不选</span>
			</label>
			<a href="javascript:void(0)" class="invertSelect">反选</a>
		</p>
        <p><label>
				<input type="checkbox" name="lang" value="javascript">JavaScript
		</label></p>
        <p><label>
			<input type="checkbox" name="lang" value="python"> Python</label></p>
        <p><label>
			<input type="checkbox" name="lang" value="ruby"> Ruby</label></p>
        <p><label>
			<input type="checkbox" name="lang" value="haskell"> Haskell
		</label></p>
        <p><label>
			<input type="checkbox" name="lang" value="scheme"> Scheme
		</label></p>
		<p><button type="submit">Submit</button></p>
    </fieldset>
</form>
```

绑定合适的事件处理函数，实现以下逻辑：

当用户勾上“全选”时，自动选中所有语言，并把“全选”变成“全不选”；

当用户去掉“全不选”时，自动不选中所有语言；

当用户点击“反选”时，自动把所有语言状态反转（选中的变为未选，未选的变为选中）；

当用户把所有语言都手动勾上时，“全选”被自动勾上，并变为“全不选”；

当用户手动去掉选中至少一种语言时，“全不选”自动被去掉选中，并变为“全选”。

```javascript
'use strict';

var
    form = $('#test-form'),
    langs = form.find('[name=lang]'),
    selectAll = form.find('label.selectAll :checkbox'),
    selectAllLabel = form.find('label.selectAll span.selectAll'),
    deselectAllLabel = form.find('label.selectAll span.deselectAll'),
    invertSelect = form.find('a.invertSelect');

deselectAllLabel.hide();

selectAll.change(function () {
    if (selectAll.is(":checked")) {
        selectAllLabel.hide();
        deselectAllLabel.show();
        langs.prop("checked", true);
    } else {
        deselectAllLabel.hide();
        selectAllLabel.show();
        langs.prop("checked", false);
    }
})
invertSelect.click(function () {
    langs.prop("checked", function () {
        return !this.checked
    })
})
langs.change(function () {
    var allState = langs.map(function () {
        return this.checked;
    }).get();
    console.log(allState);
    if (allState.indexOf(true) === -1) {
        selectAll.prop("checked", false);
        selectAll.change();
    } else if (allState.indexOf(false) === -1 ) {
        selectAll.prop("checked", true);
        selectAll.change();
    } else {
        selectAll.prop("checked", false);
        deselectAllLabel.hide();
        selectAllLabel.show();
    }
})
```

# 动画

用JavaScript实现动画，原理非常简单：只需要以固定的时间间隔（例如，0.1秒），每次把DOM元素的CSS样式修改一点（例如，高宽各增加10%），看起来就像动画了。

使用jQuery实现动画大致就是这样，代码也很简单：

## show/hide

直接以无参数形式调用`show()`和`hide()`，会显示和隐藏DOM元素。但是，只要传递一个时间参数进去，就变成了动画：

```javascript
var div = $('#test-show-hide');
div.hide(3000); // 在3秒钟内逐渐消失
```

时间以毫秒为单位，但也可以是`'slow'`，`'fast'`这些字符串：

```javascript
var div = $('#test-show-hide');
div.show('slow'); // 在0.6秒钟内逐渐显示
```

`toggle()`方法则根据当前状态决定是`show()`还是`hide()`。

## slideUp/slideDown

你可能已经看出来了，`show()`和`hide()`是从左上角逐渐展开或收缩的，而`slideUp()`和`slideDown()`则是在垂直方向逐渐展开或收缩的。

`slideUp()`把一个可见的DOM元素收起来，效果跟拉上窗帘似的，`slideDown()`相反，而`slideToggle()`则根据元素是否可见来决定下一步动作：

```javascript
var div = $('#test-slide');
div.slideUp(3000); // 在3秒钟内逐渐向上消失
```

## fadeIn / fadeOut

`fadeIn()`和`fadeOut()`的动画效果是淡入淡出，也就是通过不断设置DOM元素的`opacity`属性来实现，而`fadeToggle()`则根据元素是否可见来决定下一步动作：

```
var div = $('#test-fade');
div.fadeOut('slow'); // 在0.6秒内淡出
```

## animate

`animate()`可以实现任意动画效果，需要传入的参数就是DOM元素最终的CSS状态和时间，jQuery在时间段内不断调整CSS直到达到设定的值：

```javascript
var div = $('#test-animate');
div.animate({
    opacity: 0.25,
    width: '256px',
    height: '256px'
}, 3000); // 在3秒钟内CSS过渡到设定值
```

`animate()`还可以再传入一个函数，当动画结束时，该函数将被调用：

```javascript
var div = $('#test-animate');
div.animate({
    opacity: 0.25,
    width: '256px',
    height: '256px'
}, 3000, function () {
    console.log('动画已结束');
    // 恢复至初始状态:
    $(this).css('opacity', '1.0').css('width', '128px').css('height', '128px');
});
```

## 串行动画

通过`delay()`方法可以串行执行动画效果，还可以实现更复杂的动画效果：

```javascript
var div = $('#test-animates');
// 动画效果：slideDown - 暂停 - 放大 - 暂停 - 缩小
div.slideDown(2000)
   .delay(1000)
   .animate({
       width: '256px',
       height: '256px'
   }, 2000)
   .delay(1000)
   .animate({
       width: '128px',
       height: '128px'
   }, 2000);
}
```

因为动画需要执行一段时间，所以jQuery必须不断返回新的Promise对象才能后续执行操作。简单地把动画封装在函数中是不够的。

## 练习

对于以下表格， 在表格删除一行的时候添加一个淡出的动画效果： 

```html
<table id="test-table">
    <thead>
        <tr>
            <th>Name</th>
            <th>Email</th>
            <th>Address</th>
            <th>Status</th>
        </tr>
    </thead>
	<tbody></tbody>
</table>
<button id="test-add-button">Add</button>
<button onclick="deleteFirstTR()">Delete</button>

<script>
'use strict';

$(function () {
    var trs = [['Bart Simpson', 'bart.s@primary.school', 'Springfield', 'Active'],
               ['Michael Scofield', 'm.scofield@escape.org', 'Fox River', 'Locked'],
               ['Optimus Prime', 'prime@cybertron.org', 'Cybertron', 'Active'],
               ['Peter Parker', 'spider@movie.org', 'New York', 'Active'],
               ['Thor Odinson', 'thor@asgard.org', 'Asgard', 'Active']];
    var tbody = $('#test-table>tbody');
    var i;
    for (i=0; i < trs.length; i++) {
        tbody.append('<tr><td>' + trs[i].join('</td><td>') + '</td></tr>');
    }
    i = 0;
    $('#test-add-button').click(function () {
        if (i>=trs.length) {
            i = 0;
        }
        tbody.append('<tr><td>' + trs[i].join('</td><td>') + '</td></tr>');
        i++;
    });
});
function deleteFirstTR() {
    var tr = $('#test-table>tbody>tr:visible').first();
    tr.fadeOut(600, ()=>tr.remove());
}
</script>
```

<script src="http://code.jquery.com/jquery-1.11.3.min.js"></script>
<table id="test-table">
    <thead>
        <tr>
            <th>Name</th>
            <th>Email</th>
            <th>Address</th>
            <th>Status</th>
        </tr>
    </thead>
	<tbody></tbody>
</table>
<button id="test-add-button">Add</button>
<button onclick="deleteFirstTR()">Delete</button>

<script>
'use strict';
$(function () {
    var trs = [['Bart Simpson', 'bart.s@primary.school', 'Springfield', 'Active'],
               ['Michael Scofield', 'm.scofield@escape.org', 'Fox River', 'Locked'],
               ['Optimus Prime', 'prime@cybertron.org', 'Cybertron', 'Active'],
               ['Peter Parker', 'spider@movie.org', 'New York', 'Active'],
               ['Thor Odinson', 'thor@asgard.org', 'Asgard', 'Active']];
    var tbody = $('#test-table>tbody');
    var i;
    for (i=0; i < trs.length; i++) {
        tbody.append('<tr><td>' + trs[i].join('</td><td>') + '</td></tr>');
    }
    i = 0;
    $('#test-add-button').click(function () {
        if (i>=trs.length) {
            i = 0;
        }
        tbody.append('<tr><td>' + trs[i].join('</td><td>') + '</td></tr>');
        i++;
    });
});
function deleteFirstTR() {
    var tr = $('#test-table>tbody>tr:visible').first();
    tr.fadeOut(600, ()=>tr.remove());
}
</script>

# AJAX

用jQuery的相关对象来处理AJAX，不但不需要考虑浏览器问题，代码也能大大简化。

## ajax

jQuery在全局对象`jQuery`（也就是`$`）绑定了`ajax()`函数，可以处理AJAX请求。`ajax(url, settings)`函数需要接收一个URL和一个可选的`settings`对象，常用的选项如下：

- async：是否异步执行AJAX请求，默认为`true`，千万不要指定为`false`；
- method：发送的Method，缺省为`'GET'`，可指定为`'POST'`、`'PUT'`等；
- contentType：发送POST请求的格式，默认值为`'application/x-www-form-urlencoded; charset=UTF-8'`，也可以指定为`text/plain`、`application/json`；
- data：发送的数据，可以是字符串、数组或object。如果是GET请求，data将被转换成query附加到URL上，如果是POST请求，根据contentType把data序列化成合适的格式；
- headers：发送的额外的HTTP头，必须是一个object；
- dataType：接收的数据格式，可以指定为`'html'`、`'xml'`、`'json'`、`'text'`等，缺省情况下根据响应的`Content-Type`猜测。

下面的例子发送一个GET请求，并返回一个JSON格式的数据：

```javascript
var jqxhr = $.ajax('/api/categories', {
    dataType: 'json'
});
// 请求已经发送了
```

不过，如何用回调函数处理返回的数据和出错时的响应呢？

jQuery的jqXHR对象类似一个Promise对象，可以用链式写法来处理各种回调，从而处理返回的数据和出错时的响应：

```javascript
'use strict';

function ajaxLog(s) {
    var txt = $('#test-response-text');
    txt.val(txt.val() + '\n' + s);
}

$('#test-response-text').val('');
var jqxhr = $.ajax('/api/categories', {
    dataType: 'json'
}).done(function (data) {
    ajaxLog('成功, 收到的数据: ' + JSON.stringify(data));
}).fail(function (xhr, status) {
    ajaxLog('失败: ' + xhr.status + ', 原因: ' + status);
}).always(function () {
    ajaxLog('请求完成: 无论成功或失败都会调用');
});
```

## get

对常用的AJAX操作，jQuery提供了一些辅助方法。由于GET请求最常见，所以jQuery提供了`get()`方法，可以这么写：

```javascript
var jqxhr = $.get('/path/to/resource', {
    name: 'Bob Lee',
    check: 1
});
```

第二个参数如果是object，jQuery自动把它变成query string然后加到URL后面，实际的URL是：

```
/path/to/resource?name=Bob%20Lee&check=1
```

这样我们就不用关心如何用URL编码并构造一个query string了。

## post

`post()`和`get()`类似，但是传入的第二个参数默认被序列化为`application/x-www-form-urlencoded`：

```javascript
var jqxhr = $.post('/path/to/resource', {
    name: 'Bob Lee',
    check: 1
});
```

实际构造的数据`name=Bob%20Lee&check=1`作为POST的body被发送。

## getJSON

由于JSON用得越来越普遍，所以jQuery也提供了`getJSON()`方法来快速通过GET获取一个JSON对象：

```javascript
var jqxhr = $.getJSON('/path/to/resource', {
    name: 'Bob Lee',
    check: 1
}).done(function (data) {
    // data已经被解析为JSON对象了
});
```

## 安全限制

jQuery的AJAX完全封装的是JavaScript的AJAX操作，所以它的安全限制和前面讲的用JavaScript写AJAX完全一样。

如果需要使用JSONP，可以在`ajax()`中设置`jsonp: 'callback'`，让jQuery实现JSONP跨域加载数据。

# 编写jQuery插件

## 扩展函数

想要高亮显示某些DOM元素，用jQuery可以这么实现：

```javascript
$('span.hl').css('backgroundColor', '#fffceb').css('color', '#d85030');
$('p a.hl').css('backgroundColor', '#fffceb').css('color', '#d85030');
```

为了把逻辑统一起来，就可以编写jQuery插件实现如下调用效果：

```javascript
$('span.hl').highlight();
$('p a.hl').highlight();
```

给jQuery对象绑定一个新方法是通过扩展`$.fn`对象实现的：

```javascript
$.fn.highlight1 = function () {
    // this已绑定为当前jQuery对象:
    this.css('backgroundColor', '#fffceb').css('color', '#d85030');
    return this;
}
```

注意到函数内部的`this`在调用时被绑定为jQuery对象，所以函数内部代码可以正常调用所有jQuery对象的方法。

为了支持链式操作所以最后一定要`return this;`

```javascript
$('span.hl').highlight1().slideDown();
```

希望高亮的颜色能自己来指定，可以给方法加个参数，让用户自己把参数用对象传进去。于是有了第二个版本的`highlight2()`：

```javascript
$.fn.highlight2 = function (options) {
    // 要考虑到各种情况:
    // options为undefined
    // options只有部分key
    var bgcolor = options && options.backgroundColor || '#fffceb';
    var color = options && options.color || '#d85030';
    this.css('backgroundColor', bgcolor).css('color', color);
    return this;
}
```

对于默认值的处理，用了一个简单的`&&`和`||`短路操作符，总能得到一个有效的值。

调用方式：

```javascript
$('#test-highlight2 span').highlight2({
    backgroundColor: '#00a8e6',
    color: '#ffffff'
});
```

jQuery提供的辅助方法`$.extend(target, obj1, obj2, ...)`，它把多个object对象的属性合并到第一个target对象中，遇到同名属性，总是使用靠后的对象的值，也就是越往后优先级越高：

```javascript
// 把默认值和用户传入的options合并到对象{}中并返回:
var opts = $.extend({}, {
    backgroundColor: '#00a8e6',
    color: '#ffffff'
}, options);
```

可修改默认值的版本：

```javascript
$.fn.highlight = function (options) {
    // 合并默认值和用户设定值:
    var opts = $.extend({}, $.fn.highlight.defaults, options);
    this.css('backgroundColor', opts.backgroundColor).css('color', opts.color);
    return this;
}

// 设定默认值:
$.fn.highlight.defaults = {
    color: '#d85030',
    backgroundColor: '#fff8de'
}
```

使用时，只需一次性设定默认值：

```javascript
$.fn.highlight.defaults.color = '#fff';
$.fn.highlight.defaults.backgroundColor = '#000';
```

然后就可以非常简单地调用`highlight()`了。

调用示例：

```html
<!-- HTML结构 -->
<div id="test-highlight">
    <p>如何编写<span>jQuery</span> <span>Plugin</span></p>
    <p>编写<span>jQuery</span> <span>Plugin</span>，要设置<span>默认值</span>，并允许用户修改<span>默认值</span>，或者运行时传入<span>其他值</span>。</p>
</div>
```

脚本：

```javascript
$.fn.highlight.defaults.color = '#659f13';
$.fn.highlight.defaults.backgroundColor = '#f2fae3';

$('#test-highlight p:first-child span').highlight();
$('#test-highlight p:last-child span').highlight({
    color: '#dd1144'
});
```

编写一个jQuery插件的原则：

1. 给`$.fn`绑定函数，实现插件的代码逻辑；
2. 插件函数最后要`return this;`以支持链式调用；
3. 插件函数要有默认值，绑定在`$.fn..defaults`上；
4. 用户在调用时可传入设定值以便覆盖默认值。

## 针对特定元素的扩展

jQuery对象的有些方法只能作用在特定DOM元素上，比如`submit()`方法只能针对`form`。

借助`filter()`方法可实现针对特定元素的扩展，实现如下的调用效果：

```javascript
$('#main a').external();
```

编写一个`external`扩展：

```javascript
$.fn.external = function () {
    // return 返回的 each()返回结果，支持链式调用:
    return this.filter('a').each(function () {
        // 注意: each()内部的回调函数的this绑定为DOM本身!
        var a = $(this);
        var url = a.attr('href');
        if (url && (url.indexOf('http://')===0 || url.indexOf('https://')===0)) {
            a.attr('href', '#0')
             .removeAttr('target')
             .append(' <i class="uk-icon-external-link"></i>')
             .click(function () {
                if(confirm('你确定要前往' + url + '？')) {
                    window.open(url);
                }
            });
        }
    });
}
```

对如下的HTML结构：

```html
<!-- HTML结构 -->
<div id="test-external">
    <p>如何学习<a href="http://jquery.com">jQuery</a>？</p>
    <p>首先，你要学习<a href="/wiki/1022910821149312">JavaScript</a>，并了解基本的<a href="https://developer.mozilla.org/en-US/docs/Web/HTML">HTML</a>。</p>
</div>
```

调用：

```javascript
$('#test-external a').external();
```



# 相关问题

## `JQuery`不能使用箭头函数 

因为箭头函数在创建时就已经绑定了`this`, 后面在执行时不能重新绑定, 它的this和距离最近的函数作用域的this保持一致 。 

而`JQuery`在执行回调函数时需要动态为函数绑定一个`this`。

```javascript
() => {};
// 相当于：
funciotn(){}.bind(this);
```

## 为什么有的动画没有效果

有的动画如`slideUp()`根本没有效果。这是因为jQuery动画的原理是逐渐改变CSS的值，如`height`从`100px`逐渐变为`0`。但是很多不是block性质的DOM元素，对它们设置`height`根本就不起作用，所以动画也就没有效果。

此外，jQuery也没有实现对`background-color`的动画效果，用`animate()`设置`background-color`也没有效果。这种情况下可以使用CSS3的`transition`实现动画效果。