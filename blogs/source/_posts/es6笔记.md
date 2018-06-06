---
title: es6基础知识点
date: 2018-4-10
tags: [js]
categories: [前端, js]
---

#### 关于定义(声明)变量:
- 之前:     var a=12;
        let a=12

作用域:
    全局
    函数作用域

- let       相当于之前的 var
- const     定义完变量，必须有值，不能后赋值，不能修改

    let注意:
    1. 没有预解析，不存在变量提升
        在代码块内，只要let定义变量，在之前使用，都是报错
        先定义完，在使用

    2.  同一个作用域里， 不能重复定义变量
    3.  for循环，for循环里面是父级作用域，里面又一个作用域
    

```
let a = 5;
function fn() {
    alert(a);
    let a = 12;//报错
}
fn();
```


```
for(let i =0; i <3; i++) {
    //let i = 'aba';
    console.log(i);
}
```

```
{
    let a = 5;
    {
        console.log(a);
    }
}
```
- 块级作用域
```
块级作用域:
    {
        //块级作用域
    }
    
    {{{{let a = 12}}}}
    
    if(){xx}
    for(){}
    while(){}
```

- Object.freeze(对象)

```
const config={
    host: 'qq'
    username: '33'
    password: '44'
    version: '55'
}
const arr = Object.freeze(['aaa', 'bbb', 'ccc']);
arr.push('ddd'); //报错

const http = require('http');//
```

#### 解构赋值
*  非常有用，特别在做数据交互  ajax
*  注意: 左右两边，结构格式要保持一致'
*  解构可以给默认值  let [a, b, c='暂无数据'] = ['aa', 'bb'];
*  交换两个数的位置
*  


```
json:
        let json = {
            name:'Strive',
            age:18,
            job:'码畜'
        };

        let {name:n,age:g, job:a} = json;
        console.log(n, g, a)
        
        //赋默认值
        let [a, b, c='暂无数据'] = ['aa', 'bb'];
        let [a, b, c='暂无数据'] = ['aa', 'bb', null];
        let [a, b, c='暂无数据'] = ['aa', 'bb', undefined];
        console.log(a, b, c);
        
        
        let {a, b} = {a: 'aaa', b: 'bbb'};
        console.log(a, b)
        
        
        let a;
        ({a} = {a:'apple', b:'bbb'});
        console.log(a)
        
        //交换两个数的位置
        let a = 12;
        let b = 13;
        [a, b] = [b, a];
        console.log(a, b);
        
        <!--赋值-->
        function getPos() {
            return {
                left: 20,
                right: 30
            }
        }

        // ({left, top} = getPos());
         // ({left, right} = getPos());
         //let {left, right} = getPos();
        let {left, top:t} = getPos();  //top has already declared
        console.log(left, t);
         alert(top); //[object Window]
         
         
         //传参
         function show(a, b='moren') {
            console.log(a,b)
        }
        show (3);
        
         function show({a='aaa', b='moren'}={}) {
            console.log(a,b)
        }
        // show ({});
        show ();
```
#### 字符串模板 ---关于字符串
支持换行
新增功能：
- 字符串查找 
    - str.includes(substr, index)  //区分大小写
    - str.startsWith()  //以谁开头
    - str.endsWith() //以谁结尾
- 字符串重复
    - str.repeat(count)

- 字符串填充： 如果原字符串的长度，等于或大于指定的最小长度，则返回原字符串。
    - str.padStart(整个字符串长度, 填充内容) 往前填充
    - str.padEnd() 往后填充

```
//赋值格式 `${{}}`
let name = 'yaaa';
let age = 18;
let str = `her name is ${name},age is ${age}`;
console.log(str);

//查找字符串
let str = 'apple banana pear';
console.log(str.includes('apple')); //true false

<!--填充字符串-->
let str = '12222';
console.log(str.padStart(6, 'xxx'));  // x12222

```

#### 函数
- 函数默认参数
    ```
    function show({x = 0, y = 0} = {}) {
        console.log(x, y);
    }
    // show ('', 'world');
    show (); 
    ```

- 函数参数默认已经定义了，不能再用let const声明

    ```
    function show(a =18) {
        let a = 101; //函数参数默认已定义，不能重复定义
        console.log(a);
    }
    show() 
    ```

