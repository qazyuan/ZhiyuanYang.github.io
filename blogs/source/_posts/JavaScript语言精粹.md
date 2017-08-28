---
title: javaScript语言精粹笔记
date: 2017-06-13 02:10:45
tags: [JS, notes]
categories: notes
---

### 第一章 精华
- JavaScript是Web浏览器的语言，与浏览器的结合使它成为世界上最流行的编程语言之一
- 优秀的想法：函数、弱类型、动态对象、对象字面量表示法(基于json)、原型继承


### 第二章 语法
#### 空白
- 分隔字符序列
- 注释：两种方法// /**/

#### 标识符
- 由一个字母开头，其后可选择性地加上一个或者多个字母、数字或下划线
- 用于语句、变量、参数、属性名、运算符和标记

#### 数字
- 只有一个数字类型，它的内部被表示为64位浮点数
- NaN是一个数值，不能产生正常结果的运算结果 isNaN(number)检测NaN

#### 字符串
- 转义字符 “\”

#### 语句

- 语句通常是按照从上到下的顺序被执行。JavaScript可以通过条件语句(if和switch)、循环语句(while, for 和 do)、强制跳转语句(break、return、和throw)和函数调用来改变语句执行序列
- try语句执行一个代码块，并捕获该代码块抛出的任何异常。catch从句定义一个新的变量variable来接收抛出的异常对象。
- throw语句抛出一个异常。如果throw语句在一个try代码块中，那么控制流会跳转到catch从句中。如果throw语句在函数中，则该函数调用被抛弃，控制流跳转到调用该函数的try语句的catch从句中。

#### 表达式

#### 字面量
- 一种可以方便地按指定规格创建新对象的表示法。

<<<<<<< HEAD
#### 函数
=======
##### 函数

>>>>>>> 326e2a4607a1558025aa29c9f5a4e3cbc17317a9

### 第三章 对象
- js简单数据类型：数字、字符串、布尔值、null值和undefined值。
- 对象是可变的键控集合，数组、函数、正则表达式都是对象
- 对象是属性的容器，每个属性都拥有名字和值。 js里的对象是无类型(class-free)的：它对新属性的名字和属性的值没有限制。
- js包含一种原型链的特性，允许对象继承另一个对象的属性。

#### 对象字面量
    一个对象字面量就是包围在一对花括号中的0或多个“名/值”对。
```
var flight = {
    airline: "One",
    number: 715,
    departure: {
        IATA: "jjj",
        time: "2017",
        city: "shanghai"
    },
    arrival: {
        time: "2018",
        city: "xian"
    }
}

```

#### 检索
    要检索对象里包含的值，可以采用在[]后缀中括住一个字符串表达式的方式。也可以用.表示法代替
        console.log(flight.arrival["time"]);  ----2018
        console.log(flight.arrival.time);     ----2018
    ||运算符可以用来填充默认值
        var middle = flight.arrivar.time || " "
#### 更新
    对象里可以通过赋值语句来更新。
    如果属性名已经存在于对象里，那么这个属性的值就会被替换
    如果对象之前没有该属性名，那么该属性就被扩充到对象中。

#### 引用
    对象通过引用来传递。它们永远不会被赋值
    var x = stooge;
    x.nickname = "Curly"; //x和stooge是指向同一个对象的引用

    var a = {}, b= {}, c = {};// a、b、c每个都引用一个不同的空对象

#### 原型
-  每个对象都连接到一个原型对象，并且它可以从中继承属性。所有通过对象字面量创建的对象都连接到Object.prototype,它是JavaScript中标配的对象。
-  原型连接在更新时是不起作用的。当我们对某个对象作出改变时，不会触及该对象的原型。
-  原型连接只有在检索值得时候才会用到。当我们尝试去获取对象的某个属性值，但该对象没有此属性名，那么js会试着从原型对象中获取属性值。如果那个原型对象也没有该属性，那么再从它的原型中寻找。
-   原型关系是一种动态的关系。如果我们添加一个新的属性到原型中，该属性会立即对所有基于该原型创建的对象可见

