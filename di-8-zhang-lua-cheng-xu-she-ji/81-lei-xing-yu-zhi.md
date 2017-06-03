##类型与值

&emsp;&emsp;Lua是一种动态类型的语言。在语言中没有类型定义的语法，每个值都“携带”了它自身的类型信息。


&emsp;&emsp;函数type可根据一个值返回其类型名称。type函数总是返回一个字符串

                print(type("Hello wold"))        -- string
                print(type(10.4*3))              -- number
                print(type(print))               -- function
                print(type(type))                -- function
                print(type(true))                -- boolean
                print(type(nil))                 -- nil
                print(type(type(X)))             -- string


&emsp;&emsp;变量没有预定义的类型，任何变量都可以包含任何类型的值

                print(type(a))                -- nil （a尚未初始化）
                a = 10
                print(type(a))                -- number
                a = "a string!!"
                print(type(a))                -- string
                a = print                     -- 是的，这是合法的！
                a(type(a))                    -- function

---

• nil（空）

                “无效值“
                将nil赋予一个全局变量等同于删除它。

---                
• boolean（布尔）

                有两个可选值：false和true
                
                然后boolean却不是一个条件值的唯一表示方式。
                在Lua中任何值都可以表示一个条件。Lua将值false和nil视为“假”，而除此之外的其他值视为“真”。
                请注意，Lua在条件测试中，将数字零和空字符串也都视为“真”。
       
---         
• number（数字）

---
• string（字符串）

                不可变性。
                需要以一对匹配的单引号或双引号来界定：
                                a = "a line"
                                b = 'another line'
                可以包含类似于C语言中的转义序列
                                
|转义符|说明|
|:--|:--|
|`\a`|响铃|
|`\b`|退格|
|`\f`|提供表格|
|`\n`|换行|
|`\r`|回车|
|`\t`|水平tab|
|`\v`|垂直tab|
|`\\`|反斜杠|
|`\"`|双引号|
|\'|单引号|

&emsp;&emsp;另外，还可以用一对匹配的双方括号来界定一个字母字符串，就像写“块注释”那样。以这种形式书写的字符串可以延伸多行，Lua不会解释其中的转义序列。

        例如：

                page = [[
                <html>
                <head>
                <title>An HTML Page</title>
                </head>
                <body>
                <a href="http://www.lua.org">Lua</a>
                </body>
                </html>
                ]]
                
                write(page)


&emsp;&emsp;有时字符串中可能需要包含这样的内容：a=b[c[i]]。或者，可能需要包含已经被注释掉的代码。

&emsp;&emsp;为了应付这种情况，需要在两个左方括号间加上任意数量的等号，就像[===[。经过这样修改后，字面字符串只有在遇到一个内嵌有相同数量等号的双右方括号时才会结束（就前例而言，即]===]）。如果一组左右方括号中的等号数量不等，那么Lua会忽略它。通过选择适当数量的等号，就可以在不加转义的情况下，直接嵌入任意的字符串内容了。

&emsp;&emsp;这套机制同样适用于注释。例如，以“--[=[”开始的一个块注释将延伸至“]=]”结束。如此便简化了注释那些“已经包含了注释块”的代码。


&emsp;&emsp;Lua提供了运行时的数字与字符串的自动转换。

1、在一个字符串上应用算术操作时，Lua会尝试将这个字符串转换成一个数字：

                print("10" + 1)                -- 11
                print("10 + 1")                -- 10 + 1
                print("-5.3e-10" * "2")        -- -1.06e-09
                print("hello" + 1)             -- 错误（不能转换“hello”）
                10=="10"                       -- false

> 显式地将一个字符串转换成数字，可以使用函数tonumber。当这个字符串的内容不能表示一个正确的数字时，tonumber将返回nil。

2、相反，在Lua期望一个字符串但却得到一个数字时，它也会将数字转换成字符串

                print(10 .. 20)                -- 1020

在Lua中，“`..`”是字符串连接操作符。当直接在一个数字后面输入它的时候，必须要用一个空格来分隔它们。不然，Lua会将第一个点理解为一个小数点。

                print(tostring(10)=="10")      -- true
                
`tostring`函数可将一个数字转换成字符串

3、在Lua 5.1中，可以在字符串前放置操作符“#”来获取该字符串的长度

                a = "hello"
                print(#a)                 -- 5
                print(#"good\0bye")       -- 8

---

• table

table类型实现了“关联数组”。“关联数组”是一种具有特殊索引方式的数组。不仅可以通过整数来索引它，还可以使用字符串或其他类型的值（除了nil）来索引它。

table没有固定的大小，可以动态地添加任意数量的元素到一个table中。

table是Lua中主要的（事实上也是仅有的）数据结构机制，具有强大的功能。

在Lua中，table既不是“值”也不是“变量”，而是“对象”。

table的创建是通过“构造表达式”完成的，最简单的构造表达式就是{ }。

                a = { }             -- 创建一个table，并将它的引用存储到a
                k = "x"
                a[k] = 10           -- 新条目，key = "x", value = 10
                a[20] = "great"     -- 新条目，key = 20, value = "great"
                print(a["x"])       -- 10
                k = 20
                print(a[k])         -- “great"
                a["x"] = a["x"] + 1 -- 递增条目"x"
                print(a["x"])       -- 11

table永远是“匿名的”，一个持有table的变量与table自身之间没有固定的关联性。

                a = { }
                a["x"] = 10
                b = a               -- b与a引用了同一个table
                print(b["x"])       -- 10
                b["x"] = 20
                print(a["x"])       -- 20
                a = nil             -- 现在只有b还在引用table
                b = nil             -- 再也没有对table的引用了
                -- 当一个程序再也没有对一个table的引用时，Lua的垃圾收集器最终会删除该table，并复用它的内存。
                
所有table都可以用不同类型的索引来访问value（值），当需要容纳新条目时，table会自动增长。

                a = { }
                
                -- 创建1000个新条目
                for i=1, 1000 do a[i] = i*2 end
                print(a[9])         -- 18
                a["x"] = 10
                print(a["x"])       -- 10
                print(a["y"])       -- nil 该元素没有初始化
                -- 可以将nil赋予table的某个元素来删除该元素。

Lua对于诸如a["name"]的写法提供了一种更简便的“语法糖（syntactic sugar）”，可以直接输入a.name。

                a.x = 10            -- 等同于a["x"] = 10
                print(a.x)          -- 等同于print(a["x"])
                print(a.y)          -- 等同于print(a["y"])
                
                -- 对于Lua来说，这两种形式是等价的，可供自由选择使用。
                a.x                 -- 等同于a["x"]
                a[x]                -- 以变量x的值来索引table
                
                ----------
                a = { }
                x = "y"
                a[x] = 10           -- 将10放入字段“y”
                print(a[x])         -- 10 字段“y”的值
                print(a.x)          -- nil 字段“x”（未定义）的值
                print(a.y)          -- 10 字段“y”的值

若要表示一个传统的数组或线性表，只需以整数作为key来使用table即可。这里不需要（也没有必要）声明一个大小值，直接初始化元素就可以了

                -- 读取10行内容，并存储到一个table中
                
                a = { }
                for i=1,10 do
                    a[i] = io.read()
                end
                
虽然可以用任何值作为一个table的索引，也可以用任何数字作为数组索引的起始值。但就Lua的习惯而言，数组通常以1作为索引的起始值。并且还有不少机制依赖于这个惯例。