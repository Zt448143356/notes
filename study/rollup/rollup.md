# rollup

## 简介

`rollup` 是一款像 `webpack` 一样的JS代码打包工具。它特别适合类库的维护，有了它你可以把单个复杂庞大的类库拆分成多个文件模块编写，最终打包成符合`UMD`、`AMD`、`CJS`、`ESM`、等格式的单个或多个文件。它可以利用最新的`ES6+ modules` 规范，`Tree-Shaking` 不需要的代码，这样你就可以放心的引入你喜欢类库中的某个方法，而不必担心引入整个类库。

+ 