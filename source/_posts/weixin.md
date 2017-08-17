---
title: 小程序开发总结
date: 2017-08-14 17:05:03
tags:
---
## （以前的博客忘记备份了，导致换了电脑之后，以前写的博文都不见了，只能重新开始了心累）
### 最近一直在写微信小程序，想着把遇到的一些问题都梳理和总结下：

- 最诡异的还是机型的适配，为了对数据请求进行优化，我将原生的wx.request()用promise进行了封装，结果导致在iphone5s上所有数据无法进行请求。至今我还没找到问题的关键所在。


- 以前在用vue进行开发的时候，对于post请求和get请求（基于axios）我们常常不需要主动设置content-type。对于post请求将数据放在querystring上，我们只需要将 数据用 nodejs自带的querystring来进行转换，会自动将content-type 设置为 'application/x-www-form-urlencoded' for example:

``` js
vue.js
import qs from 'querystring'
import {getList} from 'servixe'
let data = qs({
  mobile: '1575717xxxx'
})
这样会将contype-type从'application/json' 自动转换成'x-www-form-urlencoded'
而在微信小程序中则必须要设置content-type 值来区分url传参还是body 还是post的url传参
```
