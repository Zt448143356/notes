# Git提供了一个换行符检查功能（core.safecrlf），可以在提交时检查文件是否混用了不同风格的换行符。这个功能的选项如下：

* false - 不做任何检查
* warn - 在提交时检查并警告
* true - 在提交时检查，如果发现混用则拒绝提交

建议使用最严格的 `true` 选项。

* 团队合作，一部分人在Windows上编程，一部分人在其他系统上，在这些情况下，可能就会遇到行尾结束符问题。
* Windows/Dos CRLF \r\n
* Linux/Unix LF \n
* MacOS CR \r

---

* Git可以在你提交时自动地把行结束符CRLF转换成LF，而在签出代码时把LF转换成CRLF。用core.autocrlf来打开此项功能。
* 若在Windows系统上，把它设置成true，这样当签出代码时，LF会被转换成CRLF：

---

# AutoCRLF
## 提交时转换为LF，检出时转换为CRLF
`git config --global core.autocrlf true`

## 提交时转换为LF，检出时不转换
`git config --global core.autocrlf input`

## 提交检出均不转换
`git config --global core.autocrlf false`
# SafeCRLF

## 拒绝提交包含混合换行符的文件
`git config --global core.safecrlf true`

## 允许提交包含混合换行符的文件
`git config --global core.safecrlf false`

## 提交包含混合换行符的文件时给出警告
`git config --global core.safecrlf warn`

---

