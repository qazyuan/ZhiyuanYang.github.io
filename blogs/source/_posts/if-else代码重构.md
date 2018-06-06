---
title: if else代码重构
date: 2017-12-21 10:46:12
tags:
categories:
---
#### 一. If else代码出现场景
异常逻辑处理和不同状态处理
##### 1.  异常逻辑处理： 只能一个分支是正常流程
##### 2. 不同状态处理： 所有的分支都是正常流程
例子：
```
//举例一：异常逻辑处理例子
    var obj = getObj();
    if (obj != null) {
        //do something 正常的处理流程
    }else{
        //do something 出错的处理流程
    }


//举例二：状态处理例子
    var obj = getObj();
    if (obj.getType == 1) {
        //do something
    }else if (obj.getType == 2) {
        //do something
    }else{
        //do something
    }
```

#### 二. if-else代码的缺点
##### 1. 最大的问题是代码逻辑复杂，维护性差，极容易引发bug。
##### 2. 如果使用if-else，说明if分支和else分支的重视是同等的，但大多数情况并非如此，容易引起误解和理解困难。

#### 三. 优化方案
原则： 尽可能地维持正常流程代码在最外层。

实现的手段：减少嵌套、移除临时变量、条件取反判断、合并条件表达式等

#### 四. 异常逻辑处理型重构
##### 1. 实例一： 合并条件表达式
    
```
//重构前：
    function disablityAmount(){
        if(_seniority < 2) 
            return 0;
    
        if(_monthsDisabled > 12)
            return 0;
    
        if(_isPartTime)
            return 0;
    
        //do somethig
    
    }
    
//重构后：
    function disablityAmount(){
        if(_seniority < 2 || _monthsDisabled > 12 || _isPartTime) 
            return 0;
    
        //do somethig
    }
```

##### 2. 实例二： 避免逻辑嵌套太深

如果if-else嵌套没有关联性，直接提取到第一层，一定要避免逻辑嵌套太深。尽量减少临时变量改用return直接返回。

```
//重构前
    function getPayAmount(){
        var result;
        if(_isDead) {
            result = deadAmount();
        }else{
            if(_isSeparated){
                result = separatedAmount();
            }
            else{
                if(_isRetired){
                    result = retiredAmount();
                else{
                    result = normalPayAmount();
                }
            }
        }
    return result;

//重构后：
    function getPayAmount(){
        if(_isDead) 
            return deadAmount();
    
        if(_isSeparated)
            return separatedAmount();
    
        if(_isRetired)
            return retiredAmount();
    
        return normalPayAmount();
    }
```
##### 3. 实例三： 异常情况先退出，让正常流程维持在主干流程

```
//重构前：
    function getAdjustedCapital(){
        var  result = 0.0;
        if(_capital > 0.0 ){
            if(_intRate > 0 && _duration >0){
                resutl = (_income / _duration) *ADJ_FACTOR;
            }
        }
        return result;
    }
    
//第一步: 减少嵌套和移除临时变量：
    var getAdjustedCapital(){
        if(_capital <= 0.0 ){
            return 0.0;
        }
        if(_intRate > 0 && _duration >0){
            return (_income / _duration) *ADJ_FACTOR;
        }
        return 0.0;
    }
//这样重构后，还不够，因为主要的语句`(_income / _duration) *ADJ_FACTOR;`在if内部，并非在最外层，根据优化原则（尽可能地维持正常流程代码在最外层），可以再继续重构：
    
    var getAdjustedCapital(){
        if(_capital <= 0.0 ){
            return 0.0;
        }
        if(_intRate <= 0 || _duration <= 0){
            return 0.0;
        }
    
        return (_income / _duration) *ADJ_FACTOR;
    }
```
##### 4. 实例四： 异常条件先退出，保持主干流程

