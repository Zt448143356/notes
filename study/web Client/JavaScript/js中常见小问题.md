# 平时项目中常见小问题总结和拓展

## 克隆

### 浅度克隆

``` JS
let obj = {
  a:100,
  b:[10,20],
  c:{
    x : 10
  },
  d: /^\d+$/
}

let obj2 = {};

for(let key in obj){
    if (obj.hasOwnProperty(key)){
      obj2[key] = obj[key];
    }
}
```

注意，`for ... in`对`Array`的循环得到的是`String`而不是`Number`。

### 深度克隆

平时项目基本使用

``` JS
let obj;
let newObj = JSON.parse(JSON.stringify(obj));
```

这个平时是没有问题的。某次项目这个方法出问题，然后研究了下。

``` JS
let obj={
  a:Date(),
  b:undefined,
  c:function(){},
  d:/js$/,
  e:Error(1)
};
let newObj = JSON.parse(JSON.stringify(obj));

/*
newObj{
a: "Sat Jul 18 2020 19:54:16 GMT+0800 (中国标准时间)"
d: {}
e: {}
}*/
```

``` JS
function deepClone(obj){
  if (obj === null) {
    return null;
  }
  if (typeof obj !== 'object') {
    return obj;
  }
  if (obj instanceof RegExp){
    return new RegExp(obj);
  }
  if (obj instanceof Date) {
    return new Date(obj);
  }

  //let newObj = {};
  //let newObj = new Object();
  //保持克隆结果和传入克隆对象有相同的所属类
  let newObj = new obj.constructor;
  for(let key in obj){
    if (obj.hasOwnProrerty(key)) {
      newObj[key] = deepClone(obj[key])
    }
  }
  return newObj;
}
```


# 业内的三座大山

## 原型和原型链

+ 笔记中已经简单的简述了下原型和原型链等，此处做一些补充和重点再提

+ 原型分两类：显式原型(`prototype`)和隐式原型(`__proto__`)

### 通用规则

1. 所有函数都有`prototype`属性，属性值是一个普通对象。
2. 所有的引用类型（数组、对象和函数），都具有自由扩展属性（也就是原型属性啦）。
3. 所有的对象都具有`__proto__`属性，并且指向他们的构造函数的`prototype`属性（js有是万物都是对象，所以所有的都有`__proto__`属性）
4. 如果一个对象找不到本身的属性，那么会寻找构造这个对象的属性。



## 作用域和闭包

## 异步和单线程