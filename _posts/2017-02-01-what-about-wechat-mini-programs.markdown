---
layout:     post
title:      "what about wechat mini programs"
subtitle:   "knowledge points summary"
date:       2017-02-15 22:0:00
author:     "Lucas Lu"
header-img: "img/post-bg-05.jpg"
---


<h3>1.外联公共样式的方法</h3>

<p>根据微信的官方文档，是支持@import的方式一如外联的公共样式的

使用@import语句可以导入外联样式表，@import后跟需要导入的外联样式表的相对路径，用;表示语句结束。
文档地址：https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxss.html

但是在实际的开发过程中如果通过@import '../../common.wxss' 的方式引入外联的公共样式common.wxss 却是不生效的。如果是引用文件和被引用文件在相同的目录下是生效的。即@import 'common.wxss'的方式

那么怎么管理我们的公共样式呢？根据官方文档。在根目录的app.wxss中定义的样式在所有页面中都生效。

所以我们的wechat-sdk的公共样式都是在根目录的app.wxss中定义的。</p>

<h3>2.关于toast提示</h3>

<p>微信小程序的 API 默认是支持toast的。 但是wx.showToast(OBJECT)方法的弹层却是默认带有一个icon。只可以配置success和loading两种方式。在我们的项目中不实用。所以在我们的项目中使用自定义的方式。

使用方法：

在需要使用toast的wxml中添加下面一段代码(因为是fixed定位可以放在任意位置，一般放在页面最底部):</p>

<blockquote>
<xmp>
<view style="display:{{toast?'block':''}}" class="bd-toast" > {{toasttxt}} </view>

在对应的.js文件中通过import的方式引入toast方法

import { toastFn } from '../../utils/toastFn';
在需要弹层的地方直接调用就可以了

/**
 *@ _this  指向当前的page对象
 *@text toas中显示的文案
 */
toastFn(_this,[text]);
</xmp>
</blockquote>

<h3>3.使用ES6开发提升开发效率</h3>

<p>

小程序是默认支持ES6语法的，而且在上传代码的时候会自动把ES6编译为ES5。 

在第二小节中使用import 的方式引入toastFn就是使用ES6的import

那么使用它有什么好处呢？好处之一就是不用引入第三方库就可以实现代码模块化。

比如我在目录requestapi主要定义的是关于接口请求的代码块。在 utils 中定义的是功能代码块。在需要他们的地方直接通过 import的方式引入就可以直接使用。import可以把代码做很好的隔离。

</p>

<h3>4.config.js 配置文件的使用</h3>

<p>为了便于线下联调和测试。把依赖环境的项通过配置文件的方式管理起来。在需要的地方直接import来引用。如果需要上线的话只需要在这一个文件中打开相应的注释和关闭相应的注释就可以了。
<blockquote>
<xmp>
//qatestaaac测试环境域名
export let hostName='https://wappass.qatestaaac.baidu.com/';

//线上环境域名
// export let hostName='https://wappass.baidu.com/';

//产品线配置验证成功后跳转的url
export let jumpProductUrl = 'https://www.baidu.com/test';
</xmp>
</blockquote>

比如说接口的请求都是通过以下的方式来写，达到统一管理测试环境和线上环境的目的：</p>
<blockquote>
<xmp>
import {hostName}  from '../config';
wx.request({
    //接口请求
        url: hostName+'wp/api/security/checkvcode',
        data: {
            verifycode: vcode,
            codestring: _this.data.imgcode
        },
        method: 'GET',
        header: {
            'content-type': 'application/x-www-form-urlencoded'
        },
        success: function (res) {

        }
    })
</xmp>
</blockquote>
<h3>5.怎么获取表单的数据</h3>

因为小程序的开发思想是借鉴 Vue.js 这类MVVM框架的数据绑定的思想。我们是没办法直接操作所谓的“DOM”的。那我们怎么获取页面的数据或者是更新页面上的数据呢？下面介绍下数据绑定的思想

比如页面上有下面这样的一个input输入框。


<blockquote>
<xmp>
<input type="number" bindinput="phoneInput" maxlength="13" class="bd-phonenum" 
placeholder="请输入手机号（无需注册）" value="{{inputValue}}" />
Page({
    data:{
        inputValue:'input的默认值'   //wxml中会把使用{{inputValue}}的地方的值和这里绑定起来
    },
    phoneInput:function(e){
        let value = e.detail.value;//获取输入框的值
    },
    updatePageData:function(){
        //这里是去更新页面中input 中的值

        this.setData({
            inputValue:'更新的值'    //会更新页面中绑定了{{inputValue}}的节点
        })
    }
})
</xmp>
</blockquote>
<p>
从上面的代码可以了解到为什么基于数据绑定的类MVVM框架火起来的原因。前端一直再谈就是操作DOM影响页面性能。要尽量少的操作DOM。而上面的代码在没有操作的DOM的情况下就完成了获取页面上的数据和更新页面的数据。而且代码更加的简洁。
</p>