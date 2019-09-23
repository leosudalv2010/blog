---
title: "Chrome DevTools跟踪变量值"
date: "2019-08-26"
draft: false
tags: [
    "变量",
]
topics: [
    "Chrome DevTools",
]
summary: "Chrome DevTools跟踪变量值的三种方法。"

---

示例Demo：https://googlechrome.github.io/devtools-samples/debug-js/get-started <br>
reference：https://developers.google.com/web/tools/chrome-devtools/javascript/

## The Scope Pane
当代码停留在断点处时，Scope窗格会显示函数内的变量值以及全局变量值，双击变量可以修改变量值。

![](/post/pics/6/6_1.png)

<br>
## Watch Expressions
Watch窗格用来监测变脸值的变化，除此之外，还可以在Wathc窗格添加javascript表达式，例如下图中的typeof sum。

![](/post/pics/6/6_2.png)

<br>
## The Console 

在Console窗格中直接输入javascript表达式进行测试。

![](/post/pics/6/6_3.png)