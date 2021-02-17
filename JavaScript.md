# JavaScript

## 1 快速入门

### 1.1 引入JavaScript

在html中的引用

```html
<!--方法1 内部标签-->
<script>
	alert('hello,world');  <!--弹窗-->
</script>

<!--方法2 外部引入-->
<!--不要写成自闭合标签，必须成对出现-->
<script src="js/qj.js"></script>
```

### 1.2 基本语法

```html
<script>
  // 1. 定义变量
  var score = 1;
  // alert(score);
  
  // 2. 条件控制，keqiantao
  if (score>60 && score<70) {
    alert('60~70');
  }else if (score>70) {
    alert('>70');
  }else{
    alert('other');
  }
  
  //console.log(score) 在浏览器的控制台打印变量
  /*
  * 多行注释
  * */
</script>
```

### 1.3 数据类型

变量

```javascript
var a = 1;  //变量名可为中文
```

number，不区分小数和整数

```javascript
123 //整数
123.1 //浮点数
1.123e3 //科学计数法
-99 //负数
NaN //not a number
Infinity //无限大
```

字符串

```javascript
'abc'
"abc"
```

布尔值

```javascript
true
false
```

逻辑运算

```javascript
&&
||
!
```

比较运算

```javascript
=
== //等于（类型不一样，值一样，也会判断为true），坚持不要使用==比较
=== //绝对等于，NaN与所有数值不相等，包括自己，需要通过isNaN(NaN)来判断
```

null与undefined

```javascript
null //空
undefined //未定义
```

数组

```javascript
var arr = [1, 2, 'hhh', true, null];
//取数组下标越界，则为undefined
```

对象

```javascript
var person = {
  name: "hhh",
  age: 3,
  tags: ['js', 'web', '...']
}
```

### 1.4 严格检查格式

```javascript
//ES6
//'use strict';严格检查模式，必须写在第一行，预防JavaScript的随意性导致的一些问题

'use strict';

//局部变量建议使用let定义

let i = 1;
```

## 2 数据类型

### 2.1 字符串

正常字符串使用单引号/双引号

注意转义字符

多行字符串编写

```javascript
let msg = `hello
world
！`;
```

模板字符串

```javascript
let name = 'world';

let msg = `hello, ${name}`;
```

字符串长度

```javascript
str.length
```

字符串的可变性：不可变

大小写转换

```javascript
str.toUpperCase()
str.toLowerCase()
```

获取下标

```javascript
str.indexOf('t')
```

截取字符串

```javascript
str.substring(1) //从第一个字符截取到最后一个字符
str.substring(1,3) //包含第一个，不包含第三个
```

### 2.2 数组

Array可以包含任意的数据类型

```javascript
var arr = [1,2,3,4,5,6,"1","2"]; //通过下标取值和赋值
```

长度可变

```javascript
arr.length //可以赋值，多余的为undefined，减小的丢失
```

获取下标

```javascript
arr.indexOf("1")
```

截取数组

```javascript
arr.slice(3)
arr.slice(2,5)
```

push、pop尾部

```javascript
arr.push('a','b') //最后面添加
arr.pop() //弹出最后一个
```

unshift、shift头部

```javascript
arr.unshift('a','b')
arr.shift()
```

排序

```javascript
arr.sort()
```

反转

```javascript
arr.reverse()
```

拼接

```javascript
arr.concat([1,2,3]) //返回新的数组，并不修改arr
```

连接符join，打印拼接数组，使用特定的字符串连接

```javascript
arr.join('-')
```

多维数组

```javascript
var arr = [[1,2],[3,4],["5","6"]]
```

### 2.3 对象

若干个键值对，所有键都是字符串，值任意

```javascript
var 对象名 = {
  属性名: 属性值,
  ……: ……，
  属性名: 属性值
}
```

对象赋值

```javascript
对象名.属性名 = 属性值
```

使用一个不存在的对象属性，不会报错，undefined

动态删减属性

```javascript
delete 对象名.属性名
```

动态添加属性

```javascript
对象名.新属性名 = 属性值
```

判断属性名是否在这个对象中

```javascript
属性名 in 对象名 //true
//继承，可找到父类方法
```

