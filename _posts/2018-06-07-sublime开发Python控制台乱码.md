---
layout: post
title: "sublime����Python������̨����"
description: sublime����Python���Python����������̨����
date: 2018-06-07
categories: Python
tags: [Python, sublime, ��������, ��������]
---


��sublime text 3����Python������̨���롣��ӡӢ��Ҳ���룬���Բ������ĵ�ԭ������һ��python���뻷���ͺ�
print("ok");

1.  �� Tools -> Build System -> New Build System
�ڴ򿪵��ļ���ճ��һ�´��룬ע��cmd��ǩ��python.exe�ĵ�ַҪ����Python��װ��ַ
~~~python
{
	"cmd": ["C:\\Users\\Administrator\\AppData\\Local\\Programs\\Python\\Python37\\python.exe", "-u", "$file"],
    "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
    "selector": "source.python",
    "encoding": "gbk"
}
~~~
����Ϊpy.sublime-build

2.  ���ñ��뻷��Tools -> Build System -> py
���о�ͨ����






