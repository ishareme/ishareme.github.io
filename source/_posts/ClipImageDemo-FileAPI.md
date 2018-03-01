---
title: Html5 File API 与 ClipImage结合 Demo 
date: 2018-02-27 23:56:01
comments: true

tags:
    - Canvas
    - FileAPI
categories:
     - 渣技术
---

用ClipImage和File Api 写的一个截图小🌰
> [example](http://f2er.meitu.com/hmz/ClipImageDemo/example/)

> [github](https://github.com/ishareme/ClipImageDemo)
<!-- more -->

## File API

在File API 没有出来的时候，如果想在页面中一上传就直接预览所上传的图片一般要配合后端实现获取到图片的url，就又发起了请求，不能实时预览图片。File API 也可以实现多文件上传、上传图片实时预览、断点续传、文件下载等等。File API 主要有以下接口
##### - Blob
##### - File
##### - FileList
##### - FileReader

#### Blob
Blob对象是一个代表二进制数据的对象
```bash
let blob = new Blob(blobParts[, options])

```
注意Blob这个构造器的参数比较诡异，第一个参数是一组数据，所以必须是数组，即使像上面的例子一样只有一个字符串也必须用数组装起来。第二个参数是对这一Blob对象的配置属性.

**blob属性**
- size
- type

**blob方法**
- slice([start,[ end ,[contentType]]])  返回一个新的 Blob对象，包含了源 Blob对象中指定范围内的数据。

#### File
File 对象包含文件的一些信息，比如name、size、type、lastModifiedDate等

File对象可以用来获取某个文件的信息，还可以用来读取这个文件的内容。通常情况下，
1. File对象是来自用户在一个input元素上选择文件后返回的FileList对象
2. 来自由拖放操作生成的 DataTransfer对象.

File 接口没有定义任何方法，但是继承了**Blob 接口的方法**。

#### FileList
FileList包含File对象列表，类数组结构

**属性**
- length 返回FileList的长度
** 方法** 
- item(index) 取某个File,可以直接下标

#### FileReader
```
let reader = new FileReader()

//四种异步读取文件方法
FileReader.onabort
处理abort事件。该事件在读取操作被中断时触发。

FileReader.onerror
处理error事件。该事件在读取操作发生错误时触发。

FileReader.onload
处理load事件。该事件在读取操作完成时触发。

FileReader.onloadstart
处理loadstart事件。该事件在读取操作开始时触发。

FileReader.onloadend
处理loadend事件。该事件在读取操作结束时（要么成功，要么失败）触发。

FileReader.onprogress
处理progress事件(进度)。该事件在读取Blob时触发。

// 读数据
FileReader.abort()
中止读取操作。在返回时，readyState属性为DONE。

FileReader.readAsArrayBuffer()
开始读取指定的 Blob中的内容, 一旦完成, result 属性中保存的将是被读取文件的 ArrayBuffer 数据对象.

FileReader.readAsBinaryString() 
开始读取指定的Blob中的内容。一旦完成，result属性中将包含所读取文件的原始二进制数据。

FileReader.readAsDataURL()
开始读取指定的Blob中的内容。一旦完成，result属性中将包含一个data: URL格式的字符串以表示所读取文件的内容。

FileReader.readAsText()
开始读取指定的Blob中的内容。一旦完成，result属性中将包含一个字符串以表示所读取的文件内容。

```
```js
//二进制流上传文件
var fileInput = document.getElementById("fileInput");
fileInput.addEventListener('change', function(event) {
    var file = fileInput.files[0];
    if (file) {
        var reader = new FileReader();  
        var xhr = new XMLHttpRequest();
        xhr.onprogress=function(e){
            var percentage = Math.round((e.loaded * 100) / e.total);
            console.log("percentage:"+percentage);
        }
        xhr.onload=function(e){
            console.log("percentage:100");
        }
        xhr.open("POST", "这里填写服务器地址");  
        reader.onload = function(evt) {
            xhr.send(evt.target.result);
        };
        reader.readAsBinaryString(file);
    }
});
```

## URL对象
我们除了可以使用base64字符串作为内容的DataURI将一个文件嵌入到另外一个文档里，还可以使用URL对象。URL对象用于生成指向File对象或Blob对象的URL。

```js
let objectURL =  window.URL.createObjectURL(file/blob)
img.src = objectURL

//静态方法用来释放一个之前通过调用 window.URL.createObjectURL() 创建的已经存在的 URL 对象。
//当你结束使用某个 URL 对象时，应该通过调用这个方法来让浏览器知道不再需要保持这个文件的引用了。	
window.URL.revokeObjectURL(objecturl)


//such this example 使用对象URL来显示图片
var img = document.createElement("img");
img.src = window.URL.createObjectURL(blob);
img.height = 60;
img.onload = function(e) {
    window.URL.revokeObjectURL(this.src);
}
document.body.appendChild(img);
```


#### CLipImage
[点我！！！！](https://blog.huangmingzhao.cn/ClipImage.html)


## 结语
ps: 有空写下断点续传吧！！！巴拉巴拉~~
最近状态慢慢开始有所调整了哈，继续加油~~买了书，买了椅子，买了显示器，但是还没到！！有点小期待~期待自己接下来的生活，好好吃饭、好好睡觉、好好读书。晚安，睡觉.....
   　　　　　　   
   　　　　　　                        
   　　　　　　       








