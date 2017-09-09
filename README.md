<p align="center">
    H
</p>
<p align="center">让http请求的调用更优雅</p>


## 概述
当我们提到java调用http请求时，我们想到的是HttpClient或是内置的HttpUrlConnention。
然后会写下如下一串的代码访问http接口：
``` bash
        HttpClient client = new HttpClient();
        client.getHostConfiguration().setProxy("127.0.0.1", 8888);
        client.getHostConfiguration().setHost("bl.ocks.org", 80, "http");
        GetMethod getMethod = new GetMethod("/mbostock/raw/4090846/us-congress-113.json");
        client.executeMethod(getMethod);
        //打印服务器返回的状态
        System.out.println(getMethod.getStatusLine().getStatusCode());
        if(getMethod.getStatusLine().getStatusCode() == 200){
            //打印结果页面
            String response = new String(getMethod.getResponseBodyAsString().getBytes("8859_1"));

            //打印返回的信息
            System.out.println(response);
        }
        getMethod.releaseConnection();
 ```
 
 可是我们是不是有一种更优雅的方式呢？类似于MyBatis，通过一定的配置，然后在调用的时候只需要执行一个接口便可以完成上述代码的工作。
 
 这便是HttpFetch的初衷，让http的调用更优雅。
 

## 下载
``` bash
git clone https://github.com/youzan/zanui-weapp.git
```

## 预览
打开[微信web开发者工具](https://mp.weixin.qq.com/debug/wxadoc/dev/devtools/download.html)，'本地小程序项目 - 添加项目'，把 zanui-weapp 添加进去就可以查看组件源码、预览示例demo了。

![](https://img.yzcdn.cn/public_files/2017/02/08/a5e6445075826183659742cc6946c477.png)

## 使用

1. 使用 [ZanUI-WeApp] 前请确保已经学习过微信官方的 [小程序简易教程] 和 [小程序框架介绍]。
2. 然后用 [Bower] 将 [ZanUI-WeApp] 添加到你的项目中使用。
3. 你也可以 fork 出一份你自己的 [ZanUI-WeApp]，这样可以获得更稳定的代码和更方便的进行个性定制。

我们推荐在你的`app.wxss`直接引入`zanui-weapp/dist/index.wxss`。

根据功能的不同，可以将组件大致的分为4类：

#### 1. 简单组件

如按钮组件，只要按照wxml结构写就好了

~~~html
<!-- example/btn/index.html -->

<view class="zan-btn">按钮</view>
~~~

![](https://img.yzcdn.cn/public_files/2017/02/08/1b1e39ed3dc6b63519a68ba1e2650cfc.png)

#### 2. 复杂组件

如加载更多组件，需要先引入定义好的模版，然后给模版传递数据

~~~html
<!-- example/loadmore/index.html -->

<!-- 引入组件模版 -->
<import src="path/to/zanui-weapp/dist/loadmore/index.wxml" />

<!-- 加载中 -->
<template is="zan-loadmore" data="{{loading: true}}" />

<!-- 一条数据都没有 -->
<template is="zan-loadmore" data="{{nodata: true}}" />

<!-- 没有更多数据了 -->
<template is="zan-loadmore" data="{{nomore: true}}" />
~~~

![](https://img.yzcdn.cn/public_files/2017/02/08/b96fdc7971577b32915604c5b2c1a3bb.png)

#### 3. 带事件回调的组件

如数量选择组件，需要先引入模版，然后给模版传递数据

~~~html
<!-- example/quantity/index.html -->

<import src="path/to/zanui-weapp/dist/quantity/index.wxml" />

<template is="zan-quantity" data="{{ ...quantity, componentId: 'customId' }}" />
~~~

然后通过`Zan.Quantity`把相关回调注入到页面中

~~~js
// example/quantity/index.js

var Zan = require('path/to/zanui-weapp/dist/index');

Page(Object.assign({}, Zan.Quantity, {
  data: {
    quantity: {
      quantity: 10,
      min: 1,
      max: 20
    },
  },

  handleZanQuantityChange(e) {
    // 如果页面有多个Quantity组件，则通过唯一componentId进行索引
    var compoenntId = e.componentId;
    var quantity = e.quantity;

    this.setData({
      'quantity.quantity': quantity
    });
  }
}));
~~~

![](https://img.yzcdn.cn/public_files/2017/02/08/b791dfef150b01a7ce1e9aa9e60e0038.png)

#### 4. API类组件

如Toast组件，需要先引入模版，并在页面上使用。

> 注意`zanToast`这个数据也是通过`Zan.Toast`注入到页面的

~~~html
<!-- example/toast/index.html -->

<import src="path/to/zanui-weapp/dist/toast/index.wxml" />

<view bindtap="showToast">显示toast</view>

<template is="zan-toast" data="{{ zanToast }}"></template>
~~~

将API注入到页面后，就可以通过`this`来直接调用相应的API了

~~~js
<!-- example/toast/index.js -->

var Zan = require('path/to/zanui-weapp/dist/index');

Page(Object.assign({}, Zan.Toast, {
  showToast() {
    this.showZanToast('toast的内容');
  }
}));

~~~

![](https://img.yzcdn.cn/public_files/2017/02/08/ada80798c88df08060ce96964384e88e.png)

更多示例可以在项目的`example`目录中查看

## 开源协议
本项目基于 [MIT](https://zh.wikipedia.org/wiki/MIT%E8%A8%B1%E5%8F%AF%E8%AD%89)协议，请自由地享受和参与开源。

## 贡献

如果你有好的意见或建议，欢迎给我们提 [issue] 或 [PR]，为优化 [ZanUI-Weapp] 贡献力量
