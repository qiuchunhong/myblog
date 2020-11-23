---
title: js 字符串常用方法总结 #文章页面上的显示名称，一般是中文
categories: js #分类
tags: [js] #文章标签，可空，多标签请用格式，注意:后面有个空格
summary: 
---

- str.substr(start[, length]) 截取指定长度字符串    不推荐使用
- str.substring(indexStart[, indexEnd]) 开始索引到结束索引之间
- str.slice(beginIndex[, endIndex])
> 情景模式-参数为负数
substr() 会将第一个负参数与字符串长度相加，第二个负参数转化为 0
substring() 将所有的负参数转化为 0
slice() 会将所有的负数于字符串的长度相加

- indexOf
- lastIndexOf
- includes str.includes('@')
- toUpperCase
- toLowerCase
- split 字符串转数组 
- replace
- match str.match(regexp)
- trim 去掉两边的空格
- trimEnd || trimRight
- trimStart || trimLeft