---
title: JavaScript高级程序设计笔记(部分)
date: 2017-07-01 21:17:30
tags: [JS, notes]
categories: [JS]
---
#### 6.1 理解对象
- 属性类型

    属性类型分为访问器属性和数据属性
    
    **访问器属性:**
    
    **数据属性:** 
- 定义多个属性

    defineProperties() 接收两个对象参数：一个是要添加和修改其属性的对象；另一个是第一个对象中要添加和修改的属性
- 读取属性的特性


#### 6.2 创建对象
- **工厂模式(定义函数，引用函数)**
- **构造函数模式(构造函数)**
- **原型模式**
    
    ```
    function Person() {
    }
    
    Person.prototype.name = "yang";
    Person.prototype.age = 90;
    Person.prototype.job = "software";
    Person.prototype.sayName = function () {
        console.log(this.name);
    }
    var person1 = new Person();
    ```

    - **理解原型对象**
        
        创建一个函数的时候回根据一组特定的规则为该函数传建一个prototype的属性，这个属性指向函数的原型对象。
        
        在默认情况下，所有原型对象都会自动获得一个constructor属性，这个属性的值是一个指向prototype属性所在函数的指针
    - **原型对象与in操作符**

        **in操作符(两种使用方式)：** 一是for (var in )   二是单独使用判断对象是否能够访问给定的属性
        
        ```
        function Person() {
        }
        Person.prototype.name = "yang";
        Person.prototype.age = 90;
        Person.prototype.job = "software";
        Person.prototype.sayName = function () {
            console.log(this.name);
        }
        var person1 = new Person();
        person1.age = 78;
        for (var item in person1) {
            console.log(item);
        }//name, age, job, sayName
        console.log("name" in person1); //true
        console.log(person1.hasOwnProperty('name'));//false
      ```

    - **更简单的原型语法**
            ```
            function Person1() {
            }
            Person1.prototype = {
                constructor: Person1,  //需要重新给constructor赋值
                name: "yang",
                age: 90,
                job: "software",
                sayName: function () {
                    console.log(this.name)
                }
            }

            var friend = new Person1();
            console.log(friend.constructor == Person1);//true
            ```

    - **原型的动态性**
        
        我们对原型对象所做的修改能够立即在实例上反映出来
            
            ```
            function Person3 () {
            }
            var friend11 = new Person3();
            Person3.prototype = {
                name: "yy",
                constructor: Person3
            } 
            //以字面量的方式重写原型对象会切断现有原型与任何之前已经存在的对象实例之间的联系
            console.log(friend11.name); //undefined
            ```

    - **原生对象的原型**
    
        通过原生对象的原型，不仅可以取得所有默认的方法的应用，也可以定义新的方法
    - **原型对象的问题**
    
        原型中所有属性是被很多实例共享的，这种共享对于函数非常合适，但对于包含引用类型的值得属性来说就不方便了。
        
            ```
            function Person() {};
            Person.prototype = {
                constructor: Person,
                name: ["11", "222"]
            }
            var person1 = new Person();
            var person2 = new Person();
            person1.name.push("333");
            console.log(person2.name); //"11", "222", "333"
            ```
        
        
- **组合使用构造函数模式和原型模式**

    创建自定义类型最常见的方式，构造函数模型用于定义实例属性，而原型模式用于定义方法和共享的属性。
    
    ```
    function Person22(name, age, job) {
        this.name = name;
        this.age = age;
        this.job = job;
        this.friends = ["ann", "may"];
    };

    Person22.prototype = {
        constructor: Person22,
        sayName: function() {
            console.log(this.name);
            console.log(this);//指向调用它的实例对象
        }
    };

    var people1 = new Person22("lily", "34", "doctor");
    var people2 = new Person22("jue", "45", "master");
    people1.friends.push("Van");
    people1.sayName(); // lily
    console.log(people1.friends === people2.friends); //false
    console.log(people1.sayName === people2.sayName); //true
    ```


- **动态原型模式**
    
    可以通过检查某个应该存在的方法是否有效来决定是否需要初始化原型。
    
    ```
    function Person(name, age) {
        this.name = name;
        this.age = age;
        if(typeof this.sayName != 'function') { //根据判断来动态构建原型
            Person.prototype.sayName = function () {
                console.log(this.name);
            }
        }
    }

    var friend = new Person("yyy", "89");
    friend.sayName(); //yyy
    ```
    

- **寄生构造函数模式**
    
    构造函数在不返回值的情况下,默认会返回新对象实例。而通过在构造函数的末尾添加一个return语句，可以重写调用构造函数时返回的值.
    ```
    function Person(name, age) {
        var o = new Object();
        o.name = name;
        o.age = age;
        o.sayName = function () {
            console.log(this.name);
        }
        return o;
    }

    var people = new Person("yyyy", '89');
    people.sayName(); //yyyy
    ```
    例：创建一个具有额外方法的特殊数组，由于不能修改Array构造函数，因此可以使用这个模式
    
    ```
    function specialArray() {
        var values = new Array();
        values.push.apply(values, arguments);
        values.toFormatString = function() {
            return this.join('|');
        }
        return values;
    }
    
    var values = new specialArray("red", "blue");
    console.log(values.toFormatString());
    ```

    
- **稳妥的构造函数模式**

    稳妥对象是指没有公共属性，而且其他方法也不引用this的对象。
    
    ```
    function Person(name, age) {
        var o = new Object();
        o.name = name;
        o.age = age;
        o.sayName = function () {
            console.log(name);
        }
        return o;
    }

    var people = Person("yyyy", '89');
    people.sayName(); //yyyy
    ```
