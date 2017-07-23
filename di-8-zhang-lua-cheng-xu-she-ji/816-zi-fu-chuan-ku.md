##字符串库

&emsp;&emsp;原始的Lua解释器操作字符串的能力是很有限的。一个程序只能创建字符串常量、连接字符串及获取字符串的长度。它无法提取子串或者检索字符串的内容。在Lua中真正的字符串操作能力来源于字符串库。

&emsp;&emsp;字符串库中的所有函数都导出在模块string中。在Lua5.1中，它还将这些函数导出作为string类型的方法。这样，假设要将一个字符串转换到大写形式，可以写string.upper(s)，也可以写s:supper()。但是为了避免与Lua5.0不兼容，在本书的大多数示例中将采用基于模块的写法。

&emsp;&emsp;

####基础字符串函数

&emsp;&emsp;字符串库中有一些函数非常简单。函数string.len(s)可返回字符串s的长度。函数string.rep(s, n)（或s:rep(n)）可返回字符串s重复n次的结果。例如，可以用string.rep("a", 2^20)来创建一个1MB的字符串。函数string.lower(s)可返回一份s的副本，其中所有的大写字母都被转换成小写形式，而其他字符则保持不变。string.upper与之相反，它将小写转换成大写。大小写转换函数有一个典型用途，假设要对一个字符串数组进行排序，并且不区分大小写，可以这样写：

```lua
    table.sort(a, function(a, b)
        return string.lower(a) < string.lower(b)
    end)
```