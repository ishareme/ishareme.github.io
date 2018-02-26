---
title: 实习&andyMove小结
date: 2018-02-05 17:01:01
comments: true

tags:
    - 美图
    - H5项目
    - 实习
categories:
     - 渣技术
---

这学期的实习也快结束了，想想还是挺感慨的。这次对andyMove的总结就当做对自己这学期实习的总结吧。
<!-- more -->

在美图也快满4个月了，总感觉自己进步的非常慢，有时候会在想是不是自己变得不上心了？变得不进取了？或许是有所松懈，但这完全不是我！！！！当然在这四个月里或多或少还是比刚进美图的时候有所收获。从刚开始什么都不太懂的情况下开始的第一个H5,到现在相对比较清楚的开发(bug满天飞)。真的非常感谢有这个平台让我更加清楚的看清自己的不足，在这里真的非常感谢板栗兄一对一的辅导，自己的渣技术对他造成的不便深感自责。非常感谢晓东兄照顾~~

**1** 不管做什么事，**细节**真的很重要，在开发中，正如晓东说的多去注意一些细节的东西，这个项目就会更好~~

**2** 在开发**效率**方面真的需要再提升提升，比如像一些比较麻烦的animation动画可以先不用处理，写animation有时候真的会挺花费时间的，导致于项目进展的比较慢，有可能会误期，所以可以先把项目做出来，逻辑处理好了再去处理animation等等。

**3** 与人**沟通**方面，能不麻烦别人的就不麻烦别人，坐在板栗旁边，有时候就产生了过度依赖的问题，一遇到bug自己不能解决的情况，就去问，自己并没有好好想想自己如何解决，产生依赖。

**4** 这次的开发由于bug比较多，导致于bug解决完之后自己这边忘记测一遍，可能有时候bug并未解决，导致于测试那边做无用功。惭愧惭愧！！

**5** 正如晓东说的目前自己写的**代码**可能还是比较面条化，没有很好的去优化，重复性太强，没有模块化

**6** 对于**css**也同样，一定要有模块化，不然复用性太强。

**7** 要有**模块化**的思想，减少代码耦合


    
    
    
下面是一些这次开发过程中出现的一些问题：
#### QQ平台，结果页不能滑动
由于iOS下z-index的层级影响，导致于有些平台在ios下会很难出现滑动的情况(不顺畅)。
目前解决方案:
```code
css:

目前
page{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
    //transition: transform .5s;
    //opacity: 0;
    overflow-x: hidden;
    overflow-y: auto;
    &.transition{
        transition: all .3s;
    }
    &.show{
        transform: translateX(0);
    }
    &.hide-after{
        transform: translateX(100%);
    }
    &.hide-before{
        transform: translateX(-100%);
    }
}

之前
page{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
    transition: transform .5s;
    opacity: 0;
    overflow-x: hidden;
    overflow-y: auto;
    &.in {
        opacity: 1;
        z-index: 2;
        //animation: in 0.5s ease both;
        //
        //@keyframes in {
        //    0% { opacity: 0; }
        //    100% { opacity: 1; }
        //}
    }

    &.out {
        opacity: 0.5;
        z-index: 1;
        //transform: translateX(-100%);
        //transition: all .5s;
    }
}
```

#### 上传完图片要进行预加载
在上传完图片生成url的时候一般都需要在接下来的页面中用到，故要对其进行预加载。在做帧动画更换图片的时候要注意，也是要预加载

#### ios秀秀端结果页转发qq和qq空间转发不了
这个要注意，在做拼接url的时候要对带有中文或者其他字符做encodeURIComponent处理，再到要用的地方(分享页)进行decodeURIComponent获取

#### 图片repeat重复问题
动态加上图片的时候要记得加上background-repeat:no-repeat，不要只加background-image

#### share页也要做分享，auth权限模块是异步执行的
在share页的时候一般也要加上分享，auth权限模块是异步执行的~~

#### 微博下的视频问题
微博下视频默认是全屏播放的，点击返回视频在没有控制条control的情况下会不能再次播放，因此在weibo下要加上control

#### gif在Android下会出现显示一段时间之后不显示，但是可以保存和转发(后端算法已解决，无需降级)
此次采用降级的方案，直接放图片，子元素gif的opacity为0

#### video 在微信下会显示内置的播放器
```codejs
<video class="js-video"
        loop
        airplay="allow"
        x5-playsinline
        playsinline
        webkit-playsinline
        preload="auto"
>
</video>
```

#### iPhoneX在video问题
iPhoneX的video在有poster的情况下，播放视频poster不会自动去掉，要自己逻辑手动去掉poster

#### 尽量不要去用touchstart
touchstart时间间隔太小，有可能不会生效，一般使用tap，click，touchend，而touchstart主要用于事件状态方面

#### 在开发要尽可能模块化
对于css也是，类似的css可以抽出模块，js更是，逻辑尽可能写成模块

#### 代码要尽可能简洁，能优化的逻辑尽可能优化，以后要全使用ES6！！ES6！！！！

#### 视频播放延迟问题(后端算法已解决)

#### ios低端机下gif保存为静态




请假了，想想还是挺纠结的，想了好久，也是这段的实习让我看到了自己身上各个方面的不足，也借请假期间好好再充电充电，调整调整心态。2018想想应该会是一个充实的一年，想做的事情很多，希望自己能够把它过的充实，善待于它。

美图，6月份再见！希望来时是一个不一样的自己。

于 美图公司














