##函数

>一个函数若只有一个参数，并且此参数是一个字面字符串或table构造式，那么圆括号便是可有可无的。

```lua
    -- 例如：
    
    print "Hello World"    <-->    print("Hello World")
    dofile 'a.lua'         <-->    dofile('a.lua')
    print [[a multi-line   <-->    print([[a multi-line
      message]]                      message]]
    f{x=10, y=20}          <-->    f({x=10, y=20})
    type{}                 <-->    type({})
```

&emsp;&emsp;Lua为面向对象式的调用也提供了一种特殊的语法---冒号操作符。

```lua
    o.foo(o, x)      -- 另一种写法是 o::foo(x)
```

&emsp;&emsp;冒号操作符使调用o.foo时将o隐含地作为函数第一个参数。

&emsp;&emsp;一个Lua程序既可以使用以Lua编写的函数，又可以调用以C语言编写的函数，调用时没有任何区别。

&emsp;&emsp;调用函数时提供的实参数量可以与形參数量不同。Lua会自动调整实参的数量，以匹配参数表的要求。

```lua
    -- 假设一个函数如下：
    function f(a, b) return a or b end
    
    -- 在以下几种调用中，实参与形參的对应关系为：
    -- 调用        形參
    f(3)          a=3, b=nil
    f(3, 4)       a=3, b=4
    f(3, 4, 5)    a=3, b=4 (5被丢弃了) 
```

```lua
    -- 默认实参的应用
    
    function incCount(n)
        n = n or 1
        count = count + n
    end
```

####多重返回值

&emsp;&emsp;Lua允许函数返回多个结果。例如，用于字符串中定位一个模式的函数string.find。该函数若在字符串中找到了指定的模式，将返回匹配的起始字符和结尾字符的索引。

```lua
    startIndex, endIndex = string.find("hello Lua users", "Lua")
    print(startIndex, endIndex)        --> 7   9
```

&emsp;&emsp;以Lua编写的函数同样可以返回多个结果，只需在return关键字后列出所有的返回值即可。

```lua
    -- 查找数组中的最大元素，并返回该元素的位置：
    
    function maximum(a)
        local index = 1         -- 最大值的索引
        local max = a[index]    -- 最大值
        for i,val in ipairs(a) do
            if val > max then
                max = val; index = i
            end
        end
        return max, index
    end
    
    print(maximum{8, 10, 23, 12, 5})    --> 23   3
```

&emsp;&emsp;Lua会调整一个函数的返回值数量以适应不同的调用情况。若将函数调用作为一条单独语句时，Lua会丢弃函数的所有返回值。若将函数作为表达式的一部分来调用时，Lua只保留函数的第一个返回值。只有当一个函数调用是一系列表达式中的最后一个元素（或仅有一个元素）时，才能获得它的所有返回值。
&emsp;&emsp;这里所谓的“一系列表达式”在Lua中表现为4种情况：多重赋值、函数调用时传入的实参列表、table的构造式和return语句。

```lua
    function foo0() end                    -- 无返回值
    function foo1() return "a" end         -- 返回1个结果
    function foo2() return "a", "b" end    -- 返回2个结果
```

```lua
    x,y = foo2()                           -- x="a", y="b"
    x = foo2()                             -- x="a", "b"被丢弃
    x,y,z = 10, foo2()                     -- x=10, y="a", z="b"
    
    x,y = foo0()                           -- x=nil, y=nil
    x,y = foo1()                           -- x="a", y=nil
    x,y,z = foo2()                         -- x="a", y="b", z=nil
    
    -- 函数调用不是表达式最后一个元素，将只产生一个值：
    x,y = foo2(), 20                       -- x="a", y=20
    x,y = foo0(), 20, 30                   -- x=nil, y=20, 30被丢弃
    
    -- 函数调用作为另一个函数调用的最后一个（或仅有的）实参时，第一个函数的所有返回值都将作为实参传入第二个函数。如print：
    
    print(foo0())            -->
    print(foo1())            --> a
    print(foo2())            --> a  b
    print(foo2(), 1)         --> a  1
    print(foo2() .. "x")     --> ax
```

&emsp;&emsp;table构造式可以完整地接收一个函数调用的所有结果：

```lua
    t = {foo0()}            -- t = {}  (一个空的table)
    t = {foo1()}            -- t = {"a"}
    t = {foo2()}            -- t = {"a", "b"}
```

&emsp;&emsp;不过，这种行为只有当一个函数调用作为最后一个元素时才会发生，而在其他位置上的函数调用总是产生一个结果值：

```lua
    t = {foo0(), foo2(), 4}        -- t[1] = nil, t[2] = "a", t[3] = 4
```

&emsp;&emsp;最后一种情况是return语句，诸如return f()这样的语句将返回f的所有返回值：

```lua
    function foo(i)
        if i==0 then return foo0();
        elseif i==1 then return foo1();
        elseif i==2 then return foo2()
        end
    end
    
    print(foo(1))        -- a
    print(foo(2))        -- a  b
    print(foo(0))        -- (无返回值)
    print(foo(3))        -- (无返回值)
```

&emsp;&emsp;也可以将一个函数调用放入一对圆括号中，从而迫使它只返回一个结果：

```lua
    print((foo0()))      -- nil
    print((foo1()))      -- a
    print((foo2()))      -- a
```

&emsp;&emsp;请注意return语句后面的内容是不需要圆括号的。例如return (f(x))，将只返回一个值，而无关乎f返回了几个值。

