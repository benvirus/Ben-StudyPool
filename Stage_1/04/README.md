# mouseover && mouseout  VS mouseenter && mouseleave

聊一下鼠标移动事件，主要讲解 `mouseover` 和 `mouseout` 与 `mouseenter`和`mouseleave`的区别。

### 字面分析

但从字面分析：`over`这个词意为在什么之上，所以结合鼠标可以解读为当鼠标在什么什么之上的时候触发这个事件。

`out`这个词单独去解读没什么含义，但是因为mouseout 和 mouseover是相对的，那么既然mouseover是在某元素之上的时候触发，所以，这个就应该是不再在某个元素的时候触发。（不能理解为移除，为什么，看下面）。

`enter`意为进入。这个很好理解为进入某元素的范围内的时候触发。

`leave`意为离开。结合mouseenter则很好理解为当离开某个元素范围内的时候触发事件。

### 差别在哪里

差别在于：如果一个元素parent有个儿子元素son ,son只占有parent的部分空间，那么从parent进入son所在区域的时候，mouseout 会不会触发？从son进入parent的时候会不会触发parent的mouseover事件？

    -----------------------------------
    | parent                          |
    |   -------------------------     |
    |   |    son                |     |
    |   -------------------------     |
    |                                 |
    -----------------------------------

### 实例测试

    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <title>Mouse Event</title>
      <style type="text/css" media="screen">
        #parent{
          box-sizing: border-box;
          width: 200px;
          height: 200px;
          background: red;
          padding-top: 50px;
        }
        #son{
          width: 100%;height: 100%;background: black;
          margin-left: 50px;
        }
      </style>
    </head>
    <body>
      <div id="parent">
        <div id="son">
          
        </div>
      </div>
      <script  type="text/javascript" charset="utf-8">
        window.onload=function(){

          var parent = document.getElementById('parent');
          var son = document.getElementById('son');

          parent.addEventListener('mouseover',function(){
            console.log('parent',':','mouseover');
          });
          parent.addEventListener('mouseout',function(){
            console.log('parent',':','mouseout');
          });
          parent.addEventListener('mouseenter',function(){
            console.log('parent',':','mouseenter');
          });
          parent.addEventListener('mouseleave',function(){
            console.log('parent',':','mouseleave');
          });

          son.addEventListener('mouseover',function(e){
            e.cancelBubble = true;
          })
          son.addEventListener('mouseout',function(e){
            e.cancelBubble = true;
          })

        }
      </script>
    </body>
    </html>

### 得出结果

1、当鼠标进入parent的时候，会触发mouseover 和 mouseenter 事件。（这个没有疑问）
2、当鼠标移出parent的时候，会触发mouseout 和  mouseleave事件。（这个没有疑问）
3、当鼠标从parent进入son的区域的时候，触发了parent的mouseout事件。mouseleave没有触发。
4、当鼠标从son进入parent的时候，触发了parent的mouseover事件，mouseenter没有触发。

### 不是最终结果的结果

由此可以看出，虽然son的区域也属于parent区域，但是对于mouseover来说，我不管你是谁的区域啊，我只关系我在谁的上面啊，parent不开心了，在son的上面TMD不就是也在我的上面啊，mouseover说了，对不起，我只关心谁在我的 “直接” 下面，谁在我的 “直接” 下面，我就触发谁。parent没话说了。son又说，你要不要伤心，我走的时候不是给了你一个mouseout事件。。。

### 还没完

事情发展到现在，作为好学的我们，能提出什么问题来呢？

1、如果我让son全部占满parent，即:
    
    #son{
        width:100%;
        height100%;
    }

会发生什么样的情况？当鼠标进入的时候，还能不能触发`parent`的`mouseover`事件呢？提出你的猜想。

2、这个over我们真的解读对了吗？是真的因为son在鼠标的“直接”下面才触发它的吗？还是说因为son是parent的子元素才触发son啊，是不是只要是从父元素进入子元素都会触发父元素的mouseout事件呢？提出你的猜想。1

### 去试验吧

针对每个问题去写试验案例

### 又一个结论

这个结论得由你自己来得出。

### 科学的学习方法

    提出问题 <--
      ⬇️       | 
    给出猜想    |
      ⬇️       |
    试验验证    |
      ⬇️       |
    结果分析    |
      ⬇️       |
    得出结论 -->
    