判断一个属性是否是这个对象自身拥有的

```javascript
对象名.hasOwnProperty(属性名) //true
```

### 2.4 流程控制

if判断

```javascript
var age = 3;
if (age>3){
  alert('haha');
} else {
  alert('wawa');
}
```

while循环

```javascript
while(age<100){
  age = age + 1;
  console.log(age);
}

do{
  age = age + 1;
  console.log(age);
}while(age<100)
```

for循环

```javascript
for(let i=0;i<100;i++){
  age = age + 1;
  console.log(age);
}
```

foreach循环

```javascript
var age = [1,2,3,4,5,6];
age.foreach(function(value){
  console.log(value);
})
```

for…in

```javascript
for(var num in age){
  console.log(age[num]); //num是索引
}
```

### 2.5 Map与Set

ES6新特性

Map

```javascript
var map = new Map([['Tom',100],['Jack',90],['Mike',80]]);
var name = map.get('Tom'); //通过key获得value
map.set('Rose',60); //增加、修改
map.delete('Tom'); //删除
```

Set

```javascript
var set = new Set([3,1,1,1,1,1]); //去重
set.add(2);
set.delete(1);
console.log(set.has(3)); //是否包含一个元素
```

### 2.6 Iterato

ES6新特性

遍历数组

```javascript
//通过for of遍历值（for in是下标）
var arr = [3,4,5];
for(let x of arr){
  console.log(x);
}
```

遍历map

```javascript
var map = new Map([['Tom',100],['Jack',90],['Mike',80]]);
for(let x of map){
  console.log(x);
}
```

遍历Set

```javascript
var set = new Set([5,6,7]);
for(let x of set){
  console.log(x);
}
```

## 3 函数

### 3.1 函数定义

> 定义方式一

绝对值函数

```javascript
function abs(x){
  if(x>=0){
    return x;
  }else{
    return -x;
  }
}
```

如果没有执行return，函数执行完也会返回结果undefined

> 定义方式二

```javascript
var abs = function(x){
  if(x>=0){
    return x;
  }else{
    return -x;
  }
}
//这是一个匿名函数，但是可以把结果赋值给abs，通过abs就可以调用函数
```

> 调用函数

```javascript
abs(10) //10
abs(-10) //10
```

参数问题：可以传递任意个参数，也可以不传递参数

假设不存在参数，如何规避？

```javascript
var abs = function(x){
  //手动抛出异常来判断
  if(typeof x !== 'number'){
    throw 'Not a Number';
  }
	if(x>=0){
    return x;
  }else{
    return -x;
  }
}
```

> arguments

代表传递进来的所有参数是一个数组

```javascript
var abs = function(x){
  
  console.log("x=>"+x);
  
  for(var i=0;i<arguments.length;i++){
    console.log(arguments[i]);
  }
  
	if(x>=0){
    return x;
  }else{
    return -x;
  }
}
```

arguments包含所有的参数，有时候需要多余的参数来进行附加的操作，需要排除已有参数

> rest

以前：

```javascript
function aaa(a,b) {
  console.log("a=>"+a);
  console.log("b=>"+b);
  if(arguments.length>2){
    for(var i=2;i<arguments.length;i++){
      //……
    }
  }
}
```

ES6新特性，获取已经定义的参数之外的所有参数

现在：

```javascript
function aaa(a,b,...rest) {
  console.log("a=>"+a);
  console.log("b=>"+b);
  console.log(rest);
}
```

### 3.2 变量作用域

> 局部变量

var定义变量实际是有作用域的

```javascript
function qj(){
  var x = 1;
  x = x + 1;
}

x = x + 2; //Uncaught ReferenceError: x is not defined
```

内部函数可以访问外部函数的成员，反之则不行

```javascript
function qj(){
  var x = 1;
  
  function qj2(){
    var y = x + 1; //2
  }
  
  var z = y + 1; //Uncaught ReferenceError: y is not defined
}
```

提升变量的作用域

```javascript
function qj(){
  var x = "x" + y;
  console.log(x);
  var y = 'y';
}
qj()
//结果x undefined
//说明js执行引擎，自动提升了y的声明，但是不会提升y的赋值
```

养成规范：变量声明放在头部

> 全局变量

