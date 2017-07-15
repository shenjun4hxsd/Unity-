##环境

&emsp;&emsp;Lua将其所有的全局变量保存在一个常规的table中，这个table称为“环境（environment）”。这种组织结构的优点在于，其一，不需要再为全局变量创造一种新的数据结构，因此简化了Lua的内部实现。另一个优点是，可以像其他table一样操作这个table。为了便于实施这种操作，Lua将环境table自身保存在一个全局变量`_G`中。例如，以下代码打印了当前环境中所有全局变量的名称：

```lua
    for n in pairs(_G) do print(n) end
```

&emsp;&emsp;在本章中，将看到几种关于环境操作的实用技术。

&emsp;&emsp;

####具有动态名字的全局变量

&emsp;&emsp;对于访问和设置全局变量，通常赋值操作就可以了。不过，有时也会用到一些元编程（meta-programming）的形式。例如，当操作一个全局变量时，而它的名称却存储在另一个变量中，或者需要通过运行时的计算才能得到。为了获取这个变量的值，许多程序员都试图写出这样的代码：

```lua
    value = loadstring("return " .. varname)()
```

&emsp;&emsp;如果varname是x，那么连接操作的结果就是字符串“return x”。这段代码就执行了这个字符串，并得到了x的值。然而，在这段代码中包含了一个新程序块的创建和编译。因此可以使用以下代码来完成相同的效果，但效率却比上例高出一个数量级：

```lua
    value = _G[varname]
```

&emsp;&emsp;正因为环境是一个常规的table，才可以使用一个key（变量名）去直接索引它。类似地，还可以动态地计算出一个名称，然后将一个值赋予具有该名称的全局变量：

```lua
    _G[varname] = value
```

&emsp;&emsp;不过注意，有些程序员对于该技能的运用就有些过度了，他们写出的`_G["a"] = _G["var1"]`，其实就是简单的一句`a=var1`。

&emsp;&emsp;上面问题的一般化形式是，允许使用动态的字段名，如“io.read”或“a.b.c.d”。如果直接写_G["io.read"]则不会从table io中得到字段read。但可以写一个函数`getfield`来实现这个效果，即通过调用`getfield("io.read")`返回所要求的结果。这个函数是一个循环，从`_G`开始逐个字段地深入求值：

```lua
    function getfield(f)
        local v = _G        -- 从全局变量的table开始
        for w in string.gmatch(f, "[%w_]+") do
            v = v[w]
        end
        return v
    end
```

&emsp;&emsp;依靠string库中的gmatch来遍历f中所有的单词。

&emsp;&emsp;与之对应的设置字段的函数则稍显复杂。像`a.b.c.d = v`这样的赋值等价于以下代码：

```lua
    local temp = a.b.c
    temp.d = v
```

&emsp;&emsp;也就是说，必须一直检索到最后一个名称，然后分别进行操作。下面这个函数setfield就完成了这项任务，并且创建路径中间那些不存在的table。

```lua
    function setfield(f, v)
        local t = _G            -- 从全局变量的table开始
        for w, d in string.gmatch(f, "([%w_]+)(%.?)" do
            if d == "." then    -- 是最后一个字段吗？
                t[w] = t[w] or {}    -- 如果不存在就创建table
                t = t[w]        -- 获取该table
            else                -- 最后的字段
                t[w] = v        -- 完成赋值
            end
        end
    end
```

&emsp;&emsp;上例中用到了一种字符串模式（pattern），通过这种模式就可以将字段名捕获到变量w中，并将一个可选的句号捕获到d中。通过调用上面这个函数：

```lua
    setfield("t.x.y", 10)
```

&emsp;&emsp;便创建了两个table：全局t和t.x，并将10赋予t.x.y：

```lua
    print(t.x.y)                --> 10
    print(getfield("t.x.y"))    --> 10
```

&emsp;&emsp;

####全局变量声明