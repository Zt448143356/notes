# 平时项目中常见小问题总结和拓展

## 盒子水平垂直居中

### 定位

父元素
``` CSS
position:relative;
```

子元素(一定要知道宽高)
``` CSS
width:100px
height:50px
position:absolute
top:50%
left:50%
margin-top:-25px
margin-left:-50px
```

----------------------

子元素（但是一定要有宽高）
``` CSS
width:100px
height:50px
position:absolute
top:0
left:0
right:0
bottom:0
margin:auto
```

--------------------------

子元素(兼容性不好)
``` CSS
position:absolute
top:50%
left:50%
transform:translate(-50%,-50%)
```

----------------------------

### flex
父元素(移动端常用，兼容不好)
``` CSS
display:flex
justfy-content:center
align-items:center
```

---------------------------

### table-cell
(文本的居中，一定要求父元素有固定宽高)
父元素
``` CSS
display:table-cell
vertical-align:middle
text-align:center
width:100px
height:50px
```

子元素：
``` CSS
display:inline-block
```

--------------------------

## 清除浮动

### 加高度

+ 给父类加高度（当时的脑子傻了的想法。）

### 加CSS

+ 给父元素添加属性 overflow: hidden;

### 子元素加div

``` CSS
div{
  clear: both;
  height: 0;
  overflow: hidden;
}
```

### 万能清除浮动法

``` CSS
父元素:after{
  content: "";
  height: 0;
  clear: both;
  overflow: hidden;
  display: block;
  visibility: hidden;
}
```