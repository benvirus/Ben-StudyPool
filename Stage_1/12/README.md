# 触发文件选择器的几种方法以及从本地加载文件预览

本篇主要讲解触发文件选择器的几种方法。

### 问题

在做文件上传的时候，我们经常会碰到这样的问题，对`<input type="file">`的样式相当的不爽，那么就想修改这个样式，同时又要保证能触发文件选择器。

### 透明度的

基本原理就是，把整个组件分两层，在底层实现样式，在上层放一个`<input type="file" >`然后把透明度调为0；这是最基本的做法，也是很low的写法。但是好多小公司都在用。

    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <title>File</title>
      <style type="text/css" media="screen">
        .file-uploader{
          padding: 20px;
          position: relative;
          cursor: pointer;
        }
        .file-uploader .file,.file-uploader .file-resetstyle{
          position: absolute;
          width: 100%;
          height: 100%;
          left: 0px;
          top: 0px;
        }
        .file-uploader .file-resetstyle{
          color: #DDD;
          color: red;
        }
        .file-uploader .file{
          opacity: 0;
        }
        
      </style>
    </head>
    <body>
      文件上传：
      <div class="file-uploader">
        <div class="file-resetstyle">
          这里是下面一层表现
        </div>
        <input class="file" type="file" name="">
      </div>
    </body>
    </html>

基本上没优点，缺点就是不够灵活。写法上不够优雅。

### 利用label标签特性

`label`标签有这样的一个特性：

如果您在 label 元素内点击文本，就会触发此控件。就是说，当用户选择该标签时，浏览器就会自动将焦点转到和标签相关的表单控件上。怎么做到相关呢？以`label`标签的`for`属性的值为`id`的节点。

example:

    <label for="nihao">你好</label>
    <input id="nihao" type="text">

通过这个特性，我们就可以利用它来触发`<input type="file">`了。

    <label for="fileselect">选择文件</label>
    <input id="fileselect" type="file" hidden>

当我们点击`label`的时候就相当于触发了`<input type="file">`，这个的好处是不再把file放到文档流中，节点比较简洁，可以对`label`进行样式设置。

### js触发

摆脱label的束缚

    <input id="haha" type="file" hidden>
    <a id="fileselect" href="javascript:;">选择文件</a>
    <script type="text/javascript">
        var fileSelect = document.getElementById('fileselect');
        var file = document.getElementById('haha');
        fileSelect.onclick = function(){
            file.click();
        }
    </script>

### 谈一谈web文件上传的基本思路

一句话：

文件系统 -> 浏览器内存 -> 服务器

那么：

理论上，当把文件加载到浏览器内存的时候就可以预览了。（我们现在系统是每次都上传到服务器之后返回图片uri再预览）。这样做可以减少不必要的文件上传，减少服务器压力。

来个栗子🌰 

    <img id="imgshow" src="" alt="">
    <video id="videoshow" width="400px" src="" autoplay></video>
    <input id="haha" type="file" hidden>
    <a id="filehandler" href="javascript:;">选择文件</a>
    <script type="text/javascript">
        var filehandler = document.getElementById('filehandler');
        var file = document.getElementById('haha');
        var imgshow = document.getElementById('imgshow');
        var videoshow = document.getElementById('videoshow');
        var reader = new FileReader();

        reader.onload = function(e){
            var blob = new Blob([e.target.result],{type:'video/quicktime'});
            videoshow.src = URL.createObjectURL(blob);
            console.log(e);
        }
        reader.onprogress = function(e){
            console.log(e.loaded/e.total*100+'%');
        }

        filehandler.onclick = function(e){
            file.click();
            e.preventDefault();
        }
        file.addEventListener('change',function(e){
            reader.readAsArrayBuffer(this.files[0]);
        });
    </script>
    

上次我讲过从服务端获取数据，这次又讲了从本地获取数据。用来讲解，数据的传输都是相同的，数据的转化也是一样的。

### 主系统值得优化的地方

本地选择文件，不上传，直接预览，然后剪裁，只把用户确认的有价值的图片上传上去。



























