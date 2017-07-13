---
title: ES5基本规范
date: 2017-07-12 22:40:00
categories:
- 前端-js-规范
tags:
- 前端
- js
- 规范
- ES5
---

> 参考：[编写可维护的JavaScript](https://www.amazon.cn/%E5%9B%BE%E4%B9%A6/dp/B00BQ7RMW0/ref=sr_1_1?s=books&ie=UTF8&qid=1499870567&sr=1-1&keywords=%E7%BC%96%E5%86%99%E5%8F%AF%E7%BB%B4%E6%8A%A4%E7%9A%84javascript)

### 基本规范

- 使用递归方式，一定要有结束条件，如果结束条件不确定，要有调用次数
- 每一层级4个空格缩进，禁止使用制表符缩进（可以设置webstorm tab键为四个空格）
```
if (someCondition) {
    doSomething();
}
```
- 每行的长度尽量不超过120个字符（webstorm代码编辑页面中间有一条线，以那条线为准），如果超过应当在一个运算符（逗号、加号、逻辑运算符等）后换行，下一行应当相对增加一级缩进（4个空格）（还是两级？8个空格？）
```
一级缩进：

doSomething(argument1, argument2, argument3, argument4, argument5, argument6, argument7, argument8, argument9, argument10,
    argument11)
    
两级缩进：

doSomething(argument1, argument2, argument3, argument4, argument5, argument6, argument7, argument8, argument9, argument10,
        argument11)
```
- 初始化变量时字符串应当始终使用双引号，避免使用单引号，避免使用undefined，可以使用null
- 函数内用var声明变量时一定要在函数开始的地方，避免在if、for等语句块内声明，因为JavaScript的变量声明会提前。最好在函数的第一句使用一个var声明变量，一个变量一行，添加相应缩进并添加相应注释。
```
function doSomething(items) {
    var i,  // for循环变量
        length,
        value = 10,
        result = value + 10;
    
    for (i = 0, length = items.length; i < length; i++) {
        xxx;
    }
}
```
- 语句末尾一定加分号
- 给变量赋值时如果右侧是含有比较语句的表达式，需要用圆括号包裹
```
var flag = (i < count);
```
- 使用===、!== ，避免使用==和!=
- if语句可以进行相应的简化（特殊情况特殊处理）
```
if (count === 10) {
    success = true;
} else {
    success = false;
}
```
可以简化为:
```
success = (count === 10);
```
```
if (hasClick === true) {
    ...
}
if (hasClick === false) {
    ...
} 
```
可以简化为
```
if (hasClick) {
    ...
}
if (!hasClick) {
    ...
}
```

### 间距

- 运算符间距。二元运算符前后使用一个空格，包括赋值运算符、逻辑运算符等。
```
var found = (values[i] === item);

if (found && (count > 10)) {
    doSomething();
}

for (i = 0; i < count; i++) {
    process(i);
}
```
- 括号间距。紧接左括号之后和紧接右括号之前不应该有空格
- 对象内间距:
    - 每个属性名后紧跟冒号，不加空格，冒号后加一个空格然后写属性值
    - 对象内要有恰当的缩进
    - 一组相关的属性前后插入一个空行（不要多）
- 空行。在上下逻辑不相关的地方插入一个空行（有且只有一个空行，不要多）

### 命名

- 驼峰式命名，首字母小写，后面单词首字符大写
- 变量名一定要有含义，避免a, b, c, x, y这样的命名，for循环中的i, j, k等变量除外，但是要写好注释

#### 常量命名
- 全大写字母加下划线，下划线用来分隔单词

#### 变量命名
- 变量名的第一个单词应当是名词（而非动词）
- 不要在变量命名中使用下划线

#### 函数命名
- 函数名的第一个单词应当是动词（而非名词）
- 不要再函数命名中使用下划线

### 注释

> 不要写无意义的注释

#### 变量/表达式注释
- 在变量上方或后面用//进行单行注释，//后应当加一个空格
- 如果在上方使用独立行注释，应当有适当的缩进，并且注释的上方应当有一个空行
- 如果在后面进行注释，//前应有两个空格的缩进

#### 方法注释(webstorm在写好的方法上方输入/**然后回车)
- 说明方法的作用或意义或内容
- 说明每个参数的类型（类型不确定可以进行相应说明）及含义
- 如果有返回值说明返回内容
```    
/**
 * 把源对象的属性扩展到目标对象
 * @param {Any} target 参数
 * @param {Any*} source 源对象。若有同名属性，则后者覆盖前者
 * @return {Any} 目标对象
 */
function extend(target, source) {
    return xxx;
}
```

#### 注释声明
- TODO 说明代码还未完成，应当包含下一步要做的事情
- HACK 说明代码实现走了一个捷径，应当包含为何使用hack，也表明该问题可能会有更好的解决办法
- XXX  说明代码是有问题的并应尽快修复
- REVIEW 说明代码任何可能的修改都需要评审

> 这些声明可以在单行注释或者多行注释中使用

示例:

```
// TODO: 我希望有更好的方式
doSOmething();

/*
 * HACK: 不得不针对IE做特殊处理，我计划后续有时间重写这部分，
 * 这些代码可能需要在1.0.60版本之前替换掉。
 */
if (document.all) {
    doSomething();
}

// REVIEW: 有更好的方法吗？
if (document.all) {
    doSomething();
}
```

### js、css、html代码分离

- 避免在html写css样式
- 尽量避免js中处理DOM，如果必要则必须写清楚注释并且把DOM操作部分独立出来，和逻辑处理分开

### 事件处理

- 应用逻辑和事件处理程序独立
- 事件处理程序进入应用逻辑之前可以对event进行必要的操作，比如阻止默认事件(event.preventDefault)或者冒泡事件(event.stopPropagation)等

### 比较

- 不要用null来比较判断是否空值
```
错误代码：
if (something === null) {
    xxx
}
```
- 使用typeof来判断基本类型
```
用法： typeof 变量
各个类型变量返回值:
字符串： "string"
数字： "number"
布尔值： "boolean"
null：  null(不是字符串)
undefined： "undefined"
```
- 用instanceof检查引用值（日期Date，错误Error等）
```
用法： value instanceof constructor

// 检测日期
if (value instanceof Date) {
    console.log(value.getFullYear());
}

// 检测正则表达式
if (value instanceof RegExp) {
    console.log(value);
}
```
> 不要用instanceof检测Object，因为所有引用值都是继承Object

- 用Array.isArray(value)检测数组
```
Array.isArray([1, 2, 3])
```
- 使用in检测对象属性是否存在
```
var object = {
    count: 0,
    related: null
}

if ("count" in object) {
    xxx
}
```

### js逻辑相关

- 逻辑运算优化! [https://zh.wikipedia.org/wiki/%E9%80%BB%E8%BE%91%E4%BB%A3%E6%95%B0](https://zh.wikipedia.org/wiki/%E9%80%BB%E8%BE%91%E4%BB%A3%E6%95%B0)
- 避免看起来很混乱的逻辑判断，如果实在需要复杂的逻辑，一定写清楚注释

