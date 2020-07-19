# babel-plugin-transform-remove-console

+ 配置只在发布阶段移除所有`console`

## Vue中配置（官方没有提供配置方案）

### 安装

`npm install babel-plugin-transform-remove-console --save-dev`

### 配置

+ 在babel中配置如下内容

``` JS
//生产环境插件
const prodPlugins = [];
if(process.env.VUE_APP_CURRENTMODE === 'prod'){
  //移除console插件
  prodPlugins.push('transform-remove-console')
}

module.exports = {
  presets: [
    '@vue/app'
  ],
  plugins: [
    ...prodPlugins
  ]
}
```
