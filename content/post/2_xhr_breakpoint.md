---
title: "js逆向之XHR断点"
date: "2019-08-20"
draft: false
tags: [
    "go",
    "golang",
    "templates",
    "themes",
    "development",
]
topics: [
    "Development",
    "golang",
]
summary: "sss"
---

目标网站：https://nyloner.cn/proxy <br>
爬取目标：列表页的代理信息

## 确定目标数据位置
除了之前提到查看页面源码的方法之外，另一种判断数据是否是通过AJAX加载的方法是禁用页面的js，如下图所示，点击网站设置并禁用JavaScript：

![](/post/pics/2/2_1.png)

![](/post/pics/2/2_2.png)

然后刷新页面，若数据未加载出来则可以确定数据是通过AJAX加载的。在XHR请求的返回中发现数据是加密的，且请求的参数中存在加密参数：

![](/post/pics/2/2_3.png)

![](/post/pics/2/2_4.png)

## 解析加密与解密的实现方式
这时候我们可以通过添加XHR断点的方式来研究加密参数是如何生成的，在Sources选项卡中：

![](/post/pics/2/2_5.png)

接下来刷新页面，在调用栈中找到了生成加密参数的方法get_proxy_ip，同时在这个方法中也找到了解密的位置：

![](/post/pics/2/2_6.png)

加密参数的生成方法比较简明易懂，md5可以在python中调库hashlib实现，也可以在js中把md5方法找出来。在此主要介绍解密的方法。在解密的位置打上断点，进入其中找到具体的解密方法，将其复制到WebStorm中执行。

![](/post/pics/2/2_7.png)

```
function decode_str(scHZjLUh1) {
    scHZjLUh1 = Base64["\x64\x65\x63\x6f\x64\x65"](scHZjLUh1);
    var key = '\x6e\x79\x6c\x6f\x6e\x65\x72';
    var len = key["\x6c\x65\x6e\x67\x74\x68"];
    var code = '';
    for (let i = 0; i < scHZjLUh1["\x6c\x65\x6e\x67\x74\x68"]; i++) {
        var coeFYlqUm2 = i % len;
        code += window["\x53\x74\x72\x69\x6e\x67"]["\x66\x72\x6f\x6d\x43\x68\x61\x72\x43\x6f\x64\x65"](scHZjLUh1["\x63\x68\x61\x72\x43\x6f\x64\x65\x41\x74"](i) ^ key["\x63\x68\x61\x72\x43\x6f\x64\x65\x41\x74"](coeFYlqUm2))
    }
    return Base64["\x64\x65\x63\x6f\x64\x65"](code)
}
```

执行时发现报错："window is not defined". 将十六进制字符串在控制台输出，发现"\x53\x74\x72\x69\x6e\x67"代表"String"，将代码修改后执行得到解密后的数据。

![](/post/pics/2/2_8.png)

```
function decode_str(scHZjLUh1) {
    scHZjLUh1 = Base64["\x64\x65\x63\x6f\x64\x65"](scHZjLUh1);
    var key = '\x6e\x79\x6c\x6f\x6e\x65\x72';
    var len = key["\x6c\x65\x6e\x67\x74\x68"];
    var code = '';
    for (let i = 0; i < scHZjLUh1["\x6c\x65\x6e\x67\x74\x68"]; i++) {
        var coeFYlqUm2 = i % len;
        code += String["\x66\x72\x6f\x6d\x43\x68\x61\x72\x43\x6f\x64\x65"](scHZjLUh1["\x63\x68\x61\x72\x43\x6f\x64\x65\x41\x74"](i) ^ key["\x63\x68\x61\x72\x43\x6f\x64\x65\x41\x74"](coeFYlqUm2))
    }
    return Base64["\x64\x65\x63\x6f\x64\x65"](code)
}
```
![](/post/pics/2/2_9.png)

完成。