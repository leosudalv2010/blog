---
title: "js逆向之XHR断点2"
date: "2019-08-20"
draft: false
tags: [
    "Ajax",
    "XHR",
    "断点调试",
    "call stack",
	"cookies",
]
topics: [
    "python爬虫",
    "js逆向",
]
summary: "XHR断点调试的另一个例子，同时介绍了请求需要带cookies时如何应对。"
---

目标网站：http://fanyi.youdao.com/ <br>
爬取目标：有道翻译接口

## 确定目标数据位置
打开开发者工具，在翻译框中输入内容，发现了一个XHR请求，其中salt、sign、ts、bv四个参数看起来都是加密参数。

![](/post/pics/3/3_1.png)

## 解析加密参数的生成方式
设置XHR断点，在翻译框中输入内容，不断回溯调用栈，发现在t.translate这个函数中，已经生成了加密参数的值，如图所示。这就意味着加密参数生成的位置就在这个函数中（当前位置上方）或者需要继续回溯调用栈。

![](/post/pics/3/3_2.png)

结果在当前位置上方我们找到了生成加密参数的方法g.generateSaltSign。

![](/post/pics/3/3_3.png)

追进这个方法，我们找到了这个方法是如何定义的，下一步的关键是解析这里的md5方法。在调用md5方法的位置打上断点，刷新页面，继续追进去看（注意此时要取消XHR断点）。

![](/post/pics/3/3_4.png)

找到md5方法定义的位置，复制到WebStorm当中，采用缺啥补啥的策略补齐缺失的方法，发现缺失的方法都在md5定义位置的上方，全部复制过来，最终成功获得了加密参数的生成方法。

![](/post/pics/3/3_5.png)

## 模拟请求
在python中调用js的方法生成加密参数，带入请求体模拟post请求，发现返回结果带有错误码，没有获得翻译的结果。大胆猜测请求需要带cookies。修改程序，首先请求网站主页获得cookies，然后再请求翻译接口，最终成功获得了翻译的结果。

```
# 先访问主页获取cookie，再访问翻译接口
session = requests.session()
session.get('http://fanyi.youdao.com/', headers=headers, timeout=10)
response = session.post(url, data=data, headers=headers, timeout=10)
print(response.text)
```

![](/post/pics/3/3_6.png)

