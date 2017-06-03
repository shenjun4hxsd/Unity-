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