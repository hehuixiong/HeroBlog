---
title: Css实现网页png/svg等透明图片实时变色功能
date: 2021-11-21 16:00:00
cover: https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20220223175343.png
top_img: https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20220223175343.png
categories:
  - Css
tags:
  - css
---

# 前言
工作中经常会用到各种图标，我们会要求设计师ued同学放到iconfont中，但是有时候iconfont要求比较高，png或者svg图片没有问题，上传到iconfont中有时候会出现异常情况，ued同学很苦恼。iconfont的好处就是可以实时修改图标的颜色。一个图标，多处使用。那么png和svg图片能否达到iconfont的效果呢？可以随时修改png或者svg等图片图标的颜色呢？答案是有的，今天就给大家介绍一下，png或者svg如何更换颜色。

# 使用mask遮罩实现png等图标更换颜色
代码如下：
```css
<span class="changecolor"></span>

.changecolor {
  display: inline-block;
  width: 432px; height: 99px;
  background-color: #dd1b14;
  -webkit-mask:url(./test.png) no-repeat;
  mask:url(./test.png) no-repeat;
  -webkit-mask-size: 100% 100%;
  mask-size: 100% 100%;
}
```

备注，本地直接html演示会提示跨域等问题，建议在localhost服务下面运行，本地可以把png转为base64来使用。

# CSS mask遮罩介绍
CSS mask属性在使用的时候就是mask: xxx，但是现在随着这个属性的规范化，mask属性实际上已经成为了诸多mask-*的缩写，这和background, border性质是一样的。
具体是哪些属性的缩写呢，可以参见下面的列表：
```css
mask-image
mask-mode
mask-repeat
mask-position
mask-clip
mask-origin
mask-size
mask-type
mask-composite
```
我们写
```css
mask: url(./haorooms.svg) no-repeat center / 100%; 
```
实际就是 mask-image mask-repeat mask-position 的缩写。

> mask-image

mask-image 的作用就是用这个图片的不透明部分镂空显示底部的颜色。

> mask-mode

mask-mode属性的默认值是match-source，意思是根据资源的类型自动采用合适的遮罩模式。

例如，如果我们的遮罩使用的是SVG中的< defs >中的< mask >元素，则此时的mask-mode属性的计算值是luminance，表示基于亮度遮罩。如果是其他场景，则计算值是alpha，表示基于透明度遮罩。

因此，mask-mode支持下面3个属性值：
```css
mask-mode: alpha;此关键字指示应使用掩码层图像的透明度（阿尔法通道）值作为掩码值。
mask-mode: luminance;此关键字指示掩膜层图像的亮度值应用作掩码值。
mask-mode: match-source;（默认值）根据资源的类型自动采用合适的遮罩模式。
```

> mask-repeat，mask-position，mask-clip，mask-origin， mask-size

这些属性基本和background一样，可以对比记忆。

> mask-type属性

mask-type属性功能上和mask-mode类似，都是设置不同的遮罩模式。但还是有个很大的区别，那就是mask-type只能作用在SVG元素上，本质上是由SVG属性演变而来，因此，Chrome等浏览器都是支持的。但是mask-mode是一个针对所有元素的CSS3属性，Chrome等浏览器并不支持，目前仅Firefox浏览器支持。

由于只能作用在SVG元素上，因此默认值表现为SVG元素默认遮罩模式，也就是默认值是luminance，亮度遮罩模式。如果需要支持透明度遮罩模式，可以这么设置：
```css
mask-type: alpha;
```

> mask-composite属性详细介绍

mask-composite表示当同时使用多个图片进行遮罩时候的混合方式。支持属性值包括：

```css
add;遮罩累加。
subtract;遮罩相减。也就是遮罩图片重合的地方不显示。意味着遮罩图片越多，遮罩区域越小。
intersect;遮罩相交。也就是遮罩图片重合的地方才显示遮罩，。
exclude;遮罩排除。也就是后面遮罩图片重合的地方排除，当作透明处理。
```