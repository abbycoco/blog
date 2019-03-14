+++
title = "Gatsby"
date = 2019-03-14T16:51:52+08:00
tags = ["Gatsby"]
categories = ["Gatsby"]
draft = false
description = 'Gatsby的使用'
+++
# Gatsby React网站快速生成器

## 安装

> npm install --global gatsby-cli

#Gatsby快速入门

## 使用Gatsby命令行

1. 创建新项目  `gatsby new gatsby-site`

2. `cd gatsby-site`

3. `gatsby develop`  Gastby 将会开启一个热加载环境，可以在本地8000端口看到

4. 在src/pages下进行编辑javascript页面。保存改动后，浏览器会实时刷新。

5. `gatsby build`  Gastby 将会执行优化的生成构建，生成静态HTML和JavaScript包文件。

6. `gatsby serve` Gastby开启一个本地HTML服务去测试你构建的网站。

   ## 目录

   * 基础 --开始新项目、开发部署网站

   * 介绍如何在Gatsby中使用css。在css模块和Typography.js 中探索。

   * 探索嵌套布局。布局是网站的一部分，可以在多个页面（如页眉和页脚）中重复使用。

   * 了解如何使用Gatsby的数据层。探索源代码和变压器插件。介绍编程页面以及如何编写GraphQL查询。在本教程的这一部分，我们将构建一个简单的降价博客。

     ​

## 基础

环境要求：Node back to v4 and npm to v3.

> import React from "react";
>
> export default () => <div style={{ color: `blue` }}>Hello Gatsby!</div>;

### 页面之间进行跳转

1.`import Link from "gatsby-link";`

2.`<Link to="/page-2/">Link</Link>`

3.src/pages/page-2.js就是你将要跳转的页面

### 部署

`gatsby build`

## 创建全局样式

每个网站都会有一些全局样式。一般会包含网站模版和背景颜色。

### Typography.js

Typography.js是一个javascript库生成typographic CSS。

取代直接为不同的HTML元素设置不同的font-size，你告诉Typography.js你想要的基础的baseFontSize和baseLineHeight，他会为你所有的元素生成基础CSS。

```
这使得更改站点上所有元素的字体大小而无需直接修改数十个CSS规则变得微不足道。
```

使用它就像下面这样：

```
import Typography from "typography";

const typography = new Typography({
  baseFontSize: "18px",
  baseLineHeight: 1.45,
  headerFontFamily: [
    "Avenir Next",
    "Helvetica Neue",
    "Segoe UI",
    "Helvetica",
    "Arial",
    "sans-serif",
  ],
  bodyFontFamily: ["Georgia", "serif"],
});
```

## 安装使用

>  npm install --save gatsby-plugin-typography
>
> module.exports = {
>   plugins: [`gatsby-plugin-typography`],
> };

### CSS Modules

> 引入.module.css
>
> 使用import styles from "./about-css-modules.module.css"
>
> className={styles.username}

## 布局

Gatsby 帮你创造一个布局组件。布局组件是你网站的一部分你可以分享在多个页面。例如，Gatsby通常会有一个共享的头部和底部。另外常见的就是侧边栏和导航栏。

在这个页面，左侧导航栏(假设你有一个大设备)和头部导航是gatsbyjs.org’s的页面组件。

让我们潜入并且探索Gatsby的布局吧。

首先，创建一个新的网站为这部分的教程。

```
gatsby new tutorial-part-three https://github.com/gatsbyjs/gatsby-starter-hello-world
```

一旦开始了命令，就是在安装gatsby-plugin-typography。为了保证Typography.js版本，让我们尝试Fairy Gates版本：

```
npm install --save gatsby-plugin-typography typography-theme-fairy-gates
```

创建一个src/utils目录，然后再创建typography模版文件在`src/utils/typography.js`:

```
import Typography from "typography";
import fairyGateTheme from "typography-theme-fairy-gates";

const typography = new Typography(fairyGateTheme);

export default typography;

```

然后把我们网站的gatsby-config.js放在网站根部，并且添加如下代码：

`module.exports = {  plugins: [    {      resolve: `gatsby-plugin-typography`,      options: {        pathToConfigModule: `src/utils/typography.js`,      },    },  ],};`

现在，让我们添加几个不同的界面：一个前端界面，一个关于界面和一个关于我们。

src/pages/index.js

import React from "react";

export default () => (
  <div>
​    <h1>Hi! I'm building a fake Gatsby site as part of a tutorial!</h1>
​    <p>
​      What do I like to do? Lots of course but definitely enjoy building
​      websites.
​    </p>
  </div>
);



`src/pages/about.js`