#### 反射
```
typeof操作符对确定属性的类型是很有帮助的
注：原型链中的任何属性都会产生值
typeof(flight.toString)  //'function'
typeof(flight.constructor)  //'function'

有两种方法去处理掉这些不需要的属性：
1.让程序检查并丢弃值为函数的属性
2.使用hasOwnProperty()方法，如果对象拥有独有的属性将返回true, 原型链中的属性返回false
```

#### 枚举
for in语句可以遍历一个对象中所有的属性名。
```
var flight = {
    airline: "One",
    number: 715
}

for(var name in flight) {
    if(flight){
        console.log(flight[name] + '\n');
    }
}

// One 715

```
可以创建数组，在其中以正确的顺序包含属性名，然后可以以键值对的形式输出对象各个属性
```
    var properties = ['airline','number'];
    for(var i = 0; i <properties.length; i++) {
        console.log(properties[i] + ':' + flight[properties[i]]);
    }
    // airline:One
    // chapter1.js:84 number:715
```

取对象属性的方法
```
function allPros (obj) {
    for(var name in obj) {
        if(obj){
            if(typeof(obj[name]) == 'object') {
                for(var secName in obj[name]) {
                    console.log(secName + ':' + obj[name][secName]);
                }
            } else {
                console.log(name + ':' + obj[name] + '\n');
            }
        }
    }
}
allPros(flight);
```
#### 删除
delete运算符可以用来删除对象的属性。如果对象包含该属性，那么该属性就会被移除。不会触及原型链中的任何对象

```
var stooge = {
    first_name: 'liu',
    second_name: 'tongtong'
};
var another_stooge = Object.create(stooge);
another_stooge['first_name'] = 'yang';

console.log(another_stooge['first_name']); //yang
// 删除对象的属性可能会让来自原型链中的属性透现出来
delete another_stooge['first_name'];
console.log(another_stooge['first_name']); //liu
```
#### 减少全局变量污染
JavaScript可以随意地定义全局变量来容纳你的应用的所有资源。但是全局变量削弱了程序的灵活性，应该避免使用。

最小化使用全局变量的方法是指穿件一个唯一的全局变量

```
var Myapp = {};
Myapp.flight = {....};
Myapp.name = {....};
```

### 第四章 函数

#### 函数对象
JavaScript中的函数就是对象。每个函数对象在创建时也分配一个prototype属性。它的值是一个拥有constructor属性且值为该函数的对象。
函数是对象，所以它可以像任何其他的值一样被使用。函数可以保存在变量、对象和数组中。函数可以被当作参数传递给其他函数，函数也可再返回函数，函数也可以拥有方法。它的不同之处在于它们可以被调用。

#### 函数字面量
函数对象通过函数字面量来创建
```

var add = function (a, b) {
    return a + b;
};

函数字面量包括4个部分：
- 保留字 function
- 函数名，可省略
- 参数，多个参数用逗号分隔
- 大括号的一组语句。函数的主体，函数被调用时执行
```
函数字面量可以出现在任何允许表达式出现的地方。函数也可以被定义在其他函数中。一个内部函数除了可以访问自己的参数和变量，同时，他也能自由地访问把它嵌套在其中的父函数的参数与变量。通过函数字面量创建的函数对象包含一个连到外部上下文的链接。这被称为闭包。

#### 调用
调用一个函数会暂停当前函数的执行，传递控制权和参数的新函数。除了声明时定义的形式参数，每个函数还接收两个附加的参数：this 和 arguments。 参数this在面向对象中的值取决于调用的模式。arguments？

调用运算符是跟在任何一个函数值的表达式之后的一对圆括号。圆括号内可包含0个或多个用逗号隔开的表达式。

