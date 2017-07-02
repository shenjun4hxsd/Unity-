##语句

&emsp;&emsp;Lua支持的常规语句基本上与C语言或Pascal语言中所支持的那些语句差不多。这些语句包括赋值、控制结构和过程调用。另外，Lua还支持一些不太常见的语句，例如多重赋值和局部变量声明。

&emsp;&emsp;

###• 赋值

```lua
    a = "hello" .. "world"
    t = { n = 0 }
    t.n = t.n + 1
```

&emsp;&emsp;Lua允许“多重赋值”，也就是一下子将多个值赋予多个变量。每个值或每个变量之间以逗号分隔。

```lua
    a, b = 10, 2*x    -- a为10，b为2*x
```

&emsp;&emsp;在多重赋值中，Lua先对等号右边的所有元素求值，然后才执行赋值。这样便可以用一句多重赋值来交互两个变量了。

```lua
    x, y = y, x                    -- 交换x与x
    a[i], a[j] = a[j], a[i]        -- 交换a[i]与a[j]
```

&emsp;&emsp;Lua总是会将等号右边值的个数调整到与左边变量的个数相一致。规则是：若值的个数少于变量的个数，那么多余的变量会被赋予nil；若值的个数更多的话，那么多余的值会被“静悄悄”丢弃掉：

```lua
    a, b, c = 0, 1
    print(a, b, c)            --> 0 1 nil
    a, b = a+1, b+1, b+2      -- 其中b+2会被忽略
    print(a, b)               --> 1 2
    a, b, c = 0
    print(a, b, c)            --> 0 nil nil

```

&emsp;&emsp;

###• 局部变量与块（block）

&emsp;&emsp;相对于全局变量，Lua还提供了局部变量。通过local语句来创建局部变量：

```lua
    x = 10                -- 全局变量
    local i = 1           -- 程序块中的局部变量
    
    while i <= x do
        local x = i*2     -- while循环体中的局部变量
        print(x)          --> 2, 4, 6, 8, ...
        i = i + 1
    end
    
    if i > 20 then
        local x           -- then中的局部变量
        x = 20
        print(x+2)        -- 如果测试成功会打印22
    else
        print(x)          --> 10 （全局变量）
    end
    
    print(x)              --> 10 （全局变量）
```

&emsp;&emsp;在交互模式中每行输入内容自身就形成了一个程序块。为了解决这个问题，可以显式地界定一个块，只需将这些内容放入一对关键字do-end中即可。每当输入了do时，Lua就不会单独地执行后面每行的内容，而是直至遇到一个相应的end时，才会执行整个块的内容。

&emsp;&emsp;如果需要更严格地控制某些局部变量的作用域时，这些do块也会有所帮助：

```lua
    do
        local a = 1
        local b = 2
    end            -- a和b的作用域至此结束
```

&emsp;&emsp;

###• 控制结构

&emsp;&emsp;Lua提供了一组传统的、小巧的控制结构，包括用于条件执行的if，用于迭代的while、repeat和for。所有的控制结构都有一个显式的终止符：if、for和while以end作为结尾，repeat以until作为结尾。

&emsp;&emsp;控制结构中的条件表达式可以是任何值，Lua将所有不是false和nil的值视为“真”。

&emsp;&emsp;

#### if then else

```lua
    if a < 0 then a = 0 end
    
    if a < b then return a else return b end
    
    if op == "+" then
        r = a + b
    elseif op == "-" then
        r = a - b
    elseif op == "*" then
        r = a * b
    elseif op == "\" then
        r = a / b
    else
        error("invalid operation")
    end
```

>&emsp;&emsp;Lua不支持switch语句。

&emsp;&emsp;

####while

```lua
    local i = 1
    while a[i] do
        print(a[i])
        i = i + 1
    end
```

&emsp;&emsp;

####repeat

&emsp;&emsp;一条repeat-until语句重复执行其循环体直到条件为真时结束。循环体至少会执行一次。

```lua
    repeat
        line = io.read()
    until line ~= ""
    print(line)
```

>&emsp;&emsp;与其他大多数语言不同的是，在Lua中，一个声明在循环体中的局部变量的作用域包括了条件测试：

```lua
    local sqr = x/2
    repeat
        sqr = (sqr + x/sqr)/2
        local error = math.abs(sqr^2 - x)
    until error < x/10000        -- 在此仍可以访问error
```

&emsp;&emsp;

####数字型for

&emsp;&emsp;for语句有两种形式：数字型for和泛型for。

&emsp;&emsp;数字型for的语法如下：

```lua
    for var=exp1, exp2,exp3 do
        <执行体>
    end
```

&emsp;&emsp;var从exp1变化到exp2，每次变化都以exp3作为步长（step）递增var，并执行一次“执行体”。第三个表达式是可选的，若不指定的话，Lua会将步长默认为1。

```lua
    for i=1, f(x) do print(i) end
    for i=10, 1, -1 do print(i) end
```

&emsp;&emsp;如果不想给循环设置上限的话，可以使用常量math.huge：

```lua
    for i=1, math.huge do
        if(0.3*i^3 - 20*i^2 - 500 >= 0) then
            print(i)
            break
        end
    end
```

&emsp;&emsp;首先，for的3个表达式是在循环开始前一次性求值的。例如，上例中的f(x)只会执行一次。其次，控制变量会被自动地声明为for语句的局部变量，并且仅在循环体内可见。

&emsp;&emsp;

####泛型for

&emsp;&emsp;泛型for循环通过一个迭代器函数来遍历所有值：

```lua
    -- 打印数组a的所有值
    for i,v in ipairs(a) do print(v) end
```

&emsp;&emsp;Lua的基础库提供了ipairs，这是一个用于遍历数组的迭代器函数。在每次循环中，i会被赋予一个索引值，同时v被赋予一个对应于该索引的数组元素值。

```lua
    -- 打印table t中所有的key
    for k in pairs(t) do print(k) end
```

>&emsp;&emsp;迭代table元素的（pairs）、迭代数组元素的（ipairs）、迭代字符串中单词的（string.gmatch）等。

&emsp;&emsp;

**泛型for循环与数字型for循环有两个相同点：**

        循环变量是循环体的局部变量；
        绝不应该对循环变量作任何赋值。
        
```lua
    days = {"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"}
    
    -- 需要创建如下talbe
    revDays = {["Sunday"] = 1, ["Monday"] = 2, ["Tuesday"] = 3, ["Wednesday"] = 4, ["Thursday"] = 5, ["Friday"] = 6, ["Saturday"] = 7}
    
    -- 则可以按如下实现
    revDays = {}
    for k, v in pairs(day) do
        revDays[v] = k
    end
```