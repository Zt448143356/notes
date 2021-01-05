# Module 的语法

本笔记摘录于[ECMAScript 6 入门](https://es6.ruanyifeng.com/#docs/module#export-命令)

## 概述

1. 在ES6之前，主要的是CommonJS和AMD两种。
2. ES6模块的设计思想是尽量静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS和AMD模块，都只能在运行时确定这些。
3. CommonJS模块就是对象，输入时必须查找对象属性
4. ES6 的模块自动采用严格模式，不管你有没有在模块头部加上`"use strict"`;

``` JS
// CommonJS模块
let { stat, exists, readfile } = require('fs');

// 等同于
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
```

上面代码的实质是整体加载`fs`模块（即加载`fs`的所有方法），生成一个对象（`_fs`），然后再从这个对象上面读取 3 个方法。这种加载称为“运行时加载”，因为只有运行时才能得到这个对象，导致完全没办法在编译时做“静态优化”。

**ES6模块不是对象**，而是通过`export`命令显式指定输出的代码，再通过`import`命令输入。

``` JS
// ES6模块
import { stat, exists, readFile } from 'fs';
```

上面代码的实质是从`fs`模块加载 3 个方法，其他方法不加载。这种加载称为“编译时加载”或者静态加载，即 ES6 可以在编译时就完成模块加载，效率要比 CommonJS 模块的加载方式高。当然，这也导致了没法引用 ES6 模块本身，因为它不是对象。

由于 ES6 模块是编译时加载，使得静态分析成为可能。有了它，就能进一步拓宽 JavaScript 的语法，比如引入宏（macro）和类型检验（type system）这些只能靠静态分析实现的功能。

## export命令

+ `export`命令用于规定模块的对外接口

``` JS
// profile.js
export var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;

//或者

var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export { firstName, lastName, year };
```
推荐使用后者，因为后者可以在文件尾部一眼看清楚输出了什么变量

``` JS
export function multiply(x, y) {
  return x * y;
};

//或者

function v1() { ... }
function v2() { ... }

export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};
```

上面代码使用`as`关键字，重命名了函数`v1`和`v2`的对外接口。重命名后，`v2`可以用不同的名字输出两次。

``` JS
// 报错
export 1;

// 报错
var m = 1;
export m;

// 报错
function f() {}
export f;

//正确的写法

// 写法一
export var m = 1;

// 写法二
var m = 1;
export {m};

// 重命名
var n = 1;
export {n as m};

// 正确
export function f() {};

// 正确
function f() {}
export {f};

//错误
function foo() {
  export default 'bar' // SyntaxError
}
foo()

```

`export`命令可以出现在模块的任何位置，只要处于模块顶层就可以。如果处于块级作用域内，就会报错.这是因为处于条件代码块之中，就没法做静态优化了，违背了 ES6 模块的设计初衷。

> 需要特别注意的是，`export`命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。(来自[ECMAScript 6 入门](https://es6.ruanyifeng.com/#docs/module#export-命令)) (个人不太理解)

## import 命令

``` JS
//加载
import { firstName, lastName, year } from './profile.js';

//重命名
import { lastName as surname } from './profile.js';

//只读（不允许修改接口，但是可以修改接口内的属性）
import {a} from './xxx.js'
a = {}; // Syntax Error : 'a' is read-only;

a.foo = 'hello'; // 合法操作。不推荐使用。不方便差错

```

上面代码的`import`命令，用于加载`profile.js`文件，并从中输入变量。`import`命令接受一对大括号，里面指定要从其他模块导入的变量名。大括号里面的变量名，必须与被导入模块（`profile.js`）对外接口的名称相同。

``` JS
foo();

import { foo } from 'my_module';
```

`import`命令具有提升效果，会提升到整个模块的头部，首先执行。本质：`import`命令是编译阶段执行的，在代码运行之前。


``` JS
// 报错
import { 'f' + 'oo' } from 'my_module';

// 报错
let module = 'my_module';
import { foo } from module;

// 报错
if (x === 1) {
  import { foo } from 'module1';
} else {
  import { foo } from 'module2';
}
```

由于`import`是静态执行，所以不能使用表达式和变量，这些只有在运行时才能得到结果的语法结构。

## *整体加载

``` JS
// circle.js

export function area(radius) {
  return Math.PI * radius * radius;
}

export function circumference(radius) {
  return 2 * Math.PI * radius;
}

// main.js

import { area, circumference } from './circle';

console.log('圆面积：' + area(4));
console.log('圆周长：' + circumference(14));

//or

import * as circle from './circle';

console.log('圆面积：' + circle.area(4));
console.log('圆周长：' + circle.circumference(14));

//注意，模块整体加载所在的那个对象（上例是circle），应该是可以静态分析的，所以不允许运行时改变。下面的写法都是不允许的。

import * as circle from './circle';

// 下面两行都是不允许的
circle.foo = 'hello';
circle.area = function () {};
```

## export default命令

使用`import`命令的时候，用户需要知道所要加载的变量名或函数名，否则无法加载。为了给用户提供方便，让他们不用阅读文档就能加载模块，就要用到`export default`命令，为模块指定默认输出。

``` JS
// export-default.js
export default function foo() {
  console.log('foo');
}

// 或者写成

function foo() {
  console.log('foo');
}

export default foo;

//or

function foo() {
  console.log('foo');
}

export default {foo};
```