#### this
this的四种调用模式：方法调用模式、函数调用模式、构造器调用模式和apply调用模式。<这些模式在如何初始化关键参数this上存在差异>
```
- 方法调用
    当一个函数被保存为对象的一个属性时，我们称它为一个方法。
    当一个方法被调用时，this被绑定到该对象。
    var myObject = {
        value: 0,
        increment: function (inc) {
            this.value += (typeof(inc) == 'number') ? inc : 1;
        }
    }

    object.increment(2);
    console.log(myObject.value);

    - 方法可以使用this访问自己所属的对象，所以它能从对象中取值或者对对象进行修改。
    - this到对象的绑定发生在调用的时候。这个“超级”延迟绑定使得函数可以对this高度复用。
    - 通过this可取得它们所属对象的上下文的方法称为公共方法

```

```
    - 函数调用
    当一个函数并非一个对象的属性时，那么它就是被当作一个函数来调用的
    以此模式调用函数时，this被绑定到全局对象。
    解决方案：如果该方法定义一个变量并给它赋值为this, 那么内部函数就可以通过那个变量访问到this.
    通常把变量名命名为that.
    myObject.double = function () {
        var that = this; //this是指myObject对象
        var helper = function () {
            //helper里的this被绑定到全局对象
            that.value = add(that.value, that.value);
        }
        helper();
    }

    function add(a, b) {
        return a+b;
    }
    myObject.double();
    document.write(myObject.value);
            -匿名函数自调用
            (function(a1,a2){
                alert("Here!"+(a1+a2));
            })(1,2);

            (function(a1,a2){
                alert("Here!" +(a1+a2));
            }(1,2));

            void function(a1,a2){
                alert("Here!" +(a1+a2));
            }(1,2);

            var f = function(a1,a2){
                alert("Here!" +(a1+a2));
            }(1,2);
```


```
    - 构造器调用
    js是一门基于原型继承的语言。对象可以直接从其他对象继承属性。js是无类型的。js提供了一套和基于类的语言类似的对象构建语法。
    如果在一个函数前面带上new来调用，那么将会创建一个连接到该函数的prototype成员的新对象，同时，this会被绑定到新的对象上。
    // 构造器调用模式
        var Quo = function (string) {
            this.status = string;
        };

        Quo.prototype.get_status = function(){
            return this.status
        }

        var myQuo = new Quo("confused");
        document.write(myQuo.get_status()); //打印输出confused

    一个函数，如果创建的目的是希望结合new前缀来调用，那么它就被称为构造器函数。
    按照约定，他们保存在以大写格式命名的变量里。
```

```
    - apply调用
    apply方法可以构建一个参数数组传递给调用函数。apply方法接收两个参数，第一个是要绑定给this的值，第2个是一个参数数组
    // apply调用模式
        var array = [3, 4];
        var sum = add.apply(null, array);

        var statusObject = {
            status: "A-OK"
        };
        // statusObject并没有继承自Quo.prototype,但是我们可以在statusObject
        // 上调用get_status方法
        var status = Quo.prototype.get_status.apply(statusObject);
        console.log(status); //输出为'A-OK'

```
#### 参数
    当函数被调用时，得到arguments数组。
    函数可以通过此参数访问所有它被调用时传递给它的参数列表。

```
        var sum = function() {
            var i, sum = 0;
            for(i = 0; i < arguments.length; i++) {
                sum += arguments[i];
            }
            return sum;
        }
        document.write(sum(4, 8, 9, 6)); // 27

        因为语言的设计错误，arguments并是不一个真正的数组。它只是一个“类似数组”的对象。arguments拥有一个length属性，但它没有任何数组的方法。


```

#### 返回
    return语句可用来使函数提前返回。当return被执行时，函数立即返回而不再执行余下语句。
    一个函数总是会返回一个值。如果没有指定返回值，则返回undefined
    如果函数在调用时前面加上了new前缀，且返回值不是一个对象，则返回this(该新对象)

