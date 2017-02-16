---
layout: post
title: "HTML中嵌入特殊字体"
date: 2017-02-16 11:01:41 +0800
comments: true
categories: 
---
通常情况下，网页是不能使用一些特殊字体的，而且设计上也不推荐。但是有时为了满足某种效果又不得不引入新的字体，虽然我们可以通过图片、Flash等技术使用特殊字体，但是这样不利于SEO搜索。最近我们也遇到了类似的问题，所以也就去研究了一下。

## @font-face

这是一个css2的属性，在css中可以通过@font-face属性来实现网页中嵌入特殊的字体。

通过情况下，为了满足所有主流浏览器的支持，我们需要获取3中字体格式的文件，但是正常我们只有一种文件比如.ttf,这个时候我们需要借助工具生成其他类型的文件。[这里](https://onlinefontconverter.com/)有一个工具可以帮助我们生成其他类型的文件，亲测可以。

@Font-face目前浏览器的兼容性： 

* Webkit/Safari(3.2+)：TrueType/OpenType TT (.ttf) 、OpenType PS (.otf)； 
* Opera (10+)： TrueType/OpenType TT (.ttf) 、 OpenType PS (.otf) 、 SVG (.svg)； 
* Internet Explorer： 自ie4开始，支持EOT格式的字体文件；ie9支持WOFF； 
* Firefox(3.5+)： TrueType/OpenType TT (.ttf)、 OpenType PS (.otf)、 WOFF (since Firefox 3.6) 
* Google Chrome：TrueType/OpenType TT (.ttf)、OpenType PS (.otf)、WOFF since version 6

## 使用@font-face

```
/*
	1 font-family: 自定义字体的名字;
	2 src: 自定义字体的存放路径;
	3 font-weight: 定义字体是否加粗;
	4 font-style: 定义字体的其他样式;
*/
@font-face {
    font-family: 'SourceHanSansSC';
    src: url('/assets/fonts/SourceHanSansSC.eot');
    src: url('/assets/fonts/SourceHanSansSC.ttf') format('truetype');
    font-weight: normal;
    font-style: normal;
}

.container {
    text-align: center;
    font-size: 25px;
    font-family: SourceHanSansSC;
}
```

## webpack配置
如果我们的项目使用webpack进行打包，这里就需要配置字体文件的loader

```
{
    test   : /\.woff/,
    loader : 'url?prefix=font/&limit=10000&mimetype=application/font-woff'
},
{
    test   : /\.ttf/,
    loader : 'file?prefix=font/'
},
{
    test   : /\.eot/,
    loader : 'file?prefix=font/'
},
{
    test   : /\.svg/,
    loader : 'file?prefix=font/'
},
```

### 备注
[demo源码](https://github.com/ucooling/es6-webpack-env/tree/load-special-font)


