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