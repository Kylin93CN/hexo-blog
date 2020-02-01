---
title: '[译]Google JavaScript值得注意的样式指南'
date: 2019-02-22 21:16:14
tags: javascript
comments: true
categories:
  - [javascript]
---

>原文链接：[13 Noteworthy Points from Google’s JavaScript Style Guide](https://medium.com/free-code-camp/google-publishes-a-javascript-style-guide-here-are-some-key-lessons-1810b8ad050b) by Daniel Simmons

注：本文在译文基础上，会做部分修改。

# 引言

谷歌提供的[JS代码样式](https://google.github.io/styleguide/jsguide.html)，该指南提供了（看起来像Google认为的）编写清晰易懂的代码的最佳样式实践。
Airbnb也有类似的[样式指南](https://github.com/airbnb/javascript)。

以下是我认为是Google的JS样式指南中最有趣和最相关的规则中的一些。

# 正文

## 使用空格，而不是制表符

>除了行终止符序列之外，ASCII水平空格字符（0x20）是唯一出现在源文件中任何地方的空格字符。这意味着…制表符不用于缩进。

使用2个空格作为缩进

```
// 不推荐
function foo() {
∙∙∙∙let name;
}

// 不推荐
function bar() {
∙let name;
}

// 推荐 ✅
function baz() {
∙∙let name;
}
  ```

## 需要分号

>每个语句必须以分号结尾。禁止依靠自动分号插入。

尽管我无法想象为什么有人会反对这个想法，但是在JS中对分号的一致使用正成为新的“空格与制表符”辩论。**Google**在捍卫分号方面坚定地走在这里。

```
// 不推荐
let luke = {}
let leia = {}
[luke, leia].forEach(jedi => jedi.father = 'vader')

// 推荐 ✅
let luke = {};
let leia = {};
[luke, leia].forEach((jedi) => {
  jedi.father = 'vader';
});
```

## 不建议水平对齐
水平对齐是在代码中添加可变数量的其他空格的一种做法，以使某些标记直接出现在前几行中某些其他标记的下方。
```
// 不推荐
{
  tiny:   42,  
  longer: 435, 
};
// 推荐 ✅
{
  tiny: 42, 
  longer: 435,
};
```

## 不再使用var

> 用const或声明所有局部变量let。默认情况下使用const，除非需要重新分配变量。不再使用var关键字。

```
// 不推荐
var example = 42;

// 推荐 ✅
let example = 42;
example = 11;

const arr = [];
```

## 首选箭头函数
> 箭头函数提供了简洁的语法并解决了this指向的难题。优先使用箭头函数而不是function关键字，尤其是对于嵌套功能。

```
// 不推荐
[1, 2, 3].map(function (x) {
  const y = x + 1;
  return x * y;
});

// 推荐 ✅
[1, 2, 3].map(x => {
  const y = x + 1;
  return x * y;
});
```
## 使用字符串模板代替字符串拼接
```
// 不推荐
function sayHi(name) {
  return 'How are you, ' + name + '?';
}

// 不推荐
function sayHi(name) {
  return ['How are you, ', name, '?'].join();
}

// 不推荐
function sayHi(name) {
  return `How are you, ${ name }?`;
}

// 推荐 ✅
function sayHi(name) {
  return `How are you, ${name}?`;
}
```

## 不要对长字符串使用换行符
>请勿在普通或模板字符串文字中使用换行符（即，在字符串文字中的行以反斜杠结束）。即使ES5允许这样做，但如果在斜杠后出现任何尾随空格，则可能会导致棘手的错误，并且对读者而言不太明显。

有趣的是，这是**Google**和**Airbnb**意见不一致的一条规则（这是Airbnb的[规格](https://github.com/airbnb/javascript#strings--line-length)）。
虽然Google建议串联较长的字符串（如下所示），但Airbnb的样式指南建议实质上不做任何事情，并允许长字符串在需要时继续使用。

```
// 不推荐 在移动设备上无法很好的显示
const longString = 'This is a very long string that \
    far exceeds the 80 column limit. It unfortunately \
    contains long stretches of spaces due to how the \
    continued lines are indented.';
// 推荐 ✅
const longString = 'This is a very long string that ' + 
    'far exceeds the 80 column limit. It does not contain ' + 
    'long stretches of spaces since the concatenated ' +
    'strings are cleaner.';
```
## “ for…of”是“ for loop”的首选类型
>ES6中，有三种不同的for循环。所有都可以使用，但在可能的情况下，for- of循环应首选。

如果您问我，这很奇怪，但是我想我将其包括在内，因为Google声明了首选的for循环类型非常有趣。
我总是觉得 for... in 循环更适合对象，而 for... of 更适合数组。 一种“适合正确工作的正确工具”类型的情况。
尽管**Google**的规范不一定与该思想相抵触，但有趣的是，他们特别喜欢此循环。

## 不适用eval()
>不要使用eval或Function(...string)构造函数（代码加载器除外）。这些功能具有潜在的危险，根本无法在CSP环境中使用。

[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval)页中的eval()甚至有一段名为“Never use eval()”
```
// 不推荐
let obj = { a: 20, b: 30 };
let propName = getPropName();  // returns "a" or "b"
eval( 'var result = obj.' + propName );

// 推荐 ✅
let obj = { a: 20, b: 30 };
let propName = getPropName();  // returns "a" or "b"
let result = obj[ propName ];  //  obj[ "a" ] is the same as obj.a
```
## 常量命令全部为大写字母
>常量名称使用CONSTANT_CASE：所有大写字母，单词之间用下划线分隔。

```
// 不推荐
const number = 5;
// 推荐 ✅
const NUMBER = 5;
```

## 每次声明一个变量

```
// 不推荐
let a = 1, b = 2, c = 3;
// 推荐 ✅
let a = 1;
let b = 2;
let c = 3;
```

## 使用单引号，而不是双引号

>普通字符串文字用单引号（'）而不是双引号（"）分隔。
提示：如果字符串包含单引号字符，请考虑使用模板字符串，以避免不得不对引号进行转义。

```
// 不推荐
let directive = "No identification of self or mission."
// 不推荐
let saying = 'Say it ain\u0027t so.';
// 推荐 ✅
let directive = 'No identification of self or mission.';
// 推荐 ✅
let saying = `Say it ain't so`;
```

# 最后的笔记
这些不是强制性的。**Google**只是众多科技巨头之一，而这些只是推荐。
也就是说，看看像**Google**这样的公司提出的样式建议很有趣，该公司雇用了许多才华横溢的人，他们花费大量时间编写出色的代码。
如果您想遵循“符合**Google**要求的源代码”的准则，则可以遵循这些规则-但是，当然，很多人对此表示反对，您可以随意将其中的一部分或全部清除掉。
我个人认为，在很多情况下，**Airbnb**的规格比**Google**更具吸引力。无论您对这些特定规则采取何种立场，编写任何类型的代码时都要牢记**样式的一致性仍然很重要**。
