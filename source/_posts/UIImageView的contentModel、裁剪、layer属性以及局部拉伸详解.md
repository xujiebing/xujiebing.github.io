---
title: UIImageView的contentModel、裁剪、layer属性以及局部拉伸详解
toc: true
date: 2019-06-22 22:46:29
tags:
- UI
categories:
- iOS
type:
---

在我们开发过程中，UIImageView是一个使用频率相对较高的控件。那么对该控件做一个全面的认识，会对我们业务开发起到十分重要的作用。这篇文章主要讲解UIImageView的contentMode属性和图片的裁剪关系，layer属性以及图片的局部拉伸。 

[Demo链接](https://github.com/xujiebing/UIImageViewDemo) 

## contentMode 

该属性是UIView的属性，表示view内容的填充样式，不同属性的值的效果可参考demo 。contentModel一共有13种填充模式，分别是： 

* UIViewContentModeScaleToFill

   这是图片显示的默认模式。图片进行非等比例缩放，直到填铺满整个View区域。所以往往造成图片的变形。也就是图片的长度上缩放一定的比例填满显示区域，在宽度上缩放一定的比例填满显示区域。

* UIViewContentModeScaleAspectFit

  这是等比例缩放，所以使用这种缩放模式的图片永远不会变形。图片按一定比例缩放，直到在长度上或者宽度上达到View的边界就停止。没有填满区域就显示View的背景。 

* UIViewContentModeScaleAspectFill

  这也是等比例缩放，图片也不会变形。这种缩放和上面的ScaleAspectFit正好相反，图片按一定比例缩放，直到最短的边达到View的边界。所以这种缩放一定会铺满View，超出View的图片你可以选择截掉或者不截掉。 

* UIViewContentModeRedraw

  重绘。该模式比较特别，它不是用来说明如何展示图片的，而是告诉视图在每次设置或者更改frame的时候自动调用drawRect：方法 

* UIViewContentModeCenter

  等比缩放，居中显示。 

* UIViewContentModeTop

  等比缩放，顶部对齐显示。 

* UIViewContentModeBottom

  等比缩放，底部对齐显示。 

* UIViewContentModeLeft

  等比缩放，左侧对齐显示。 

* UIViewContentModeRight

  等比缩放，右侧对齐显示。 

* UIViewContentModeTopLeft

  等比缩放，左上角对齐显示。 

* UIViewContentModeTopRight

  等比缩放，右上角对齐显示。 

* UIViewContentModeBottomLeft

  等比缩放，左下角对齐显示。 

* UIViewContentModeBottomRight

  等比缩放，右下角对齐显示。 

## contentModel与图片裁剪 

通过demo效果，我们发现contentModel也可以实现裁剪功能。那么普通的裁剪方法跟contentModel属性有什么区别，我们可以通过demo来观察。 

layer属性的简单介绍 

layer是UIView的一个属性，属于CALayer。CALayer可以在以后专门做一个专题来分享，这里只是对layer做个简单的介绍。 

可以通过设置UIView的CALayer实现阴影、边框、圆角和透明等效果 

CALayer没有处理事件响应的接口 

UIView主要实现UI视觉上的管理，CALayer主要实现UI内容的绘制 

## 图片的局部拉伸 

根据不同的业务需求，contentModel以及图片裁剪已经不能满足业务需求，这时候局部拉伸就能发挥作用了。 

-(UIImage *)resizableImageWithCapInsets:(UIEdgeInsets)capInsets; 

该方法是图片的局部拉伸方法。UIEdgesets是设置图片拉伸区域距离图片的顶部、左部、下部、有部的距离。例如，一个50*50像素的图片，UIEdgeInsets全部设置为5，表示对图片中间40 * 40的区域进行拉伸。效果参考demo。 

## Android的ImageView的相关属性 

scaleType属性，控制图片缩放或移动 

属性值说明 

center无缩放，按原图大小显示图片，当图片宽高大于View的宽高时，截取图片中间部分显示 

centerCrop按比例拉伸原图直至于填充满View宽高，并显示在View的中间。 

centerInside当View的宽高>=图片的宽高时，图片居中显示原大小反之将原图按比例缩放至View的宽高居中显示。 

fitCenter按比例拉伸原图直至等于View某边的宽高，且显示在View的中间 

fitStart按比例拉伸图片，且显示在View的左上边 

fitEnd按比例拉伸图片，且显示在View的右下边 

fitXY拉伸图片（不按比例）以填充View的宽高 

matrix用矩阵来绘图 

tint属性，为图片设置渲染颜色，单独设置时，会覆盖掉原有背景图片 