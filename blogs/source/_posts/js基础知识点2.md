---
title: js基础知识点2
date: 2018-1-20
tags: [js]
categories: [前端, js]
---

### 作用域和闭包
- 执行上下文

    - 范围： 针对一段<script>或者一个函数里面都会生成一个执行上下文
    - 全局：针对<script>生成全局上下文，进行全局的变量定义、函数声明
    - 函数：在函数内部，进行局部的变量定义、函数声明、this赋值、arguments赋值
    
    ```
    console.log(a); //undefined
    var a = 100;
    
    fn('yang');
    function fn (name) {
        age = 20;
        console.log(name, age);
        var age;
    }
    //"yang" 20
    ```

- this    
    this要在执行时才能确认值，定义时无法确认
    this执行的四种场景
    - 构造函数执行 
    - 作为对象属性执行 
    - 作为普通函数执行 this指向window
    - call, applay, bind
    
    ```
    //作为构造函数执行
    function Name(name) {
        this.name = name;
        this.printName = function () {
            console.log(this.name)
        }
    };
    var newFn = new Name("yang");
    newFn();
    
    
    作为对象属性，this指向该对象
    var a = {
        name: 'A',
        fn: function () {
            console.log(this.name, this.class)
        },
        class: "classB"
    }
    a.fn() //A  this==a 
    
    //
    a.fn.call({name: "B"}) //this == {name: "B"}
    
    //作为普通函数执行
    var fn1 = a.fn;  //this == window
    fn1()
    
    //call
    function fn1 (name, age) {
        alert(name);
        console.log(this);
    }
    fn1.call({x:100}, "zhangsan", 20)
    fn1.apply({x:100}, ["yang", 20]) //传参写法不一样
    
    //bind
    var fn2 = function (name, age) {
        console.log(name + ':' + age);
        console.log(this);
    }.bind({x:'12'});
    fn2("yang", 20);
    ```
    
- 作用域
    
    ```对于函数作用域，
    
    //函数和全局作用域
    var a = 100;
    function fn() {
        var a = 200;
        console.log('fn', a); 
    }
    
    console.log('global', a); //global 100
    fn();  //fn 200
    
    <!--函数作用域-->
    function fn() {
        var b = 200;
        console.log('fn', b);
    }
    
    console.log('global', b); //error
    ```
- 自由变量： 当前作用域没有定义的变量，即“自由变量”

    ```
    var a = 100;
    function name () {
        var b = 300;
        //当前作用域没有定义的变量，即“自由变量”
        console.log(a);
        
        console.log(b);
    }
    name (); //100 300
    ```

- 作用域链

    ```
    var a = 100;
    function F1() {
        var b = 200;
        function F2() {
            var c = 300;
            console.log(a);
            console.log(b);
            console.log(c);
        }
        F2(); // 100 200 300
    } 
    F1()  //100 200 300 
    ```

- 闭包

    - 使用场景1：函数作为返回值
    
        ```
        function fn1 () {
            var a = 100;
            return function () {
                console.log(a)
            }
        }
        var a = 200;
        var fn2 = fn1();
        fn2 (); //100
        ```
    - 使用场景2：函数作为参数传递
    
        ```
        function fn1 () {
            var a = 100;
            return function () {
                console.log(a)
            }
        }
        
        var fn2 = fn1();
        
        function fn3 (fn) {
            var a = 200;
            fn();
        }
        
        fn3(fn2); //100
        ```       

### 异步和单线程
- **异步**
    - 同步和异步的区别:
    同步会阻塞程序的执行，同步相当于alert,一件事完成之后才做另一件事。
    
        ```
        //异步
        console.log ('100');
        setTimeout(function () {
            console.log('200');
            
        }, 1000);
        console.log('300');
        
        //同步
        console.log('100');
        alert('200');
        console.log('300');
        ```
    - 异步使用场景
        - 定时任务 setTimeout setInterval
        - 网络请求 ajax 动态图片加载
        - 事件绑定
- **单线程**    
    - 一次只能干一件事


### 日期
常用的获取日期

    // 获取当前时间的毫秒数
    Date.now() 
    
    
    //获取当前时间
    dt1 = new Date(); 
    
    //获取dt1时间的毫秒数
    dt1.getTime(); 
    
    //获取dt1的年份
    dt1.getFullYear();
    
    //获取dt1的月份 0-11
    dt1.getMonth();
    
    //获取dt1的日期1-31
    dt1.getDate()
    
    //获取dt1的时间
    dt1.getHours()
    dt1.getMinutes()
    dt1.getSeconds()
     
    ```
    
### Math.random()
0~1之间的小数，且长度随机

```
获取位数一致的随机数
var random = Math.random() + '0000000000';
random = random.slice(0, 10);
console.log(random);

