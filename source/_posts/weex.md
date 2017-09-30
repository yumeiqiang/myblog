---
title: weex 开发总结
date: 2017-09-30 16:09:35
tags:
---

## 总结一下weex开发中的遇到的一些问题以及解决方案
#### 初始化weex项目。如果需要添加ios和android项目。
``` bash
weex create 替代 weex init
```
### css 方面
- 文本需要用text标签包裹，否则无法渲染
- css属性不能简写。例如：margin，border，padding
- 图片不支持本地图片
- 布局不支持百分比布局，单位为px；默认为flex布局
- 页面向下滑动需要包裹在list或者scoller标签、
- 定位布局不支持层级定位z-index；标签越靠后层级越高
- image标签需要设置宽高，否则无法显示

### js方面
- class条件渲染需要用数组包裹 例如：
``` bash
 vuejs: :class="type==1?'classOne':'classTwo'"
 weex:  :class="[type==1?'classOne':'classTwo']"
```
- 控制隐藏不支持 v-show，可用v-if替代

### 与iOS和安卓交互
### 因为po主做的是cdn下发的jsbudles文件，客户端内嵌weex页面
- weex=>native的数据传递（安卓为例）
``` bash
例如客户端注册了一个modules为 weex_module以及改module下的一个getToken 方法
{
    @JSMethod
    public void getToken(JSCallback callback){
        //获取定位代码.....
        Map<String,String> data=new HashMap<>();
        data.put("token","11111");
        //通知一次
        callback.invoke(data);
        //持续通知
      //  callback.invokeAndKeepAlive(data);

        //invoke方法和invokeAndKeepAlive两个方法二选一
    }
}
```
``` bash
vuejs 调用
const value = weex.requireModule('weex_module')
export default {
  create(){
    value.getToken(function(v) {
    console.log(v.token)  // '11111'
    })
  }
}
```
- weex.config.env.platform 判断操作系统，安卓端为'android' 而不是 'Android'
- 为了保证客户端weex的流畅体验，我使用的多页面打包而不是web端流行的spa页面。页面之间跳转依赖navigator而非vue-router
