# 申请全局唯一自增量

今天来聊聊申请全局唯一自增量。两个要求：

- 自增
- 全局唯一

### 用处

- 就是当在一个页面中，存在多个相同组件的实例时，做必要的区分。
- 就是当同一个函数通过bind绑定多个执行上下文时做必要的区分。
- 反正就是一个全局唯一个id。

### 你会怎么做？

自觉思考十秒钟。。。

##### 方法一：手工打造全局变量，我不嫌烦！

```js
var guid = 0;

console.log( guid++ );
console.log( guid++ );
console.log( guid++ );
```


##### 方法二：单利模式，每次都调用方法！

```js    
var GUID = {
    _guid:0,
    newGUID:function(){
        return this._guid++;
    }
}

console.log(GUID.newGUID());
console.log(GUID.newGUID());
console.log(GUID.newGUID());
```

##### 方法三：函数对象，其实和单例差不多!

```js
function newGUID(){
    return newGUID._guid++;
}
newGUID._guid = 0;

console.log(newGUID());
console.log(newGUID());
console.log(newGUID());
```


##### 方法四：利用闭包（推荐）

```js
var newGUID = (function(){
    var _guid = 0;
    return function(){
        return _guid++;
    }
})();

console.log(newGUID());  //1
console.log(newGUID());  //2
console.log(newGUID());  //3
```

### 优缺点分析

##### 方法一：

毫无优点可言。完全十年前面向过程的编程方式，丝毫没有发挥js函数式编程的优势。变量不穿衣服裸奔，完全暴露在全局变量空间，容易被覆盖重写，而且，不利于多人合作。

##### 方法二：

虽然穿了一层衣服，但是衣服是透明的。有被重写的可能性，但是比第一个好一些了。但是使用方式麻烦啊，每次还得去吊用这个对象的方法。

##### 方法三：

从安全性上和方法二没什么区别，因为本身单例模式和函数就没什么区别。比方法二唯一提升的就是实用方式上会更人性化。多人开发时，只需要告诉被人我这里有个申请GUID的方法叫newGUID，你们用的时候调一下就好了。

##### 方法四：

这个就没什么好说的了，私有变量`_guid`对别人完全不可见，完全操作不到，对外只提供一个newGUID的方法可以申请新的guid。

### 去尝试一下吧，屡试不爽。