```


### 数组API

- forEach
- every: 所有元素都必须满足条件才会返回true
    
    ```
    var arr= [1, 2, 3, 4, 5];
    var result = arr.every(function (item) {
        if (item < 4){
            return item;
        }  
    });
    console.log(result); //false
    }
    ```
- some: 有元素满足条件就会返回true
- sort 排序
- map：重新组装元素生成新的数组

    ```
    var arr= [1, 2, 3, 4, 5];
    var result = arr.map(function (item) {
        return "<span>" + item + "</span>"
    });
    console.log(result); 
    ```
- filter

### 对象API
- for in


### DOM操作 Document object model
- DOM本质 

    浏览器把html文本结构化成浏览器可识别和js可操作的一种模型。树结构
- DOM操作
    - DOM节点操作
        - 获取节点信息
            ```
            var dom1 = document.getElementById('name'); //元素
            document.getElementsByClassName('name')//集合
            document.getElementsByTagName('p')//集合
            document.querySelectorAll('div')//集合
            
            //property:js对象中的标准属性的获取和修改
            dom1.style.width;
            dom1.className
            dom1.nodeName
            
            //attribute： html标签属性的获取和修改
            dom1.getAttribute('data-name');
            dom1.setAttribute('data-name', 20);
            
            dom1.getAttribute('style');
            dom1.setAttribute('style', "font-size:20px");
            
            ```
        - 新增节点元素
        
            ```
            <div id="name"></div>
            <p id="class">111</p
            var div1 = document.getElementById('name');
            //新增节点
            var p1 = document.createElement('p');
            p1.innerHtml = 'yang';
            //添加节点
            p1.appendChild(div1);
            
            //移动已有节点
            var p2 = document.getElementById('class');
            div1.appendChild(p2);
            
            //获取父元素
            var parent = div1.parentElement; 
            
            //获取子元素
            var children = div1.childNodes; //列表
            
            //删除子元素
            div1.removeChild(children[0])
            
            ```

        - 获取父元素 div1.parentElement
        - 获取子元素 div1.childNodes //列表
        - 删除节点 removeChild
            
### BOM browser object model
- 检测浏览器的类型
    - navigator 浏览器相关信息
    ```
    // 判断是否是chrome浏览器
    var ua = navigator.userAgent;
    ua.indexOf('Chrome') > 0
    ```
    - screen 获取屏幕的宽和高
    
    ```
    screen.width
    screen.height
    ```
- 拆解URL各个部分
    - location
    
    ```
    http://tieba.baidu.com/f?kw=ireader&fr=index&red_tag=w2931729402
    hash:""
    host:"tieba.baidu.com" //域名
    hostname:"tieba.baidu.com"
    href:"http:/tieba.baidu.com/f?kw=ireader&fr=index&red_tag=w2931729402"
    origin:"http:/tieba.baidu.com"
    pathname:"/f"
    port:""
    protocol:"http:"
    search: "?kw=ireader&fr=index&red_tag=w2931729402" //参数
    ```
    - history
    
    ```
    history.back();
    history.forward();
    ```
### 事件
- 事件绑定
    ```
    var div1 = doocument.getElementById('#name');
    div1.addEventListenr('click', function () {
        console.log('111');
    })
    ```
- 事件冒泡
    e.stopPropagation() //阻止事件冒泡

- 事件代理
    利用事件冒泡将点击事件绑定在上层dom中
    
- 通用事件绑定方法demo

    ```
    <div id="btnWrapper">
        <a href="www.baidu.com">点击1</a>
        <a href="www.baidu.com" id="clickEvent2">点击2</a>
        <a href="www.baidu.com" id="clickEvent3">点击3</a>
        <p id="clickEvent1">撤销1</p>
    </div>
    
    var btns = document.getElementById('btnWrapper');
    var clickEvent = document.getElementById('clickEvent1');
    function bindEvent (ele, type, selector, fn) {
        var target;
        if (!fn) {
            fn = selector;
            selector = null;
        }
        ele.addEventListener(type, function (e) {
            target = e.target;
            if (target.matches(selector)) {
                // 事件代理
                fn.call(target, e)
            } else {
                fn(e);
            }
        })
    }
    bindEvent (btns, 'click', 'a', function (e) {
        e.preventDefault();
        console.log(this.innerHTML);
    });

    bindEvent (clickEvent, 'click', function (e) {
        e.stopPropagation();
        console.log(clickEvent.innerHTML);
    });
    ```


### Ajax
- XMLHttpRequest及状态码

    ```
    var xhr = new XMLHttpRequest();
    xhr.open('GET', '/api', false);
    xhr.onreadystatechange = function () {
        if (xhr.readyState == 4) {
            if(xhr.status == 200) {
                alert(xhr.responseText)
            }
        }
    }
    xhr.send(null)
    
    ```
- 状态码说明readyState和status
    - readystate
        - 0: 未初始化，还没有调用send()方法
        - 1：载入，已调用send()方法，正在发送请求
        - 2：载入完成，send()方法执行完成，已经接收到全部响应内容
        - 3：交互，正在解析响应内容
        - 4：完成，响应内容解析完成，可以在客户端调用了
        
    - status
        - 2XX 表示成功处理请求
        - 3XX 需要重定向，浏览器直接跳转
        - 4XX 客户端请求错误 如404
        - 5XX 服务端错误

- 跨域
    - 相关概念
    
        - 浏览器有同源策略，不允许ajax访问其他域接口
        
        - 跨域条件：协议、域名、端口，有一个不同就算跨域
        
        - http默认端口80 https默认端口403
        
        - 所有的跨域请求都必须经过信息方允许
        
        - 如果未经过允许即可获取，那是浏览器的同源策略出现漏洞
        
    - 可跨域的三个标签
        - <img src="XXXX"> 用于打点统计
        - <link href=""> 可以使用CDN
        - <script src=""></script> JSONP
        
    - JSONP实现原理
    
        ```
        <script>
            window.callback = function (data) {
                // 跨域得到的消息
                console.log(data)
            }
        </script>
        
        <!--当访问src里面地址时，服务器会拼接数据并返回callback()方法-->
        <script src="www.imook.com/api.js"></script>
        ```
    - 服务端设置http header也可以实现跨域
    
### 存储
- cookie    
    
    - 本身用于客户端和服务端通信。但它有本地存储的功能，于是被‘借用’存储本地数据
    - 缺点： 
        
        ```
        存储量大小，只有4kb;
        所有http请求都带着，会影响获取资源的效率;
        api简单，如果数据量过多的话需要封装才能使用; document.cookie = document.cookie="name="+username;
        ```
- locationStorage 和 sessionStorage

    - HTML5专门为存储而设计，最大容量5M
    - API 简单易用 localStorage.setItem(key, vulue); localStorage.getItem(key)

### Git
- git常用命令


### 模块化

- AMD 异步模块定义
    
    全局define函数 全局require函数 依赖js会自动、异步加载
    

- CMD 同步模块定义
    

### linux基本命令

### 页面加载
- 加载资源形式
    - 输入url(或跳转页面)加载html
    - 加载html中的静态资源
- 加载一个资源的过程
    - 浏览器根据DNS服务器得到域名的IP地址
    - 向这个IP的机器发送http请求
    - 服务器收到、处理并返回http请求
    - 浏览器得到返回内容
- 浏览器渲染页面的过程
    - 根据HTML结构生成DOM Tree
    - 根据CSS生成CSSDOM
    - 将DOM和CSSDOM整合形成RenderTree
    - 遇到<script>时，会执行并阻塞渲染
    
- window.onload 和 DOMContentLoaded
    - onload也难的全部资源加载完才会执行，包括图片、视频等
    - DOM渲染完即可执行，此时图片、视频还可能没有加载完
    
### 性能优化
- 原则
    - 多使用内存、缓存或者其他方法
    - 减少CPU计算、减少网络
    
- 从哪里入手
    - 加载页面和静态资源
    - 页面渲染
    
- 加载资源优化
    - 静态资源的压缩合并
    - 静态资源缓存
    - 使用CDN让资源加载更快
    - 使用SSR后端渲染，数据直接输出到HTML里面
    
- 渲染优化
    - css放前面，js放后面
    - 懒加载(图片懒加载、下拉加载更多)
    - 减少DOM查询，对DOM查询做缓存
    - 减少DOM操作，多个操作尽量合并在一起执行
    - 事件节流
    - 尽早执行操作(如DOMContentLoaded)
    
### 安全性
- XSS跨站请求攻击
    - 含有<scirpt></script>代码
    - 解决办法：前端转义

- XSRF跨站请求伪造
