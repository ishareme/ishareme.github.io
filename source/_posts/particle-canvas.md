---
title: canvas粒子背景
date: 2018-02-28 10:31:01
comments: true

tags:
    - Canvas
categories:
     - 渣技术
---

根据网上的思路用canvas实现之前知乎的粒子背景。 [github 地址](https://github.com/ishareme/ClipImage/)

<!-- more -->

## 主要思路
- 需要一张 Canvas 画布
- 在画布上绘制许多个小点
- 小点会来回移动
- 小点移动到边框的时候需要回弹
- 每两个小点之间在一定距离之内，绘制线
- 当鼠标移入的时候，让内部的小点跟随鼠标的移动


canvas实现裁剪的方法比较多，比如

#### 需要一张 Canvas 画布
```js
        this.ctx = this.canvas.getContext('2d');
        this.canvas.width = window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth;
        this.canvas.height = window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight;
        this.drawBackground();
```

#### 在画布上绘制许多个小点
```
for(let i = 0; i < this.options.particleNum; i++){
            let circleX = utils.randomNumber(this.canvas.width);
            let circleY = utils.randomNumber(this.canvas.height);
            let circleVX = utils.randomNumber(this.options.particleSpeed, this.options.particleSpeed * -1) / 20;
            let circleVY = utils.randomNumber(this.options.particleSpeed, this.options.particleSpeed * -1) / 20;
            let circleR = this.options.particleR;
            let circleColor = utils.randomColor();
            this.circleArr.push(new Circle(circleX, circleY, circleVX, circleVY, circleR, circleColor));
        }

        //包括鼠标的位置
        this.circleArrAll = [this.mousePoint].concat(this.circleArr);
```
#### 小点会来回移动 & 边界回弹
```
item.x += item.vx;
        item.y += item.vy;
        //圆点移动到边框的时候需要回弹
        item.vx *= (item.x <= 0 || item.x >= (this.canvas.width - item.r)) ? -1 : 1;
        item.vy *= (item.y <= 0 || item.y >= (this.canvas.height - item.r)) ? -1 : 1;
```

#### 每两个小点之间在一定距离之内，绘制线
```
this.circleArr.forEach((item) => {
            this.ctx.fillStyle = item.color;
            this.ctx.beginPath();

            //圆点移动
            this.circleMove(item);

            this.ctx.arc(item.x, item.y, item.r, 0, 2 * Math.PI, true);
            this.ctx.fill();

            for (var i=0; i < this.circleArrAll.length; i++){
                var currCircle = this.circleArrAll[i];
                //如果是同项 或者 当 mousePoint 还未赋值就跳过
                if (item === currCircle || currCircle.x === null || currCircle.y === null) continue;
                //每两个圆点之间在一定距离(this.options.lineLength)之内，绘制线
                if (utils.dis(item, currCircle) < this.options.lineLength){
                    //当鼠标移入的时候，让内部的圆点跟随鼠标的移动
                    //在 距离(utils.dis2(item, currCircle) > (this.options.lineLength * this.options.lineLength) /2)内 圆点跟随鼠标的移动
                    if (currCircle === this.mousePoint && utils.dis2(item, currCircle) > (this.options.lineLength * this.options.lineLength) /2){
                        //圆点跟随鼠标的移动
                        this.moveToMousePoint(item, currCircle);
                    }
                    //绘制线
                    this.drawLine(item, currCircle);
                }
            }
        });
```

#### 当鼠标移入的时候，让内部的小点跟随鼠标的移动
```
if (utils.dis(item, currCircle) < this.options.lineLength){
                    //当鼠标移入的时候，让内部的圆点跟随鼠标的移动
                    //在 距离(utils.dis2(item, currCircle) > (this.options.lineLength * this.options.lineLength) /2)内 圆点跟随鼠标的移动
                    if (currCircle === this.mousePoint && utils.dis2(item, currCircle) > (this.options.lineLength * this.options.lineLength) /2){
                        //圆点跟随鼠标的移动
                        this.moveToMousePoint(item, currCircle);
                    }
                    //绘制线
                    this.drawLine(item, currCircle);
                }
                
moveToMousePoint(item, currCircle){
        // 小圆点的向鼠标点移动
        item.x -= (item.x - currCircle.x) * 0.03;
        item.y -= (item.y - currCircle.y) * 0.03;
    },
```

## Basic Usage
```sh
Particle


        //参数一 canvas的id, class 或 canvas [必填]
        //参数二 配置项 [可选]
        new Particle('#test',{
            //画布背景(默认 #FFF)[可选]
            background: '#000',
            //粒子数(默认 300)[可选]
            particleNum: 300,
            //粒子半径(默认 1)[可选]
            particleR: 1,
            //粒子移动速度(默认 10)[可选]
            particleSpeed: 10,
            //线长度(默认 70)[可选]
            lineLength: 30,
            //线宽度(默认 0.5)[可选]
            lineWidth: 0.5,
            //鼠标心颜色(默认 #000)[可选]
            mousePointColor: '#FFF'
        })

```



   　　　　　　   
   　　　　　　                        
   　　　　　　       








