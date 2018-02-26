# 计算图片实际尺寸

本篇内容主要介绍在前端获得图片的实际大小以及在在裁剪之前做必要的缩放。

### 场景需求

课程图片上传：/course/3/manage/picture
课程图片剪裁：/course/3/manage/picture/crop

### 这中间做了哪些事情？

    public function pictureCropAction(Request $request, $id)
    {
        $course = $this->getCourseService()->tryManageCourse($id);

        if ($request->getMethod() == 'POST') {
            $data = $request->request->all();
            $this->getCourseService()->changeCoursePicture($course['id'], $data["images"]);
            return $this->redirect($this->generateUrl('course_manage_picture', array('id' => $course['id'])));
        }

        $fileId                                      = $request->getSession()->get("fileId");
        list($pictureUrl, $naturalSize, $scaledSize) = $this->getFileService()->getImgFileMetaInfo($fileId, 480, 270);

        return $this->render('TopxiaWebBundle:CourseManage:picture-crop.html.twig', array(
            'course'      => $course,
            'pictureUrl'  => $pictureUrl,
            'naturalSize' => $naturalSize,
            'scaledSize'  => $scaledSize
        ));
    }

从上面的方法可以看出，上传完成跳转到新的剪裁图片页面的这个过程中，服务端主要就做了两个事情：

- 根据要剪裁的最大尺寸，计算图片在剪裁时应该显示的尺寸。（为什么？自己去思考研究。）
- 将原图片的实际尺寸返回给浏览器。（为什么？自己去思考研究。）

因为上传页面和剪裁中大部分内容都是重复的，所以对于course数据的读取重复读取，都是在浪费服务器性能。所以，如果我们前端能解决掉这两个问题，理论上就能实现上传后在当前页面进行图片剪裁。

okay，那就干吧。

### 逐个击破：计算图片实际尺寸

##### HTML5：

    var src = 'http://imgsrc.baidu.com/forum/pic/item/8713c8fcc3cec3fdfa631ae0d688d43f869427c2.jpg';
    function getNaturalDemention(imgSrc){
        var img = document.createElement('img');
        img.onload = function(){
            console.log(this.naturalWidth);
            console.log(this.naturalHeight);
        }
    }
    getNaturalDemention(src);

嗯，看起来很不错，可是IE8就挂了呀，，，肿么办。。。

##### IE8不兼容？WebAPI来搞定！

    var src = 'http://imgsrc.baidu.com/forum/pic/item/8713c8fcc3cec3fdfa631ae0d688d43f869427c2.jpg';
    function getNaturalDemention(imgSrc){
        var img = document.createElement('img');
        img.onload = function(){
            if(this.naturalWidth){
                console.log(this.naturalWidth);
                console.log(this.naturalHeight);
            }else{
                var img = new Image(this);
                img.src = this.src;
                console.log(img.width);
                console.log(img.height);
            }
        }
    }
    getNaturalDemention(src);

好了，就这样愉快的搞定了，可以在onload的回调里面的到图片的实际尺寸了。

### 还有一个：缩放图片

为什么要缩放呢？想象一下，你想剪裁的最大尺寸时100*100，但是你这个图有500*500，显示那么大有意义吗？异口同声：没有。很好，既然没有，那我们可以根据想剪裁的最大尺寸进行缩放显示啊，只要让图片的显示不小于想剪裁的最大尺寸就好了啊。想象也想CSS里面`background-size`属性的`cover`值。

okay ,,理解了为什么，那我们就动手缩放吧。可是怎么缩放才能保证既不小于要剪裁的尺寸，又要保证原图比例，不会被压缩呢？思考10s

    function scaleImg(cropWidth,cropHeight,naturalWidth,naturalHeight){
        var ratioWidht = cropWidth/naturalWidth;
        var ratioHeight = cropHeight/naturalHeight;
        var ratio = ratioWidht > ratioHeight ? ratioWidht :ratioHeight ;
        var showDem = {
            scaledWidth : naturalWidth * ratio ,
            scaledHeight : naturalHeight * ratio
        }
        return showDem;
    }

okay ,搞定，的到了缩放后的显示尺寸。

### 准备好，开始剪裁。

但是……等等……

这不是此次的主讲内容，自己去研究吧。。。。哈哈哈哈

这片demo写的虽然还标粗糙，但是比较详细了，有兴趣的可以去看看。