```
//重构前：
   /* 查找年龄大于18岁且为男性的学生列表 */
    function getStudents(int uid){
        var result = [];
        stu = getStudentByUid(uid);
        if (stu != null) {
            teacher = stu.getTeacher();
            if(teacher != null){
                students = teacher.getStudents();
                if(students != null){
                    for(Student student : students){
                        if(student.getAge() > = 18 && student.getGender() == MALE){
                            result.push(student);
                        }
                    }
                }else {
                    console.error("获取学生列表失败");
                }
            }else {
                console.error("获取老师信息失败");
            }
        } else {
            console.error("获取学生信息失败");
        }
        return result;
    }
    
//嵌套过深，解决方法是异常条件先退出，保持主干流程是核心流程：

//重构后：

   /* 查找年龄大于18岁且为男性的学生列表 */
    function getStudents(uid){
        result = [];
        stu = getStudentByUid(uid);
        if (stu == null) {
            console.error("获取学生信息失败");
            return result;
        }

        var teacher = stu.getTeacher();
        if(teacher == null){
            console.error("获取老师信息失败");
            return result;
        }

        var students = teacher.getStudents();
        if(students == null){
            console.error("获取学生列表失败");
            return result;
        }

        for(Student student : students){
            if(student.getAge() > 18 && student.getGender() == MALE){
                result.push(student);
            }
        }
        return result;
    }
```

#### 五. 状态处理型重构

##### 1. 实例一： 把if-else内的代码都封装成一个公共函数

```
//重构前
    function getPayAmount(){
        var obj = getObj();
        var money = 0;
        if (obj.getType == 1) {
            var objA = obj.getObjectA();
            money = objA.getMoney()*obj.getNormalMoneryA();
        }
        else if (obj.getType == 2) {
            var objB = obj.getObjectB();
            money = objB.getMoney()*obj.getNormalMoneryB()+1000;
        }
    }

//重构后
    function getPayAmount(){
        var obj = getObj();
        if (obj.getType == 1) {
            return getType1Money(obj);
        }
        else if (obj.getType == 2) {
            return getType2Money(obj);
        }
    }
    
    function getType1Money(obj){
        var objA = obj.getObjectA();
        return objA.getMoney()*obj.getNormalMoneryA();
    }
    
    function getType2Money(obj){
        ObjectB objB = obj.getObjectB();
        return objB.getMoney()*obj.getNormalMoneryB()+1000;
    }
```

##### 2. 实例二： 用多态取代条件表达式
条件表达式，它根据对象类型的不同而选择不同的行为。将这个表达式的每个分支放进一个子类内的覆写函数中，然后将原始函数声明为抽象函数

```
//重构前
    function getSpeed(){
        switch(_type){
            case EUROPEAN:
                return getBaseSpeed();
            case AFRICAN:
                return getBaseSpeed()-getLoadFactor()*_numberOfCoconuts;
            case NORWEGIAN_BLUE:
                return (_isNailed)?0:getBaseSpeed(_voltage);
        }
    }
    
//重构后
    class Bird{
        abstract double getSpeed();
    }
    
    class European extends Bird{
        double getSpeed(){
            return getBaseSpeed();
        }
    }
    
    class African extends Bird{
        double getSpeed(){
            return getBaseSpeed()-getLoadFactor()*_numberOfCoconuts;
        }
    }
    
    class NorwegianBlue extends Bird{
        double getSpeed(){
            return (_isNailed)?0:getBaseSpeed(_voltage);
        }
    }
```
#### 综合实例
##### 综合实例一
```
function categoryHandle(category) {
  if(category !== 'A') {
    if(category === 'D') {
      console.log('D');
    } else {
      console.log('B,C');
    }
    console.log('B, C ,D')
  } else {
    console.log('A');
  }
}

function categoryHandle(category) {
  if(category === 'A') {
    console.log('A');
  } else if (category === 'B'){
    console.log('B');
  } else if (category === 'C'){
    console.log('D');
  }else if (category === 'D'){
    console.log('D');
  }
}

function categoryHandleRefactor(category) {
  var categoryAction = {
    'A': {
      run: function () {
        console.log('A')
      }
    },
    'B': {
      run: function () {
        console.log('B')
      }
    },
    'C': {
      run: function () {
        console.log('C')
      }
    },
    'D': {
      run: function () {
        console.log('D')
      }
    }
  };
  categoryAction[category].run();
}
```


#### 六. 总结
if-else代码是每一个程序员最容易写出的代码，稍不注意，就产生一堆难以维护和逻辑混乱的代码。

针对条件型代码重构把握一个原则
尽可能地维持正常流程代码在最外层，保持主干流程是正常核心流程。

为维持这个原则：合并条件表达式可以有效地减少if语句数目；减少嵌套能减少深层次逻辑；
异常条件先退出自然而然主干流程就是正常流程。

针对状态处理型重构方法有两种：一种是把不同状态的操作封装成函数，简短if-else内代码行数；另一种是利用面向对象多态特性直接去掉了条件判断。

