# This的绑定策略

this 是js的核心功能，为js的函数提供了非常灵活的用法，一种类似于动态作用于的用法。同时也带来了很多迷惑，其实弄懂this并不难，只要分清this的使用场景。

总的来说，this有四种绑定策略。（有心的你可能会发现和函数调用模式的种数是一样的，更有心的你肯定会发现this只会用在函数调用中）。

### 策略一：默认绑定。

    function name(){
        console.log(this.name);
    }
    var name = 'ben;
    name();  // ben

在没有其它明确的绑定的情况下，函数内不的this会默认绑定到window对象。所以上面会输出全局变量ben。但是在严格模式下，默认的this会绑定到undefined.所以上面的函数在严格模式下会报错。

### 策略二：隐式绑定。

    function name(){
        console.log(this.name);
    }
    var person = {
      name:'ben',
      getName:name
    }
    person.getName();

再来看
    
    function name(){
        console.log(this.name);
    }
    var obj1 = {
        name:'chen',
        getName:name
    }
    var obj2 = {
        name:'ben',
        obj1:obj1
    }

    obj2.obj1.getName(); //chen
    //结论：在调用中，只有最后一层，直接调用函数的那个对象起作用。

还有

    function a(){
        console.log(this.a);
    }

    var obj = {
        a:2,
        getA:a
    }

    var getA = obj.getA;
  
    getA();  // undefined

问题：

1、如果obj1没有name属性，会输出什么？

2、如果把obj2改成如下样子呢？

    var obj2 = {
        name:'ben',
        getName:obj1.getName
    }
    obj2.getName();  //会输出什么呢？？

总的来说：隐式绑定比较难发现，调试也比较麻烦。但是灵活性比较高。

### 策略三：显式绑定。

显示绑定的三种用法apply，call和bind

    //apply
    function getAge(){
        console.log(this.age);
    }
    var obj = {
        age:25
    }
    getAge.apply(obj);

    //bind
    getAge.bind(obj)();

### 策略四：new 绑定。

    function Person(){
        this.name = 'chen';
    }
    var ben = new Person();

此时的this会绑定到新创建的对象。

### 四种绑定策略的优先级

就是在一个函数的调用中能对应到多种策略，那到底this会按照哪种规则绑定呢？优先级是如何的呢？

1、new绑定优先级最高。不管你的函数是否已经进行过隐式绑定还是显式绑定，当进行构造调用的时候，都会遵循new绑定策略。
2、显式绑定。即使函数之前进行了隐式绑定，但是在调用的时候进行了apply，call和bind，都会根据显示绑定的this进行调用。
3、隐式绑定。
4、默认绑定。
