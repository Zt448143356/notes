# less

[详查此处](https://less.bootcss.com/)

## 注释（Comments）

* 以//开头的注释，是不会被编译到css文件里的
* 以/**/包裹的注释会被编译到css文件中

## 变量（Variables）

用@来申明一个变量：@color:red; 

1. 作为普通属性值来使用：直接使用@color
2. 作为选择器和属性名：@{selector}的形式
3. 作为URL：@{url}
4. 变量的延时加载(多次定义变量，使用最后一次定义的变量)
5. 属性作为变量(使用$)(v3.0.0支持)

``` less
//less
@width: 10px;
@height: @width + 10px;
@header: header;
@images: "../img";

#@{header} {
  @var: red;
  width: @width;
  height: @height;
  color: $background-color;
  background-color: @var;
  background: url("@{images}/white-sand.png");
  @var: black;
}
```

编译后(Compiles to)

``` CSS
#header {
    width: 10px;
    height: 20px;
    background-color: black;
    background: url("../img/white-sand.png");
}
```

## 嵌套规则

1. 基本嵌套规则
2. &（父类选择）的使用

``` less
a {
  color: blue;
  &:hover {
    color: green;
  }
}
```

编译后(Compiles to)

``` CSS
a {
  color: blue;
}

a:hover {
  color: green;
}
```

## 继承

### 基础使用

+ 语法`:extend(继承的名称)`

``` less
nav ul {
  &:extend(.inline);
  background: blue;
}
.inline {
  color: red;
}
```


编译后(Compiles to)

``` CSS
nav ul {
  background: blue;
}
.inline,
nav ul {
  color: red;
}
```

#### 两种用法相同

``` less
.a:extend(.b) {}

// the above block does the same thing as the below block
.a {
  &:extend(.b);
}
```

#### all

``` less
.c:extend(.d all) {
  // extends all instances of ".d" e.g. ".x.d" or ".d.x"（继承“ .d”的所有实例，例如“ .x.d”或“ .d.x”）
}
.c:extend(.d) {
  // extends only instances where the selector will be output as just ".d"（只继承.d）
}
```

#### 继承多个

``` less
.e:extend(.f) {}
.e:extend(.g) {}

// the above and the below do the same thing
.e:extend(.f, .g) {}
```

#### 继承必须在最后

``` less
//This is NOT allowed
pre:hover:extend(div pre).nth-child(odd)
```

#### 继承嵌套选择器

``` less
.bucket {
  tr { // nested ruleset with target selector
    color: blue;
  }
}
.some-class:extend(.bucket tr) {} // nested ruleset is recognized
```

编译后(Compiles to)

``` CSS
.bucket tr,
.some-class {
  color: blue;
}
```

+ 是去继承编译后的css，不是原less文件

``` less
.bucket {
  tr & { // nested ruleset with target selector
    color: blue;
  }
}
.some-class:extend(tr .bucket) {} // nested ruleset is recognized
```


编译后(Compiles to)

``` CSS
tr .bucket,
.some-class {
  color: blue;
}
```


## 混合

### 普通混合

``` less
.a, #b {
  color: red;
}
.mixin-class {
  .a();
}
.mixin-id {
  #b();
}
```

编译后(Compiles to)

``` CSS
.a, #b {
  color: red;
}
.mixin-class {
  color: red;
}
.mixin-id {
  color: red;
}
```

### 不带输出的混合

``` less
.my-mixin {
  color: black;
}
.my-other-mixin() {
  background: white;
}
.class {
  .my-mixin();
  .my-other-mixin();
}
```

编译后(Compiles to)

``` CSS
.my-mixin {
  color: black;
}
.class {
  color: black;
  background: white;
}
```

### ！important

+ 全部带有！important

``` less
.foo (@bg: #f5f5f5, @color: #900) {
  background: @bg;
  color: @color;
}
.unimportant {
  .foo();
}
.important {
  .foo() !important;
}
```

编译后(Compiles to)

``` CSS
.unimportant {
  background: #f5f5f5;
  color: #900;
}
.important {
  background: #f5f5f5 !important;
  color: #900 !important;
}
```

### 混合参数

+ 多参数最好使用;好分割

#### 带参数

``` less
.border-radius(@radius) {
  -webkit-border-radius: @radius;
     -moz-border-radius: @radius;
          border-radius: @radius;
}

.button {
  .border-radius(6px);
}
```

#### 带参数且有默认值

``` less
.border-radius(@radius: 5px) {
  -webkit-border-radius: @radius;
     -moz-border-radius: @radius;
          border-radius: @radius;
}

.button {
  .border-radius();
}
```

#### 命名参数

+ 可以通过名称来引用参数（不用管顺序）

``` less
.mixin(@color: black; @margin: 10px; @padding: 20px) {
  color: @color;
  margin: @margin;
  padding: @padding;
}
.class1 {
  .mixin(@margin: 20px; @color: #33acfe);
}
.class2 {
  .mixin(#efca44; @padding: 40px);
}
```


编译后(Compiles to)

``` CSS
.class1 {
  color: #33acfe;
  margin: 20px;
  padding: 20px;
}
.class2 {
  color: #efca44;
  margin: 10px;
  padding: 40px;
}
```

#### @arguments变量

``` less
.box-shadow(@x: 0; @y: 0; @blur: 1px; @color: #000) {
  -webkit-box-shadow: @arguments;
     -moz-box-shadow: @arguments;
          box-shadow: @arguments;
}
.big-block {
  .box-shadow(2px; 5px);
}
```

编译后(Compiles to)

``` CSS
.big-block {
  -webkit-box-shadow: 2px 5px 1px #000;
     -moz-box-shadow: 2px 5px 1px #000;
          box-shadow: 2px 5px 1px #000;
}
```

#### @rest变量

``` less
.mixin(...) {        // matches 0-N arguments
.mixin() {           // matches exactly 0 arguments
.mixin(@a: 1) {      // matches 0-1 arguments
.mixin(@a: 1; ...) { // matches 0-N arguments
.mixin(@a; ...) {    // matches 1-N arguments

.mixin(@a; @rest...) {
  + @rest is bound to arguments after @a（绑定除a之后所有的参数）
  + @arguments is bound to all arguments(绑定所有参数)
}
```

#### 模式匹配

##### 参数名称匹配

+ `@_`参数是让调用mixin混合匹配前都先调用一次这个

``` less
.mixin(dark; @color) {
  color: darken(@color, 10%);
}
.mixin(light; @color) {
  color: lighten(@color, 10%);
}
.mixin(@_; @color) {
  display: block;
}


@switch: light;

.class {
  .mixin(@switch; #888);
}
```

编译后(Compiles to)

``` CSS
.class {
  color: #a2a2a2;
  display: block;
}
```

##### 参数数量匹配

``` less
.mixin(@w){
  width: @w;
}
.mixin(@w; @h){
  width:@w;
  height:@h;
}

.class{
  .mixin(10px);
}

.class1{
  .mixin(10px,20px)
}
```

编译后(Compiles to)

``` CSS
.class{
  width:10px;
}
.class1{
  width:10px;
  height:20px;
}
```

#### 混合调用选中属性和变量（3.5.0）

``` less
.average(@x, @y) {
  @result: ((@x + @y) / 2);
}

div {
  // call a mixin and look up its "@result" value
  padding: .average(16px, 50px)[@result];
}
```

编译后(Compiles to)

``` CSS
div {
  padding: 33px;
}
```

+ 当没有选择[]中的内容时，会直接选择最后一个

#### 递归混合

``` less
.loop(@counter) when (@counter > 0) {
  .loop((@counter - 1));    // next iteration
  width: (10px * @counter); // code for each iteration
}

div {
  .loop(5); // launch the loop
}
```

编译后(Compiles to)

``` CSS
div {
  width: 10px;
  width: 20px;
  width: 30px;
  width: 40px;
  width: 50px;
}
```

#### 混合守护

+ [详见](https://less.bootcss.com/features/#mixins-mixin-guards)

``` less
.mixin(@a) when (lightness(@a) >= 50%) {
  background-color: black;
}
.mixin(@a) when (lightness(@a) < 50%) {
  background-color: white;
}
.mixin(@a) {
  color: @a;
}
```

编译后(Compiles to)

``` CSS
.class1 { .mixin(#ddd) }
.class2 { .mixin(#555) }
```

## 免编译








































































































































