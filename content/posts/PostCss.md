+++
title = "PostCss"
date = 2019-03-14T16:53:19+08:00
tags = ["tools"]
categories = ["tools"]
draft = false
description = 'css'
+++

# PostCSS

# 为什么要使用PostCss

css规范在浏览器兼容性方面一直存在各种各样的问题，不同浏览器在css规范的实现方面的进度也存在很大差异。

## PostCSS介绍

PostCSS 是一个允许使用 JS 插件转换样式的工具。

- 检查（lint）你的 CSS
- 支持 CSS Variables 和 Mixins
- 编译尚未被浏览器广泛支持的先进的 CSS 语法
- 内联图片

## 使用方法

1. 在你的构建工具中查找并添加PostCSS扩展
2. 选择插件并将他们添加到你的PostCSS处理队列中

```
在 webpack.config.js 里使用 postcss-loader :
{
        loader: 'postcss-loader'
 }
```

- 创建postcss.config.js

  ```
  module.exports = {
      plugins: [
          require('precss'),
          require('autoprefixer')
      ]
  }
  ```

  ​

## 提前使用先进的CSS特性

### autoprefixer 添加了vender浏览器前缀。

Autoprefixer是postCSS的一个插件用来转换css并且根据css规则填充属性前缀。