```javascript
var x = 1;
function f(){
  console.log(x);
}
f();
console.log(x);
```

全局对象window

```javascript
var x = 'xxx';
alert(x);
window.alert(window.x); //默认所有的全局对象都会自动绑定在window对象上
```

alert()这个函数本身也是一个window的变量

```javascript
var x = 'xxx';
window.alert(x);
var old_alert = window.alert;
old_alert(x);

window.alert = function(){
  
};
window.alert(123) //失效
window.alert = old_alert //恢复
```

js实际上只有一个全局作用域，任何变量（函数也可以视为变量），假如没有在函数作用范围内找到，就会向外查找，如果在全局作用域都没有找到，则报错

> 规范

由于所有的全局变量都会绑定到window上，如果不同的js文件使用了相同的全局变量，则会发生冲突，如何减少冲突？

```javascript
//唯一全局变量
var SoApp = {};

//定义全局变量
SoApp.name = 'hhhhhhh';
SoApp.add = function(a,b){
  return a+b;
}
```

把自己的代码全部放入自己定义的唯一空间名字中，降低全局命名冲突的可能性

> 局部作用域let

```javascript
function aaa(){
  for(var i = 0; i < 100; i++){
    console.log(i);
  }
  console.log(i+1); //i出了这个作用域还可以使用
}
```

ES6新特性

若使用let关键字，解决局部作用域冲突的问题

> 常量const

ES6新特性

```javascript
const PI = '3.14';
```

### 3.3 方法

方法就是把函数放在对象的里面

```javascript
var nihao = {
  name: 'Mike',
  birth: 2021,
  age: function() {
    var now = new Date().getFullYear();
    return now - this.birth;
  }
}
//属性
nihao.name
//方法
nihao.age()
```

也可以拆开上面的代码

```javascript
function getAge() {
    var now = new Date().getFullYear();
    return now - this.birth;
}

var nihao = {
  name: 'Mike',
  birth: 2021,
  age: getAge
}
// nihao.age() ok
// getAge.apply(nihao,[]) ok
// getAge() NaN
```

### 3.4 闭包



### 3.5 箭头函数



## 4 内部对象

> 标准对象

```javascript
typeof 123 //"number"
typeof '123' //"string"
typeof true //"boolean"
typeof NaN //"number"
typeof [] //"object"
typeof {} //"object"
typeof Math.abs //"function"
typeof undefined //"undefined"
```

### 4.1 Date

```javascript
var now = new Date(); //Tue Feb 16 2021 22:54:01 GMT+0800 (中国标准时间)
now.getFullYear(); //年
now.getMonth(); //月
now.getDate(); //日
now.getDay(); //星期
now.getHours(); //时
now.getMinutes(); //分
now.getSeconds(); //秒
now.getTime; //时间戳，全世界统一，1970 1.1 0:00:00 毫秒数

console.log(new Date(75912376589)) //时间戳转为时间
```

### 4.2 JSON

任何js支持的类型都可以用JSON来表示

格式：

* 对象都用{}
* 数组都用[]
* 所有键值对都用key: value

```javascript
var user = {
  name: "Mike",
  age: 3
}
//对象转化为json字符串
var jsonUser = JSON.stringify(user) //{"name":"Mike","age":3}

//json字符串转化为对象
var obj = JSON.parse('{"name":"Mike","age":3}')
```

## 5 面向对象编程

原型

```javascript
var Student = {
  name: "hhh",
  age: 3,
  run: function() {
    console.log(this.name+" run……")
  }
}
var xiaoming = {
  name: "xiaoming"
}
//xiaoming的原型是Student
xiaoming.__proto__ = Student;
xiaoming.run();
```

> class继承

ES6新特性

定义一个类，属性，方法

```javascript
//原来
function Student(name) {
  this.name = name;
}

//给student新增一个方法
Student.prototypr.hello = function() {
  alert('Hello');
}

//上下等价

//ES6之后
//定义一个学生的类
class Student{
  constructor(name){
    this.name = name;
  }
  hello(){
    alert('Hello');
  }
}

var xiaoming = new Student('xiaoming');
xiaoming.name;
xiaoming.hello();
```

继承

