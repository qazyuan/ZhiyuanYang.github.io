---
title: VUE基础知识点
date: 2018-3-20
tags: [VUE, js]
categories: [前端, VUE]
---
### 实例化Vue对象
### 数据和方法
    data: {},
    methods: {}
    
### 属性绑定
    :href
    :value
    :style

### 鼠标点击事件
    @click
    @dblclick

### 键盘事件
    @keyup
    
### 事件修饰符
- 点击事件： prevent stop once
- 键盘事件修饰符： enter alt

### 给组件绑定原生事件
@click.native
```
<div id="root">
    <child @click.native="handleClick"></child>
</div>
<script type="text/javascript">
    Vue.component('child', {
        template: '<p>dddd</p>'
    })

    new Vue({
        el: '#root',
        methods: {
            handleClick: function() {
                alert('click');
            }
        }

    })
</script>
```


### 双向绑定 select/input/textarea
- v-model
- ref通过dom绑定

```
ref绑定父组件dom
<div id="root">
    <counter @change="changHandle" ref="num1"></counter>
    <counter @change="changHandle" ref="num2"></counter>
    <div ref="total">{{total}}</div>
</div>
Vue.component('counter', {
    data(){
        return {
            number: 0
        }
    },
    template: "<span @click='changeNumber'>{{number}}</span>",
    methods: {
        changeNumber: function () {
            this.number++;
            this.$emit('change');
        }
    }
});
var vm = new Vue({
    el: '#root',
    data: {
        total: 0
    },
    methods: {
        changHandle: function () {
            let num1 = this.$refs.num1.number; //子组件ref取值
            let num2 = this.$refs.num2.number;
            let originTotal = this.$refs.total.innerText;//父组件dom取值
            this.total = (+num1) + (+num2);
        }
    }
})
```

### 计算属性 computed
- 计算属性和methods区别

        methods执行所有的方法
        computed当虚拟dom和实际dom不一致时才会执行里面的func
- 当一个功能能用watch\computed\methods实现时，优先使用computed

### 动态绑定样式
可以通过点击事件等触发样式的改变，用computed里面的属性来实时更改样式
    
- class的数组绑定
    
```
.init {
    border: 1px solid #555;
}
.actived {
    color: red;
}
<!--通过给数组里面的元素赋值来加载样式-->
<h2 id="app" @click="changeClassEvent" :class="[init, actived]">hello world</h2>

new Vue({
    el: '#app',
    data: {
        init: 'init',
        actived: ''
    },
    methods: {
        changeClassEvent: function () {
            this.actived = (this.actived == 'actived' ? '' : 'actived');
        }
    }
})
```
- class的对象绑定
    
```
<!--通过对象的值来判断是否加载样式-->
<h2 id="app" @click="changeClassEvent" :class="{init: isActived, actived: isActived}">hello world</h2>

new Vue({
    el: '#app',
    data: {
        isActived: false
    },
    methods: {
        changeClassEvent: function () {
            this.isActived = !this.isActived;
        }
    }
})
```
- 内联样式style绑定

```
与绑定class一样可以绑定数组和对象
在绑定数组中数组中的每个元素也是对象
<h2 id="app3" @click="changeClass" :style="[styleObj1, styleObj2]">hello, world</h2>
new Vue({
    el: '#app3',
    data: {
        styleObj1: {
            color: ''
        },
        styleObj2: {
            'font-size': '12px'
        }
    },
    methods: {
        changeClass: function () {
            let color = this.styleObj1.color;
            let size = this.styleObj2['font-size'];
            this.styleObj1.color = (color == 'red' ? '' : 'red');
            this.styleObj2['font-size'] = (size == '12px' ? '20px' : '12px')
        }
    }
});


内联样式绑定写法
:style="{backgroundImage: 'url(' + img.url + ')'}"
```