- 重置参数
    ```
    function show(...a) {
        console.log(a);
    }
    show (1, 2,3,4)
    ```

    ```
    function show1() {
        let a = Array.prototype.slice.call(arguments).sort();
        console.log(a);
    }
     function show2(...a) {
        console.log(a.sort());
    }
    show1(1, 2,-3,4);
    show2(1, 2,-3,4);
    ```


    ```
    function show(a, b, ...c) {
        console.log(a, b); // 1 9
        console.log(c); //[8, 4, 5]
    }
    show (...[1,9,8,4,5]);

    //...必须放在最后一个
    function show(a, ...b, c) {
        console.log(a, b);
        console.log(c);
    }

    show (...[1,9,8,4,5]);
    ```

    ```
    //对象不适用
    let json = {
        a: 1,
        b: 2,
        c: 3
    };
    console.log(...json);
    ```


    ```
    //复制数组
    let arr = [1, 2, 3, 4];
    let arr2 = [...arr];
    let arr2 = Array.from(arr);
    arr.push(5);
    // let arr2 = [...arr];
    console.log(arr2); //[1,2,3,4]
    ```

- 箭头函数
    - 结构
    
    ```
    () => {
        语句;
        return;
    }
    
    let show = (a = 0,b = 0) => {
        console.log(a, b);
        return a + b;
    };
    console.log(show(12,7));
    ```
    - 注意点1 this问题，定义函数时所在的对象不再是运行时所在的对象
    
    ```
    var id = 12;
    let json = {
        id: 2,
        show1: function () {
            setTimeout(() => {
                console.log(this.id); //2
            },1000);
        },
        show2: function () {
            setTimeout(function () {
                console.log(this); //this指window
            }, 1000);
        }
    };
    // json.show1();
    json.show2();
    
    ```
    - 注意点2 没有arguments参数
    
    ```
    let show = (...argus) => {
        // console.log(arguments);
        console.log(argus);
    }
    show(1,2,3,4)
    ```
    - 不能用于构造函数
        
    ```
    // function show() {
    //     this.name = 'abc';
    // }

    let show = () => {
        this.name = 'abs';
    }

    let s = new show();
    alert(s.name);
    ```





#### 扩展运算符||rest运算符 ...
- 展开数组
-  1,2,3,4 --> ...1,2,3,4 --->[1,2,3,4]
```
let arr = ['apple', 'bannan', 'orange'];
        // console.log(arr);
console.log(...arr);  //apple bannan orange
```


##### 数组新增内容
- es5循环

    for while
    - forEach() filter() some() every() reduce() reduceRight()
    两个参数： 回调函数， this指向谁
    回调函数可以改为箭头函数
    
    ```
    var arr = ['apple', 'banana', 'orange'];
        arr.forEach(function (val, index) {
            console.log(this + val + index);
        }, '111');  
        arr.forEach(function (val, index) {
            console.log(this + val + index);
        }.bind('111')); 
        // 111apple0 10 111banana1  10 111orange2
        
    arr.forEach((val, index) => {
            console.log(this, val, index);
        }, 1111)  //修改为1111无效，this指向window
    ```

    - arr.map() 做数据交互映射 正常情况下需要配合return,返回一个新的数组；若无return返回则相当于forEach()
    
    ```
    var arr = [
        {name: 'aaa', value: '111', isHot: true},
        {name: 'bbb', value: '2222', isHot: false},
        {name: 'ccc', value: '3333', isHot: true},
        {name: 'ddd', value: '4444', isHot: false},
    ];
    let newArr = arr.map((item, index, arr) => {
        let json = {};
        json.n = `${item.name}` + '999';
        json.v = +item.value + 200;
        json.isHot = item.isHot ? '666': '000';
        return json;
    })

    console.log(newArr); // 
    ```
    - arr.filter()  过滤数组，如果回调函数返回true，则保留
    
    ```
    let newArr = arr.filter((item, index, arr) => {
        // if (item.isHot) {
        //     let json = {};
        //     json.n = `${item.name}` + '999';
        //     json.v = +item.value + 200;
        //     json.isHot = item.isHot ? '666': '000';
        //     return json;
        // }
        return item.isHot == true;

    })
    ```

    - arr.some() 类似查找数组里面某一个元素符合条件，返回true
    - arr.every()数组里所有元素都符合条件才返回true
     
    ```
    let numArr = [2, 3, 6, 4];
    let newArr = numArr.every((item, index) => {
        return item % 2 == 0;
    })
    console.log(newArr)
    ```
    - arr.reduce(prev, cur, index, arr)  //从左往右
    
    - arr.reduceRight() //从右往左
    prev上一次计算的结果
    - for...of  arr.keys()//数组下标  arr.entries()//数组某一项