#### 异常

```
    var add = function(a, b) {
        if(typeof a != 'number' || typeof b != 'number'){
            throw {
                name: "TypeError",
                message: "add needs number",
                mayDay: "juejiang"
            };
        }
        return a + b;
    }
    // throw语句中断函数的执行。它抛出一个exception对象，
    // 该对象包含一个用来识别异常类型的name属性和一个描述性的message属性。也可以添加其他属性
    // 该exception对象将被传递到一个try语句的catch从句

    var try_it = function () {
        try {
            add("seven");
        } catch (e) {
            // document.write(e.name + ":" + e.message);
            document.write(e.mayDay);
        }
    }
    try_it();
    // 如果在try代码块中抛出了一个异常，控制权就会跳转到它的catch从句

```

#### 扩充类型的功能
JavaScript允许给语言的基本类型扩充功能。通过给Object.prototype添加方法，可以让该方法对所有对象都可用。这样的方式对函数、数组、字符串、数字、正则和布尔值同样适用。


```
例如：js没有专门的整数类型，通过给Number.prototype增加一个integer方法来改善
    // Function.prototype增加方法来使得该方法对所有函数可用
    Function.prototype.method = function(name, func) {
        this.prototype[name] = func;
        return this;
    }
    // 通过给Function.prototype增加一个method方法，下次增加就不必输入prototype这几个字符。

    Number.method('integer', function() {
        return Math[this < 0 ? 'ceil' : 'floor'](this);
    });

    //直接在Function上添加number方法
    Function.prototype.number = function(num) {
    return Math[num < 0 ? 'ceil' : 'floor'](num);
    }

    // var newFunc = new Function;
    document.write(Function.number(-10/3)); //-3
因为js原型继承的动态本质，新的方法立刻就被赋予到所有的对象实例上，哪怕对象实例实在方法被增加之前就创建好了。

```

#### 递归

#### 作用域

```
    作用域控制变量与参数的可见性及生命周期。
    // 作用域
    var foo = function () {
        var a = 3, b = 5;
        var bar = function () {
            var b = 7, c = 11;
            // 此时a=3,b=7,c=11
            a += b+c;
            // 此时a=3, b=7, c=21
        };

        // 此时a=3,b=5,c没定义
        bar();
        // 此时a=21,b=5
    };

    js有函数作用域。
    定义在函数中的参数和变量在函数外部是不可见的；
    在一个函数中的任何位置定义的变量在该函数中的任何地方都可见
```

#### 闭包
    该函数可以访问它被创建时所处的上下文环境，称为闭包。

#### 回调

```
request = prepare_the_request ();
send_request_asynchronously(request, functon(respons){
    display(response);
})
传递了一个函数作为参数给send_request_asynchronously函数，它将在收到响应时被调用。
```

#### 模块
模块是一个提供接口缺隐藏状态与实现的函数或对象
模块模式的一般形式是：
1. 一个定义了私有变量和函数的函数，
2. 利用闭创建可以访问私有变量和函数的特权函数；
3. 返回特权函数或者把他们保存到一个可访问到的地方

#### 级联
在一个级联中，我们可以在单独一条的语句中依次调用同一个对象的很多方法。

#### 套用
函数也是值，套用允许我们将函数与传递给它的参数相结合区产生一个新的函数。

#### 记忆
函数可以用对象去记住先前操作的结果，从而能避免无谓的运算。这种优化被称为记忆。

### 第五章 继承
在基于类的语言中，对象是类的实例，并且类可以从另一个类继承。

#### 伪类
当一个函数对象被创建时，Function构造器产生的函数对象会运行类似这样的一些代码：

