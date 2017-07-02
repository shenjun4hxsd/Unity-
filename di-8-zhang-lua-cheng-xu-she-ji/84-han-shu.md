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

&emsp;&emsp;一个Lua程序既可以使用以Lua编写的函数，又可以调用以C语言编写的函数，没有任何区别。

函数可以存储到变量中（无论全局变量还是局部变量）或table中，可以作为实参传递给其他函数，还可以作为其他函数的返回值。

一个函数可以嵌套在另一个函数中，内部的函数可以访问外部函数中的变量。

函数与其他值一样都是匿名的。当讨论一个函数名时（例如print），实际上是在讨论一个持有某函数的变量。

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