- ES 6新增
    - Array.from() 把类数组（获取一组元素，arguments）等转成数组
    
        ```
        let json = {
            0: 'apple',
            1: 'banana',
            2: 'cancel',
            length: 3  //
        };
        let aa = Array.from(json);
        console.log(aa);  //有length 打印正常，无length打印undefined
        
        let json = {
            e: 'apple',
            f: 'banana',
            g: 'cancel',
            length: 3  //
        };  //undefined undefined undefined
        
        let json = {
                2: 'apple',
                3: 'banana',
                4: 'cancel',
                length: 3
            }; //undefined undefined apple
        ```

    - Array.of()把一组值转成数组
        ```
        Array.of('apple', 'banana', 'orange')        
        ```
    - arr.find() //返回第一个符合条件的元素，若没有返回undefined
    - arr.fundIndex()//返回第一个符合条件的元素的位置，若没有返回-1
    - arr.fill(填充东西，开始位置，结束位置) //包括开始位置，不包括结束位置

        ```
        let arr = new Array(4);
            arr.fill('000', 1, 3);
            console.log(arr);  // empty, "000", "000", empty

        ```
    - arr.inclueds() //返回bool值

##### 对象

- 对象简介
    
    ```
    let name = 'nike';
    let age = '18';
    let json = {
        name,
        age,
        showName() {
            console.log(this.name);
        },
        showNameA: () => {
            console.log(this.name);
        } //?
        
    };
    json.showName();
    ```
- Object.is()比较两个值是否相等

    ```
    console.log(Object.is(NaN, NaN)); //true
    console.log(Object.is(+0, -0)); //false
    ```
- Object.assign(目标对象, 源对象1, 源对象2... )//合并对象

    作用： 复制一个对象、合并参数
        
    ```
    let jsona = {a : '111'};
    let jsonb = {b : '222'};
    let jsonc = {c : '333'};
    let jsond = {d : '444', a: '5555'};
    let jsonO = Object.assign({}, jsona, jsonb, jsonc, jsond);
    console.log(jsonO);
    ```

- Object.keys() Object.entries() Object.values() ES2017引入
    
    ```
    let {keys, values, entries} = Object;
    let json = {
        a: '1',
        b: '2',
        c: '3'
    };
    for (let key of keys(json)){
        console.log(key);
    }
    for (let val of values(json)) {
        console.log(val);
    }
    
    for (let [key, val] of entries(json)) {
        console.log([key, val]);
    }
    ```
##### Promise解决异步回调的问题

- 传统解决方案： 回调函数，事件驱动
- 语法
    
    ```
    let promise = new Promise(function (resolve, reject){
        
    })

    promise.then(res=>{}, err=>{});

    promise.catch(err=>{}) //发生错误别名


    eg:
    let a = 10;
    let promise = new Promise(function (resolve, reject) {
        if (a == 5) {
            resolve('success');
        } else {
            reject('fail');
        }
    });
    promise.then(res => {
        console.log(res); //success
    }, err => {
        console.log(err) //fail
    }).catch(err => {
        console.log(err); //fail
    });
    ```
常用法： new Promise().then().catch()

- Promise.resolve('aa') 将现有的转成resolve状态的promise对象

    ```
    new Promise(resolve => {
        resolve('aaa');
    }); //等价于Promise.resolve('aaa')
    ```

- Promise.reject('aaa') 将现有的状态转成reject状态的promise对象

    ```
    new Promise(reject => {
        reject('aaa');
    }); //等价于Promise.reject('aaa')
    ```
- Promise.all([p1, p2, p3]):把Promise打包，放进一个数组中 必须确保所有的promise对象都是resolve状态

    ```
    let p1 = Promise.resolve('aaaa');
    let p2 = Promise.resolve('bbb');
    let p3 = Promise.resolve('cccc');
    Promise.all([p1, p2, p3]).then(res => {
        console.log(res)
    });
    ```

- Promise.race([p1, p2, p3]) //只要有先改变状态就返回


##### 模块化

- es6之前基本模块
    - Commonjs 主要在服务端 nodeJs require('http')
    - AMD requireJS
    - CMD seaJS
    
- 如何定义模块
    
    ```
    export default a = 12;
    引用的时候可以不用{}

    export 
    一个文件里面可以导出多个

    定义别名
    const a = 110;
    const b = 2;
    const c = 22;

    export {
        a as apple,
        b as banana,
        c as ddd

    }

    ```


