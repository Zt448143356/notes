# fs模块

* fs.stat检测是文件还是目录
* fs.mkdir创建目录
* fs.writeFile创建写入文件
* fs.appendFile追加文件
* fs.readFile读取文件
* fs.readdir读取目录
* fs.rename重命名，复制文件
* fs.rmdir删除目录
* fs.unlink删除文件
* fs.createReadStream从文件流中读取数据
* fs.createWriteStream写入文件
* pipe复制文件可以


# http模块


# path模块

* `__dirname` 获取当前文件所属目录的绝对路径

  ``` JS
  console.log(__dirname);//C:\Users\HARBOUR\Desktop
  ```

* `__filename` 获取当前文件的绝对路径

  ``` JS
  console.log(__filename);//C:\Users\HARBOUR\Desktop\1.js
  ```

* `basename(path ,[ext]):` 返回尾缀

  ``` JS
  path.basename('/aaa/bbb/123.css');//123.css
  ```

* `dirname(path):` 返回目录名

  ``` JS
  path.dirname('/aaa/bbb/123.css');// /aaa/bbb
  ```

* `extname(path):` 返回拓展名(后缀)

  ``` JS
  path.extname('/aaa/bbb/123.css');//.css
  ```

* `format(pathObject):` 根据路径对象(pathObject)生成路径字符串(path)

  ``` JS
  path.format({
    root: '/ignored',
    dir: '/home/user/dir',
    base: 'file.txt'
  }); 
  // /home/user/dir\file.txt
  ```

* `isAbsolute(path):` 判断是不是绝对路径

  ``` JS
  console.log(path.isAbsolute('C:\\Users\\HARBOUR\\Desktop'));//true
  console.log(path.isAbsolute('C:/Users/HARBOUR/Desktop'));//true
  console.log(path.isAbsolute('Users\\HARBOUR\\Desktop'));//false
  console.log(path.isAbsolute('.Users\\HARBOUR\\Desktop'));//false
  console.log(path.isAbsolute('/Users/HARBOUR/Desktop'));//true
  ```

* `join([...paths]):` 使用平台特定的分隔符把全部给定的path片段连接到一起

  ``` JS
  path.join('/aaa', 'bbb', 'ccc/ddd', 'eee', '..');// '\aaa\bbb\ccc\ddd'
  ```

* `normalize(path):` 规范化给定的path，并解析 '`..`' 和 '`.`'

  ``` JS
  path.normalize('/aaa/bbb//ccc/ddd/eee/..');// '\aaa\bbb\ccc\ddd'
  ```

* `parse(path):` 解析`path`为路径对象(`pathObject`)

  返回的对象将具有以下属性:
    + dir `<string>`
    + root `<string>`
    + base `<string>`
    + name `<string>`
    + ext `<string>`
    
  ``` JS
  path.parse('C:/Users/HARBOUR/Desktop/1js')
  // { root: 'C:/',
  //   dir: 'C:/Users/HARBOUR/Desktop',
  //   base: '1.js',
  //   ext: '.js',
  //   name: '1' }
  ```

* `relative(from, to):` 返回从`from`到`to`的相对路径

  ``` JS
  path.relative('/aaa/bbb/cc.js', '/aaa/ddd/ee.js');// '..\..\ddd\ee.js'
  ```

* `resolve([...paths]):` 将路径或路径片段的序列解析为一个绝对路径

  ``` JS
  //e是我当前的运行的盘,notes是当前终端的目录
  console.log(path.resolve('/aa/cc' , 'bb'));// 'e:\aa\cc\bb'
  console.log(path.resolve('/aa/cc' , '/bb'));// 'e:\bb'
  console.log(path.resolve('/aa/cc' , './bb'));// 'e:\aa\cc\bb'
  console.log(path.resolve('/aa/cc' , '../bb'));// 'e:\aa\bb'

  console.log(path.resolve('aa/cc' , 'dd/ee' , '../bb'));// 'e:\notes\aa\cc\dd\bb'
  console.log(path.resolve('aa/cc' , '/dd/ee' , '../bb'));// 'e:\dd\bb'
  console.log(path.resolve('aa/cc' , './dd/ee' , '../bb'));// 'e:\notes\aa\cc\dd\bb'
  ```
  



