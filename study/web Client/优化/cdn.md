# CDN

+ 通过cdn方式来减少打包产生的文件过大问题

## Vue中操作如下

1. 在`vue.config.js`文件中通过`chainWebpack`去配置`webpack`

``` JS
module.exports = {
    优化cdn提速
    chainWebpack: config => {
        config.set('externals', {
            vue: 'Vue',
            'vue-router': 'VueRouter',
            axios: 'axios'
        })
    }
}
```

2. 在`public/index.html`中的`<head>`中添加外部CDN资源(注意`css`放`JavaScript`前面为了渲染效果)

``` HTML
<head>
<!--    添加cdn-->
    <script src="https://cdn.bootcdn.net/ajax/libs/axios/0.18.1/axios.min.js"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/vue-router/3.1.5/vue-router.min.js"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/vue/2.6.11/vue.min.js"></script>
</head>
```
