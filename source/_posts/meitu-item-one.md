---
title: 美图项目一总结
date: 2017-10-28 21:57:01
comments: true

tags:
    - 美图
    - H5项目
categories:
     - 渣技术
---

来美图实习也一个月了~从校招之后可能有点放松过头，导致与最近感觉把学习给放一边了，前几天对校招做了个总结(详见=>[秋招面试总结](https://blog.huangmingzhao.cn/autumn-recruit-Interview.html))，所以最近要好好的收拾下心态，继续勤勤恳恳在前端路探索，接下来会对每个项目做一个总结，嘿嘿...技术很渣，看官勿喷。。

<!-- more -->

在美图的第一个小项目([请移动端模式观看哦~](http://h5.meitu.com/m8s_strut/)),一上线还是出现了挺多bug。嘿嘿。。虽然有点那个什么，大家凑合着玩哈，写项目总结是想把工作中遇到的问题和解决方法整理一下，记录记录。

## 开发中的问题

H5框架用的是公司自己的mapp框架，刚进公司的时候就看mapp的文档，有点vue的味道~~

### 1. canvas

之前并没有多大的接触到canvas，伟程师傅给我做的时候，我说不太会，好尴尬~,因为项目中的一个需求就是要在一张图片中合成字体，所以用到canvas，感觉公司h5项目都会用到canvas。上传图片到服务器之类直接调用公司的Upload组件

``` bash
var canvas = document.getElementById('js-canvas')

//getContext() 方法返回一个用于在画布上绘图的环境上下文。
var ctx = canvas.getContext('2d');

//创建一个image对象
var poster = new Image();

//设置跨域
//并不是说 canvas 使用的 image 不能跨域，而是说 canvas 使用了没有权限的跨域图片,在使用 canvas.toDataURL()等数据导出函数的时候会报错！
poster.crossOrigin = "anonymous";

//预加载图片
poster.onload = () => {   
   //drawImage() 方法在画布上绘制图像 context.drawImage(img,x,y,width,height);  xy为图片在画布的位置，wh是图片的宽高
   ctx.drawImage(poster,0,0,750,1000);    
   ctx.font = "normal 24px webfont";    
   ctx.fillStyle = "#3d2a2a";  
   
   //fillText() 方法在画布上绘制填色的文本。 context.fillText(text,x,y,maxWidth);
   ctx.fillText(array[0], 300, 556)    
   ctx.fillText(array[1], 300,586)    
   
   //toDataURL() 方法返回一个包含图片展示的 data URI  返回base64  第二个参数是0-1的图片质量
   var base64 = canvas.toDataURL("image/jpeg",0.8); 
   
   //使用公司的upload组件，上传到七牛云
   var upload = new Up({width : 600,});    
   //传入base64图片
   upload.up(base64, {
       //成功时的回调
       success(data) { 
           //data.img是返回线上的图片链接
           app.preload(data.img).done(() => {                
           console.log(data.img);            
               
           })        
           
       },       
       error(data) {            
       console.log('合成上传失败！');        
           
       },    
   })
}

poster.src = 'dist/images/poster.jpg';

```

## 测试中的问题 

### 1. 在T8中，一键生成跳不过去，别的手机又是正常的

这里洪卿大哥安利了一个在移动端调试小工具 http://h5.meitu.com/js/console.min.js

**解决：**  一步步排查，原来是 

``` bash
var base64 = canvas.toDataURL("image/jpg",0.8);  //这边出错了，第一个参数的类型写错了并没有jpg类型，应该为jpeg，所以正确代码如下：

var base64 = canvas.toDataURL("image/jpeg",0.8); 
```

### 2. 用户保存图片时把背景保存下来

**解决：**  由于第一个项目的图片较少，自己之前也没注意到这个问题，就是在移动端中长按会保存img标签的图片，而我为了省事正好把有用到的图片全给用了img标签，测试尽然没测出来，上线用户一体验就出现bug，啊啊啊啊~ 


所以正确的做法是，希望让用户保存的图片用img标签例如结果页，而像每页的背景图片什么的最好说在css中设置background-image


### 3. 使用backgroud-size:cover的问题

``` bash
background-size：100% auto;    ---按容器比例撑满，图片变形；因为是移动端所以height为auto自动调整就好，只填第一个参数第二参数默认为auto

background-size：cover;   ---	把背景图像扩展至足够大，以使背景图像完全覆盖背景区域。背景图像的某些部分也许无法显示在背景定位区域中。

background-size：contain;    ---把图像图像扩展至最大尺寸，以使其宽度和高度完全适应内容区域。

```

### 4. 在ios系统中，输入框输入完成后，跳转页面之后，输入面板还在

**解决：**  $('.js-text').blur() 手动去焦点




## 结尾
   第一次参加公司的开发，感觉做点有点不足，程序上是一方面，感觉沟通也存在不足，不会先做好沟通再写代码，而是一拿到psd就开始切，写代码，以至于有时候会做一些无用功，这是我站在面前完成两个项目的情况下思考这个问题(第一个项目比较简单可能没什么发现，第二个项目就非常明显)，所以还是要先好好沟通完之后，再下手。才发现一个项目下来沟通真的至关重要，跟运营跟测试等等，这可能也是做开发有趣的一面。公司人才好多哦~，在里面自己真的是渣渣，努力吧，嘿嘿...相信勤能补拙。有空想看看公司一些组件的源码，感觉我这个层次的会看不懂，扎心·······再说说公司的氛围吧，同事都挺好玩的，互相调侃，哈哈，喜欢这样的氛围，一个月下来还是感觉还没有适应似的，跟大家也还不太熟悉，希望在接下来的日子可以用心对待，对人对事！加油，实习生！！！(顺便说下我喜欢的冬天快到了，哈哈哈哈哈哈................)
   　　　　　　   
   　　　　　　                        
   　　　　　　                        
   
   

> 不知道说什么  加油  ٩(๑❛ᴗ❛๑)۶

　于 厦门前埔 自己的小屋
   　　　　　　                        　　　　　
   　　　　　　                        　　　　　
   　　　　　　                        　　　　　
   　　　　　　                        　　　　　

本文首发于个人博客　>> [黄明照--一个在路上慢慢行走的前端人](http://www.huangmingzhao.cn/)
