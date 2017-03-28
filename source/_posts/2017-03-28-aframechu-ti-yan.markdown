---
layout: post
title: "Aframe初体验"
date: 2017-03-28 14:29:33 +0800
comments: true
categories: 
---
###为什么会有这篇博文
最近一直在研究图片的全景展示，看了很多实现，基本上都是基于Three.js实现的，Aframe也不例外。但是Aframe相对于其他的框架有一个好的地方是它对WebVR做了一个很好的封装。以及对于熟悉HTML的web开发人员来说，入门成本基本上可以小到忽略不计，本片文章，会介绍如何从零到一的来实现一个全景看房的实例，在开篇之前稍微再对Aframe做一个简单的介绍。

###什么是Aframe
[Aframe](https://aframe.io/)是一个开源框架，用于使用自定义的HTML元素创建WebVR体验，可用于在PC浏览器，只能手机，以及头戴式VR设备上建立虚拟场景。Aframe可以让你轻松的实现一个WebVR场景。

###实例介绍
本文将实现一个全景开放的例子，用户可以看不同的房间。可以使用VR设备进行体验，如果你们发现有什么不对的地方欢迎留言，或者有更好的方法欢迎交流。

###实现
* 基本结构，引入aframe.js

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>VR全景看房</title>
    <meta name="description" content="VR全景看房">
    <script src="https://aframe.io/releases/0.3.2/aframe.min.js"></script>
  </head>
  <body id="main"></body>
</html>
```

* 初始化场景(下面的代码都是写在body体中)

```
<a-scene>
  <a-assets>
    <img id="quanjing1" src="pic/quanjing1.jpg">
	<img id="arrow" src="pic/arrow.png">
	<img id="quanjing2" src="pic/quanjing2.jpg">
  </a-assets>
</a-scene>
```
对于Aframe所有的元素必须加入到场景中，类似于Three.js，在渲染一个场景时，必须实例化三个实例（Scene、Camera、Renderer）。`<a-assets></a-assets>`元素根据我的理解是预加载场景中会用到的实例，这个和Phaser.js比较相似。这里定义了两个图片和一个箭头

* 初始化背景

```
<a-scene>
  <a-assets>
    <img id="quanjing1" src="pic/quanjing1.jpg">
	<img id="arrow" src="pic/arrow.png">
	<img id="quanjing2" src="pic/quanjing2.jpg">
  </a-assets>
  <a-sky id="room1" src="#quanjing1"></a-sky>
  <a-sky id="room2" src="#quanjing2"></a-sky>
</a-scene>
```
这里的背景其实就是我们要展示的全景，我们把两个全景图都定义了，这样会在JS中来切换两个背景，下文会说，背景可以是颜色，一副全景图像，或者也可以使一个360度的视频。

* 添加箭头

```
<a-scene>
  <a-assets>
    <img id="quanjing1" src="pic/quanjing1.jpg">
	<img id="arrow" src="pic/arrow.png">
	<img id="quanjing2" src="pic/quanjing2.jpg">
  </a-assets>
  <a-sky id="room1" src="#quanjing1"></a-sky>
  <a-sky id="room2" src="#quanjing2"></a-sky>
 
  <a-curvedimage id="arrow1" src="#arrow" theta-length="8" height=".18" rotation="0 180 0" scale="0.5 0.5 0.5">
		<a-animation attribute="position"  to=".2 0 0" repeat="indefinite" dur="3000" direction="alternate"></a-animation>
  </a-curvedimage>
</a-scene>
```
这里为箭头添加了一个动画，在Aframe中，如果要为一个实例添加动画，需要使用`<a-animation></a-animation>`元素

* 添加光标

```
<a-scene>
  <a-assets>
    <img id="quanjing1" src="pic/quanjing1.jpg">
	<img id="arrow" src="pic/arrow.png">
	<img id="quanjing2" src="pic/quanjing2.jpg">
  </a-assets>
  <a-sky id="room1" src="#quanjing1"></a-sky>
  <a-sky id="room2" src="#quanjing2"></a-sky>
 
  <a-curvedimage id="arrow1" src="#arrow" theta-length="8" height=".18" rotation="0 180 0" scale="0.5 0.5 0.5">
		<a-animation attribute="position"  to=".2 0 0" repeat="indefinite" dur="3000" direction="alternate"></a-animation>
  </a-curvedimage>
  <a-entity>
      <a-entity camera look-controls wasd-controls>
          <a-entity cursor id="cursor"
                    geometry="primitive: ring; radiusOuter: 0.015;
                              radiusInner: 0.01; segmentsTheta: 32"
                    material="color: white; shader: flat"
                    position="0 0 -0.75">
          </a-entity>
      </a-entity>
  </a-entity>

</a-scene>
```
在场景中定义一个鼠标，鼠标必须用`<a-entity></a-entity>`包起来才能生效。

* 切换场景

```
<script>
var room1 = document.querySelector('#room1');
var room2 = document.querySelector('#room2');
var arrow = document.querySelector('#arrow1');
var cursor = document.querySelector('#cursor');

arrow.addEventListener('click', function (evt) {
	var time=300;
	var i = time;
	var j = 0;
	function fadeOut(picTarget)
	{   
		room1.setAttribute('src',picTarget);
		room2.setAttribute('opacity',(1/time)*i);
		room1.setAttribute('opacity',(1/time)*j);
		j++;i--;
		if(i>0) setTimeout(function(){fadeOut(picTarget)},1);
		else setTimeout(skyFront.setAttribute('src',picTarget),time);
	}
	if(room2.getAttribute('src')=='#floor'){
		setTimeout(fadeOut('#room2'),1000);
	}else{
		setTimeout(fadeOut('#room1'),1000);
	}	
});
</script>
```
###最后

这样一个VR看房的简单Demo就已经完成了，这个实现我没有传到github上，其实这里基本已经实现了，感兴趣可以自己实际试一下。如果想了解更多的关于Aframe的知识，以及自定义标签，可以去官网查询。
