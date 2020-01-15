# location [=|~|~*|^~] /uri/ { … }用法


# proxy_pass

比如访问 ： `http://localhost/dev/test.html`

### 第一种情况：
```
　　location ^~ /dev/{
　　　　proxy_pass http://localhost:8082/;
　　}
```
结果：`http://localhost:8082/test.html`

### 第二种情况：
```
　　location ^~ /dev/{
　　　　proxy_pass http://localhost:8082; #这次不带/
　　}
```
结果：`http://localhost:8082/dev/test.html`

### 第三种情况：
```
　　location ^~ /dev/{
　　　　proxy_pass http://localhost:8082/test/;
　　}
```
结果：`http://www.baidu.com/test/test.html`

### 第四种情况：
```
　　location ^~ /dev/{
　　　　proxy_pass http://localhost:8082/test; #这次不带 /
　　}
```
结果： `http://localhost:8082/testtest.html`