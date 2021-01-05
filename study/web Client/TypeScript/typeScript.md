# 基础

## 原始数据类型

JavaScript 的类型分为[两种](../JavaScript/javaScript.md#数据类型和变量)：原始数据类型（Primitive data types）和对象类型（Object types）。

原始数据类型包括：布尔值、数值、字符串、`null`、`undefined` 以及 ES6 中的新类型 `Symbol` 和 `BigInt`。

### 布尔值(boolean)

``` TS
let isDone: boolean = false;//编译通过

let createdByNewBoolean: boolean = new Boolean(1);//编译报错

let createdByNewBoolean: Boolean = new Boolean(1);//编译通过

let createdByNewBoolean: boolean = Boolean(1);//编译通过

new Boolean(1);//返回Boolean {true}是一个Boolean对象
Boolean(1);//true
```

在 TypeScript 中，`boolean` 是 JavaScript 中的基本类型，而 `Boolean` 是 JavaScript 中的构造函数。[详见](../JavaScript/三座大山.md#原型和原型链)

### 数值(number)

使用 `number` 定义数值类型：

``` TS
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
// ES6 中的二进制表示法
let binaryLiteral: number = 0b1010;
// ES6 中的八进制表示法
let octalLiteral: number = 0o744;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;
```

编译后(Compiles to)

``` JS
var decLiteral = 6;
var hexLiteral = 0xf00d;
// ES6 中的二进制表示法
var binaryLiteral = 10;
// ES6 中的八进制表示法
var octalLiteral = 484;
var notANumber = NaN;
var infinityNumber = Infinity;
```

其中 `0b1010` 和 `0o744` 是 [ES6 中的二进制和八进制表示法](https://es6.ruanyifeng.com/#docs/number#二进制和八进制表示法)，它们会被编译为十进制数字。

### 字符串(string)

使用 `string` 定义字符串类型：

``` TS
let myName: string = 'Tom';
let myAge: number = 25;

// 模板字符串
let sentence: string = `Hello, my name is ${myName}.I'll be ${myAge + 1} years old next month.`;
```

编译后(Compiles to)

``` JS
var myName = 'Tom';
var myAge = 25;
// 模板字符串
var sentence = "Hello, my name is " + myName + ".\nI'll be " + (myAge + 1) + " years old next month.";
```

### 空值（Void）

JavaScript 没有空值（Void）的概念,`void`类型像是与`any`类型相反，在 TypeScript 中，可以用 `void` 表示没有任何返回值的函数：

``` TS
function alertName(): void {
    alert('My name is Tom');
}
```

声明一个 `void` 类型的变量没有什么用，因为你只能将它赋值为 `undefined` 和 `null`：

``` TS
let unusable: void = undefined;
```

### Null 和 Undefined

在 TypeScript 中，可以使用 `null` 和 `undefined` 来定义这两个原始数据类型：

``` TS
let u: undefined = undefined;
let n: null = null;
```

与 `void` 的区别是，`undefined` 和 `null` 是所有类型的子类型。也就是说 `undefined` 类型的变量，可以赋值给 `number` 类型的变量：

``` TS
// 这样不会报错
let num: number = undefined;

// 这样也不会报错
let u: undefined;
let num: number = u;

//这样不行
let u: void;
let num: number = u;
```

## 任意值(Any)

任意值（Any）用来表示允许赋值为任意类型。

``` TS
let myFavoriteNumber: any = 'seven';
myFavoriteNumber = 7;
```

### 任意值的属性和方法

``` TS
let anyThing: any = 'hello';
console.log(anyThing.myName);
console.log(anyThing.myName.firstName);//ts编译不会报错，js执行会报错

let anyThing: any = 'Tom';
anyThing.setName('Jerry');//ts编译不会报错，js执行会报错
anyThing.setName('Jerry').sayHello();//ts编译不会报错，js执行会报错
anyThing.myName.setFirstName('Cat');//ts编译不会报错，js执行会报错
```

有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 `any`类型来标记这些变量

### 未声明类型的变量

变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型：

``` TS
let something;
something = 'seven';
something = 7;

something.setName('Tom');
```

编译后(Compiles to)

``` JS
let something: any;
something = 'seven';
something = 7;

something.setName('Tom');
```

## 类型推论

如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。

### 演示

``` TS
let myFavoriteNumber = 'seven';
myFavoriteNumber = 7;

//error TS2322: Type 'number' is not assignable to type 'string'.
```

等价于

``` TS
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;

//error TS2322: Type 'number' is not assignable to type 'string'.
```

**如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 any 类型而完全不被类型检查**

``` TS
let myFavoriteNumber;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

等价于

``` TS
let myFavoriteNumber : any;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

## 联合类型

### 基本使用

``` TS
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

这里的 `let myFavoriteNumber: string | number` 的含义是，允许 `myFavoriteNumber` 的类型是 `string` 或者 `number`，但是不能是其他类型。

### 访问联合类型的属性或方法

当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，**我们只能访问此联合类型的所有类型里共有的属性或方法**：

``` TS
function getLength(something: string | number): number {
    return something.length;
}
// error TS2339: Property 'length' does not exist on type 'string | number'.
// Property 'length' does not exist on type 'number'.
```

## 对象的类型——接口

在 TypeScript 中，我们使用接口（`Interfaces`）来定义对象的类型。

### 接口

+ 面向对象语言中，接口（`Interfaces`）是一个很重要的概念，它是对行为的抽象，而具体如何行动需要由类（`classes`）去实现（`implement`）。

+ `TypeScript` 中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状（`Shape`）」进行描述。

### 例子

``` TS
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};
```

上面的例子中，我们定义了一个接口 `Person`，接着定义了一个变量 `tom`，它的类型是 `Person`。这样，我们就约束了 `tom` 的形状必须和接口 `Person` 一致。**不可多也不可少。**

**接口一般首字母大写。有的编程语言中会建议接口的名称加上 `I` 前缀。**

### 可选属性

``` TS
interface Person {
    name: string;
    age?: number;
}

let tom: Person = {
    name: 'Tom'
};
```

加`?`即可，此时这个元素可选。可以缺元素，但是不可以多属性。

### 任意属性

``` TS
interface Person {
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    name: 'Tom',
    gender: 'male'
};
```

使用 `[propName: string]` 定义了任意属性取 `string` 类型的值。

**一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集：**

``` TS
interface Person {
    name: string;
    age?: number;
    [propName: string]: string;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};
//Property 'age' of type 'number' is not assignable to string index type 'string'.
//Type '{ name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//Property 'age' is incompatible with index signature.
//Type 'number' is not assignable to type 'string'.

//改成这样既可
interface Person {
    name: string;
    age?: number;
    [propName: string]: string | number;
}
```

### 只读属性

``` TS
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: any;
}

let tom: Person = {
    id: 89757,
    name: 'Tom',
    gender: 'male'
};

tom.id = 9527;
//Cannot assign to 'id' because it is a read-only property.
```

使用 `readonly` 定义的属性 `id` 初始化后，又被赋值了，所以报错了。

## 数组的类型

### 类型+方括号

+ 最简单的方法是使用「类型 + 方括号」来表示数组：

``` TS
let fibonacci: number[] = [1,1,2,3,5];
```

### 数组泛型

+ 我们也可以使用数组泛型`（Array Generic） Array<elemType>` 来表示数组

``` TS
let fibonacci: Array<number> = [1, 1, 2, 3, 5];
```

### 用接口表示数组

接口表示数组

``` TS
interface NumberArray {
    [index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];
```

### 类数组

类数组（Array-like Object）不是数组类型，比如 `arguments`：

``` TS
function sum() {
    let args: number[] = arguments;
}

//Type 'IArguments' is missing the following properties from type 'number[]': pop, push, concat, join, and 15 more.

//上例中，arguments 实际上是一个类数组，不能用普通的数组的方式来描述，而应该用接口：
function sum() {
    let args: {
        [index: number]: number;
        length: number;
        callee: Function;
    } = arguments;
}

//事实上常用的类数组都有自己的接口定义，如 IArguments, NodeList, HTMLCollection 等
function sum() {
    let args: IArguments = arguments;
}

//IArguments如下
interface IArguments {
    [index: number]: any;
    length: number;
    callee: Function;
}
```

### any在数组中的应用

一个比较常见的做法是，用 `any` 表示数组中允许出现任意类型：

``` TS
let list: any[] = ['xcatliu', 25, { website: 'http://xcatliu.com' }];
```

## 函数的类型

### ts函数基础

``` JS
// 函数声明（Function Declaration）
function sum(x, y) {
    return x + y;
}

// 函数表达式（Function Expression）
let mySum = function (x, y) {
    return x + y;
};
```

``` TS
function sum(x: number, y: number): number {
    return x + y;
}

//在 TypeScript 的类型定义中，=> 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```

### 接口定义函数形状

``` TS
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
```

### 可选参数

``` TS
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

**需要注意的是，可选参数必须接在必需参数后面。换句话说，可选参数后面不允许再出现必需参数**

### 参数默认值

``` TS
function buildName(firstName: string = 'Tom', lastName: string) {
    return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let cat = buildName(undefined, 'Cat');
```

### 剩余参数

注意，`rest` 参数只能是最后一个参数

``` TS
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3);
```

### 函数重载

``` TS
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```

上例中，我们重复定义了多次函数 `reverse`，前几次都是函数定义，最后一次是函数实现。在编辑器的代码提示中，可以正确的看到前两个提示。

注意，`TypeScript` 会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面。

上述方法可以传入`null`和`undefined`.不会有任何输出。因为`null`和`undefined`是所有类型的子类型。

## 类型断言

语法

``` TS
值 as 类型//建议使用

//or

<类型>值
```

### 用途

当 `TypeScript` 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型中共有的属性或方法：

``` TS
interface Cat {
    name: string;
    run(): void;
}
interface Fish {
    name: string;
    swim(): void;
}

function getName(animal: Cat | Fish) {
    return animal.name;
}

//报错
//roperty 'swim' does not exist on type 'Cat | Fish'.
//Property 'swim' does not exist on type 'Cat'.
function isFish(animal: Cat | Fish) {
    if (typeof animal.swim === 'function') {
        return true;
    }
    return false;
}

//断言
//这样就可以解决访问 animal.swim 时报错的问题
function isFish(animal: Cat | Fish) {
    if (typeof (animal as Fish).swim === 'function') {
        return true;
    }
    return false;
}
```

类型断言只能够「欺骗」TypeScript 编译器，无法避免运行时的错误，反而滥用类型断言可能会导致运行时错误

``` TS
function swim(animal: Cat | Fish) {
    (animal as Fish).swim();
}

const tom: Cat = {
    name: 'Tom',
    run() { console.log('run') }
};
swim(tom);
// Uncaught TypeError: animal.swim is not a function`
```

+ 联合类型可以被断言为其中一个类型

+ 将一个父类断言为更加具体的子类

+ 将任何一个类型断言为 any

+ 将 any 断言为一个具体的类型