&emsp;&emsp;关于多重返回值还要介绍一个特殊函数 --unpack。它接受一个数组作为参数，并从下标1开始返回该数组的所有元素：

```lua
    print(unpack{10, 20, 30})            --> 10  20  30
    a,b = unpack{10, 20, 30}             --> a=10, b=20, 30被丢弃
```
>Lua5.1及之前的版本中，unpack作为全局函数使用，可以直接
>```lua
    unpack(arg)
```
>而5.2之后，unpack被移到了table下面，于是直接unpack就会导致报错，新的调用应该为：
>```lua
    table.unpack(arg)
```

>同时修改的还有另外一个：arg
以前 `...` 可以在函数内直接使用`arg`来处理，但是5.1之后，就需要自己手动变换成`arg`了

>```lua
    local arg = {...}
    print(arg[1])
```

>提供一种兼容的方法

>```lua
    function test(...)
        if arg ~= nil then
            arg = {...}
        end
        if unpack != nil then		-- 5.1及之前的版本
            print(unpack(arg))
        else						-- 之后的版本
            local arg = {...}
            print(table.unpack(arg))
        end
    end
```

```lua
    f = string.find
    a = {"hello", "ll"}
```

f(unpack(a))将返回3和4，这与直接调用string.find("hello", "ll") 所返回的结果一摸一样。
虽然这个预定义函数unpack是用C语言直接编写的，但是仍可以在Lua中通过递归实现一样效果：

```lua
    function unpack(t, i)
        i = i or 1
        if t[i] then
            return t[i], unpack(t, i+1)
        end
    end
```



####变长参数

Lua中的函数可以接受不同数量的实参。

```lua
    -- 这个函数返回了所有参数的总和：
    
    function add( ... )
        local s = 0
        for i, v in ipairs( ... ) do  -- 表达式{...}表示一个由所有变长参数构成的数组。
            s = s + v
        end
        return s
    end
    
    print(add(3, 4, 10, 25, 12))        --> 54
```

&emsp;&emsp;参数中的3个点（...）表示该函数可接受不同数量的实参。

&emsp;&emsp;Lua提供了专门用于格式化文本（string.format）和输出文本（io.write）的函数。

```lua
    function fwrite(fmt, ...)
    return io.write(string.format(fmt, ...))
```
&emsp;&emsp;注意在3个点前有一个固定参数fmt。具有变长参数的函数同样也可以拥有任意数量的固定参数，但固定参数必须放在变长参数之前。

&emsp;&emsp;变长参数中可能会包含一些故意传入的nil，那么此时就需要用函数select来访问变长参数了。调用select时，必须传入一个固定实参selector(选择开关)和一系列变长参数。如果selector为数字n，那么select返回它的第n个可变实参；否则，selector只能为字符串“#”，这样select会返回变长参数的总数。

```lua
    for i=1, select('#', ...) do
        local arg = select(i, ...)		-- 得到第i个参数
        <循环体>
    end
```

&emsp;&emsp;特别需要指出的是，select('#', ...)会返回所有变长参数的总数，其中包括nil。


####具名实参

```lua
    -- 无效的演示代码
    rename(old="temp.lua", new="temp1.lua")   
```

&emsp;&emsp;Lua并不直接支持这种语法，但可以通过一种细微的改变来获得相同的效果。主要是将所有实参组织到一个table中，并将这个table作为唯一的实参传给函数。另外，还需要用到一种Lua中特殊的函数调用语法，就是当实参只有一个table构造式时，函数调用中的圆括号是可有可无的：

```lua
    rename{old="temp.lua", new="temp1.lua"}
```

&emsp;&emsp;另一方面，将rename改为只接受一个参数，并从这个参数中获取实际的参数：

```lua
    function rename(arg)
        return os.rename(arg.old, arg.new)
    end
```

&emsp;&emsp;若一个函数拥有大量的参数，而其中大部分参数是可选的话，这种参数传递风格会特别有用。例如在一个GUI库中，一个用于创建新窗口的函数可能会具有许多的参数，而其中大部分都是可选的，那么最好使用具名实参：


```lua
    w = Window{ x=0, y=0, width=300, height=200, 
                title="Lua", background="blue", border=true
              }
```

&emsp;&emsp;Window函数可以根据要求检查一些必填参数，或者为某些参数添加默认值。假设“_Window”才是真正用于创建新窗口的函数，它要求所有参数以正确的次序传入，那么Window函数可以这么写：

```lua
    function Window(options)
        -- 检查必要的参数
        if type(options.title) ~= "string" then
            error("no tile")
        elseif type(options.width) ~= "number" then
            error("no width")
    end

    -- 其他参数都是可选的
    _Window(options.title,
            options.x or 0,            -- 默认值
            options.y or 0,            -- 默认值
            options.width, options.height,
            options.backgournd or "white",        -- 默认值
            options.border            -- 默认值为false(nil)
    )
    end
```

####深入函数

&emsp;&emsp;函数可以存储到变量中（无论全局变量还是局部变量）或table中，可以作为实参传递给其他函数，还可以作为其他函数的返回值。

&emsp;&emsp;一个函数可以嵌套在另一个函数中，内部的函数可以访问外部函数中的变量。

&emsp;&emsp;函数与其他值一样都是匿名的。当讨论一个函数名时（例如print），实际上是在讨论一个持有某函数的变量。



🔚