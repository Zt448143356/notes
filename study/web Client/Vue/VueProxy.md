# 在目录中添加`Vue.config.js`文件
文件写入
``````JavaScript
module.exports = {
    publicPath: process.env.VUE_APP_NGINX_DIR,
    assetsDir: 'static',
    productionSourceMap: false,
    devServer: {
        port: 8081,
        open: true,
        proxy: {
            '/dev': {
                target: `http://127.0.0.1:8082/`,//代理地址
                changeOrigin: true,//如果设置成true：发送请求头中host会设置成target
                pathRewrite: {
                    '^/dev': ''//把/dev换成''
                }
            }
        }
    }
}
```