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
