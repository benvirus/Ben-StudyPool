#  给你一个图片地址，我们到底有多少中方法加载显示一张图片?

本篇文章通过一张图片来讲解几种数据加载的方式。计算机专业有很多不同类别的东西在底层的实现上都是一样的，所以往往越是深入的去研究一下越是发现它如此的简单。

### 问题介绍

现在给定一张图片的URI，分别通过几种不同的方式来加载显示图片。

### 谁都会

利用DOM操作

    var imgDom = document.createElement('img');
    imgDom.src = 'http://h.hiphotos.baidu.com/zhidao/pic/item/0eb30f2442a7d933df010120af4bd11373f00133.jpg';
    document.body.appendChild(imgDom);

### WebAPI是个好东西

    var Img = new Image();
    Img.src = 'http://h.hiphotos.baidu.com/zhidao/pic/item/0eb30f2442a7d933df010120af4bd11373f00133.jpg';
    document.body.appendChild(Img);
    

和上面的方式其实是一样的，js内部也是调用的webAPI。

### 格式化数据块

所谓的格式化数据块，就是按照某种格式编码的二进制数据块，保存在内存中，然后告诉浏览器一个可以访问的内存地址，浏览器就可以获得这个块儿，然后展示。不仅仅限于图片。所有资源都可以。

首先我们要借助ajax加载资源的二进制数据：

    var Img = document.createElement('img');
    url  = '/dist/test.jpg';
    var xhr = new XMLHttpRequest();
    xhr.open('GET',url);
    xhr.responseType='arraybuffer';
    xhr.onload=function(){
        var buffer = xhr.response;
        var blob = new Blob([buffer],{type:'image/jpeg'});
        Img.src = URL.createObjectURL(blob);
        document.body.appendChild(Img);
    }
    xhr.send();

### 转化成base64数据

    function arrayBuffer2Base64(buffer){
        var str = '';
        var bytes = new Uint8Array(buffer);
        var length = bytes.byteLength;
        for(var i = 0;i<length;i++){
            str += String.fromCharCode(bytes[i]);
        }
        return window.btoa(str);
    }

    var Img = document.createElement('img');
    url  = '/dist/test.jpg';
    var xhr = new XMLHttpRequest();
    xhr.open('GET',url);
    xhr.responseType='arraybuffer';
    xhr.onload=function(){
        showImg.src = 'data:data:image/JPEG;base64,' + arrayBuffer2Base64(xhr.response);
        document.body.appendChild(Img);
    }
    xhr.send(); 

### 强制以某种格式解析

    var Img = document.createElement('img');
    var url = '/dist/test.jpg';
    var xhr = new XMLHttpRequest();
    xhr.open('GET',url);
    xhr.onload=function(){
      showImg.src = URL.createObjectURL(xhr.response);
      document.body.appendChild(Img);
    }
    xhr.overrideMimeType('image/jpeg; charset=x-user-defined');
    xhr.responseType='blob';
    xhr.send();

### 目标

希望能通过本篇介绍，让大家理解数据之间的转化关系。弄明白buffer是什么、blob是什么、Base64是什么，以及js和WebAPI的关系。
