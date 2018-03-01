---
title: canvas API 汇总
date: 2018-02-24 00:02:01
comments: true

tags:
    - canvas
categories:
     - 渣技术
---
canvas 的 API 汇总

```js
rect( x, y, width, height )   绘制矩形

fillRect( x, y, width, height )  绘制被填充的矩形

strokeRect( x, y, width, height )  绘制矩形（边框，无填充）

clearRect( x, y, width, height ) 清除指定的矩形内的像素



fill()  填充当前绘图（路径）

stroke() 绘制已定义的路径

beginPath()  起始（重置）当前路径

moveTo( x, y )  将笔触移动到指定的坐标(x,y)

lineTo( x, y )  绘制一条从当前位置到指定的坐标(x,y)的直线

clip()  从原始画布剪切任意形状和尺寸的区域

quadraticCurveTo()  创建二次贝塞尔曲线

bezierCurveTo()   创建三次贝塞尔曲线

arc( x, y, radius, startAngle, endAngle, anticlockwise)  绘制圆或圆弧

arcTo( x1, y1, x2, y2, radius)  根据给定点画圆弧，再以直线连接两个点

isPointInPath( x, y )  检测指定的点是否在当前路径中，在则返回true。



fillStyle  设置或返回用于填充绘画的颜色、渐变或模式

strokeStyle  设置或返回用于笔触的颜色、渐变或模式

shadowColor  设置或返回用于阴影的颜色

shadowBlur   设置或返回用于阴影的模糊级别

shadowOffsetX  设置或返回阴影与形状的水平距离

shadowOffsetY  设置或返回阴影与形状的垂直距离



lineCap  设置或返回线条的结束点样式（butt、round、square）

lineJoin  设置或返回当两条线交汇时，边角的类型（bevel、round、miter）

lineWidth  设置或返回当前的线条宽度

miterLimit  设置或返回最大斜接长度



createLinearGradient( x0, y0, x1, y1 )  创建线性渐变

createPattern( image/canvas/video, repeat )  在指定的方向内重复绘制指定的元素

createRadialGradient( x0, y0, r0, x1, y1, r1 ) 创建径向渐变

addColorStop( stop, color )  规定渐变对象中的颜色和停止位置



font  设置或返回文本内容的当前字体属性（和css的font一样）

textAlign  设置或返回文本内容的当前对齐方式

textBaseline  设置或返回在绘制文本时使用的当前文本基线

fillText( text, x, y )  在画布上绘制“被填充”的文本

strokeText( text, x, y )  在画布上绘制文本（无填充）

measureText( text )  返回包含指定文本宽度的对象（属性width获取宽度）

drawImage( image/canvas, x, y )、drawImage( image/canvas, x, y, width, height )、drawImage( image/canvas, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight )  在画布上绘制图像、画布或视频

createImageData( width, height )、createImageData(imageData)  绘制ImageData对象

getImageData( x, y, width, height )  返回ImageData对象，该对象为画布上指定的矩形复制像素数据。

putImageData( ImageData, x, y )、putImageData( imageData, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight )  把图像数据放回画布上。

width  返回ImageData对象的宽度

height  返回ImageData对象的高度

data  返回一个对象，包含指定的ImageData对象的图像数据


globalAlpha  设置或返回绘图的当前alpha或透明度

globalCompositeOperation  设置或返回新图像如何绘制到已有的图像上。


scale( x, y )  缩放当前绘图

translate( x, y )  重新设置画布上的(0,0)位置

rotate( angle )  选择当前绘图，单位为“弧度”，角度转弧度公式（ degrees*Math.PI/180）

transform( m11, m12, m21, m22, dx, dy )  替换绘图的当前转换矩阵

setTransform()  将当前转换重置为单元矩阵，然后运行transform()



save()  保存当前环境的状态

restore()  恢复之前保存过的路径状态和属性



getContext('2d')  获取2d对象

toDataURL()  将canvas转换成图片，返回地址
```