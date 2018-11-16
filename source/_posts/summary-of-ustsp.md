---
title: 高校科服平台项目总结
date: 2018-11-13 20:44:51
tags:
---

# 项目背景

这是我在读研时做的项目，项目名为「高校科技服务平台」，平台主要为高校提供人才社交、项目发布与承接这两大功能。我主要负责项目的客户端开发与项目管理：Web 浏览器端（PC 端）和移动端（IOS + Android）。

<!-- more -->

# 开发

## PC 端

### 技术栈

视图层框架选择了 React，选择了 create-react-app v1 这个脚手架生成这个项目。

路由框架选择了 React Router。

状态管理框架选择了 MobX 而不是 Redux，原因是

- 项目的规模不大，MobX 更适合
- 前者有着更平和的学习曲线
- 前者的代码量更小

使用 axios 作为项目的 HTTP 库，加上 `async-await` 语法糖的加持，异步代码写得很顺畅。

使用了阿里的 antd 组件库，更高效地进行页面开发工作，有助于快速成型。

为了解决 CSS 的模块问题，项目使用了 CSS Modules；但没有使用 CSS 预处理器，前者的上手成本更低。

为了统一项目的 JavaScript 代码风格，在与团队成员讨论之后，使用 [standard](http://www.standardjs.com) 这个代码风格作为 ESlint 的基本配置，在此之上再进行一些自定义。

### 目录结构规划

由于项目是由 create-react-app 脚手架生成，项目目录已经基本成型了，所以这里主要关注 src 目录下的目录结构规划，如下（有省略）。

```
src
├── ajax # 业务请求
├── assets # 静态资源
├── common # 公用数据，实用工具等
├── components # 页面里的组件
├── config # 生产与开发环境下的不同配置
├── store # MobX
└── views # 页面
```

其中值得一提的是 views 和 components 两个目录的结构规划。当我们提到前端工作化时，会想到[组件化以及分治思想](https://github.com/fouber/blog/issues/10)。简单来说：整个 web 应用由若干页面组成；页面由组件组成。因此，划分了 views 和 components 目录分别存放页面和组件。views 目录的结构如下（有省略）。

```
views
├── home # 首页
│   ├── Home.jsx # 首页页面
│   └── home.css # 样式文件
└── Login
    ├── Login.jsx
    └── login.css
```

components 目录下存放的是页面下的组件，如 header、表单之类的，目录下的文件夹命名按照页面进行划分，如下（有省略）：

```
components
├── common # 公共组件
│   ├── footer
│   ├── header
│   ├── project-item # 一条项目
│   ├── projects-recommendation # 项目推荐卡片
├── home
│   ├── carousel
│   ├── footer
│   ├── project # 首页项目list
│   ├── search
│   └── talent # 首页人才list
├── login
│   └── login-form
└── search
    ├── project
    ├── search-bar
    └── talent
```

如上所示，可以看见目录的组织是以页面为中心的，再将页面切成一块一块的组件，通用的组件放在 componets/common 目录下。但是后续开发中发现了该组织方法的弊端，即业务通用组件全都放在 componets/common 目录下导致目录结构臃肿，同时体现不出组件与模块之间的从属关系，在之后的移动端开发时目录做出了调整，下文再做展开。

## 移动端

### 技术栈

因为团队内没有 IOS or Android 开发者，都是前端开发者，也考虑到 App 的快速开发，最终选择了 React Native 来开发移动端。项目基于 create-react-native-app 这个脚手架生成的。

状态管理方面选择了 Redux，原因是社区中一些优秀的调试工具对 Redux 有着优秀的支持，另一方面也是想在实战方面和 MobX 进行比较。为了解决异步 action 的问题，引入了 redux-thunk 而不是 redux-saga，原因是前者的学习成本更低，加上项目本身没有复杂的异步业务场景。

路由框架选择了 react-navigation，相比于 react-native-navigation 前者是 JS-based 框架而后者是原生的实现，因此后者具有更好的性能。另一方面，考虑到开发成本，前者更适合没有原生开发经验的开发者，同时移动端本身的业务更轻量级。所以最后选择了 react-navigation。

HTTP 库依旧是选择了 axios，和 Web 端统一，当然 fetch 也不失为一种选择。

另外还引入了一些优秀的社区组件库，帮助 App 的快速开发。

 JavaScript 代码风格还是继续使用了 [standard](http://www.standardjs.com)

### 目录结构规划

由于项目是由 create-react-native-app 脚手架生成，项目目录已经基本成型了，所以这里主要关注 src 目录下的目录结构规划。这里的目录规划和 Web 端的大抵相同，区别点主要在于与 Redux 相关的目录。如下（有省略）。

```
src
├── actions # Redux Actions
├── ajax
├── components # UI通用组件
├── constants # action-types
├── img
├── reducers # Redux Reducers
├── selectors # 相当于Redux state的getter
├── store # Redux Stre
├── styles # 公用样式属性
├── utils # 实用工具集
└── views # 页面
```

为了解决之前提到的 Web 端目录结构存在的问题——components 目录过于臃肿。具体的做法是 views 目录下的页面就近维护其子组件。views 的目录如下（有省略）

```
views
├── home
│   ├── HomeScreen.js
│   └── components
│       ├── Menu.js
│       ├── ProjectCard.js
│       ├── Projects.js
│       ├── Search.js
│       └── Talents.js
├── login
│   └── LoginScreen.js
└── register
    ├── RegisterAccountScreen.js
    ├── RegisterClaimScreen.js
    ├── RegisterCompleteScreen.js
    ├── RegisterEmailScreen.js
    ├── RegisterPasswordScreen.js
    └── RegisterUserTypeScreen.js
```

可见页面下的组件从 src/components 目录下移到了 views 目录下相应模块的目录下，当然这样的组件划分仍然是遵循了分治的理念，由此解决了components 目录过于臃肿的问题。但还是有个问题没有解决——跨页面的相同业务组件的复用问题，这个问题在后面「不足之处」小节会展开。

### 开发碎碎念

在进行开发之前，其实对 RN 开发坑多有所耳闻，自己真正参与到开发时确实遇到了不少问题。好在大多数问题通过 Stack Overflow 或是 Github issues 就可以解决。

但是还是遇到过一个问题 [IOS 平台下 controlled TextInput 组件的中文输入问题](https://github.com/facebook/react-native/pull/18456)，属于 RN 框架本身的 bug，直到 v0.57 正式发布之前，要根据官方的源码修改文档修改 TextInput 组件代码来修复这个bug。

## 不足之处

两个项目都缺少测试代码的编写，在项目的预上线期间发现了不少 bug，我也在思考编写测试代码的成本是否小于修改 bug 的成本，写测试对项目健壮性的帮助不言而喻。在项目时间充裕的情况下，可以推动团队成员一起来写测试代码，这对团队本身也是一种成长。

整个项目有测试环境和生产环境。前端项目使用了 Jenkins 来做自动化部署：当 pull request 的代码通过 code review 并被 merge 到 dev branch 时，会触发 Jenkins 任务——部署前端静态资源到 nginx（位于实验室的测试环境）。在上一段有提到项目缺少测试，于是代码提交与自动化部署环节缺少测试也是一种遗憾。

由于项目时间紧迫，加上没有多余的开发人员来完成 API 文档的制定，mock server 自然也就没有了。开发过程中缺少 mock server，当前端开发速度大于后端时，这个问题会被放大。这个问题我认为还有一种解决思路，一个开发负责一个模块的前后端开发，也就是所谓的全栈，这对工程师本身的能力要求较高。

之前有提到目录划分还存在跨页面的相同业务组件复用的问题。举个例子，注册最后一步的完善信息表单与个人中心的修改信息表单，他们的 UI 层是相同的，可以复用，但是这两个表单位于不同的父页面子目录下，这时只有将该组件移动到 components 目录下，这样的话又会导致 components 目录过于臃肿。于是，再次做出调整——views 目录下的划分以页面为中心，components 目录下的划分以模块为核心，这样的话跨页面的相同业务组件就移动到了 components 目录下，从而解决了问题。

# 项目管理

主要使用了 teambition 进行项目管理，整个工作台分为三栏：待处理、进行中和已完成，和任务管理的理念挺相似的。我所要做的是制定、拆分和分配任务。

回想起来整个项目的进度管理做得不够好，没有一个系统的管理方式，或使用甘特图来解决这个问题。

以及项目没有一个 bug 管理系统，我们当时的 bug 管理的流程是这样的：开发或产品在生产环境发现了 bug 之后，在 qq 群里以 qq 消息的方式上报 bug，之后我会在 teambition 的待处理栏创建一条解决 bug 的任务，然后再将 bug 优先分配给产生 bug 的开发，最后解决并让 bug 发现者再测试。可以发现整个流程存在着一些问题，如以 qq 消息的方式上报 bug，存在着消息被忽略的问题。使用一个轻量级的 bug 管理系统或许是一个不错的方案。

# 在最后

这个项目现在已经告一段落，进入维护阶段了。通过 learn by doing 收获了挺多，对 react 技术栈、前端工程化和项目管理也有了实战上的理解。也感受到在做一个项目的过程中，只是复制粘贴，当一个 API caller，没有解决并记录自己遇到的问题，更别说站在高点思考整个架构，这么做完一个项目也只是原地踏步罢了。PS：当编程不仅仅用来谋生，也是一种生活方式，这样真好啊。