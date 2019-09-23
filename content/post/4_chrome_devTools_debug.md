---
title: "Chrome DevTools代码调试总结"
date: "2019-08-22"
draft: false
tags: [
	"Chrome",
    "调试",
]
topics: [
    "Chrome DevTools",
]
summary: "Chrome DevTools代码单步与多步调试方法的总结。"

---

示例Demo：https://googlechrome.github.io/devtools-samples/debug-js/get-started <br>
reference：https://developers.google.com/web/tools/chrome-devtools/javascript/reference

## Step over line of code
若对当前停留位置的函数执行细节不感兴趣，使用Step over执行该函数而不进入该函数。

![](/post/pics/4/4_1.png)

例子：当代码执行停留在A时，使用Step over将执行当前函数（B&C），然后DevTool将停留在D。

```
function updateHeader() {
  var day = new Date().getDay();
  var name = getName(); // A
  updateName(name); // D
}
function getName() {
  var name = app.first + ' ' + app.last; // B
  return name; // C
}
```

<br>
## Step into line of code
若对当前停留位置的函数执行细节感兴趣，使用Step into进入该函数内部。

![](/post/pics/4/4_2.png)

例子：当代码执行停留在A时，使用Step into将进入该函数，然后DevTool将停留在B。

```
function updateHeader() {
  var day = new Date().getDay();
  var name = getName(); // A
  updateName(name);
}
function getName() {
  var name = app.first + ' ' + app.last; // B
  return name;
}
```

<br>
## Step out of line of code
若对当前函数剩余的内容不感兴趣，使用Step out将直接执行该函数的剩余内容并停留在下一个函数。

![](/post/pics/4/4_3.png)

例子：当代码执行停留在A时，使用Step out将执行B，然后DevTool将停留在C。

```
function updateHeader() {
  var day = new Date().getDay();
  var name = getName();
  updateName(name); // C
}
function getName() {
  var name = app.first + ' ' + app.last; // A
  return name; // B
}
```
<br>
## Run all code up to a certain line
若想快速跳过大量代码行，在下一个想要暂停的位置右键，点击Continue to Here，DevTools将会执行中间的所有代码并停在该处。

![](/post/pics/4/4_4.png)

<br>
## Resume script execution
继续执行代码，直到下一个断点暂停（若有下一个断点）。

![](/post/pics/4/4_5.png)

<br>
## Force script execution
继续执行代码，忽略之后的所有断点。

![](/post/pics/4/4_6.png)