---
title: Python+selenium针对网银控件过登录取数据
date: 2019-03-21 16:53:30
tags: [Python, 网银, selenium, 爬虫]
urlname: Python_WY_Spider
categories: 
- Python
---

## Python获取WY数据的方法总结：

- 由于WY控件加密方式相比较一般网站比较特殊，需在驱动层进行操作
- 由于WY控件的特殊性，获取交易数据的时候需要传递KEY (以CCB为例)
- 由于取数据时的问题，所有操作均应该在驱动层完成
- 要想实现实时更新数据，需要不断地登录，大部分WY都有强制退出操作

## 理清思路

登录的过程中，由于安全控件的限制，需要绕过登录限制，此处思路借鉴了 [爬虫应对银行安全控件](https://blog.csdn.net/Bone_ACE/article/details/80765299)，由此可知，需要绕过登陆限制需从驱动层入手

两种方案
- Python可以使用 Win32api 模块来模拟键盘指令，类似于按键精灵的概念
- Python使用 Photomjs 无界面浏览器配合Selenium Webdirver

尝试使用Win32api时，由于需要配合鼠标操作，需要获取句柄坐标，且开发难度较高，尝试更换另一种方式
更换 Photomjs ，模拟登录时发现 Photomjs 并没有附带安全控件，所传输的值不会自动加密，尝试更换为 ChromDriver
使用 ChromDriver 尝试模拟登陆

```python
username = browser.find_element_by_name('USERID')
username.send_keys(username)
password = browser.find_element_by_name('LOGPASS')
password.send_keys(password)
tjButton = browser.find_element_by_id('loginButton')
tjButton.click()
```
登录成功！

当越过了登录后就需要获取交易信息，交易信息这一块，CCB的查询地址附带了一个SKEY，每次查询信息的时候都需要一个SKEY验证，如果不正确将不会返回正确的结果！
如何获取SKEY，涉及到WY的信息，这里不便细说（PS：细心地同学一定可以找到）

取到SKEY后即可构造查询地址，然后使用WebDriver模拟访问
需要注意的一点是，WY的大部分数据均是用iframe嵌套的，因此需要多处过iframe

演示代码
```python
# 定位到iframe
iframe = browser.find_element_by_id("iframe")
# 切换到iframe
browser.switch_to_frame(iframe)
```
取数据这一块不多说，每个WY也都不同
获取到数据后就是数据处理了，根据系统不同，我这里直接使用Python向固定地址POST传值

取数据后为了获取实时数据，需要定时向固定地址提交数据，大部分的WY都有长时间自动登出的骚操作，对此，思路也很多

大致几个想法
- 自动刷新，保持登录状态
- 重复登录，更换SKEY，反复操作
- 模拟点击，保持登录状态

自动刷新方案在一开始就失败了，多次频繁的刷新，会导致弹出手机验证码
重复登录，由于上次的失败，设置了延时，效果还可以，只是数据总会有延迟
模拟点击，无效，仍然会自动登出

采用重复登录的方式，递归实现！

## 综上所述，我们可以将交易流程如此划分：
![WY数据.jpg](http://fulicos.sbcoder.cn/2019/03/22/5c9438269d9c1.jpg)

## 成果演示
![WY演示.jpg](http://fulicos.sbcoder.cn/2019/03/22/5c944f127c9cc.jpg)

Tips:本文仅做思路分享，切勿用在实际生产环境！