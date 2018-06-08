---
layout: post
title: "sublime开发Python，控制台乱码"
description: sublime开发Python，搭建Python环境，控制台乱码
date: 2018-06-07
categories: Python
tags: [Python, sublime, 开发环境, 中文乱码]
---


用sublime text 3开发Python，控制台乱码。打印英文也乱码，所以不是中文的原因，配置一下python编译环境就好
print("ok");

1.  打开 Tools -> Build System -> New Build System
在打开的文件中粘贴一下代码，注意cmd标签的python.exe的地址要换成Python安装地址
~~~python
{
	"cmd": ["C:\\Users\\Administrator\\AppData\\Local\\Programs\\Python\\Python37\\python.exe", "-u", "$file"],
    "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
    "selector": "source.python",
    "encoding": "gbk"
}
~~~
保存为py.sublime-build

2.  设置编译环境Tools -> Build System -> py
运行就通过了