```
this.prototype = {constructor: this};
新函数对象被赋予一个prototype属性，其值是包含一个constuctor属性且属性值为该函数的对象。该prototype对象是存放继承特征的地方。
// 定义一个构造器并扩充原型
        var Mammal = function(name) {
            this.name = name;
        };

        Mammal.prototype.get_name = function() {
            return this.name;
        };

        Mammal.prototype.says = function() {
            return this.saying || '';
        };

        var myMammal = new Mammal('Herb the');
        var name = myMammal.get_name();

        // console.log(myMammal);
        // console.log(name);

        // 构造Cat伪类来继承Mammal，这是通过定义它的constructor函数
        // 并替换它的prototype为一个Mammal的实例来实现。
        var Cat = function (name) {
            this.name = name;
            this.saying = 'meow';
        };
```

#### 对象说明符
构造器可能接受一大串的参数，因此要记住参数的顺序可能非常困难。
解决方案是在编写构造器时使其接受一个简单的对象说明符可能会更加友好。

```
var myObject = maker({

    first: f,
    last: l,
    state: s,
    city: c
});
```

#### 原型？？？
一个新对象可以继承一个旧对象的属性。


#### 函数化？？？
构造一个将产生对象的函数开始，该函数包括四个步骤：
1. 它创建一个新对象。
2. 选择性地定义私有实例变量和方法。（通过var语句定义的普通变量）
3. 新对象扩充方法
4. 返回新对象

#### 部件


### 第六章 数组
#### 数组字面量
数组字面量继承自Array.prototype, 而numbers_object继承自Object.prototype. 所以numbers继承了大量有用的方法。还有一个length属性，而numbers_object则没有。

```
javaScript允许数组包含任意混合类型的值
var misc = [
    'string', 98.6, true, flase, null, undefined, ['nested', 'array'], {object: true}, NaN, Infinity
];
```

#### 长度
length属性值是这个数组的最大属性名加上1.它不一定等于数组里的属性个数。
```
var myArray[100000] = true;
myArray.length  //100001
```
可以直接设置length的值。设置更大的length无需给数组分配更多的空间。而把length设小将导致所有下标大于等于新length的属性被删除。



```
通过把下标指定为一个数组的当前length,可以附加一个新元素到该数组的尾部
var numbers = ["zero", 'one', 'two', 'three', 'four', 'five', 'six'];
numbers.legth = 3;
numbers[numbers.length] = 'shi';
// 等效于
numbers.push('shi')
console.log(numbers);
```


```
// 删除数组中第3个元素
delete numbers[2]
console.log(numbers[2]); //undefined
删除该位置上的元素但是不会递减后面每元素的下标


// 任何额外的参数会在序号那个点的位置被插入到数组中
// 这对于大型数组来说可能会效率不高
第一个参数是数组中的一个序号。第二个参数是要删除的元素个数
var numbers = ["zero", 'one', 'two', 'three', 'four', 'five', 'six'];
numbers.splice(2, 2);
console.log(numbers)  //["zero", 'one',  'four', 'five', 'six'];
```

#### 枚举

```
js的数组其实就是对象。所以可以用for in语句来遍历一个数组的所有属性。
        var myArray = ["100","","11"];
        // myArray[100] = true;
        console.log(myArray.length);

        for(var i = 0; i < myArray.length; i++) {
            console.log(myArray[i]);
        }

        // for (item in myArray) {
        //     console.log(myArray[item]);
        // }
        //100  11
```
#### 混淆的地方

```
    // 数组与对象使用规则：
    // 当属性名是小而连续的整数时，你应该使用数组，否则使用对象
    var testArray = [];
    console.log(typeof(testArray)) //object
```

#### 方法