```javascript
class College extends Student{
  constructor(name, grade){
    super(name);
    this.grade = grade;
  }
  
  myGrade(){
    alert('I am a college student.')
  }
}
var xiaohong = new College('xiaohong',1)
```

本质：查看对象原型

> 原型链

\__proto__

## 6 操作BOM对象

BOM：浏览器对象模型

> window对象

window代表浏览器窗口

```javascript
window.alert()
window.innerHeight
window.innerWidth
window.outerHeight
window.outerWidth
```

> Navigator，window.navigator. …

封装了浏览器的信息

> screen

计算机屏幕

> location（重要）

代表当前页面的URL信息

```javascript
host:"www.baidu.com"
href:"https://www.baidu.com/"
protocol:"https:"
reload:f reload() //刷新网页

location.reload()

location.assign('https://…………') //设置新的地址
```

> document

代表当前的页面，HTML DOM文档树

获取具体的文档树节点

```html
<dl id='app'>
  <dt>HELLO</dt>
  <dt>WORLD</dt>
</dl>

<script>
	var dl = document.getElementById('app');
</script>
```

获取cookie

```javascript
document.cookie
```

> history

```javascript
history.back() //后退
hisotory.forward() //前进
```

## 7 操作DOM对象

DOM：文档对象模型

> 核心

浏览器网页就是一个Dom树形结构

* 更新：更新DOM节点
* 遍历：得到DOM节点
* 删除：删除DOM节点
* 添加：添加新的节点

要操作一个DOM节点，必须要先获得这个DOM节点

```html
<div id="top">
  <h1>标题一</h1>
	<p id="p1">p1</p>
	<p class="p2">p2</p>
</div>

<script>
  //对应css选择器
  var h1 = document.getElementsByTagName('h1');
  var p1 = document.getElementById('p1');
  var p2 = document.getElementByClassName('p2');
  var top = document.getElementById('top');
  
  var childrens = top.children; //获取父节点下的所有子节点
  //top.firstChild
  //top.lastChild
</script>
```

这是原生代码，之后尽量使用jQuery

### 7.1 更新节点

```html
<div id='id1'></div>

<script>
	var id1 = document.getElementById('id1');
  id1.innerText='456'; //修改文本的值
  id1.innerHTML='<strong>123</strong>'; //可以解析HTML文本标签
  
  id1.innerText='abx'; //更改样式 属性使用字符串包裹
  id1.style.color='red';
  id1.style.fontSize='20px'; //_转驼峰命名
  id1.style.padding'2em';
</script>
```

### 7.2 删除节点

删除节点的步骤：

1. 获取父节点
2. 通过父节点删除自己

```html
<div id="top">
  <h1>标题一</h1>
	<p id="p1">p1</p>
	<p class="p2">p2</p>
</div>

<script>
  var self = document.getElementById('p1');
  var father = self.parentElement;
  father.removeChild(self); //删除方式1
 
  //删除是一个动态的过程
  father.removeChild(father.children[0]); //删除方式2
  father.removeChild(father.children[0]);
  father.removeChild(father.children[0]);
</script>
```

### 7.3 插入节点

获得了某个DOM节点，假设这个节点是空的，通过innerHTML就可以增加一个元素，但是假如这个DOM节点已经存在元素，就不能这样做，因为会产生覆盖

```html
<p id="js">JavaScript</p>
<div id="list">
  <p id="se">JavaSE</p>
  <p id="ee">JavaEE</p>
  <p id="me">JavaME</p>
</div>

<script>
	var js = document.getElementById('js');
  var list = document.getElementById('list');
  
  list.appendChild(js); //追加已有标签到后面
  
  //通过js创建新的节点
  var newP = document.createElement('p'); //创建一个p标签
  newp.id = 'newP';
  //也可以写成newP.setAttribute('id','newP');
  newP.innerText = 'Hello';
  list.appendChild(newP);
  
  //创建一个标签节点
  var myScript = document.createElement('script');
  myScript.setAttribute('type','text/javascript');
  
  //更改body
  var body = document.getElementsByTagName('body');
  body[0].style.background = 'red';
  
  //list下js插入到ee前
  var ee = document.getElementById('ee');
  list.insertBefore(js,ee);
</script>
```

