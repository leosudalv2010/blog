---
title: "Chrome DevTools断点类型总结"
date: "2019-08-24"
draft: false
tags: [
	"Chrome",
    "断点",
]
topics: [
    "Chrome DevTools",
]
summary: "Chrome DevTools不同类型断点的使用场景与使用方法。"

---

示例Demo：https://googlechrome.github.io/devtools-samples/debug-js/get-started <br>
reference：https://developers.google.com/web/tools/chrome-devtools/javascript/breakpoints

## Line-of-code breakpoints
确切地知道在哪里暂停代码时，使用单行断点。左键点击代码行左侧序号位置即可设置单行断点，右键点击序号位置可选择设置条件断点。

![](/post/pics/5/5_1.png)

<br>
## DOM change breakpoints
使用DOM断点将会使代码停在改变该DOM节点或子节点的代码处。

![](/post/pics/5/5_2.png)

<b>Subtree modifications:</b> Triggered when a child of the currently-selected node is removed or added, or the contents of a child are changed. Not triggered on child node attribute changes, or on any changes to the currently-selected node.

<b>Attributes modifications:</b> Triggered when an attribute is added or removed on the currently-selected node, or when an attribute value changes.

<b>Node removal:</b> Triggered when the currently-selected node is removed.

<br>
## XHR/Fetch breakpoints
当XHR请求的url地址包含指定字符串时，将会在XHR请求调用send()的位置断点。

![](/post/pics/5/5_3.png)

<br>
## Event listener breakpoints
设置事件断点后，代码停在该事件触发后的事件监听代码位置。

![](/post/pics/5/5_4.png)