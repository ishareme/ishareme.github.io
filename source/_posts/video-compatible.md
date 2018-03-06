---
title: video标签移动端兼容性问题总结 
date: 2018-03-06 14:01:01
comments: true

tags:
     - video
categories:
     - 渣技术
---

由于video标签在各个平台下存在的各种各样的兼容性问题，遇到了各种坑，目前总结了下篇，待更新~~

<!-- more -->

#### 微信下内置的播放器
```
<video class="js-video"
        loop
        airplay="allow"
        x5-playsinline /**/
        playsinline /*IOS微信浏览器支持小播放*/ 
        webkit-playsinline /*这个属性是ios 10中设置可以让视频在小窗内播放，也就是不是全屏播放*/  
        preload="auto" /*预加载*/
>
</video>
```
#### 安卓微信下不能自动播放
安卓微信下autoplay无效，不能自动播放，可以通过**用户交互进行手动播放**。
```
//在安卓微信下
$('body').one('touchstart', function () {
    //ios微信下可以自动播放
    if (is.iOS) {
         return;
    
    $('.video-icon').hide()
    $('.js-video')[0].play();
});
```
#### safari下无法自动播放
在safari下，autoplay也无效，在iphone上做测试，发现autoplay属性在safari里无效，苹果对video标签的autoplay属性做了如下规定：
> In Safari on iOS (for all devices, including iPad), where the user may be on a cellular network and be charged per data unit, preload and autoplay are disabled.

其实safari的并不是autoplay的问题，而是视频有声音导致于safari不能自动播放，**给video的muted[是否静音]设置为true**，视频便会自动播放，但是视频没有声音(降级处理)

safari下，设置muted为true
```
//safari下
if(is.safari){
   $('.js-video')[0].play();
    $('.js-video')[0].muted = true; 
}
```

#### video的poster的问题
- 在安卓的有些机子下，给video增加了poster，但是还是没有效果，poster在android兼容的并不好，不如在视频上层加个div铺张图片
- iPhoneX的video在有poster的情况下，播放视频poster不会自动去掉，要自己逻辑手动去掉poster(之前测到过)

#### 黑屏问题
部分环境中视频从开始播到能展现画面会有短暂的黑屏（处理视频源数据的时间），为了避免这个黑屏，可以在视频上加个浮层或设置容器背景并在播放前隐藏video，然后监听相关事件，开始播放或认为有画面的时候再切换到Video界面。


解决方案
微信下，通过用户交互进行播放，最好给他一个播放小icon


#### 微博下的视频问题
对于小视频为了美观，一般都是把control属性拿掉，而在微博下视频默认是全屏播放的，点击返回视频在没有控制条control的情况下会不能再次播放，因此在weibo下要加上control

解决方案：
```
if(is.Weibo){
    $('.js-video')[0].control = true; 
}
```




   　　　　　　   
   　　　　　　                        
   　　　　　　       








