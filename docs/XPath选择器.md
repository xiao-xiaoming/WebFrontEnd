## 选取节点

表达式：

| 表达式   | 描述                                                       |
| :------- | :--------------------------------------------------------- |
| nodename | 选取此节点的所有子节点。                                   |
| /        | 从根节点选取。                                             |
| //       | 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置。 |
| .        | 选取当前节点。                                             |
| ..       | 选取当前节点的父节点。                                     |
| @        | 选取属性。                                                 |

基本的XPath语法类似于在一个文件系统中定位文件：

如果路径以斜线 / 开始,  那么该路径就表示到一个元素的绝对路径

`/AAA` 选择根元素AAA

`/AAA/CCC`  选择AAA的所有CCC子元素

`/AAA/DDD/BBB`  选择AAA的子元素DDD的所有BBB子元素

如果路径以双斜线 // 开头, 则表示选择文档中所有满足双斜线//之后规则的元素(无论层级关系)

`//BBB`  选择所有BBB元素

`//DDD/BBB`  选择所有父元素是DDD的BBB元素

例子：

| 路径表达式      | 结果                                                         |
| :-------------- | :----------------------------------------------------------- |
| bookstore       | 选取 bookstore 元素的所有子节点。                            |
| /bookstore      | 选取根元素 bookstore。注释：假如路径起始于正斜杠( / )，则此路径始终代表到某元素的绝对路径！ |
| bookstore/book  | 选取属于 bookstore 的子元素的所有 book 元素。                |
| //book          | 选取所有 book 子元素，而不管它们在文档中的位置。             |
| bookstore//book | 选择属于 bookstore 元素的后代的所有 book 元素，而不管它们位于 bookstore 之下的什么位置。 |
| //@lang         | 选取名为 lang 的所有属性。                                   |



## 谓语（Predicates）

谓语用来查找某个特定的节点或者包含某个指定的值的节点。

谓语被嵌在方括号中。

实例：

| 路径表达式                          | 结果                                                         |
| :---------------------------------- | :----------------------------------------------------------- |
| /bookstore/book[1]                  | 选取属于 bookstore 子元素的第一个 book 元素。                |
| /bookstore/book[last()]             | 选取属于 bookstore 子元素的最后一个 book 元素。              |
| /bookstore/book[last()-1]           | 选取属于 bookstore 子元素的倒数第二个 book 元素。            |
| /bookstore/book[position()<3]       | 选取最前面的两个属于 bookstore 元素的子元素的 book 元素。    |
| //title[@lang]                      | 选取所有拥有名为 lang 的属性的 title 元素。                  |
| //title[@lang='eng']                | 选取所有 title 元素，且这些元素拥有值为 eng 的 lang 属性。   |
| //BBB[normalize-space(@name)='bbb'] | 选择含有属性name且其值(在用normalize-space函数去掉前后空格后)为'bbb'的BBB元素 |
| /bookstore/book[price>35.00]        | 选取 bookstore 元素的所有 book 元素，且其中的 price 元素的值须大于 35.00。 |
| /bookstore/book[price>35.00]/title  | 选取 bookstore 元素中的 book 元素的所有 title 元素，且其中的 price 元素的值须大于 35.00。 |
| //@id                               | 选择所有的id属性                                             |
| //BBB[@*]                           | 选择有任意属性的BBB元素                                      |
| //BBB[not(@*)]                      | 选择没有属性的BBB元素                                        |
| //*[count(BBB)=2]                   | 选择含有2个BBB子元素的元素                                   |
| `//*[count(*)=3]`                   | 选择含有3个子元素的元素                                      |
| //*[starts-with(name(),'B')]        | 选择所有名称以"B"起始的元素                                  |
| //*[contains(name(),'C')]           | 选择所有名称包含"C"的元素                                    |
| //*[string-length(name()) > 3]      | 选择名字长度大于3的元素                                      |



## 通配符

语法：

| 通配符 | 描述                 |
| :----- | :------------------- |
| *      | 匹配任何元素节点。   |
| @*     | 匹配任何属性节点。   |
| node() | 匹配任何类型的节点。 |