```
// js提供来了一组作用数组的方法。这些方法被存储在Array.prototype中的函数
// 并且Array.prototype是可以被扩充的。
            Array.prototype.reduce = function(f, value) {
                var i;
                for(i = 0; i < this.length; i += 1) {
                    value = f(this[i], value);
                }
                return value;
            }

            var data = [4, 8, 15, 16, 23, 42];
            var add = function(a, b) {
                return a + b;
            }

            var mult = function (a, b) {
                return a * b;
            }

            var sum = data.reduce(add, 0);
            var product = data.reduce(mult, 1)
            // console.log(sum);
            // console.log(product);

// 也可以直接给一个单独的数组添加方法
                data.total = function() {
                    return this.reduce(add, 0);
                };
                total = data.total();
                console.log(total);
```

#### 维度

```
    Array.dim = function (dimension, initial) {
        var a = [], i;
        for(i = 0; i < dimension; i++) {
            a[i] = initial;
        };
        return a;
    };

    var myArray = Array.dim(10, 0);
    // console.log(myArray);

    Array.matrix = function (m, n, initial) {
        var a, i, j, mat = [];
        for(i = 0; i < m; i++) {
            a = [];
            for(j = 0; j < n; j++) {
                a[j] = 0;
            }
            mat[i] = a;
        }
        return mat;
    }
// 构造一个用0填充的4X4矩阵
    var myMatrix = Array.matrix(4, 3, 4);
    console.log(myMatrix);

// 构造一个对角矩阵的方法
    Array.identity = function (n) {
        var i, mat = Array.matrix(n, n, 0);
        for(i = 0; i < n; i++) {
            mat[i][i] = 1;
        }
        return mat;
    }

    myMatrix = Array.identity(4);

    console.log(myMatrix);
```

### 第七章 正则表达式
#### 结构
创建RegExp对象的方法：
1. 可以使用正则表达式字面量来穿件一个RegExp对象。正则表达式字面量被包围在一对斜杠中。

```
var my_regexp = /"(?:\\.|[^\\\"])*"/g;
```

2. 使用RegExp构造器。这个构造器接收一个字符串，并把它编译为一个RegExp对象

```
创建一个匹配JavaScript字符串的正则表达式
var my_regexp = new RegExp("\"(?:\\.|[^\\\\\\\"])*\"", 'g');
```

有三个标志能在RegExp中设置。可以直接添加在RegExp字面量的末尾

标志 | 含义
---|---
G | 全局的(匹配多次；准确含义随方法而变)
I | 大小写不敏感(忽略字符大小写)
M | 多行(^和$能匹配行结束符)

#### 元素
一个正则表达式选择包含1个或多个正则表达式序列。这些序列被|(竖线)字符分隔。

##### 正则表达式选择
```
"into".match(/in|int/)
将在into中匹配in.但它不会匹配int.

match() 方法可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。
该方法类似 indexOf() 和 lastIndexOf()，但是它返回指定的值，而不是字符串的位置。
```

##### 正则表达式序列
一个正则表达式序列包含1个或多个正则表达式因子。每个因子能选择是否跟随一个量词，这个量词决定着这个因子被允许出现的次数。默认为匹配1次。

##### 正则表达式因子
一个正则表达式因子可以司一个字符、一个圆括号包围的组、一个字符类，或者是一个转义序列。除了控制字符和特殊字符以外，所有的字符都将被按照字面处理

##### 正则表达式转义
反斜杠字符在正则表达式因子中与其在字符串中一样均表示转义。

```
\f 换页符  \n换行符 \r回车符 \t制表符
\w [0-9A-Z_a-z] \W则表示与其相反的
```

##### 正则表达式分组
1. 捕获型
    一个捕获型分组是一个被包围在圆括号中的正则表达式选择。任何匹配这个分组的字符将被捕获。每个捕获分组都被指定来了一个数字。默认从1开始。
