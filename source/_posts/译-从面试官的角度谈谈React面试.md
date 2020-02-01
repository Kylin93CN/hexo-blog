---
title: '[译]从面试官的角度谈谈React面试'
date: 2019-04-08 21:15:34
tags: [react]
comments: true
categories:
  - react
---
>原文链接：[A React job interview — recruiter perspective.](https://medium.com/@baphemot/a-react-job-interview-recruiter-perspective-f1096f54dd16) by Bartosz Szczeciński

# *重要说明*
*本文并非列出面试中期望回答的问题和完整的答案。这篇文章的重点是展示我提出的问题，我在寻找的答案以及为什么没有不好的答案。
如果您想查看“ 2018年最佳**React**面试问题”，请查看[https://github.com/sudheerj/reactjs-interview-questions](https://github.com/sudheerj/reactjs-interview-questions)*

我工作的一部分就是所谓的“技术面试”，评估申请“React前端开发人员”这个岗位的候选人。

如果您曾经用Google搜索过“React面试题”（或任何其他“技术面试题”），则可能会看到数不胜数“十大React面试题”的结果，这些结果要么过时，要么就是“state和props区别是什么？”或“虚拟dom使什么？”这种问题的变种。

了解这些问题的答案 *不应成为* 面试官决定是否雇用的依据。这是应聘者在日常工作中需要了解，理解和实施的东西。作为候选人，如果您是被问到这些问题，要么您面试的人没有技术背景（人力资源人员或“猎头”），要么他们认为这是一种形式。

面试不应该浪费时间。通过面试，面试官应该对候选人过去的经验，过去知识和发展机会有所了解。候选人应了解您的项目组和项目（如果可能），并获得有关其表现与您的期望的反馈。在工作面试中应该没有不好的答案（除非问题严格是技术问题）—答案应该使您对人的思考过程有深刻的了解。
本文是从进行采访的人的角度写的！

# 让我们互相认识
在许多情况下，采访将通过Skype或其他语音（或语音+视频）通信平台进行。

## 您能告诉我一些关于您以前的工作的信息吗，您是如何适应团队的？您的职责是什么？

了解此人在以前的工作地点（如果允许分享）是一个很好的起点。这使您对以前的工作经历有了一些基本的想法：软技能（“我是…的唯一开发人员”，“我和我的同事……”，“我管理的团队由6名开发人员……”）和硬技能（“ …我们创建了一个可供100万人使用的应用程序”，“…我帮助优化了应用程序的渲染时间”，“…创建了数十个自动化测试”。

## React 对您的主要吸引点是什么？您为什么选择使用React发开？

我不希望您提到JSX，VDOM等-我们已经通过阅读主页上的“功能”简介获得了这一点。为什么\*您\*开始使用React？

是不是因为“简单易学，难以掌握” API（这是相当小的，当你把它同其他解决方案比较的时候）？很好-就是说，这意味着您愿意学习新事物，并随时随地学习。

是因为“工作机会”吗？好-您是一个能够适应市场的人，并且在下一个大型框架问世后的5年内不会出现任何问题。我们已经有足够的jQuery开发人员。

可以将其想象为“电梯测验”场景（您和老板一起坐在电梯里，在他到达20楼之前，需要说服他使用新技术）。我想知道的是，你知道什么样的React才能让客户和开发者受益。

# 让我们开始谈谈更多一点关于技术的问题

正如我在开头几段中提到的那样-我不会问你什么是VDOM。我们知道，但是我会问你……

## 什么是JSX？为什么我们可以用JavaScript代码编写它-浏览器如何识别它？
您知道吗-*JSX*只是**Facebook**流行的一种表示法，这要归功于*Babel / TSC*之类的工具，它使我们能够通过` React.createElement `这种更加令人愉悦的形式编写标签。
我为什么要问这个问题？我想知道您是否了解JSX的技术方面以及由此产生的所有局限性：`import React from 'react';`即使我们不使用React代码，为什么还要在文件顶部显示；为什么组件不能直接返回多个元素。

**加分题：为什么JSX中的组件名称以大写字母开头？**

回答就是React知道如何渲染Component，而不是HTML Element应该足够好。

加分题:这个规则也有例外。例如，将一个组件分配给this.component，然后执行<this.component />就可以了。

## 您可以在React中声明的两种主要组件类型是什么，以及何时使用其中一种。

有些人会认为这是关于表示容器组件的，但这更多的是关于`React.Component`和Function组件。

正确的答案应该提到生命周期方法和组件状态。

## 既然我们提到了生命周期，那么您能否引导我完成安装有状态组件的周期？什么功能以什么顺序被调用？您将在哪里向API提出数据请求？为什么？

好的，那是一个很长的问题。随意将其分成两个较小的部分。您现在正在思考“但您说过您不询问生命周期！”。我不...不在乎生命周期。我关心最近几个月生命周期中发生的*变化*。

如果答案中包含答案`componentWillMount`，则可以假定该人员使用旧版本的React，或者已经完成了一些过时的教程。两种情况都应该引起一些关注。`getDerivedStateFromProps`是您要寻找的。

加分题:提到了服务器端流程的不同之处。

关于数据获取的问题`componentDidMount`也是如此— 是您想说/听到的。

**加分题：为什么是`componentDidMount`不`constructor`呢？**

您想听到的两个原因是：“数据不会在渲染发生之前就存在” —尽管不是主要原因，但它表明您可以理解该人员对组件的处理方式。“在React Fiber中的新异步呈现……” –该人员对面试做了功课~~。

# React生态系统
开发React应用程序是该过程的一部分-还有很多其他功能：调试，测试和记录文档。
## 您将如何调试React代码中的问题？您使用了哪些工具？您将如何调查为什么不重新渲染组件？

每个人都应该熟悉诸如linter（eslint，jslint）和调试器（React Developer Tools）之类的基本工具。
使用RDT通过检查组件状态/属性是否正确设置来调试问题是一个不错的选择，提到使用开发人员工具设置断点也是一个很好的选择。

## 您使用哪些测试工具编写单元/端到端测试？什么是快照测试，它有什么好处？
在大多数情况下，测试是不得不接受的事情，迫不得已的事情。这里有很多很好的答案：karma, mocha, jasmin, jest, cypres, selenium, enzyme, react-test-library 等。最糟糕的是候选人的回答“我们没有在我以前的公司做单元测试，只有手动测试”。
快照测试部分还取决于您在项目中使用的内容。如果您觉得它没有好处，请不要问它。但是，如果这样做，则可以对UI层（生成的HTML + CSS）进行快速简便的回归测试。

# 小代码挑战
如果可能的话，我还会做一些小的代码挑战，修复/解释不超过一两分钟，例如：
```javascript
// 此示例出了什么问题，您将如何修复或改进该组件？

class App extends React.Component {  
  constructor(props) {
    super(props);
    this.state = {
      name: this.props.name || 'Anonymous'
    }
  }    
  render() {
    return (
      <p>Hello {this.state.name}</p>
    );  
  }
}
```
解决问题的方法有多种：删除状态并使用props，实现getDerivedStateFromProps或（最好是）更改为function组件。

```javascript
/**
 * 您能解释一下将功能传递给组件的所有方式之间的区别吗？
 *
 * 单击每个按钮会发生什么？
 */
class App extends React.Component {
  
  constructor() {
    super(); 
    this.name = 'MyComponent';
    
    this.handleClick2 = this.handleClick1.bind(this);
  }
  
  handleClick1() {
    alert(this.name);
  }
  
  handleClick3 = () => alert(this.name);
render() {
    return (
      <div>
        <button onClick={this.handleClick1()}>click 1</button>
        <button onClick={this.handleClick1}>click 2</button>
        <button onClick={this.handleClick2}>click 3</button>
        <button onClick={this.handleClick3}>click 4</button>
      </div>
    );
  }
}
```
这花了一点时间，因为这里有更多的代码。如果应试者回答正确，请跟进“为什么？”。为什么会click 2按其工作方式工作？

这不是一个React问题，如果有人以“因为在React中……”开头，这意味着他们并不真正了解JS事件循环。

```javascript
/**
 * 组件有什么问题，如何修复它？
 */
class App extends React.Component {
state = { search: '' }
handleChange = event => {
/**
     * This is a simple implementation of a "debounce" function,
     * which will queue an expression to be called in 250ms and
     * cancel any pending queued expressions. This way we can 
     * delay the call 250ms after the user has stoped typing.
     */
    clearTimeout(this.timeout);
    this.timeout = setTimeout(() => {
      this.setState({
        search: event.target.value
      })
    }, 250);
  }
render() {
    return (
      <div>
        <input type="text" onChange={this.handleChange} />
        {this.state.search ? <p>Search for: {this.state.search}</p> : null}
      </div>
    )
  }
}
```
好的，这需要一些解释。防抖function没有错误。预期应用程序工作的方式是，在用户输入后250毫秒更新状态，然后呈现字符串“ Search for：…”。

**这里的问题是 event 是一个 SyntheticEvent 在React中，以及与之的互动是否延迟（例如，通过 setTimeout）将被清除，并且 .target.value 参考将不再有效。**

加分点：候选人能够解释为什么会这样。

# 结束了技术相关的问题
这应该足以使您对候选人的技术技能有所了解。您应该还有一些时间来回答更多开放性问题。

## 您过去的项目面临的最大问题是什么？您最大的成就是什么？

这可以回溯到第一个问题-答案可能因开发人员而异，并且因职位而异。一个初级开发人员会说，他最大的问题是在一个复杂的过程中引发问题，但是他们能够克服它。寻求更高职位的人将说明他们如何优化应用程序性能，而可以领导团队的人将说明他们如何通过进行配对编程来提高速度。

## 如果您的时间预算不受限制，并且可以在上一个项目中修复/改善/更改一件事，那将是什么，为什么？

另一个开放式问题，答案取决于您在候选人中寻找的内容。他们会尝试用MobX取代Redux吗？改善测试设置？写更好的文档？

# 交换角色
现在是时候更改角色了。您可能对候选人的技能和成长潜力有扎实的想法。让他问一些问题-这不仅使他可以了解更多公司和产品，他们的问题还可以为您提供一些有关他们想要发展的方向的指示。

[卡尔·维图洛（Carl Vitullo）](https://medium.com/@vcarl)写了一些很好的文章，主题是问哪些问题要问您的潜在雇主，我将带您参阅这些问题-准备回答这些问题，或者说由于NDA要求等原因而无法采取任何措施：
-  [入职和工作场所](https://medium.com/@vcarl/questions-to-ask-your-interviewer-82a26e67ce6c)
- [发展和紧急情况](https://medium.com/@vcarl/questions-to-ask-your-interviewer-development-and-emergencies-f7fbc4519e5b)
- [成长](https://medium.com/@vcarl/questions-to-ask-your-interviewer-growth-c88eed119ce2)

## 给予反馈
如果应聘者在某些问题上表现不佳或弄错了（或与您期望的有所不同），您可能需要在此时澄清这些问题。不要听起来像是在照顾这个人，只是解释一下您已经注意到的问题-提供解决方案以及他们可以用来改善的一些资源。

如果剩下的招聘程序由您决定，请告诉他们您将在X天内回去，如果不是，请告诉他们您公司的某个人会这样做。如果您知道该过程将花费超过2–3天，请告诉他们。目前，IT市场是一个很大的市场，候选人可能已经进行了多次面试-他们可能不会等您回来再接受另一份要约。

**不要忽略候选人，这实际上是人们在社交媒体上分享的主要抱怨。**









