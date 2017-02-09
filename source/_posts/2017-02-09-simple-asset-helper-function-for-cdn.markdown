---
layout: post
title: "Simple Asset Helper Function For CDN"
date: 2017-02-09 11:14:52 +0800
comments: true
categories: 
---
## 背景
通常我们为了提高网站的速度，会将站点的图片、css、以及一些库文件做CDN。对于页面中的图片的静态资源很简单，只需要修改资源的URL即可。但是对于scss文件中的图片或者svg资源怎么做CDN，很多人就不知道了，最近也是有这个需求所以自己就去探索了一下，其实也很简单。下面我们就一步一步实现。

* 定义变量
```$asset-base-path: '../assets' !default;``
可以将这个变量提到一个config文件中。这里是定义了一个资源的基地址。

* 实现方法
```
@function asset($type, $file) {
  @return url($asset-base-path + '/' + $type + '/' + $file);
}
```

* 调用
```
@function image($file) {
  @return asset('images', $file);
}
```

* 在scss中调用
```
.foo {
  background-image: image('kittens.png');
}
```

####备注
改博客只做备忘。[参考](https://css-tricks.com/snippets/sass/simple-asset-helper-functions/)