- 如何使用

    ```
    <script type="module">

    import {a as apple} from ...

    import * as moduleName from ...

    </script>

    ```
- import:
 - 相对路径或者决对路径
 - import模块只会导入一次，无论引入多少次
 - import './sss.js'只是引入文件
 - 有提升效果，会自动提升到顶部，首先执行
 - 到处去模块内容，如果里面有定时器改动，外面也会改动
 - import()类似于require,可以动态引入，默认import不能写到if之类的里面。返回值是promise对象

    ```
    import(../).then(res => {})
    1. 按需加载
    2. 可以写入if语句里面
    3. 路径也可以动态加载
    ```


---
##### asys 

##### Class
- Es6面向对象： class关键字，class里面直接加方法
- 继承
    ```
    //构造函数
    function User(name, pass) {
        this.name = name;
        this.pass = pass;
    }

    User.prototype.showName = function () {
        console.log(this.name);
    }

    User.prototype.showPass = function () {
        console.log(this.pass);
    }

    var user = new User('yang', '123456');
    user.showName();
    user.showPass();
    ```


    ```
    //class
    class User {
        constructor (name, pass) {
            this.name = name;
            this.pass = pass;
        }
        
        showName() {
            console.log(this.name);
        }
        
        showPass() {
            console.log(this.pass);
        }
    }
        
        let user = new User('yang', '222');
        user.showName();
        user.showPass();
    ```

    ```
    //继承
    function User(name, pass) {
        this.name = name;
        this.pass = pass;
    }

    User.prototype.showName = function () {
        console.log(this.name);
    }

    User.prototype.showPass = function () {
        console.log(this.pass);
    }

    function VipUser(name, pass, level) {
        User.call(this, name, pass);
        this.level = level;
    }

    VipUser.prototype = new User();
    VipUser.prototype.constructor = VipUser;
    VipUser.prototype.showLevel = function () {
        alert(this.level);
    }

    var v1 = new VipUser('yang', '123456', '4');
    v1.showName();
    v1.showPass();
    v1.showLevel();
    ```

    ```
    // class继承
    class User {
        constructor (name, pass) {
            this.name = name;
            this.pass = pass;
        }

        showName() {
            console.log(this.name);
        }

        showPass() {
            console.log(this.pass);
        }
    }

    class VipUser extends User {
        constructor (name, pass, level) {
            super(name, pass);
            this.level = level;
        }

        showLevel(){
            console.log(this.level)
        }
    }

    var user = new VipUser('yang', '123456', '4');
    user.showName();
    user.showPass();
    user.showLevel();
    ```

#####     新增运算符
- ** 幂运算  2 ** 3 === Math.pow(2, 3)

##### Map和Set数据结构 (待补充)
- Set
    - Set:类似数组，但是成员的值都是唯一的，没有重复。
Set本身是一个构造函数，用来生成Set数据结构。
    - 包含的方法：
    
        add: 添加某个值，返回Set结构本身。
        
        delete: 删除某个值，返回一个布尔值，表示是否成功；
        
        has(value): 返回布尔值，表示该值是否为Set的成员；
        
        clear():清除所有成员，没有返回值
        
        遍历操作
        keys():返回键名的遍历器
        values(): 返回健值对的遍历器
        entries():返回键值对的遍历器
        forEach(): 每个成员
- WeakSet

    1.weakSet的成员只能是对象，不能是其他类型的值
    2.weakSet对象都是弱引用。如果其他对象不再引用该对象，那么垃圾回收机制会自动回收该对象所占的内存，所以WeakSet是不可遍历的。
    
- Map
    
    - 他是键值对的集合（Hash结构）。他与Object结构的区别是：Object是一种“字符串-值”的对应，Map是“值-值”的对应。所以当需要“键值对”这样的数据结构时，Map比Object更合适。
    - 方法：set(key, value)
        get(key)
        has(key)
        delete(key)
        clear()
        遍历方法
        keys()
        values()
        entries()
        forEach()

- WeakMap
   
     WeakMap跟Map结构基本类似，区别是只接受对象(null除外)作为键名，不接受其他类型的值作为键名，而且键名所指向的对象，不计入垃圾回收机制。


#### generator 

- 走走停停 yield停止
- yield 传参 返回

    ```
    第一个next()没办法传参

    yield返回

    ```
