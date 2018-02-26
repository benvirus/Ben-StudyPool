# 不单单谈继承，还有怎么继承，和为什么能继承。

### 继承是类的一种特性,类是一种设计模式，设计模式是一种解决一类问题的通用方法，是可选的。

类的三大特性：封装，继承，多态。

我们今天只讲继承。

### 怎么继承

    //通用写法：

    function Parent(){
        this.name='ben';
        this.age=18;
    }

    Parent.prototype={
        getName:function(){
            return this.name;
        },
        getAge:function(){
            return this.age;
        }
    }

    function Son(){
        Parent.call(this);
        this.salary = 10;
    }

    Son.prototype = Object.create(Parent.prototype);
    Son.prototype.getSalary = function(){
        return this.salary;
    }

    var son1 = new Son();
    var son2 = new Son();
    onsole.log(son1.getName());
    console.log(son1.getSalary());
    console.log(son1);
    console.log(son2);

通过分析son1的继承关系可以看出原型链如下：

    {
        getName:function(){
            return this.name;
        },
        getAge:function(){
            return this.age;
        }
    }
    ⬆️
    {
        getSalary:function(){
            return this.salary;
        }
    }
    ⬆️
    {
        name:'ben',
        age:18,
        salary:10
    }

### 为什么会造就出这样的原型链？

关键在于一段代码：
    
    Son.prototype = Object.create(Parent.prototype);

这段代码实现了什么？

- 1、创造了一个关联了Parent.prototype的空对象。
- 2、让Son.prototype指向这个空对象。

因此：

用Son 实例化的对象和Son.prototype关联,而Son.prototype又是一个和Parent.prototype关联的对象，所以构成了这种原型链关系。son既能访问到 `getSalary()`方法，又能访问到`getName()`方法。

### 为了构成类的继承关系，我们人为的去引入了两个函数。而实际上用的却不是继承的特性，而是原型链，对象关联的特性。

### 抛弃类和继承吧，我们用对象关联和方法委托。

    var Parent = {
        init:function(){
            this.name = 'ben';
            this.age = 18;
        },
        getName:function(){
            return this.name;
        },
        getAge:function(){
            return this.age;
        }
    }

    var Son = {
        setup:function(salary){
            this.init();
            this.salary = salary;
        },
        getSalary:function(){
            return this.salary;
        }
    }
    
    Object.setPrototypeOf(Son,Parent);
    
    var son1 = Object.create(Son);
    son1.setup(10);

    var son2 = Object.create(Son);
    son2.setup(40);

    console.log(son1.getName());
    console.log(son1.getSalary());
    console.log(son1);
    console.log(son2);


