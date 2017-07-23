##字符串库

&emsp;&emsp;原始的Lua解释器操作字符串的能力是很有限的。一个程序只能创建字符串常量、连接字符串及获取字符串的长度。它无法提取子串或者检索字符串的内容。在Lua中真正的字符串操作能力来源于字符串库。

&emsp;&emsp;字符串库中的所有函数都导出在模块`string`中。在Lua5.1中，它还将这些函数导出作为`string`类型的方法。这样，假设要将一个字符串转换到大写形式，可以写`string.upper(s)`，也可以写`s:supper()`。但是为了避免与Lua5.0不兼容，在本书的大多数示例中将采用基于模块的写法。

&emsp;&emsp;

####基础字符串函数

&emsp;&emsp;字符串库中有一些函数非常简单。函数`string.len(s)`可返回字符串`s`的长度。函数`string.rep(s, n)`（或`s:rep(n)`）可返回字符串`s`重复`n`次的结果。例如，可以用`string.rep("a", 2^20)`来创建一个1MB的字符串。函数`string.lower(s)`可返回一份`s`的副本，其中所有的大写字母都被转换成小写形式，而其他字符则保持不变。`string.upper`与之相反，它将小写转换成大写。大小写转换函数有一个典型用途，假设要对一个字符串数组进行排序，并且不区分大小写，可以这样写：

```lua
    table.sort(a, function(a, b)
        return string.lower(a) < string.lower(b)
    end)
```

&emsp;&emsp;函数`string.upper`和`string.lower`都遵循当前的区域设置（local）。因此，如果在EuropeanLatin-1区域工作，那么表达式`string.upper("acao")`的结果就是“ACAO”。

&emsp;&emsp;函数`string.sub(s,i,j)`可以从字符串`s`中提取第`i`个到第`j`个字符。在Lua中，字符串的第一个字符的索引是1。还可以用负数索引，这样会从字符串的尾部开始计数，索引-1代表字符串的最后一个字符，-2代表倒数第二个字符，以此类推。这样，调用函数`string.sub(s,1,j)`或`s:sub(1,j)`，就可以得到字符串`s`中长度为`j`的前缀。调用`string.sub(s,j,-1)`或`s:sub(j)`，就可以得到字符串中从第`j`个字符开始的一个后缀。调用`string.sub(s,2,-2)`可以返回去掉字符串`s`的第一个和最后一个字符后的复制。

