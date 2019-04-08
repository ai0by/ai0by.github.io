---
title: api
date: 2019-04-04 15:27:21
urlname: api_qrcode
---

## 二维码生成

### 介绍

使用场景很多，简单来讲，给我地址，我给你图，直接将地址放在img标签内即可~

### 参数说明

以GET的方式提交到：[https://api.0161.org/qrcode/qrcode.php](https://api.0161.org/qrcode/qrcode.php)

参数 | 数值类型 | 示例 | 是否必传 
:-: | :-: | :-: | :-: 
url | String | https://sbcoder.cn | 是
err | String | L (L,M,Q,H四种对应容错级别，不传默认L) | 否
size| String | 7 (可以选择1~9999之间的值，对应不同大小，默认7)|否
logo| String | https://sbcoder.cn/img/avatar.jpg | 否

返回值类型 : 直接返回图片

### 演示

直接访问以下地址

```demo
https://api.0161.org/qrcode/qrcode.php?url=https://sbcoder.cn
https://api.0161.org/qrcode/qrcode.php?url=https://sbcoder.cn&err=L&size=7&logo=https://sbcoder.cn/img/avatar.jpg
```
![示例图片](https://api.0161.org/qrcode/qrcode.php?url=https://sbcoder.cn)