## 8 操作表单

表单：form

* 文本框 input='text'

* 下拉框 \<select>

* 单选框 radio

* 复选框 checkbox

* 隐藏域 hidden

* 密码框password

* ###### ……

表单的目的：提交信息

```html
<form action="#" method="post">
  <p>
  	<span>用户名：</span> <input input='text' id='username'>
  </p>
  <p>
    <span>性别：</span>
    <input type"radio" name="sex" value="male" id="male">男
    <input type"radio" name="sex" value="female" id="female">女
  </p>
</form>

<script>
  var input_text = document.getElementById('username');
  var male_radio = document.getElementById('male');
  var female_radio = document.getElementById('female');
  //得到输入框的值
  input_text.value;
  //修改输入框的值
  input_text.value = '123';
  
  //对于单选框、复选框等等固定的值，用male_radio.value只能取到当前的值
  male_radio.checked; //查看返回的结果是否为true，若是true则为被选中
</script>
```

> 提交表单，md5以及表单优化

```html
<!--
obsubmit=绑定一个提交检测的函数，true或false
将这个结果返回给表单，使用onsubmit接收
onsubmit="return aaa()"
-->
<form action="https://www.baidu.com/" method="post" onsubmit="return aaa()">
  <p>
  	<span>用户名：</span> <input input='text' id='username' name="username">
  </p>
  <p>
  	<span>密码：</span> <input input='password' id='input-password'>
  </p>
  
  <!--通过隐藏域提交-->
  <input type="hidden" id="md5-password" name="password">
  
  <!--<input type="submit">-->
  <!--使用button可以绑定事件，onclick-->
  <button type="submit">提交</button>
</form>

<script>
  function aaa(){
    var uname = document.getElementById('username');
    var pwd = document.getElementById('input-password');
    var md5pwd = document.getElementById('md5-password');
    //MD5 算法
    md5pwd.value = md5(pwd.value);
    
    //可以校验判断表单内容，true就是通过提交，false就是阻止提交
    return true;
  }
</script>
```

## 9 jQuery

js库，封装了大量js函数

> 引入jQuery

```html
<!--在线cdn引入-->
<script src="https://cdn.bootcss.com/jquery/3.4.1/core.js"></script>

<!--本地导入-->
<script src="static/jquery-3.4.1.js"></script>
```

公式

```html
$(selector).action()
```

```html
<a href="" id="test-jquery">点击</a>

<script>
	//选择器就是css选择器
  $('#test-jquery').click(function(){
    alert('hello,jquery');
  })
</script>
```

### 9.1 选择器

```html
<script>
  //原生js，选择器少
  document.getElementsByTagName(); //标签
  document.getElementById(); //id
  document.getElementsByclassName(); //class
  
  //jQuery css中的选择器都能用
  $('p').click(); //标签
  $('#id1').click(); //id
  $('.class1').click(); //class
  
</script>
```

### 9.2 事件

鼠标事件、键盘事件、其它事件

```html
<!--获取鼠标的坐标-->
mouse:<span id="mouseMove"></span>
<div id="divMove">
  在这里移动鼠标试试
</div>

<script>
	//当网页元素加载完毕之后，响应事件
  //以下为加载完成的简写
  $(function(){
    $('#divMove').mousemove(fuction(e){
				$('#mouseMove').text('x:' + e.pageX + 'y' +e.pageY)
    })
  })
</script>
```

### 9.3 操作DOM

```html
<ul id="test-ul">
  <li class="js">JavaScript</li>
  <li name="python">Python</li>
</ul>

<script>
  //节点文本操作
  //没有参数为取值，纯文本
	$('#test-ul li[name=python]').text();
  //有参数为设置值
  $('#test-ul li[name=python]').text('123');
  
  //html
  $('#test-ul li[name=python]').html();
  $('#test-ul li[name=python]').html('<strong>123</strong>');
  
  
  //css操作
  $('#test-ul li[name=python]').css({"color","red"});
  
  //元素的显示和隐藏：本质css中display: none
  $('#test-ul li[name=python]').show();
  $('#test-ul li[name=python]').hide();
</script>
```

娱乐测试

```javascript
$(window).width();
$(window).height();
```

















