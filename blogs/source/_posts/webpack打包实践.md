---
title: webpack打包实践
date: 2017-08-23 09:56:59
tags: [notes]
categories: [前端]
---
### 一 . 打包历史
1. 页面简单，js实现功能简单，不存在打包
2. AJAX出现， 各种各样前端库出现，网络速度限制，要求对js文件进行压缩和合并。
3. 文件和库依赖引用越来越复杂，出现了CommonJs的，模块化规范：大概语法来讲就是： 如果a.js依赖b.js和c.js，那么就在a.js的头部用require引入这些依赖。
4. require是同步的，所以人们就在CommonJS的基础上定义了Asynchronous Module Definition (AMD)规范(2011年), 使用了异步回调的语法来并行下载多个依赖项。
5. 对于css和html,比如Less,sass等css预处理器横空出世, 帮我们简化css的写法. html在这期间也出现了一堆模板语言, 什么handlebars,ejs,jade, 可以把ajax拿到的数据插入到模板中, 然后用innerHTML显示到页面上
6. 基于此我们要写编译脚本，但是这些命令行脚本在不同的平台上是不通的，所以我们需要一个打包工具，可以利用各种编译工具, 编译/压缩js, css, html, 图片等资源。所以grunt(2012年)产生了，接着gulp也产生了。
7. 依托AMD模块化编程, SPA(Single-page application)的实现方式更为简单清晰, 一个网页不再是传统的类似word文档的页面, 而是一个完整的应用程序. SPA应用有一个总的入口页面, 我们通常把它命名为index.html, app.html, main.html, 这个html的<body>一般是空的, 或者只有总的布局(layout)
8. 虽然AMD模块让SPA更容易地实现，但小问题也很多：
    + 不是所有的第三方库都是AMD规范的, 这时候要配置；
    + 不支持动态加载css, 变通的方法是把所有的css文件合并压缩成一个文件, 在入口的html页面一次性加载
    + SPA项目越做越大,一个应用打包后的js文件变得很大
    + 项目的文件结构不合理, 因为grunt/gulp是按照文件格式批量处理的, 所以一般会把js, html, css, 图片分别放在不同的目录下, 所以同一个模块的文件会散落在不同的目录下, 开发的时候找文件是个麻烦的事情
9. 2012webpack产生了

### 二. webpack打包简介
#### 1. webpack定义

  webpack是一个模块打包工具，处理模块之间的依赖同时生成对应模块的静态资源。
#### 2. webpack功能/作用
![image](../../img/webpack_1.jpg)

- webpack把项目中所有的静态文件都看作一个模块
- 模快之间存在着一些列的依赖
- 多页面的静态资源生成(打包之后生成多个静态文件，涉及到代码拆分)

#### 3. 基本配置
[configuration](http://webpack.github.io/docs/configuration.html)

##### 1). 基本配置 webpack.config.js
```
var webpack = require('webpack');
var commonsPlugin = new webpack.optimize.CommonsChunkPlugin('common.js');

module.exports = {
    //插件项
    plugins: [commonsPlugin],
    //页面入口文件配置
    entry: {
        index : './src/js/page/index.js'
    },
    //入口文件输出配置
    output: {
        path: 'dist/js/page',
        filename: '[name].js'
    },
    module: {
        //加载器配置
        loaders: [
            { test: /\.css$/, loader: 'style-loader!css-loader' },
            { test: /\.js$/, loader: 'jsx-loader?harmony' },
            { test: /\.scss$/, loader: 'style!css!sass?sourceMap'},
            { test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192'}
        ]
    },
    //其它解决方案配置
    resolve: {
        root: 'E:/github/flux-example/src', //绝对路径
        extensions: ['', '.js', '.json', '.scss'],
        alias: {
            AppStore : 'js/stores/AppStores.js',
            ActionType : 'js/actions/ActionType.js',
            AppAction : 'js/actions/AppAction.js'
        }
    }
};
```
- entry相关

    入口配置文件，有三种形式：单个文件入口、数组形式(把平行的不相依赖的文件打包在一起)、对象的形式(多页面配置)
- output相关

    output 是对应输出项配置,输出路径，输出名称等
- module相关
    
    配置每种文件需要使用什么加载器来处理
    
    ```
        module: {
            //加载器配置
            loaders: [
                //.css 文件使用 style-loader 和 css-loader 来处理
                { test: /\.css$/, loader: 'style-loader!css-loader' },
                //.js 文件使用 jsx-loader 来编译处理
                { test: /\.js$/, loader: 'jsx-loader?harmony' },
                //.scss 文件使用 style-loader、css-loader 和 sass-loader 来编译处理
                { test: /\.scss$/, loader: 'style!css!sass?sourceMap'},
                //图片文件使用 url-loader 来处理，小于8kb的直接转为base64
                { test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192'}
            ]
        }
    ```

- resolve相关
 
    对打包文件的一些设置，比如自动扩展文件后缀名，模块别名定义等

三. 运行webpack
1. 常用的参数
    
    ```
    $ webpack --config XXX.js   //使用另一份配置文件（比如webpack.config2.js）来打包
    
    $ webpack --watch   //监听变动并自动打包
    
    $ webpack -p    //压缩混淆脚本
    
    $ webpack -d    //生成map映射文件，告知哪些模块被最终打包到哪里了
    ```
2. webpack-html-plugin插件

    1)基本配置
        ```
        var htmlWebpackPlugin = require('html-webpack-plugin');
        plugins: [
            new htmlWebpackPlugin({
                filename: 'index.html',
                template: 'index.html',
                inject: 'body',
                title: 'fft ued',
                date: '2017-08-13',
                // minify: {
                //  removeComments: true,
                //  removeTagWhitespace: true
                // }
            })
        ]   
        ```

    2)多页面配置
        ```
        var htmlWebpackPlugin = require('html-webpack-plugin');
            plugins: [
                new htmlWebpackPlugin({
                    filename: 'aa.html',
                    template: 'index.html',
                    inject: 'body',
                    title: 'aa fft ued',
                    // chunks: ['main', 'aa']
                    excludeChunks: ['bb', 'cc']
                }),
                new htmlWebpackPlugin({
                    filename: 'bb.html',
                    template: 'index.html',
                    inject: 'body',
                    title: 'bb fft ued',
                    // chunks: ['bb']
                    excludeChunks: ['aa', 'cc']
                }),
                new htmlWebpackPlugin({
                    filename: 'cc.html',
                    template: 'index.html',
                    inject: 'body',
                    title: 'cc fft ued',
                    // chunks: ['cc']
                    excludeChunks: ['aa', 'bb']
                })

            ]
        ```