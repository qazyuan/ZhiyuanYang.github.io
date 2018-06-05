---
title: AMD、CMD和CommonJS
date: 2018-2-26
tags: [规范, js]
categories: [前端, 规范]
---

##### 总述

AMD/CMD/CommonJs是JS模块化开发的标准，目前对应的实现是RequireJs/SeaJs/nodeJs.


##### 1. CommonJs
- 定义模块

  一个单独的文件是一个模块，有单独的作用域

- 模块输出
    
    输出： module.exports对象

    ```
    //模块定义 myModel.js
    
    var name = 'Byron';
    
    function printName(){
        console.log(name);
    }
    
    function printFullName(firstName){
        console.log(firstName + name);
    }
    
    module.exports = {
        printName: printName,
        printFullName: printFullName
    }
    
    //加载模块
    
    var nameModule = require('./myModel.js');
    
    nameModule.printName();
    ```
        
- 加载模块
    
    加载模块使用require方法，读取文件并执行，返回文件内部的module.exports对象。

##### 2.AMD
AMD即Asynchronous Module Definition，中文名是异步模块定义的意思。它是依赖前置，会先尽早地执行(依赖)模块。即：所有的require都被提前执行。

对于AMD,最具代表性的就是RequireJs

    ```
    AMD规范值定义了一个函数define，它是全局变量：
    define(id, dependencies, factory)
    
    // 定义模块
    //文件名字a.js
    define(function(){
        var util = {
            name: 'Byron',
            printName: function (){
                console.log(name);
            }
        }
        return {
            util
        };
    });
    
    // 加载模块
    require(['a.js'], function (util){
    //依赖于a.js文件，返回util对象
    　 util.printName();
    });
    ```
    
- 语法
    ```
    define(id, dependencies, factory);
    id: 可选参数，用来定义模块的标识，如果没有提供该参数，模块的名称应该默认为模块加载器请求的指定脚本的名称。如果提供了该参数，模块名必须是'顶级'的和绝对的(不允许相对名称)；
    
    dependencies:是一个当前模块依赖的模块名称数组。如果忽略此参数，它应该默认为['require', 'exports', 'module'];
    
    factory：工厂方法，模块初始化要执行的函数或对象，它应该只被执行一次。如果是对象，此对象应该为模块的输出值。
    
    ```
    
    ```
    require([dependencies], function() {})
    第一个参数是一个数组:表示所依赖的模块
    第二个参数是一个回调函数，当前面指定的模块都加载成功后，它将被调用。
    ```

##### 3. CMD
CMD即Common Module Definition通用模块定义。一个模块就是一个文件，它推崇依赖就近，想什么时候require就什么时候加载，实现了懒加载(延迟执行)

对于CMD，有代表性的是SeaJS

```
语法：define(factory)
define(function(require, exports, module) {
    <!--模块代码-->
}

factory为函数时表示模块的构造方法，factory方法在执行时，默认会传入三个参数，function(require, exports, module)

- require 是一个方法，接受 模块标识 作为唯一参数，用来获取其他模块提供的接口：require(id)
- exports 是一个对象，用来向外提供模块接口
- module 是一个对象，上面存储了与当前模块相关联的一些属性和方法
```
例子
```
//定义模块 myModules.js
define(function (require, exports, module) {
  var $ = require('jquery.js');
  $('div').addClass('active');
});

//加载模块
seajs.use(['myModules.js'], function () {
    
})
```


##### Require和Sea.js的异同
- 共同点： 都是模块加载器
- 不同点： 
    - 定位有差异：
    
        RequireJS 想成为浏览器端的模块加载器，同时也想成为 Rhino / Node 等环境的模块加载器。Sea.js 则专注于 Web 浏览器端，同时通过 Node 扩展的方式可以很方便跑在 Node 环境中。
    
    - 遵循的规范不同
    - 推广理念有差异
    
        RequireJS 在尝试让第三方类库修改自身来支持 RequireJS，目前只有少数社区采纳。Sea.js 不强推，采用自主封装的方式来“海纳百川”，目前已有较成熟的封装策略。
        
    - 对开发调试的支持有差异
    
        Sea.js 非常关注代码的开发调试，有 nocache、debug 等用于调试的插件。RequireJS 无这方面的明显支持。
    
    - 插件机制不同
        
        RequireJS 采取的是在源码中预留接口的形式，插件类型比较单一。Sea.js 采取的是通用事件机制，插件类型更丰富。
    
    
##### AMD与CMD区别
- AMD/CMD区别，虽然都是并行加载js文件，但还是有所区别，AMD是预加载，在并行加载js文件同时，还会解析执行该模块（因为还需要执行，所以在加载某个模块前，这个模块的依赖模块需要先加载完成）；而CMD是懒加载，虽然会一开始就并行加载js文件，但是不会执行，而是在需要的时候才执行。

- AMD依赖前置，预执行（异步加载：依赖先执行）
- CMD依赖就近，懒(延迟)执行(运行到需加载，根据顺序执行)
    
    ```
    // CMD
    define(function(require, exports, module) {  
    
      var a = require('./a')  
    
      a.doSomething();  
    
      // 省略1万行 
    
      var b = require('./b') // 依赖可以就近书写  
    
      b.doSomething();
    
    })
    
    // AMD 默认推荐的是
    define(['./a', './b'], function(a, b) { // 依赖必须一开始就写好  
    
      a.doSomething();  
    
      // 省略1万行  
    
      b.doSomething();
    
    }) 
    ```