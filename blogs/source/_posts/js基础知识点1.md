---
title: js基础知识点1
date: 2017-11-20
tags: [js]
categories: [前端, js]
---

### js变量类型
-   Array Function Object引用类型
-   typeof运算符：只能区分值类型的变量,引用类型除了funciton之外其他的引用类型不能区分
    
    ```
    typeof undefined; //undefeined
    typeof 'abd'; //string
    typeof 123; //number
    typeof true; //boolean
    
    typeof {}; //object
    typeof []; //object
    typeof null; //object
    typeof console.log //function
    ```
- instanceof instanceof运算符用来判断一个构造函数的prototype属性所指向的对象是否存在另外一个要检测对象的原型链上
    
    ```
    //语法
    obj instanceof Object;//true 实例obj在不在Object构造函数中
    
    //用法1 obj instanceof Object 检测Object.prototype是否存在于参数obj的原型链上。
    function Person(){};
    var p =new Person();
    console.log(p instanceof Person);//true
    
    //用法2 继承中判断实例是否属于它的父类
    function Person(){};
    function Student(){};
    var p =new Person();
    Student.prototype=p;//继承原型
    var s=new Student();
    console.log(s instanceof Student);//true
    console.log(s instanceof Person);//true
    
    ```

### 变量计算
- 强制类型转换
    -   字符串拼接 
        ```
        100 + '12' = 10012 
        ```
    -   ==运算符
        ```
        100 == '100' //true
        null == undefined //true
        
        var obj = {};
        obj.a == null;//true
        obj.a == undefined; //true
        obj.a === null; //false
        obj.a === undefined; //true
        ```
    - if语句
            
        ```
        var b = 99;
        if (b) {} //b转换成boolean类型
        
        <!--if false-->
        if(0)
        if('')
        if(null)
        if(undefined)
        if(false)
        if(NAN)
        ```
    - 逻辑运算符
    
        ```
        console.log(10 && 0); //0
        console.log(10 || ''); //10
        console.log(!window.abc); //true
        
        var a = 100;
        console.log(!!a); //true
        ```
### js中的内置函数
- Object、Array、Boolean、Number、String、Function、Date、RegExp、Error

### 如何理解JSON--js对象 JSON数据格式
    
    var aa = {a: 10; b:20};
    JSON.stringfy(aa); //把对象变为字符串
    JSON.parse(aa); //把字符串变为对象

### 原型和原型链
- 原型规则和示例
    - 所有的引用类型(数组、对象、函数),都具有对象特性，即可自由扩展属性(除了null)
    - 所有的引用类型(数组、对象、函数),都有一个_proto_(也称隐式原型)属性，属性值是一个普通的对象
    - 所有的函数都有一个prototype(也称显式原型)属性，属性值也是一个普通的对象
    - 所有的引用类型(数组、对象、函数),_proto_属性值指向它的构造函数的'prototype'属性值
        ```
        var obj = {};
        obj.name="yang";
        obj.__proto__ === Object.prototype
        ```
    - 当试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么会去它的_proto_(即它的构造函数的prototype)中寻找
- 原型与原型链

    ![原型](http://source.thankjava.com/2018/5/29/1527561668147.png)
    ![原型链](http://source.thankjava.com/2018/5/29/1527561760928.png)

    - 如何判断一个变量是数组类型
    
        ```
        var a = [];
        a instanceof Array; //true
        typeof a //"object" typeof 无法判断是否是数组
        
        ```
    - 写有一个原型链继承的例子
    
        ```
        sample1：
        function School () {
            this.schoolName = 'ucla';
        }

        function Student (name, age) {
            this.name = name;
            this.age = age;
        }
        Student.prototype = new School();
        var stu = new Student('yang', 180);
        console.log(stu.schoolName);
        ```
    - 描述new一个对象的过程
        ![image](http://source.thankjava.com/2018/5/29/1527565930494.png)
    
### 函数声明与函数表达式 

    ```
    //函数声明，声明可以放在调用后面
    fn1();
    function fn1() {} //函数声明

    //函数表达式，调用放在前面会报错
    fn2(); 
    var fn2 = function () {} //函数表达式
    ```
