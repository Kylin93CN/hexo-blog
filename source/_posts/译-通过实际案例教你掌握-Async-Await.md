---
title: '[译]通过实际案例教你掌握 Async/Await'
date: 2019-09-03 22:04:39
tags:
---
>原文链接：[How To Master Async/Await With This Real World Example](https://medium.com/free-code-camp/how-to-master-async-await-with-this-real-world-example-19107e7558ad) by Adrian Hajdin

{% asset_img es7-async-await.png ES7 — Async/Await %}
# 目录
1. 简介(callbacks，promises，async/await)
2. 实际实例——异步使用获取数据的2个api的货币转换工具

## 小提示
在撰写本文的同时，我还创建了一个YouTube视频！
您可以边观看边进行编码。我建议您先阅读本文，然后与视频一起编码。

视频链接：[Learn Async/Await in This Real World Project](https://www.youtube.com/watch?v=mlb525FgU3k)

# 介绍
Async/awai是一种编写异步代码的新方法。它建立在promise之上，因此，它也是非阻塞的。

最大的区别是异步代码的外观和行为有点像同步代码。这就是它的厉害之处。

异步代码的先前选项是**callbacks**和**promises**。

## 回调函数
```javascript
setTimeout(() => {
  console.log('This runs after 1000 milliseconds.');
}, 1000);
```

## 回调函数的问题所在——臭名昭著的地狱回调
{% asset_img cb.png ES7 — 地狱回调 %}

## 地狱回调
回调嵌套在其他几个级别的回调中的情况，这可能会使理解和维护代码变得困难。

## promises
```javascript
const promiseFunction = new Promise((resolve, reject) => {
  const add = (a, b) => a + b;
  resolve(add(2, 2));
});
promiseFunction.then((response) => {
  console.log(response);
}).catch((error) => {
  console.log(error);
});
```
*promiseFunction*这个函数返回一个表示函数运行过程的 **Promise** 对象， *resolve* 函数表示任务已经完成。

然后，我们就可以在 *promiseFunction* 函数的基础上调用 **then()** 和 **catch()** 方法。
**then**: Promise 成功后执行的回调函数。
**catch**: 当遇到了错误后执行的回调函数。

## Async 函数
Async 函数为我们提供了**简洁明了的语法**，使我们可以编写更少的代码来实现与promise相同的结果。其实底层原理上，async 不过是 promises 的语法糖而已。

异步函数是通过在函数声明之前添加单词async来创建的， 如下所示：
```javascript
const asyncFunction = async () => {
  // Code
}
```
异步函数可以使用await暂停，该关键字只能在async函数内使用。Await返回异步函数完成后返回的内容。

promise和async / await之间的区别：
```javascript
// Async/Await
const asyncGreeting = async () => 'Greetings';
// Promises
const promiseGreeting = () => new Promise(((resolve) => {
  resolve('Greetings');
}));
asyncGreeting().then(result => console.log(result));
promiseGreeting().then(result => console.log(result));
```
**Async/Await** 更易于我们理解，因为它看起来像同步代码。

前面我们已经介绍过这些基础知识了，现在我们来看看在现实开发中怎么使用！

# 货币转换器
## 项目说明&初始化
在本教程中，我们将构建一个简单但具有学习意义和有用的应用程序，它将提高您对**Async / Await**的整体了解。
这个程序会接收到我们想要把什么货币转换成什么货币，以及货币金额，然后会调用相关的 API，显示正确的汇率。
在此应用程序中，我们将从两个异步源接收数据：
1. Currency Layer  —— https://currencylayer.com —你需要先免费注册账号，才能获取 API Access Key。这个 API 为我们提供了 计算货币间汇率所需的数据。
2. Rest Countries ——http://restcountries.eu/ -这个 API 为我们提供转换后的货币在哪些国家流通的信息。

首先，创建一个新目录并运行` npm init` 初始化项目，接下里我们选择默认值，跳过所有步骤，然后再输入 `npm i --save axios` 安装 axios。在当前文件夹内创建一个 `currency-convert .js` 的文件。

在` currency-convert .js` 文件中，我们先通过 `require` 语法引入 axios,
```javascript
const axios = require('axios');
```
## 深入理解 async/await
这个程序中，我们需要**三个异步函数**，第一个函数用来获取关于货币的数据；第二个函数用来获取关于国家的数据；第三个函数用来将所有信息集中起来并展示给用户看。
## 第一个函数——异步获取有关货币的数据
我们创建一个接收两个参数（fromCurrency 和 toCurrency）的异步函数。
```javascript
const getExchangeRate = async (fromCurrency, toCurrency) => {}
```
现在我们获取数据，然后通过使用 async/await，可以直接将我们想要的数据赋值给变量。调用接口之前别忘了要注册账号，才能获得 API access key。
```javascript
const getExchangeRate = async (fromCurrency, toCurrency) => {
  const response = await axios.get('http://data.fixer.io/api/latest?    access_key=[yourAccessKey]&format=1');
}
```
我们可以通过 response.data.rates 来提取我们想要的数据，然后我们将它赋值给一个变量 rate：
```javascript
const rate = response.data.rates;
```

因为所有的数据都是从欧元转换过来的，我们可以创建一个变量 euro，它的值等于：
```javascript
const euro = 1 / rate[fromCurrency];
```
最后，我们可以用欧元乘以我们要兑换的货币来得到汇率：
```javascript
const exchangeRate = euro * rate[toCurrency];

```
最后，该函数应如下所示：
{% asset_img code.png   %}

## 第二个函数——异步获取国家数据
创建一个异步函数，接收 currencyCode 作为参数
```javascript
const getCountries = async (currencyCode) => {}
```
和之前一样，获取数据，然后将其赋值给一个变量：
```javascript
const response = await axios.get(`https://restcountries.eu/rest/v2/currency/${currencyCode}`);
```
然后通过数组 map 方法将 `country.name` 提取出来，映射为一个新的数组：
```javascript
return response.data.map(country => country.name);
```
最后，代码如下：
{% asset_img code2.png   %}

## 最后一个函数——将前面的函数组合起来
创建一个异步函数，接收 *fromCurrency, toCurrency, amount* 三个参数：
```javascript
const convert = async (fromCurrency, toCurrency, amount) => {}
```
第一步，获取货币数据：
```javascript
const exchangeRate = await getExchangeRate(fromCurrency, toCurrency);
```
第二步，获取国家数据：
```javascript
const countries = await getCountries(toCurrency);

```
第三步，将转换后的金额赋值给一个变量
```javascript
const convertedAmount = (amount * exchangeRate).toFixed(2);

```
最后，将数据输出给用户：
```javascript
return `${amount} ${fromCurrency} is worth ${convertedAmount} ${toCurrency}. You can spend these in the following countries: ${countries}`;
```
最终代码如下：

{% asset_img code3.png   %}

## 使用 try/catch 来处理错误
我们将程序的逻辑用 try 语句包裹起来，如果出现错误，用 catch 语句捕捉：
```javascript
const getExchangeRate = async (fromCurrency, toCurrency) => {
  try {
    const response = await       axios.get('http://data.fixer.io/api/latest?access_key=f68b13604ac8e570a00f7d8fe7f25e1b&format=1');
    const rate = response.data.rates;
    const euro = 1 / rate[fromCurrency];
    const exchangeRate = euro * rate[toCurrency];
    return exchangeRate;
  } catch (error) {
    throw new Error(`Unable to get currency ${fromCurrency} and  ${toCurrency}`);
  }
};
```
同样第二个函数也这样处理：
```javascript
const getCountries = async (currencyCode) => {
  try {
    const response = await axios.get(`https://restcountries.eu/rest/v2/currency/${currencyCode}`);
return response.data.map(country => country.name);
  } catch (error) {
    throw new Error(`Unable to get countries that use ${currencyCode}`);
  }
};
```
因为第三个函数只是处理第一个和第二个函数的结果，所以我们不需要对它进行错误捕获

最后，我们调用函数来接收数据：
```javascript
convertCurrency('USD', 'HRK', 20)
  .then((message) => {
    console.log(message);
  }).catch((error) => {
    console.log(error.message);
  });
```
结果如下：
{% asset_img result.png   %}

# 最后