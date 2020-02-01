---
title: "[译]顶级JavaScript VSCode扩展，可加快开发速度\U0001F525"
date: 2019-03-17 14:01:05
tags: [javascript,vscode]
comments: true
categories:
  - javascript
---
>原文链接：[Top JavaScript Libraries & Tech to Learn in 2018](https://medium.com/javascript-scene/top-javascript-libraries-tech-to-learn-in-2018-c38028e028e6) by Eric Elliott

VSCode是一个开源的，跨平台的编辑器，已经成为程序员的最爱，尤其是在Web开发社区中。它快速，可扩展，可定制，并具有大量功能。如果尚未完成，则应检查出来。
VSCode已进行了成千上万的扩展。我将列出一些我日常使用的扩展名。让我们开始吧！

# Quokka.js
[Quokka.js](https://quokkajs.com/)是JavaScript和TypeScript的快速原型开发场。这意味着它会在您键入时立即运行您的代码，并在代码编辑器中显示各种执行结果。自己尝试。
安装此扩展程序后，可以按Ctrl / Cmd（⌘）+ Shift + P以显示编辑器的命令选项板，然后键入Quokka以查看可用命令的列表。选择并运行“ 新建JavaScript文件”命令。您也可以按（⌘+ K + J）直接打开文件。您在此文件中键入的任何内容都会立即执行。
{% asset_img quokka.gif Quokka %}

## 类似的插件
- [Code Runner](https://marketplace.visualstudio.com/items?itemName=formulahendry.code-runner)-支持多种语言，例如**C，C ++，Java，JavaScript，PHP，Python，Perl，Perl 6**等。
- [Runner](https://marketplace.visualstudio.com/items?itemName=mattn.Runner)

# 大括号着色器(Bracket Pair Colorizer)和缩进彩虹(indent-rainbow)🌈
括号和括号是许多编程语言不可分割的一部分。在JavaScript之类的语言中，单个页面可能会碰到这些字符，而没有简单的机制来识别开头和结尾对。欢迎[支架对着色剂Bracket Pair Colorizer](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer)和[缩进彩虹indent-rainbow](https://marketplace.visualstudio.com/items?itemName=oderwat.indent-rainbow)。这是两个不同的扩展。但是，他们确实是美好的一对。这些扩展将使您的编辑器充满各种色彩，并使代码块易于辨认并令人赏心悦目。一旦习惯了它们，VSCode就会变得平淡无奇，而没有它们的话就会变得乏味。
{% asset_img rainbow1.png 没有缩进彩虹和括号对着色器 %}

{% asset_img rainbow2.png 🌈🌈带有缩进彩虹和支架对着色器🌈🌈 %}

# 片段 Snippets
Snippets是编辑器中快速生成的代码片段。因此```import React from 'react';```，您无需输入，而是可以键入imr并按Tab键以扩展此代码段。同样，clg变为```console.log```。
对于不同的语言框架，存在很多片段： Javascript（或任何其他语言），React，Redux，Angular，Vue，Jest。我个人发现Javascript代码段非常有用，因为我主要使用JS。
## 一些还不错的snippte扩展是——
-[JavaScript (ES6) code snippets](https://marketplace.visualstudio.com/items?itemName=xabikos.JavaScriptSnippets)
-[React-Native/React/Redux snippets for es6/es7](https://marketplace.visualstudio.com/items?itemName=EQuimper.react-native-react-redux)
-[React Standard Style code snippets](https://marketplace.visualstudio.com/items?itemName=TimonVS.ReactSnippetsStandard)


# 待办事项 Todo Highlighter
它以明亮的颜色突出显示您的**TODO / FIXME**或代码中的任何其他注释，因此始终清晰可见。一个不错的功能是```List Highlighted annotations```。它列出了输出控制台中的所有TODO。
{% asset_img todo.png Todo Highlighter %}
## 类似的功能插件
- [Todo+](https://marketplace.visualstudio.com/items?itemName=fabiospampinato.vscode-todo-plus) 比Todo Highlighter更强大，具有更多功能。
- [Todo Parser](https://marketplace.visualstudio.com/items?itemName=minhthai.vscode-todo-parser)

# Import Cost
[此扩展名](https://marketplace.visualstudio.com/items?itemName=wix.vscode-import-cost)使您可以查看导入的模块的大小。这对Webpack等捆绑软件有很大的帮助。您可以查看是要导入整个库还是仅导入特定的实用程序。

*存在的一个问题是它没有显示自定义文件或模块的成本。*
{% asset_img import_cost.gif Import Cost %}

# REST客户端

作为Web开发人员，我们经常需要使用REST API。为了测试URL并查看响应，使用了诸如Postman之类的工具。但是，当您的编辑器可以轻松完成相同任务时，为什么还要使用其他应用程序。欢迎[REST客户端](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)。它允许您发送HTTP请求并直接在Visual Studio Code中查看响应。
{% asset_img rest_client.gif REST客户端 %}

# 自动关闭标签和自动重命名标签 Auto Close Tag and Auto Rename Tag

自从React在最近几年问世以来，它的吸引力不断增加，以JSX形式出现的类似HTML的语法如今风靡一时。我们再次必须使用标签 JavaScript 进行编码。任何Web开发人员都会告诉您，键入标签很麻烦。在大多数情况下，我们需要一种能够快速轻松地生成标签及其子元素的工具。[Emmet](https://emmet.io/)是VSCode已内置的一个很好的例子。但是，有时候，您只是想要一些简单明了的东西。例如自动标签关闭器，当您键入开头对时，它会生成标签的结尾对。当您更改同一标签时，结束标签也会自动更改。这两个扩展就是这样做的。
它还可以与JSX和许多其他语言一起使用，例如XML，PHP，Vue，JavaScript，TypeScript，TSX。
在此处获取它们- [自动关闭标签Auto Close Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag)和[自动重命名标签Auto Rename Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag)
{% asset_img Auto_Rename_Tag.gif Auto Rename Tag %}
{% asset_img Auto_Close_Tag.gif Auto Close Tag %}

## 类似扩展插件-
- [Auto Complete Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-complete-tag) — 结合了自动重命名和自动关闭标签的功能
- [Close HTML/XML tag](https://marketplace.visualstudio.com/items?itemName=Compulim.compulim-vscode-closetag)

# GitLens
正如其作者所言，GitLens增强了Visual Studio Code中内置的Git功能。它具有许多功能，例如通过代码镜头显示的代码作者身份，提交搜索，历史记录和GitLens资源管理器。您可以在[此处](https://github.com/eamodio/vscode-gitlens)阅读这些功能的完整说明。只需说如果您使用git做任何工作就应该安装此插件。
如果觉得GitLens很重或者您不使用它的许多功能，下面提供了一些其他的扩展，这些扩展注于特定的功能。
## 类似的扩展-
- [Git History](https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory) — 显示提交历史记录的精美图形等等。推荐。
- [Git Blame](https://marketplace.visualstudio.com/items?itemName=waderyan.gitblame) — 它使您可以在状态栏中查看当前选定行的Git Blame信息。GitLens也提供了类似的功能。
- [Git Indicators](https://marketplace.visualstudio.com/items?itemName=lamartire.git-indicators) — 它使您可以查看受影响的文件以及状态栏中已添加或删除了多少行。
- [Open in GitHub / Bitbucket / Gitlab / VisualStudio.com](https://marketplace.visualstudio.com/items?itemName=ziyasal.vscode-open-in-github) ! —它使您可以通过单个命令在浏览器中打开存储库。

# 标识器 Indenticator
它在视觉上突出显示当前的缩进深度。因此，现在您可以轻松区分以不同级别缩进的各种块。在[这里](https://marketplace.visualstudio.com/items?itemName=SirTori.indenticator)得到它。
{% asset_img Indenticator.gif Indenticator%}

## 类似的扩展-
- [Guides](https://marketplace.visualstudio.com/items?itemName=spywhere.guides)

# VSCode Icons
图标，使您的编辑更具吸引力！
{% asset_img Icons.gif Icons%}
## 类似的扩展-
- [VSCode Great Icons](https://marketplace.visualstudio.com/items?itemName=emmanuelbeziat.vscode-great-icons)
- [Studio Icons](https://marketplace.visualstudio.com/items?itemName=jtlowe.vscode-icon-theme)

# Dracula (主题)

我喜欢的一个[主题](https://marketplace.visualstudio.com/items?itemName=dracula-theme.theme-dracula)。

# 其他-
- [Prettier for VSCode](https://github.com/prettier/prettier-vscode) — 代码格式化。

- [Multiple clipboards for VSCode](https://marketplace.visualstudio.com/items?itemName=slevesque.vscode-multiclip) — 覆盖常规的“复制”和“剪切”命令，以将选择保留在剪贴板中。还增加了将多个文本块复制到单个复制缓冲区中的功能。
