---
title: '[译]通过删除“ if-else”语句来清理代码'
date: 2020-02-01 15:15:53
tags: javascript
comments: true
---
>原文链接：[Clean up your code by removing ‘if-else’ statements](https://medium.com/javascript-in-plain-english/clean-up-your-code-by-removing-if-else-statements-31102fe3b083) by bitfish

在编写JS代码时，经常会遇到复杂逻辑判断的情况。通常，可以使用if / else或switch来执行多个条件判断，但是会出现问题：随着逻辑复杂性的增加，if / else和switch中的代码将变得越来越肿。本文将带您尝试编写更优雅的判断逻辑。

例如，让我们看一段代码：
```javascript
/**
 * Button click event
 * @param {number} status 
 *    Activity status: 1 in progress, 2 in failure, 3 out of stock, 4 in success, 5 system cancelled
 */
const onButtonClick = (status)=>{
  if(status == 1){
    sendLog('processing')
    jumpTo('IndexPage')
  }else if(status == 2){
    sendLog('fail')
    jumpTo('FailPage')
  }else if(status == 3){
    sendLog('fail')
    jumpTo('FailPage')
  }else if(status == 4){
    sendLog('success')
    jumpTo('SuccessPage')
  }else if(status == 5){
    sendLog('cancel')
    jumpTo('CancelPage')
  }else {
    sendLog('other')
    jumpTo('Index')
  }
}
```
您可以在代码中看到此按钮的单击逻辑：根据不同的活动状态执行两件事，发送日志隐藏点并跳转到相应的页面，您可以通过`swtich`轻松地提出对此代码的重写。
```javascript
const onButtonClick = (status)=>{
  switch (status){
    case 1:
      sendLog('processing')
      jumpTo('IndexPage')
      break
    case 2:
    case 3:
      sendLog('fail')
      jumpTo('FailPage')
      break  
    case 4:
      sendLog('success')
      jumpTo('SuccessPage')
      break
    case 5:
      sendLog('cancel')
      jumpTo('CancelPage')
      break
    default:
      sendLog('other')
      jumpTo('Index')
      break
  }
}
```

好吧，它看起来比if / else清楚得多，细心的读者可能还会发现一个小技巧：情况2和情况3的逻辑相同，我们可以保存执行语句并中断，情况2将按照情况3的逻辑自动执行。
但是有一种更简单的编写方法。
```javascript
const actions = {
  '1': ['processing','IndexPage'],
  '2': ['fail','FailPage'],
  '3': ['fail','FailPage'],
  '4': ['success','SuccessPage'],
  '5': ['cancel','CancelPage'],
  'default': ['other','Index'],
}

const onButtonClick = (status)=>{
  let action = actions[status] || actions['default'],
      logName = action[0],
      pageName = action[1]
  sendLog(logName)
  jumpTo(pageName)
}
```

上面的代码看上去确实更简洁，这种方法的聪明之处在于它使用判断条件作为对象的属性名称，并使用处理逻辑作为对象的属性值。当单击按钮时，此方法特别适用于一元条件判断的情况，该条件通过对象属性查找进行逻辑判断。
很好，但是还有另一种编码方式吗？
是!

```javascript
const actions = new Map([
  [1, ['processing','IndexPage']],
  [2, ['fail','FailPage']],
  [3, ['fail','FailPage']],
  [4, ['success','SuccessPage']],
  [5, ['cancel','CancelPage']],
  ['default', ['other','Index']]
])

const onButtonClick = (status)=>{
  let action = actions.get(status) || actions.get('default')
  sendLog(action[0])
  jumpTo(action[1])
}
```

使用Map代替Object有很多优点，我们将在后面讨论。

Map对象和普通对象之间有什么区别？

- 一个对象通常有自己的原型，所以一个对象总是有一个“原型”键，除非我们使用`Object.create(null)`创建一个没有原型的对象；
- 对象的键只能是字符串或符号，但是Map的键可以是任何值
- 您可以通过使用size属性轻松获得Map中的键/值对数量，而对象中的键/值对数量只能手动确认

现在，让我们升级问题的难度。单击按钮时，不仅需要判断状态，还需要判断用户的身份：
```javascript
/**
 * Button click event
 * @param {number} status 
 *    Activity status: 1 in progress, 2 in failure, 3 out of stock, 4 in success, 5 system cancelled
 *
 * @param {string} identity: guest, master
 */
const onButtonClick = (status,identity)=>{
  if(identity == 'guest'){
    if(status == 1){
      //do sth
    }else if(status == 2){
      //do sth
    }else if(status == 3){
      //do sth
    }else if(status == 4){
      //do sth
    }else if(status == 5){
      //do sth
    }else {
      //do sth
    }
  }else if(identity == 'master') {
    if(status == 1){
      //do sth
    }else if(status == 2){
      //do sth
    }else if(status == 3){
      //do sth
    }else if(status == 4){
      //do sth
    }else if(status == 5){
      //do sth
    }else {
      //do sth
    }
  }
}
```

从上面的示例中可以看到，当逻辑升级为双重判断时，判断增加了一倍，代码也增加了一倍。

我们如何才能更清晰地编写代码？

这是一个解决方案：
```javascript

const actions = new Map([
  ['guest_1', ()=>{/*do sth*/}],
  ['guest_2', ()=>{/*do sth*/}],
  ['guest_3', ()=>{/*do sth*/}],
  ['guest_4', ()=>{/*do sth*/}],
  ['guest_5', ()=>{/*do sth*/}],
  ['master_1', ()=>{/*do sth*/}],
  ['master_2', ()=>{/*do sth*/}],
  ['master_3', ()=>{/*do sth*/}],
  ['master_4', ()=>{/*do sth*/}],
  ['master_5', ()=>{/*do sth*/}],
  ['default', ()=>{/*do sth*/}],
])

const onButtonClick = (identity,status)=>{
  let action = actions.get(`${identity}_${status}`) || actions.get('default')
  action.call(this)
}
```

以上代码的核心逻辑是：将两个判断条件拼接成一个字符串作为Map的键，然后在查询过程中直接搜索对应字符串的值。

当然，我们也可以在这里将Map更改为Object：

```javascript
const actions = {
  'guest_1':()=>{/*do sth*/},
  'guest_2':()=>{/*do sth*/},
  //....
}
const onButtonClick = (identity,status)=>{
  let action = actions[`${identity}_${status}`] || actions['default']
  action.call(this)
}
```

如果读者发现将查询拼写为字符串有点笨拙，那么还有另一种解决方案，即使用Map对象作为键：
```javascript
const actions = new Map([
  [{identity:'guest',status:1},()=>{/*do sth*/}],
  [{identity:'guest',status:2},()=>{/*do sth*/}],
  //...
])
const onButtonClick = (identity,status)=>{
  let action = [...actions].filter(([key,value])=>(key.identity == identity && key.status == status))
  action.forEach(([key,value])=>value.call(this))
}
```

加点难度。如果在来宾情况下status1-4处理逻辑相同怎么办？

最坏的情况是：
```javascript
const actions = new Map([
  [{identity:'guest',status:1},()=>{/* functionA */}],
  [{identity:'guest',status:2},()=>{/* functionA */}],
  [{identity:'guest',status:3},()=>{/* functionA */}],
  [{identity:'guest',status:4},()=>{/* functionA */}],
  [{identity:'guest',status:5},()=>{/* functionB */}],
  //...
])
```
更好的方法是缓存处理逻辑功能：
```javascript
const actions = ()=>{
  const functionA = ()=>{/*do sth*/}
  const functionB = ()=>{/*do sth*/}
  return new Map([
    [{identity:'guest',status:1},functionA],
    [{identity:'guest',status:2},functionA],
    [{identity:'guest',status:3},functionA],
    [{identity:'guest',status:4},functionA],
    [{identity:'guest',status:5},functionB],
    //...
  ])
}

const onButtonClick = (identity,status)=>{
  let action = [...actions()].filter(([key,value])=>(key.identity == identity && key.status == status))
  action.forEach(([key,value])=>value.call(this))
}
```

这足以满足日常需求，但严重的是，将函数A覆盖四次仍然有点烦人。

如果事情变得非常复杂，例如身份具有3个状态，状态具有10个状态，则需要定义30个处理逻辑，其中许多是相同的，这似乎是不可接受的。

您可以这样做：
```javascript
const actions = ()=>{
  const functionA = ()=>{/*do sth*/}
  const functionB = ()=>{/*do sth*/}
  return new Map([
    [/^guest_[1-4]$/,functionA],
    [/^guest_5$/,functionB],
    //...
  ])
}

const onButtonClick = (identity,status)=>{
  let action = [...actions()].filter(([key,value])=>(key.test(`${identity}_${status}`)))
  action.forEach(([key,value])=>value.call(this))
}
```

<span style="background-color: rgb(233,253,240)">**使用Map而不是Object的优势更加明显，因为Regular类型可以用作键。**</span>

如果要求变为：所有来宾情况都需要发送日志掩埋点，而不同状态情况需要单独的逻辑处理，那么我们可以编写如下：
```javascript
const actions = ()=>{
  const functionA = ()=>{/*do sth*/}
  const functionB = ()=>{/*do sth*/}
  const functionC = ()=>{/*send log*/}
  return new Map([
    [/^guest_[1-4]$/,functionA],
    [/^guest_5$/,functionB],
    [/^guest_.*$/,functionC],
    //...
  ])
}

const onButtonClick = (identity,status)=>{
  let action = [...actions()].filter(([key,value])=>(key.test(`${identity}_${status}`)))
  action.forEach(([key,value])=>value.call(this))
}
```
也就是说，利用数组循环的属性，将执行任何符合常规条件的逻辑，从而可以同时执行公共逻辑和单个逻辑。

# 结论

本文教了您八种编写逻辑判断的方法，包括：
1. if/else
2. switch
3. 一元判断：存储在Object中
4. 一元判断：保存到Map
5. 多重判断：将条件连接成字符串并将其保存在Object中
6. 多重判断：将条件连接成字符串并将其存储在Map中
7. 多重判断：将条件另存为Map中的对象
8. 多重判断：将条件另存为Map中的正则表达式


对于本文而言，如此之多，也许您的未来将充满了if / else或switch。