例子：

| 路径表达式   | 结果                              |
| :----------- | :-------------------------------- |
| /bookstore/* | 选取 bookstore 元素的所有子元素。 |
| //*          | 选取文档中的所有元素。            |
| //title[@*]  | 选取所有带有属性的 title 元素。   |

## “|”运算符-多路径选择

通过在路径表达式中使用“|”运算符，您可以选取若干个路径。

例子：

| 路径表达式                       | 结果                                                         |
| :------------------------------- | :----------------------------------------------------------- |
| //book/title \| //book/price     | 选取 book 元素的所有 title 和 price 元素。                   |
| //title \| //price               | 选取文档中的所有 title 和 price 元素。                       |
| /bookstore/book/title \| //price | 选取属于 bookstore 元素的 book 元素的所有 title 元素，以及文档中所有的 price 元素。 |

## XPath轴

轴可定义相对于当前节点的节点集。

| 轴名称             | 结果                                                     |
| :----------------- | :------------------------------------------------------- |
| ancestor           | 选取当前节点的所有先辈（父、祖父等）。                   |
| ancestor-or-self   | 选取当前节点的所有先辈（父、祖父等）以及当前节点本身。   |
| attribute          | 选取当前节点的所有属性。                                 |
| child              | 选取当前节点的所有子元素。                               |
| descendant         | 选取当前节点的所有后代元素（子、孙等）。                 |
| descendant-or-self | 选取当前节点的所有后代元素（子、孙等）以及当前节点本身。 |
| following          | 选取文档中当前节点的结束标签之后的所有节点。             |
| namespace          | 选取当前节点的所有命名空间节点。                         |
| parent             | 选取当前节点的父节点。                                   |
| preceding          | 选取文档中当前节点的开始标签之前的所有节点。             |
| preceding-sibling  | 选取当前节点之前的所有同级节点。                         |
| self               | 选取当前节点。                                           |

**轴的语法：**

```
/step/step/.../轴名称::节点测试[谓语]
或
//step/step/.../轴名称::节点测试[谓语]
```

| 例子                   | 结果                                                         |
| :--------------------- | :----------------------------------------------------------- |
| child::book            | 选取所有属于当前节点的子元素的 book 节点。                   |
| attribute::lang        | 选取当前节点的 lang 属性。                                   |
| child::*               | 选取当前节点的所有子元素。                                   |
| attribute::*           | 选取当前节点的所有属性。                                     |
| child::text()          | 选取当前节点的所有文本子节点。                               |
| child::node()          | 选取当前节点的所有子节点。                                   |
| descendant::book       | 选取当前节点的所有 book 后代。                               |
| ancestor::book         | 选择当前节点的所有 book 先辈。                               |
| ancestor-or-self::book | 选取当前节点的所有 book 先辈以及当前节点（如果此节点是 book 节点） |
| child::*/child::price  | 选取当前节点的所有 price 孙节点。                            |

示例：

| 路径表达式                           | 结果                                              |
| ------------------------------------ | ------------------------------------------------- |
| /AAA/BBB/descendant::*               | 选择/AAA/BBB的所有后代元素                        |
| //CCC/descendant::DDD                | 选择所有以CCC为祖先元素的DDD元素                  |
| //DDD/parent::*                      | 选择DDD元素的所有父节点                           |
| /AAA/BBB/DDD/CCC/EEE/ancestor::*     | 选择一个绝对路径上的所有节点                      |
| //FFF/ancestor::*                    | 选择FFF元素的所有父节点和祖先节点                 |
| /AAA/BBB/following-sibling::*        | 选择BBB元素之后的所有兄弟节点（同级节点）         |
| /AAA/XXX/preceding-sibling::*        | 选择XXX元素之后的所有兄弟节点                     |
| /AAA/XXX/following::*                | 选择XXX元素按文档顺序位于上下文节点之后的所有节点 |
| /AAA/XXX/preceding::*                | 选择XXX元素按文档顺序位于上下文节点之前的所有节点 |
| /AAA/XXX/descendant-or-self::*       | 选择XXX元素本身和该节点的所有后代节点             |
| /AAA/XXX/DDD/EEE/ancestor-or-self::* | 选择EEE元素本身和该节点的所有祖先节点             |



