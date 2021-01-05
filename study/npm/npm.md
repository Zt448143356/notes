# npm操作

[文档](https://www.npmjs.cn/)

## 换源

### 查看当前使用的仓库源
`npm config get registry`

### 切换淘宝仓库源
`npm config set registry https://registry.npm.taobao.org`

### 切换官方源
`npm config set registry http://registry.npmjs.org`

## npm 发包

### npm login(登录)

+ 要求在官方源下登录，若不是官方源请切换至官方源

`npm login`

### npm whoami(查看npm账号)

`npm whoami`

+ 返回当前账号

### npm init(初始化)

`npm init`

+ 创建`pakeage.json`一些项目信息

### npm publish

`npm publish`

发布一个包到`npm`上