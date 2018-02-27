---
title: ClipImage 基于canvas截图工具
date: 2018-02-27 17:38:01
comments: true

tags:
    - Canvas
categories:
     - 渣技术
---

最近重新看了下canvas的api，于是乎就撸了几个小demo。clipImage基于canvas的截图小工具 [github 地址](https://github.com/ishareme/ClipImage/)

<!-- more -->

## 主要代码

canvas实现裁剪的方法比较多，比如

#### 1. getImageData 和 putImageData

源图片画上一个canvas上，通过originCanvas.getImageData(sx,sy,sw,sh)获取到图片的image data，赋值于resultCanvas.putImageData(imagedata, dx, dy) 
[注：putImageData 7 个参数时后四个表示imagedata的想x,y,width,height,
resultCanvas.putImageData(imagedata, dx, dy, dirtyX, dirtyY, dirtyWidth， dirtyHeight)]
```js
//原图数据
var resultData = ctxOrigin.getImageData(x, y, width, height)

canvasResult.width = width
canvasResult.height = height

//结果的canvas绘上
ctxResult.putImageData(resultData, 0, 0)

```

#### 2. drawImage(CanvasImageSource, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)

使用多个参数的drawImage()方法

```js
    ctxResult.drawImage(CanvasImageSource, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight)
    
    sx,sy: 源图片的位置
    sw,sh: 要裁剪的相对于sx,sy的宽度和高度
    dx,dy: 要画在结果canvas的位置
    dw,dh: 要画在结果canvas的宽度和高度

```

#### 3. 使用clip()函数 [相对比较繁琐]


## 使用
# ClipImage

> [git](https://github.com/ishareme/clipimage)

> [npm](http://npm.meitu-int.com/#@meitu/ClipImage)

## Example

[example](http://f2er.meitu.com/hmz/imageclip/example/index.html)

## Getting Started
 
 ```shell
 npm set registry http://npm.meitu-int.com 
 ```
 
 ```shell
 npm i @meitu/clipimage --save
 ```

 ```shell
 import ImageClip from '@meitu/clipimage';
 ```
 
 
####  new ClipImage(image,options):

```sh
    image: 背景图片 type: url/HTMLImageElement/HTMLCanvasElement/HTMLVideoElement
    options:{
        // 结果图宽度 高度
        // 100 / '100%[相对于原图]' / '100px'
        width: 100,
        height: 100,
        //left top 从哪开始裁剪
        // 100 / '50%' / '100px' / 'center' / 'top' / 'left'
        left: 'center',
        top: 'center',
        //导出图片类型 'jpeg' / 'jpg' / 'png'
        type: 'jpg',
        //导出图片的质量 数值[0-1]
        quality: 1, 
        //成功时的回调
        success(b64){
        
        },
        //失败时的回调
        error(err){
        }
    }
    
    //默认值
    {
        width: 100,
        height: 100,
        left: 0, 
        top: 0,
        type: 'jpeg',
        quality: 1, 
        success(){},
        error(){},
    }
```

## Basic Usage
```sh
ClipImage


new ClipImage($originCanvas,{
    width: '200px',
    height: '200px',
    left: '100px', 
    top: '100px',
    type: 'jpg',
    quality: 1, 
    success(b64){
         $result.src = b64
    },
    error(){},
})

or

new ClipImage('http://mtapplet.meitudata.com/596c72073971d86b5128.jpg',{
    width: 100,
    height: 100,
    left: 'center', 
    top: 'center',
    type: 'png',
    quality: 0.8, 
}).then(function (b64) {
    $result.src = b64
})

```
   　　　　　　   
   　　　　　　                        
   　　　　　　       