### vue过渡动画效果
- 动画顺序(添加删除class)

    从隐藏到显示动画添加删除的class
    1. fade-enter fade-enter-active  
    2. ~~fade-enter~~ fade-enter-to
    3. ~~fade-enter-active fade-enter-to~~
    
    从显示到隐藏动画添加删除的class
    1. fade-leave fade-leave-active
    2. ~~fade-leave~~ fade-leave-to
    3. ~~fade-leave-active fade-leave-to~~

- 自定义动画class名字
animate.css库
```
enter-active-class="active"
leave-active-class="leave"
appear
appear-active-class=""
type=""//设置动画类型
:duration='5000' //设置动画时长
:duration="{enter: 5000, leave: 10000}"
```
- js动画函数
    
    velocity.js库
    @before-enter = "" //接收el参数
    @enter="" //接收el, done两个参数其中done为回调函数
    @after-enter=""
- mode多个动画切换样式
- 多个组件过渡动画
- transition-group

    
### 条件渲染 v-if v-else-if v-show
- 不同之处： v-show有dom结构，display: none
- v-if  v-else必须连在一起使用，中间不能插入其他dom结构
- 元素标签加key值，vue不会复用该dom结构

```
<div id="appIf">
    <h4 v-if="show=='a'">this is A</h4>
    <!--<span>333</span> 中间插入dom结构会报错-->
    <h4 v-else-if="show=='b'">this is B</h4>
    <h4 v-else>this is another one</h4>
</div>
new Vue({
    el: '#appIf',
    data: {
        show: 'a'
    }
});
```

### 列表渲染
- pop\push\shift\unshift\splice\sort\reverse这几个方法操作数组，页面数据会立即发生变化
- 直接改变数组的引用也可以更新页面数据
- <template></template> 模板占位符，页面不会渲染出来
- 对象的循环 vue.set(objName, key, value)给对象添加值

```
<div id="appIf">
    <span v-for="item in list">{{item}}-----</span>
</div>
var vm = new Vue({
    el: '#appIf',
    data: {
        list:[1, 2, 3, 4],
        show: 'a'
    }
});
vm.$set(vm.list, 1, '333') //改变list[1]的值
Vue.set(vm.list, 1, '333') //改变list[1]的值
```

    
### 初始化多个vue的实例化对象

```
<div id="app-one">
    <h2>{{title}}</h2>
</div>
<div id="app-two">
    <h2>{{title}}</h2>
    <button @click="changeTitle">changeTitleOne</button>
</div>

<script type="text/javascript">
    var one = new Vue({
        el: '#app-one',
        data: {
            'title': 'this is app-one'
        }
    });
    var two = new Vue({
        el: '#app-two',
        data: {
            'title': 'this is app-two'
        },
        methods: {
            changeTitle: function () {
                one.title = "changed title one"; //修改one中title的值
            }
        }
    });

    two.title= "changed title two";
```

### vue组件

```
var data = {
    name: luhan0
};
Vue.component('greeting', {
    template: `<p>{{name}}: this is a new vue component
    <button v-on:click="changeName">改名</button>
    </p>`,
    data: function () {
        // return {
        //     name: "luhan0"
        // };
        return data;
    },
    methods: {
        changeName: function () {
            this.name = "hello";
        }
    }
});
```

### 组件全局注册和局部注册
- 组件全局注册

        import 后用Vue.component('', 组件)全局引入
- 组件局部注册
  
        在组件内import后再component里面注册

### 组件使用小细节
- talbe\ul\select渲染小bug

```
<div id="root">
    <table> 
        <tbody> 
            <tr is="row">  </tr>
            <!--is的使用，若直接用row组件，渲染的dom为tr与table同级-->
        </tbody>
    </table>
</div>
 Vue.component('row', {
    template: '<tr><td>this is a row</td></tr>'
})
var vm = new Vue({
    el: '#root'
})
```
- 子组件中data必须是一个函数，返回一个对象
- ref操作dom