#### 6.3 继承
许多OO语言都只是两种继承方式：接口继承和实现继承。接口继承只继承方法签名，而实现继承则继承实际的方法。ECMAScript只支持实现继承，而且其实现继承主要是依靠原型链来实现的。
- **原型链**

    原型链作为实现继承的主要方法，其基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。

    ```
    实现原型链的一种基本模式：
    
    function SuperType () {
        this.property = true;
    }
    
    SuperType.prototype.getSuperValue = function () {
        return this.property;
    };
    
    function SubType () {
        this.subproperty = false;
    }
    
    SubType.prototype = new SuperType();
    SubType.prototype.getSubValue = function () {
        return this.subproperty;
    }
    
    var instance = new SubType();
    console.log(instance.getSuperValue());
    ```
    - 默认的原型
        
        所有的引用类型默认都继承了Object,这个继承也是通过原型链实现的，指向Object.prototype
    
    - 原型和实例的关系
        
        第一种方式是使用instanceof操作符
        
        第二种方式是使用isPrototypeOf()方法，只要原型链中出现过的原型，都可以说是该原型链所派生的实例的原型。
        
    - 谨慎地定义方法
    
        给原型添加方法的代码一定要放在替换原型的语句之后。
        
        使用字面量添加新方法，会导致调用构造函数的方法无效
        
        ```
        SubType.prototype = new SuperType();
        //使用字面量添加新方法，会导致上面一行代码无效
        SubType.prototype.prototype = {
            getSubValue: function() {
                return this.subproperty;
            },
            
            someOtherMethod: function() {
                return false;
            }
        }
        ```
    - 原型链的问题
    
        包含引用类型的原型属性会被所有实例共享。
    
- ****借用构造函数实现继承****
    
    基本思想：在子类型构造函数的内部调用超类型构造函数
        ```
        function SuperType() {
            this.colors = ['red', 'blue'];
        }

        function SubType() {
            SuperType.call(this);
        }

        var instance1 = new SubType();
        instance1.colors.push('black');
        console.log(instance1.colors); //red,blue,black

        var isntance2 = new SubType();
        console.log(isntance2.colors); //red,blue
        ```
    - 传递参数
    ```
    function SuperType(name){
        this.name = name;
    }

    function SubType() {
        SuperType.call(this, "Amy");
        this.age = 111;
    }

    var instance = new SubType();
    console.log(instance.name + "####" + instance.age); //Amy####111
    ```

    - 借用构造函数的问题
    
    方法都在构造函数中定义，不能进行方法复用。
    
- **组合继承**
    
    基本思想： 将原型链和借用构造函数结合，使用原型链实现对原型属性和方法的继承，通过借用构造函数来实现对实例属性的继承。
    
        ```
        function SuperType(name){
            this.name = name;
            this.colors = ["red", "blue"];
        }

        SuperType.prototype.sayName = function () {
            console.log(this.name);
        }

        function subType(name, age) {
            SuperType.call(this, name);
            this.age = age;
        }

        subType.prototype = new SuperType();
        subType.prototype.constructor = subType;
        subType.prototype.sayAge = function () {
            console.log(this.age);
        }

        var instance1 = new subType("Yan", '11');
        instance1.colors.push('black');
        instance1.sayName(); //Yan
        instance1.sayAge(); //11
        console.log(instance1.colors);

        var isntance2 = new subType('YUan', "1233");
        console.log(isntance2.colors);
        ```

- **原型式继承**
    
    基本思想：借助原型可以基于已有的对象传建新对象
    
    ```
    function obj(obj) {
        function F() {}
        F.prototype = obj;
        return new F();
    }
    var person = {
        name: "aaaa",
        friends: ["shell", "Ada"]
    };

    var aperson = obj(person);
    aperson.friends.push("aperson");
    var bperson = obj(person);
    bperson.friends.push("bbperson");
    console.log(person.friends);
    
    ```
    - ES5用Object.create()方法规范化了原型式继承
    
    ```
    var person = {
        name: "aaaa",
        friends: ["shell", "Ada"]
    };

    var anotherPerson = Object.create(person);
    anotherPerson.friends.push('another');
    console.log(person.friends); //shell, Ada, another
    ```

- **寄生式继承**
    
    基本思想： 创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来增强对象，最后再返回对象。
    
    ```
    function object(obj) {
        function F() {}
        F.prototype = obj;
        return new F();
        
    }
    

    var person = {
        name: "aaaa",
        friends: ["shell", "Ada"]
    };

    function createB (obj) {
        var clone = object(obj);
        clone.sayHi = function () {
            console.log("Hi");
        }
        return clone;
    }

    var another = createB(person);
    another.sayHi();
    ```
- 寄生组合式继承

    基本思路： 通过借用构造函数来继承属性，通过原型链的混成形式来继承方法
    
    ```
    function obj(obj) {
        function F() {}
        F.prototype = obj;
        return new F();
    }

    function inherit(subType, superType) {
        var prototype = obj(SuperType.prototype);
        prototype.constructor = subType;
        subType.prototype = prototype;
    }

    function SuperType(name) {
        this.name = name;
        this.colors = ["shell", "Ada"];
    }

    SuperType.prototype.sayName = function () {
        console.log(this.name);
    }

    function SubType (name, age) {
        SuperType.call(this, name);
        this.age = age;
    }

    inherit(SubType, SuperType);

    SubType.prototype.sayAge = function () {
        console.log(this.age);
    }

    var instance = new SubType("yy", "111");
    instance.sayName(); //yy
    ```