```
import React from "react";

export default () => (
  <div>
    <h1>About me</h1>
    <p>I’m good enough, I’m smart enough, and gosh darn it, people like me!</p>
  </div>
);

```

`src/pages/contact.js`

```
import React from "react";

export default () => (
  <div>
    <h1>I'd love to talk! Email me at the address below</h1>
    <p>
      <a href="mailto:me@example.com">me@example.com</a>
    </p>
  </div>
);

```

现在我们对于个人网站有了一个很好的开端。

但是这还有一些问题。首先，如果哪个内容在教程的屏幕正中心那就非常好了。其二，我们应该有一个全局的导航方便浏览者去观察和去浏览每个子页面。

让我们尝试解决这些问题通过创建我们的第一个页面。

### 我们的第一个布局页面

首先，在src/layouts创建一个新的目录。所有的布局组件应该在这个目录下方。

让我们创建一个最基础的布局组件在src/layouts/index.js：

```
import React from "react";

export default ({ children }) => (
  <div style={{ margin: `0 auto`, maxWidth: 650, padding: `0 1rem` }}>
    {children()}
  </div>
);

```

注意不像React其他的*children*，这个*children*传递的是一个方法并且需要被执行。

停止gatsby develop重启就会看到新的页面效果。

非常好，页面起作用了！现在我们的内容正如所期待的那样宽650px。

现在让我们增加一个相同的文件在标题文件。

```
import React from "react";

export default ({ children }) =>
  <div style={{ margin: `0 auto`, maxWidth: 650, padding: `0 1rem` }}>
    <h3>MySweetSite</h3>
    {children()}
  </div>

```

如果我们去我们的任何一个界面我们会看到相同的标题例如about页面。

[![with-title](https://www.gatsbyjs.org/static/with-title-edb105d20e6a8980efbf5313715dfb57-c4266.png)](https://www.gatsbyjs.org/static/with-title-edb105d20e6a8980efbf5313715dfb57-acb38.png)

让我们为我们的页面添加导航链接

```
import React from "react";
import Link from "gatsby-link";

const ListLink = props =>
  <li style={{ display: `inline-block`, marginRight: `1rem` }}>
    <Link to={props.to}>
      {props.children}
    </Link>
  </li>

export default ({ children }) =>
  <div style={{ margin: `0 auto`, maxWidth: 650, padding: `1.25rem 1rem` }}>
    <header style={{ marginBottom: `1.5rem` }}>
      <Link to="/" style={{ textShadow: `none`, backgroundImage: `none` }}>
        <h3 style={{ display: `inline` }}>MySweetSite</h3>
      </Link>
      <ul style={{ listStyle: `none`, float: `right` }}>
        <ListLink to="/">Home</ListLink>
        <ListLink to="/about/">About</ListLink>
        <ListLink to="/contact/">Contact</ListLink>
      </ul>
    </header>
    {children()}
  </div>

```

[![with-navigation](https://www.gatsbyjs.org/static/with-navigation-420c464b89f35cb6394f134026fe3bc1-c4266.png)](https://www.gatsbyjs.org/static/with-navigation-420c464b89f35cb6394f134026fe3bc1-acb38.png)

现在我们做到了！一个有三个页面的网站拥有一个全局的导航。

你新的布局组件你可以增加头部、尾部和全局导航，侧边栏等等。

## 第四部分

在本教程中，我们将前往新的领域，这将需要一些大脑扩展来充分理解。在接下来的两部分教程中，我们将深入到Gatsby数据层，这是Gatsby的一个强大功能，可让您轻松地从Markdown，WordPress，无头CMS和其他所有类型的数据源构建网站。

```
Gatsby的数据层由GraphQL支持。如果你是GraphQL的新手，这部分可能会感觉有些压倒性。关于GraphQL的深入教程，我们推荐如何使用GraphQL。
```

### Gatsby中的数据

一个网站拥有四部分，HTML，CSS，JS，和数据。前一半教程我们侧重在前三项，现在我们将学习如何使用数据在Gatsby网站中。

什么是数据？

一个非常科学的答案就是，数据就像“字符串”，整数、对象等等。

```
到目前为止，我们一直在编写文本并直接在组件中添加图像。这是建立许多网站的绝佳方式。但是，通常您想要将数据存储在组件外，然后根据需要将数据导入组件。
```

```
例如，如果您使用WordPress构建网站（所以其他贡献者有一个用于添加和维护内容的良好界面）和Gatsby，则该网站（网页和帖子）的数据在WordPress中，并根据需要提取数据，进入你的组件。
```

```
数据也可以生存在Markdown，CSV等文件类型以及各种数据库和API中。
```



