---
title: 常见CSS布局
date: 2018-1-16 10:55:36
tags: [布局]
categories: [前端, CSS]
---

#### 1. 图片成比例显示

    ```
    <div class="img-wrapper">
        <img src="">
    </div>
    .img-wrapper {
        overflow: hidden;
        width: 100%;
        height: 0;
        padding-bottom: 31.25% //高宽比例,
        background-image: url();
    }
    
    ```
#### 2. 文字省略号
    ```
    //单行省略
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;


    //多行省略
    overflow:hidden;
    white-space:normal;
    text-overflow: ellipsis;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
    display: -webkit-box;
    ```
#### 3. 两栏布局(左边固定，右边自适应)
- 左边浮动
    
    ```
    <style>
        .wrapper {
            width: 100%;
            position: relative;
        }
        .left {
            position: absolute;
            left: 0;
            top: 0;
            width: 300px;
            background: #eee;
        }
        .right {
            margin-left: 300px;
            background: #223aa3;
        }
    </style>
    <div class="wrapper">
        <div class="left">
            left固定
        </div>
        <div class="right">
            right自适应
        </div>
    </div>
    
    ```

- 左边absolute布局

    ```
    <style>
        .wrapper {
            width: 100%;
            position: relative;
        }
        .left {
            width: 300px;
            float: left;
            background: #eee;
        }
        .right {
            margin-left: 300px;
            background: #223aa3;
        }
    </style>
    <div class="wrapper">
        <div class="left">
            left固定
        </div>
        <div class="right">
            right自适应
        </div>
    </div>
    ```
#### 4. 三栏布局(左右固定，中间自适应)
- 使用左右两栏使用float属性，中间栏使用margin属性进行撑开
- 使用position定位实现，即左右两栏使用position进行定位，中间栏使用margin进行定位
- 使用float和BFC配合圣杯布局
- flex布局

#### 5. 多行文字垂直居中

```
<div class="parent">
    <div class="son">
        多行文字垂直居中多行文字垂直居中多行文字垂直居中多行文字垂直居中
    </div>
</div>
.parent {
    display: table;
    width: 300px;
    height: 300px;
    text-align: center;
}
.son  {
    display: table-cell;
    height: 200px;
    background-color: yellow;
    vertical-align: middle;
}
```
