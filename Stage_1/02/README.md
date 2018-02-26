# 类型检查的相关知识和用法

### 基本数据类型

javascript的基本数据类型：

    null,undefined,number,string,boolean,symbol(ES6)

### 基本数据类型的类型检查typeof

除`null`之外的其它基本数据类型的值用`typeof`去检测的时候都能够得到准确的数据类型：

    typeof 1   //'number'
    typeof 'a'   //'string'
    typeof true   //'boolean'
    typeof undefined   //'undefined'
    typeof Symbol()   //'symbol'

比较另类的是`null`

    typeof null    // object

其实也比较好理解，毕竟 Object.prototype.\_\_proto\_\_ === null

如何检查一个变量的类型是不是`null`:

    var a = null;
    if(!a && typeof a === 'object'){
        // ......
    }

### typeof的返回值是一个string类型

    typeof typeof 1   // 'string'

### 这是一个有用的"错误"

如果一个变量没有经过声明直接引用会报一个引用错误

    console.log(a)   // ReferenceError: a is not defined
    if(a){......}    // ReferenceError: a is not defined

但是：对一个没有经过申明的变量进行typeof操作

    console.log(typeof a)  // undefined 不会报错

这是javascript一个历史悠久的设计上的错误，但是现在如果去修正这个错误会带来太多的遗留问题。但是有时候这也是一个有用的错误。

当一个js插件引入之后往往会在全局空间里面创建一个特殊的变量，在插件的开始往往会通过检测这个变量是否已经存在来确定这个插件是否已经引入。比如videojs

    if(typeof videojs){
        //......
    }
    或者
    if(window.videojs){
        //去使用一个不存在的对象属性会得到undefined值，而不会报引用
    }

    而不是
    if(videojs){
        //如果没有引入过videojs,会报错，程序会卡在那里进行不下去；
    }

### 关于等于判断的几个知识点儿

    null == undefined    // ture  值等于
    null === undefined    // false  值等于并且类型相同


