---
title: "js逆向之代码溯源"
date: "2019-08-16"
draft: false
tags: [
    "Ajax",
    "XHR",
    "断点调试",
    "base64",
]
topics: [
    "python爬虫",
    "js逆向",
]
summary: "通过一个简单的例子介绍了js逆向解析加密参数的基本思路和方法。"
---

目标网站：https://www.qimingpian.cn/finosda/project/pinvestment <br>
爬取目标：列表页的企业信息

## 确定目标数据位置
首先查看源码，发现我们需要的信息并不在源码中，那么一定是通过Ajax加载的，分析XHR请求，发现返回的数据是加密的：

![加密的返回数据](/post/pics/1/1_1.png)
## 解析解密方法
接下来就需要从js中搜索解密的方法，在开发者工具的Sources选项卡中搜索XHR请求中的加密数据变量名encrypt_data，发现只存在于一个js文件中，一共出现了6次。通过分析找到最可能是解密方法的位置，打上断点。

![](/post/pics/1/1_2.png)

刷新页面，不断按F11（Step into next function call）观察函数调用的顺序，注意每次进入下一个调用栈都要打上断点作为标记，便于后面能快速定位厘清思路。首先，找到了解密方法的主入口：

![](/post/pics/1/1_3.png)

进一步按F11，找到上图中定义s方法和a.a.decode方法的位置。

a.a.decode方法，注意该方法中的变量f和c的值是在其他位置定义的，需要通过在控制台调用console.log()获取
![](/post/pics/1/1_4.png)

s方法
![](/post/pics/1/1_5.png)

将上述代码复制到WebStorm中执行，成功得到了解密后的数据：

![](/post/pics/1/1_6.png)

## python中调用js代码
最后通过python的execjs库执行js代码，将数据解密的逻辑在python中实现。需要注意的是js代码中解密得到的是js对象（调用了JSON.parse方法），在python中运行会发生错误。解决的办法是去除JSON.parse方法，生成Buffer实例，通过base64编码，然后在python中解码。
```
function o(t) {
    return new Buffer(s("5e5062e82f15fe4ca9d24bc5", adecode(t), 0, 0, "012345677890123", 1)).toString("base64")
}
```