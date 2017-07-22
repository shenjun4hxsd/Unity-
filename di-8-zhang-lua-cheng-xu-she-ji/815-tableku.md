##table库

&emsp;&emsp;`table`库是由一些辅助函数构成的，这些函数将`table`作为数组来操作。其中，有用于在列表中插入和删除元素的函数，有对数组元素进行排序的函数，还有连接一个数组中所有字符串的函数。

&emsp;&emsp;

####插入和删除

&emsp;&emsp;函数table.insert用于将一个元素插入到一个数组的指定位置，它会移动后续元素以空出空间。例如，如果`t`是数组`{10, 20, 30}`，当调用`table.insert(t, 1, 15)`后，`t`会变为`{15, 10, 20, 30}`。但有一种特殊情况，如果在调用`insert`时没有指定位置参数，则会将元素添加到数组末尾。下面这段代码逐行地读取了程序的输入，并将所有的行保存在一个数组中：

```lua
    t = {}
    for line in io.lines() do
        table.insert(t, line)
    end
    print(#t)            -->(读入的行数)
```

&emsp;&emsp;函数`table.remove`会删除（并返回）数组指定位置上的元素，并将该位置之后的所有元素前移，以填补空隙。如果在调用这个函数时不指定位置参数，它就会删除数组的最后一个元素。

&emsp;&emsp;有了这两个函数，就可以很容易地实现栈、队列和双向队列。可以用`t={}`来初始化这种结构，`table.insert(t, x)`等价于压入操作，`table.remove(t)`等价于弹出操作。`table.insert(t,1,x)`会在结构的另一端（也就是起始处）插入一个新元素，而`table.remove(t,1)`会从这一端删除元素。后两个操作不是很高效，因为它们必须移动元素。不过由于`table`库中的函数都是用C语言实现的，所以这些循环的花销并不高，对于较小的数组来说使用这种实现较好。

&emsp;&emsp;

####排序

&emsp;&emsp;另一个有用的数组函数是`table.sort`，我们在此之前已经用到过它。它可以对一个数组进行排序，还可以指定一个可选的次序函数。这个次序函数有两个参数，如果希望第一个参数在排序结果中位于第二个参数值前，就应当返回`true`。如果没有提供这个函数，`sort`就使用默认的小于操作。

&emsp;&emsp;一个常见的错误是试图对一个`table`的索引进行排序。在`table`中，索引是一个无序的集合。如果对它们进行排序，则必须将它们复制到一个数组中，然后对这个数组进行排序。下面演示一个示例，假设要读取一个源文件，并构建一个`table`记录每个函数并定义它的行号：

```lua
    lines = {
        luaH_set = 10,
        luaH_get = 24,
        luaH_present = 48,
    }
```

&emsp;&emsp;现在要求按字母次序打印这些函数名。如果使用`pairs`来遍历这个`table`，就会发现遍历所得的名称是无序的。由于这些名称是`table`的`key`，因此不能对它们进行直接排序。但是，可以将它们放到一个数组中，然后对这个数组进行排序。正确的做法是先用这些名称创建一个数组，然后对数组排序，最后打印结果：

```lua
    a = {}
    for n in pairs(lines) do a[#a+1] = n end
    table.sort(a)
    for i,n in ipairs(a) do print(n) end
```

&emsp;&emsp;注意，对于Lua来说，数组也是无序的，它们本质上是`table`。然而由于用户知道如何计算索引，因此在访问数组时，只要使用有序的索引，就可以顺序地访问数组。这就是为什么必须用`ipairs`而不是`pairs`来遍历数组的原因。前者使`key`为`1、2、......`的顺序，而后者采用`table`的原始顺序。

&emsp;&emsp;另外还有一个更高级的方法，就是写一个迭代器，使它根据`table key`的次序来进行遍历。同时，还有一个可选参数`f`，用于指定某种特殊次序。以下函数先将`key`排序到一个数组中，然后迭代这个数组，且每步都返回原`table`中`key`和`value`。

```lua
    function pairsByKeys(t, f)
        local a = {}
        for n in pairs(t) do a[#a+1] = n end
        table.sort(a, f)
        local i = 0          -- 迭代器变量
        return function()    -- 迭代器函数
            i = i + 1
            return a[i], t[a[i]]
        end
    end
```

&emsp;&emsp;通过这个函数就可以很容易地按字母次序来打印那些函数名了：

```lua
    for name, line in pairsByKeys(line) do
        print(name, line)
    end
```

&emsp;&emsp;

####连接

&emsp;&emsp;在数据结构章节中已经看到过table.concat。它接受一个字符串数组，并返回这些字符串连接后的结果。它有一个可选参数，用于指定插到字符串之间的分隔符。这个函数另外还接受两个可选参数，用于指定第一个和最后一个要连接的字符串索引。

&emsp;&emsp;下面这个函数是table.concat的一个扩展，它能处理嵌套的字符串数组：

```lua
    function rconcat(l)
        if type(l) ~= "table" then return l end
        local res = {}
        for i=1, #l do
            res[i] = rconcat(l[i])
        end
        return table.concat(res)
    end
```

&emsp;&emsp;对于数组中的每个元素，rconcat都递归地调用自己，以此来连接所有可能嵌套的字符串数组。最后，它调用table.concat来连接这些结果部分。

```lua
    print(rconcat{{"a", {" nice"}}, " and", {{" long"}, {" list"}}})
    --> a nice and long list
```


🔚