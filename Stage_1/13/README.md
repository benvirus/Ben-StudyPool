# 对象的属性描述

我对这块是不熟悉的，因为在日常的使用中是不太用得到的。但是呢，如果用好了，会让我们的程序更加的健壮和安全。

### 简介

什么叫对象的属性描述呢？就是一些用来描述对象的某个属性的值、是否可写、是否可枚举、是否可配置的一个对象。

有点绕。。。

意思就是说：针对对象的每一个属性，都存在一个对象，用来描述这个属性。

### 栗子

    var obj = {
        name:'ben',
        age:18
    }

    var nameDes = Object.getOwnPropertyDescriptor(obj,'name');
    console.log(nameDes);

    /**
    {
        value:'chen',
        configurable:true,
        writable:true,
        enumerable:true
    }
     */
    
如上所见，可以通过Object.getOwnPropertyDescriptor(obj,prop)来获取obj对象的prop属性的描述符。

### 属性描述符

- value: 当前属性的值
- configurable: 是否可配置
- writable: 是否可写
- enumerable: 是否可枚举

### configurable

我们可以通过`Object.defineProperty(obj,prop,desObj)`来配置一个对象的属性。

    Object.defineProperty(obj,'salary',{
            value:999999,
            writable:true,
            enumerable:true,
            configurable:false
        })

okay，当把configurable配置为false之后，这个属性的描述就再也不能配置了。由此可见，这个属性是单向的，当把一个对象的属性设置为不可配置的时候，他就永远不能变成可配置的了。就像死了之后就再也不能活过来了。

But !!!  但是 ！！！

这里的不可配置，仅限于通过Object.defineProperty(）来修改属性的enumerable描述和configurable描述的时候是禁止的。通过Object.defineProperty(）来修改`writable`描述和`value`都是允许的。

But !!! 但是 ！！！

在设置为不可配置之后，虽然`writable`依然是可配置的，但是当把它配置为`false`之后，`writable`描述也不能在改回`true`了.到此为止，这个属性就真的是谁也动不了了。它相对来讲是安全的了。

### enumerable

可枚举。可枚举的意思是可以通过`for...in`来枚举对象的属性；

    var obj = {
        name:'ben',
        age:18,
        salary:50
    }
    Object.defineProperty(obj,'salary',{
            value:1000000,
            configurable:false,
            writable:false,
            enumerable:false
        })
    for(var key in obj){
        console.log(key);
    }

    //
    name
    age

### writable

是否可写。

当把一个属性的可写描述符配置为false的时候，就不可以修改此属性的值了。