### css样式作用域 scoped

### 组件嵌套

### 传值问题
- 父组件向子组件传值：属性传值
    - 属性传值v-bind:  子组件通过props接收
    - 传值和传引用的区别
    - 子组件不能随意更改父组件中传过来的值
        
- 子组件向父组件传值
        
        子组件：this.$emit(event, args)
        /*
        * event: 要触发的事件
        * args: 将要传给父组件的参数
        */ 
        父组件：v-on: event = functionName()
- 非父子组件传值
    - vuex (待补充)
    - 发布订阅模式(也称观察者模式\bus\总线模式)
    
        ```
        <div id="root">
            <child content="Dell"></child>
            <child content="Yoga"></child>
        </div>
        <script type="text/javascript">
            Vue.prototype.bus = new Vue();
            Vue.component('child', {
                props: {
                    content: String
                },
                data() {
                    return {
                        selfContent: this.content
                    }
                },
                template: "<p @click='handleChange'>{{selfContent}}</p>",
                methods: {
                    handleChange: function () {
                        this.bus.$emit('change', this.content);
                    }
                },
                mounted: function () {
                    var that = this;
                    that.bus.$on('change', function(msg) {
                        that.selfContent = msg;
                    })
                }
            })
        
            new Vue({
                el: '#root'
            })
        </script>
        ```

### vue中的插槽slot

- 父组件向子组件传递dom结构

```
<div id="root1">
    <child>
        <p slot="header">header</p>
        <p slot="footer">footer</p>
    </child>
</div>
<script type="text/javascript">
    Vue.component('child', {
        template: `<div>
                        <slot name="header"></slot>
                        <p>World</p>
                        <slot name="footer"></slot>
                    </div>`
    })
    let vm = new Vue({
        el: '#root1'
    })
</script>
```
- 作用域插槽 slot

```
<child>
    <template slot-scope="props">
        <p>{{props.item}}---{{props.index}}</p>
    </template>
</child>
Vue.component('child', {
    data() {
        return {
            list: [1, 2, 3, 4, 5]
        }
    },
    template: `<div>
                    <slot v-for="(item, index) in list" :item=item :index="index"></slot>
                </div>`
})
```

### 动态加载组件

```
<div id="root2">
    <component :is='type'></component> 
    <!--通过type的值的变化来选择加载哪个组件-->
    <button @click="changeType">change</button>
</div>
<script type="text/javascript">
    Vue.component('child-one', {
        template: '<div v-once>child-one</div>'
    })
    Vue.component('child-two', {
        template: '<div v-once>child-two</div>'
        //v-onece将组件放入内存中，下次直接加载
    })
    new Vue({
        el: '#root2',
        data: {
            type: 'child-one'
        },
        methods: {
            changeType: function () {
                this.type = (this.type === 'child-one' ? 'child-two': 'child-one');
            }
        }
    })
</script>
```



### 组件参数校验与非props特性

- 组件参数校验
```
props: {
    marNum: [String, Number]
    /*限制父组件传过来的marNum的类型是数组和数字*/
}

props: {
    marNum: {
        type: String,
        required: true, /*必传*/
        default: '', /*默认值*/
        validator: function（value) {
            return (value.length > 5)
        }
    }
}
```
- 非props特性

```
<div id="root">
    <child :content="number"></child>
</div>
<script type="text/javascript">
    Vue.component('child', {
        template: '<p>dddd</p>'
    })  
    //子组件不接收父组件传过来的content时，在渲染dom中会生成这样的dom结构<p content="222">dddd</p>

    new Vue({
        el: '#root',
        data: {
            number: '222'
        }
    })
</script>
```



### vue路由
- 配置路由
    
```
    // 配置路由
    const router = new VueRouter({
        routes: [
        {path: '/', component: Home},
        {path: '/hello', component: HelloWorld}
        ],
        mode: 'history'
    });
```