## xpath数学函数

div运算符做浮点除法运算, mod运算符做求余运算, floor函数返回不大于参数的最大整数(趋近于正无穷),  ceiling返回不小于参数的最小整数(趋近于负无穷)

| 名称                 | 说明                                                         |
| :------------------- | :----------------------------------------------------------- |
| number(arg)          | 返回参数的数值。参数可以是布尔值、字符串或节点集。例子：number('100')结果：100 |
| abs(num)             | 返回参数的绝对值。例子：abs(3.14)结果：3.14，abs(-3.14)结果：3.14 |
| ceiling(num)         | 返回大于 num 参数的最小整数。例子：ceiling(3.14)结果：4      |
| floor(num)           | 返回不大于 num 参数的最大整数。例子：floor(3.14)结果：3      |
| round(num)           | 把 num 参数舍入为最接近的整数。例子：round(3.14)结果：3      |
| round-half-to-even() | round-half-to-even(0.5)结果：0<br />round-half-to-even(1.5)结果：2<br />round-half-to-even(2.5)结果：2 |

例子：

| 路径表达式                                                   | 结果                  |
| ------------------------------------------------------------ | --------------------- |
| //BBB[position() mod 2 = 0 ]                                 | 选择偶数位置的BBB元素 |
| //BBB[ position() = floor(last() div 2 + 0.5) or position() = ceiling(last()  div 2 + 0.5) ] | 选择中间的BBB元素     |



## XPath运算符

除法和等于与常规编程语言有区别：

| 运算符 | 描述           | 实例                      |
| :----- | :------------- | :------------------------ |
| \|     | 计算两个节点集 | //book \| //cd            |
| +      | 加法           | 6 + 4                     |
| -      | 减法           | 6 - 4                     |
| *      | 乘法           | 6 * 4                     |
| div    | 除法           | 8 div 4                   |
| =      | 等于           | price=9.80                |
| !=     | 不等于         | price!=9.80               |
| <      | 小于           | price<9.80                |
| <=     | 小于或等于     | price<=9.80               |
| >      | 大于           | price>9.80                |
| >=     | 大于或等于     | price>=9.80               |
| or     | 或             | price=9.80 or price=9.70  |
| and    | 与             | price>9.00 and price<9.90 |
| mod    | 计算除法的余数 | 5 mod 2                   |

## xpath字符串函数