2. 非捕获型
    非捕获型分组有一个(?:前缀。非捕获型分组仅做简单的匹配；并不会捕获所匹配文本。
3. 向前正向匹配
    向前正向匹配组有一个(?=前缀。
4. 向前负向匹配
    向前负向匹配分组有一个(?!前缀。

##### 正则表达式类
正则表达式类是一种指定一组字符的便利方式。优点：能够指定字符范围；类的求反^

##### 正则表达式类转义
在字符类中需要被转义的特殊字符： - / [ \ ] ^

##### 正则表达式量词
正则表达式因子可以用一个正则表达式量词后缀来决定这个因子应该被匹配的次数。

```
/WWW/ /w{3}/  {3, 6}将匹配3、4、5或6次。 {3, }将匹配3次或者更多次

？等同于{0, 1}  *等同于{0, }  +则等同于{1, }
```

### 第八章 方法
#### Array
1. array.concat(item)

```
    var a = ['a', 'a', 'a'];
    var b = ['b', 'b', 'b'];
    var d = ['b', {name: "yuan"}, 'b'];
    var c1 = a.concat(d, true);
    var c2 = a.concat(b, true);
    document.write(c1);
    // a,a,a,b,[object Object],b,true
    document.write(c2);
    // a,a,a,b,b,b,true
```

2. array.join(separator)
    把array构造成一个字符串，并用一个separator为分隔符把它们连接在一起。默认的separator是','

```
    var a = ['a', 'b', 'c'];
    a.push('d');
    var c = a.join('/');
    document.write(c);
    // a/b/c/d
```

3. array.pop()
```
    // pop方法移除array中的最后一个元素并返回该元素。
    // 如果该array是空的，返回unfined
        var a = ['a', 'b', 'c'];
        var c = a.pop();
        document.write(c + '<br>')
```

4. array.push(item)

```
//将一个或多个参数item附加到一个数组的尾部，会修改数组array；
// 如果参数item是一个数组，它会将参数数组作为单个元素添加到数组中。
// 返回值为这个数组的新长度
        var a = ['a', 'b', 'c'];
        var b = ['x', 'y', 'z'];
        var c = a.push(b, true);
        document.write('push:' + c + '<br>');
        console.log(a);
        // 5   ['a', 'b', 'c', ['x', 'y', 'z'], true]
```
5. array.reverse()
```
    // reverse方法反转array中的元素的顺序，返回当前的array
    var a = ['a', 'b', 'c'];
    var b = a.reverse();
    console.log(a);
    // a和b都是['c', 'b', 'a'];
```

6. array.shift()
```
// 该方法移除数组array中的第一个元素并返回该元素。
// 如果array是空的，它会返回undefined
    var a = ['a', 'b', 'c'];
    var c = a.shift()
    console.log(a);
    console.log(c);
    // a是['b', 'c'] c是'a'
```

7. array.slice(start, end)
```
// 对array中一段做浅复制
// 第一个被复制的元素是array[start]
// 一直复制到array[end]不包括array[end],end可选，默认为array.length
            var a = ['a', 'b', 'c'];
            var b = a.slice(0, 1);
            var c = a.slice(1);
            var d = a.slice(1, 2);
            console.log(b); //'a'
            console.log(c); // 'b', 'c'
            console.log(d); // 'b'
```

8.array.sort(comparefn)
// 默认是字符串排序

```
// 把item插入到array的开始部分，返回array的新的长度值。
        var a = ['a', 'b', 'c'];
        var r = a.unshift('r', '@');
        console.log(a);
        console.log(r)
```

#### Function
function.apply(thisArg, argArray)

#### Number
1. number.toExponential(fractionDigits)
```
// 把number转换成一个之书形式的字符串。
// 可选参数fractionDigits控制其小数点后的数字维数(0~20).
    document.write(Math.PI.toExponential(0)); //3e+0
    document.write(Math.PI.toExponential(1)); //3.1e+0
```

2. number.toFix(fractionDigits)
```
// 把number转换成一个十进制形式的**字符串**。
// 可选参数fractionDigits控制其小数点后的数字维数(0~20)
    document.write(Math.PI.toFixed(1)); //3.1
```
3. number.toPrecision(precision)
```
// 把这个number转换成一个十进制形式的字符串。
// 可选参数precision控制有效数字的位数(0~21)。
    document.write(Math.PI.toPrecision(3)); //3.14
```
4. number.toString(radix)
```
// 将number转换成一个字符串。
// 可选参数radix控制基数(2~36)，默认是以10为基数的。
        document.write(Math.PI.toString(2));//11.00100100001111110110101010001000100001011
        document.write(Math.PI.toString()); //3.141592653589793

```

#### Object
object.hasOwnProperty(name)
    如果这个object包含了一个名为name的属性，那么hasOwnProperty方法返回true. 原型链中的同名属性是不会被检查的。返回为false

#### RegExp
1. regexp.exec(string)
如果它成功匹配regexp和字符串string,它会返回一个数组。数组中下标为0的元素将包含正则表达式regexp匹配的子字符串。下标为i的元素是分组i捕获的文本。如果匹配失败，那么它会返回null

2. regexp.test(string)
如果该regexp匹配string，它返回true;否则返回false

#### String
1. string.chartAt(pos)
```
// 返回在string中pos位置处的字符
    var name = "Zhiyuan";
    var initial = name.charAt(0);
    document.write(initial + '<br>');//Z
```

2.string.charCodeAt(pos)
```
// 同charAt一样，但是返回值是以整数形式表示在string
// 中的pos位置处的字符的字符码位
    var initial = name.charCodeAt(0);
    document.write(initial + '<br>');//90
```
3.string.concat(string)
```
// 将其他的字符串连接在一起来构造一个新的字符串。
// 可以用+来代替
    var s = 'C'.concat('a', 't');
```

4.string.indexOf(searchString, position)
```
// 在string内查找另一个字符串searchString.
// 找到返回第一个匹配字符的位置，否则返回-1
// 可选参数position可设置从string的某个指定的位置开始查找。
    var text = 'mississing';
    var p = text.indexOf('ss'); //2
    p = text.indexOf('ss', 3); //5
    p = text.indexOf('ss', 6);// -1
```

5.string.lastIndexOf(searchString, position)
```
// 从末尾开始查找
```

6.string.localeCompare(that)

    比较两个字符串。string<that 返回负数 string=that返回0

7.string.match(regexp)

    匹配一个字符串和一个正则表达式。
8.string.replace(searchValue, replaceValue)

```
对string进行查找和替换的操作，并返回一个新的字符串
    // searchValue可以是一个字符串或一个正则表达式对象。
    // 如果是一个字符串，那么只会在string第一次出现的地方被替换
    // 如果searchValues是一个正则表达式并且带有g标志，它将替换所有匹配的字符串
    var result = "mothe_in_law".replace('_', '-');
    document.write(result);
```

9.string.search(regexp)

```
// 接受正则表达式对象作为参数
// 如果匹配成功返回第一个匹配的首字符位置，如果没有匹配返回-1
        var text = 'and in it he says " mu';
        var pos = text.search(/["']/);s
        document.write(pos + '<br>'); //18
```

10.string.slice(start, end)
    复制字符串

11.string.split(separator, limit)

```
// 把string分隔成片段来创建一个字符串数组
// limit限制被分隔的片段数量；
// separator参数是一个字符串或正则表达式
        var ip = '192.168.2.85';
        var a = ip.split('.');
        console.log(a);
    //192,168,2,85
```

12.string.substring(start, end)
    一般用slice替代它

13. string.toLocaleLowerCase()

    使用本地化的规则把string中的所有字母转换成小写格式

14. string.toLocaleUpperCase()

    使用本地化规则把string中的所有字母转换成大写格式

15. string.toLowerCase()|| string.toUpperCase()

    把string中的所有字母都被转化成小写||大写格式

16. String.fromCharCode(char)

    从一串数字中返回一个字符串

```
var a = String.fromCharCode(67, 97, 116);
document.write(a); //Cat
```


### 第九章 代码风格












































































