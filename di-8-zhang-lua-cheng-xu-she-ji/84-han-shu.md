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
```








&emsp;&emsp;函数可以存储到变量中（无论全局变量还是局部变量）或table中，可以作为实参传递给其他函数，还可以作为其他函数的返回值。

&emsp;&emsp;一个函数可以嵌套在另一个函数中，内部的函数可以访问外部函数中的变量。

&emsp;&emsp;函数与其他值一样都是匿名的。当讨论一个函数名时（例如print），实际上是在讨论一个持有某函数的变量。

####变长参数

Lua中的函数可以接受不同数量的实参。

```lua
    function add( ... )
	local s = 0
	for i, v in ipairs( ... ) do  -- 表达式{...}表示一个由所有变长参数构成的数组。
		s = s + v
	end
	return s
	end
```

参数中的3个点（...）表示该函数可接受不同数量的实参。


🔚