| 名称                                                     | 说明                                                         |
| :------------------------------------------------------- | :----------------------------------------------------------- |
| string(arg)                                              | 返回参数的字符串值。例子：string(314)结果："314"             |
| codepoints-to-string(int,int,...)                        | 根据代码点序列返回字符串。<br />例子：codepoints-to-string(84, 104, 233, 114, 232, 115, 101)<br />结果：'Thérèse' |
| string-to-codepoints(string)                             | 根据字符串返回代码点序列。<br />例子：string-to-codepoints("Thérèse")<br />结果：84, 104, 233, 114, 232, 115, 101 |
| codepoint-equal(comp1,comp2)                             | 根据 Unicode 代码点对照，如果 comp1 的值等于 comp2 的值，则返回 true，否则返回 false。 |
| compare(comp1,comp2)<br />compare(comp1,comp2,collation) | 如果 comp1 小于 comp2，则返回 -1。<br />如果 comp1 等于 comp2，则返回 0。<br />如果 comp1 大于 comp2，则返回 1。<br />例子：compare('ghi', 'ghi')结果：0 |
| concat(string,string,...)                                | 返回字符串的拼接。<br />例子：concat('XPath ','is ','FUN!')结果：'XPath is FUN!' |
| string-join((string,string,...),sep)                     | 使用 sep 参数作为分隔符，来返回 string 参数拼接后的字符串。<br />例子：string-join(('We', 'are', 'having', 'fun!'), ' ')<br />结果：' We are having fun! '<br />例子：string-join(('We', 'are', 'having', 'fun!'))<br />结果：'Wearehavingfun!'<br />例子：string-join((), 'sep')结果：'' |
| substring(string,start,len)<br />substring(string,start) | 返回从 start 位置开始的指定长度的子字符串。第一个字符的下标是 1。如果省略 len 参数，则返回从位置 start 到字符串末尾的子字符串。<br />例子：substring('Beatles',1,4)结果：'Beat'<br />例子：substring('Beatles',2)结果：'eatles' |
| string-length(string)<br />string-length()               | 返回指定字符串的长度。如果没有 string 参数，则返回当前节点的字符串值的长度。<br />例子：string-length('Beatles')结果：7 |
| normalize-space(string)<br />normalize-space()           | 删除指定字符串的开头和结尾的空白，并把内部的所有空白序列替换为一个，然后返回结果。如果没有 string 参数，则处理当前节点。<br />例子：normalize-space(' The  XML ')结果：'The XML' |
| upper-case(string)                                       | 把 string 参数转换为大写。例子：upper-case('The XML')结果：'THE XML' |
| lower-case(string)                                       | 把 string 参数转换为小写。例子：lower-case('The XML')结果：'the xml' |
| translate(string1,string2,string3)                       | 把 string1 中的 string2 替换为 string3。<br />例子：translate('12:30','30','45')结果：'12:45'<br />例子：translate('12:30','03','54')结果：'12:45'<br />例子：translate('12:30','0123','abcd')结果：'bc:da' |
| contains(string1,string2)                                | 如果 string1 包含 string2，则返回 true，否则返回 false。<br />例子：contains('XML','XM')结果：true |
| starts-with(string1,string2)                             | 如果 string1 以 string2 开始，则返回 true，否则返回 false。<br />例子：starts-with('XML','X')结果：true |
| ends-with(string1,string2)                               | 如果 string1 以 string2 结尾，则返回 true，否则返回 false。<br />例子：ends-with('XML','X')结果：false |
| substring-before(string1,string2)                        | 返回 string2 在 string1 中出现之前的子字符串。<br />例子：substring-before('12/10','/')结果：'12' |
| substring-after(string1,string2)                         | 返回 string2 在 string1 中出现之后的子字符串。<br />例子：substring-after('12/10','/')结果：'10' |
| matches(string,pattern)                                  | 如果 string 参数匹配指定的模式，则返回 true，否则返回 false。<br />例子：matches("Merano", "ran")结果：true |
| replace(string,pattern,replace)                          | 把指定的模式替换为 replace 参数，并返回结果。<br />例子：replace("Bella Italia", "l", "*")结果：'Be**a Ita*ia'<br />例子：replace("Bella Italia", "l", "")结果：'Bea Itaia' |
| tokenize(string,pattern)                                 | 例子：tokenize("XPath is fun", "\s+")结果：("XPath", "is", "fun") |

## 实例

对于xml：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	...
	<key>ExpirationDate</key>
	<date>2019-12-19T07:08:45Z</date>
	<key>Name</key>
	<string>OneAppInHouseDistribution</string>
	<key>ProvisionsAllDevices</key>
	<true/>
	<key>TeamIdentifier</key>
	<array>
		<string>7D9QM9S7WR</string>
	</array>
	<key>TeamName</key>
	<string>Daimler Greater China Ltd.</string>
	<key>TimeToLive</key>
	<integer>365</integer>
	<key>UUID</key>
	<string>9b5c67cf-51b4-4631-9304-12f7f208ff9b</string>
	<key>Version</key>
	<integer>1</integer>
</dict>
</plist>
```

取出UUID:

```
//key[text()='UUID']/following-sibling::string[1]
```

取出TeamIdentifier：

```
//key[text()='TeamIdentifier']/following-sibling::array[1]/string[1]
```

