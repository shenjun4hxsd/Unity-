##I/O库

&emsp;&emsp;I/O库为文件操作提供了两种不同的模型，简单模型（`simple model`）和完整模型（`complete model`）。简单模型假设有一个当前输入文件和一个当前输出文件，它的I/O操作均作用于这些文件。完整模型则使用显式的文件句柄。它采用了面向对象的风格，并将所有的操作定义为文件句柄上的方法。

&emsp;&emsp;在本书前面的章节示例中涉及到的简单操作都使用简单模型并且十分方便。但是对于更多的高级文件操作，例如同时读取多个文件，它就无法做到了。对于这些高级操作，需要使用完整模型。

&emsp;&emsp;

#####&emsp;&emsp;简单I/O模型

&emsp;&emsp;简单模型的所有操作都作用于两个当前文件。I/O库将当前输入文件初始化为进程标准输入（`stdin`），将当前输出文件初始化为进程标准输出（`stdout`）。在执行`io.read()`操作时，就会从标准输入中读取一行。

&emsp;&emsp;用函数`io.input`和`io.output`可以改变这两个当前文件。`io.input(filename)`调用会以只读模式打开指定的文件，并将其设为当前输入文件。之后除非再次调用`io.input`，否则所有的输入都将来源于这个文件。在输出方面，`io.output`也可以完成类似的工作。如果出现错误，这两个函数都会引发（raise）错误。如果想直接处理这些错误，则必须使用完整模型中的`io.open`。

&emsp;&emsp;通常`write`比`read`简单些，首先看一下`write`。函数`io.write`接受任意数量的字符串参数，并将它们写入当前输出文件。它也可以接受数字参数，数字参数会根据常规的转换规则转换为字符串。如果想要完全地控制这种转换，则应该使用函数`string.format`：

```lua
    >io.write("sin (3) = ", math.sin(3), "\n")
    --> sin (3) = 0.14112000805987
    >io.write(string.format("sin (3) = %.4f\n", math.sin(3)))
    --> sin (3) = 0.1411
```

&emsp;&emsp;在实际操作中应当避免写出`io.write(a..b..c)`这样的代码，而是应该调用`io.write(a,b,c)`，它能达到与`io.write(a..b..c)`相同的效果，并且可以避免连接操作，因此效率更高。

&emsp;&emsp;无论使用`print`还是`io.write`都有一个原则。即在随意编写（quick-and-dirty）的程序中，或者为调试目的而编写的代码中，提倡使用`print`；而在其他需要完全控制输出的地方使用`write`。

```lua
    >print("hello", "Lua");print("Hi")
    --> hello Lua
    --> Hi
    
    >io.write("hello", "Lua");io.write("Hi", "\n")
    --> helloLuaHi
```

&emsp;&emsp;`write`与`print`有几点不同。首先，`write`在输出时不会添加像制表符或回车这样的额外字符。其次，`write`使用当前输出文件，而`print`总是使用标准输出。最后，`print`会自动调用其参数的`tostring()`方法，因此它还能显示`table`、函数和`nil`。

&emsp;&emsp;函数`io.read`从当前输入文件中读取字符串，它的参数决定了要读取的数据：

|||
|:--|:--|
|`"*all"`|读取整个文件|
|`"*line"`|读取下一行|
|`"*number"`|读取一个整数|
|`<num>`|读取一个不超过`<num>`个字符的字符串|

&emsp;&emsp;调用`io.read("*all")`会读取当前输入文件的所有内容，以当前位置作为开始。如果当前位置处于文件的末尾，或者文件为空，那么该调用会返回一个空字符串。

&emsp;&emsp;由于Lua可以高效地处理长字符串，因此在Lua中一种简单的、编写过滤器的技术就可以将整个文件读到一个字符串中，然后处理这个字符串（通常使用`gsub`），最后把这个字符串写到输出：

```lua
    t = io.read("*all")        -- 读取整个文件
    t = string.gsub(t, ...)    -- 做相关的处理
    io.write(t)                -- 写输出
```

&emsp;&emsp;下面是一个完整的示例，这段代码使用MIME quoted-printable编码方式对文件内容进行编码。在这种编码方式中，非ASCII字符被编码为`=<xx>`的形式，其中`<xx>`是这个字符的十六进制数字代码。此外，为了保持编码的一致性，字符“`=`”也需要被编码：

```lua
    t = io.read("*all")
    t = string.gsub(t, "([\128-\255=])"), function(c)
        return string.format("=%02X", string.byte(c))
        end)
    io.write(t)
```