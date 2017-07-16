##模块与包


&emsp;&emsp;通常，Lua不会设置规则（policy）。相反，Lua会提供许多强有力的机制来使开发者有能力实现出最适合的规则。然而，这种方法对于模块就不可行了。模块系统的一个主要目标是允许以不同的形式来共享代码。但若没有一项公共的规则就无法实现这样的共享。


&emsp;&emsp;Lua从5.1开始，为模块和包（`package`）定义了一系列的规则。这些规则不需要语言引入额外的技能，程序员可以使用他们早已熟知的`table`、函数、元表和环境来实现这些规则。然而，有两个重要的函数可以很容易通过这些规则，它们是`require`（用于使用模块）和`module`（用于创建模块）。程序员完全可以使用不同的规则来重新实现这两个函数。但是，新的实现可能会使程序无法使用外部模块，或者编写的模块无法被外部程序所使用。


&emsp;&emsp;从用户观点来看，一个模块就是一个程序库，可以通过`require`来加载。然后便得到了一个全局变量，表示一个`table`。这个`table`就像是一个名称空间，其内容就是模块中导出的所有东西，例如函数和常量。一个规范的模块还应使`require`返回这个`table`。


&emsp;&emsp;使用table来实现模块的优点在于，可以像操作普通table那样来操作模块，并且能利用Lua现有的功能来实现各种额外的功能。在大多数语言中，模块不是“`第一类值（first-class value）`”，所以那些语言需要为模块实现一套专门的机制。在Lua中，可以轻易地实现所有这些功能。


&emsp;&emsp;例如，一个用户要调用一个模块中的函数。其中最简单的方法是：

```lua
    require "mod"
    mod.foo()
```


&emsp;&emsp;如果希望使用较短的模块名称，则可以为模块设置一个局部名称：

```lua
    local m = require "mod"
    m.foo()
```


&emsp;&emsp;还可以为个别函数提供不同的名称：

```lua
    require "mod"
    local f = mod.foo
    f()
```


&emsp;&emsp;上述这些方法，都不需要来自于语言的显式支持，只需使用语言现有的内容。


&emsp;&emsp;

####require函数


&emsp;&emsp;Lua提供了一个名为`require`的高层函数用来加载模块，但这个函数只假设了关于模块的基本概念。对于`require`而言，一个模块就是一段定义了一些值的代码。


&emsp;&emsp;要加载一个模块，只需简单地调用`require "<模块名>"`。该调用会返回一个由模块函数组成的`table`，并且还会定义一个包含该`table`的全局变量。然而，这些行为都是由模块完成的，而非`require`。所以，有些模块会选择返回其他值，或者具有其他的效果。


&emsp;&emsp;即使知道某些用到的模块可能已加载了，但只要用到`require`就是一个良好的编程习惯。可以将标准库排除在此规则之外，因为Lua总是会预先加载它们。不过，有些用户还是喜欢为标准库中的模块使用显式的`require`：

```lua
    local m = require "io"
    m.write("hello world\n")
```


&emsp;&emsp;以下代码详细说明了`require`的行为：

```lua
    function require(name)
        if not packag.loaded[name] then        -- 模块是否已加载？
            local loader = findloader(name)
            if loader == nil then
                error("unable to load module " .. name)
            end
            package.loaded[name] = true        -- 将模块标记为已加载
            local res = loader(name)           -- 初始化模块
            if res ~= nil then
                package.loaded[name] = res
            end
        end
        return package.loaded[name]
    end
```


&emsp;&emsp;首先，它在`table package.loaded`中检查模块是否已加载。如果是的话，`require`就返回相应的值。因此，只要一个模块已加载过，后续的`require`调用都将返回同一个值，不会再次加载它。


&emsp;&emsp;如果模块尚未加载，`require`就试着为该模块找一个加载器（`loader`），会先在`table package.preload`中查询传入的模块名。如果在其中找到了一个函数，就以该函数作为模块的加载器。通过这个`preload table`，就有了一种通用的方法来处理各种不同的情况。通常这个`table`中不会找到有关指定模块的条目，那么`require`就会尝试从Lua文件或C程序库中加载模块。


&emsp;&emsp;如果`require`为指定模块找到了一个Lua文件，它就通过`loadfile`来加载该文件。而如果找到的是一个C程序库，就通过`loadlib`来加载。注意，`loadfile`和`loadlib`都只是加载了代码，并没有运行它们。为了运行代码，`require`会以模块名作为参数来调用这些代码。如果加载器有返回值，`require`就将这个返回值存储到`table package.loaded`中，以此作为将来对同一模块调用的返回值。如果加载器没有返回值，`require`就会返回`table package.loaded`中的值。在本章后面会看到，一个模块还可以将返回给`require`的值直接放入`package.loaded`中。


&emsp;&emsp;上述代码中还有一个重要的细节，就是在调用加载器前，`require`先将`true`赋予了`package.loaded`中的对应字段，以此将模块标记为已加载。这是因为如果一个模块要求加载另一个模块，而后者又要递归地加载前者。那么后者的`require`调用就会马上返回，从而避免了无限循环。


&emsp;&emsp;若要强制使`require`对用一个库加载两次的话，可以简单地删除`package.loaded`中的模块条目。例如，在成功地`require "foo"`后，`package.loaded["foo"]`就不为`nil`了。下面代码就可以再次加载该模块：

```lua
    package.loaded["foo"] = nil
    require "foo"
```


&emsp;&emsp;在搜索一个文件时，`require`所使用的路径与传统的路径有所不同。大部分程序所使用的路径就是一连串目录，指定了某个文件的具体位置。然而，ANSIC却没有任何关于目录的概念。所以，`require`采用的路径是一连串的模式（`pattern`），其中每项都是一种将模块名转换为文件名的方式。进一步说，这种路径中的每项都是一个文件名，每项中还包含一个可选的问号。`require`会用模块名来替换每个“?”，然后根据替换的结果来检查是否存在这样一个文件。如果不存在，就会尝试下一项。路径中的每项以分号隔开。例如，假设路径为：

```lua
    ?;?.lua;c:\windows\?;/usr/local/lua/?/?.lua
```


&emsp;&emsp;那么，调用`require "sql"`就会试着打开以下文件：

```lua
    sql
    sql.lua
    c:\windows\sql
    /usr/local/lua/sql/sql.lua
```


&emsp;&emsp;`require`函数只处理了分号（作为各项之间的分隔符）和问号。其他例如目录分隔符或文件扩展名，都由路径自己定义。


&emsp;&emsp;`require`用于搜索Lua文件的路径存放在变量`package.path`中。当Lua启动后，便以环境变量`LUA_PATH`的值来初始化这个变量。如果没有找到该环境变量，则使用一个编译时定义的默认路径来初始化。在使用`LUA_PATH`时，Lua会将其中所有的子串“;;”替换成默认路径。例如，假设`LUA_PATH`为“`mydir/?.lua;;`”，那么最终路径就是“`mydir/?.lua`”，并紧随默认路径。

&emsp;&emsp;如果`require`无法找到与模块名相符的Lua文件，它就会找C程序库。这类搜索会从变量`package.cpath`（相对于`package.path`）获取路径。而这个变量则是通过环境变量`LUA_CPATH`（相对于`LUA_PATH`）来初始化。在UNIX中，它的值一般是这样的：

```lua
    ./?.so;/usr/local/lib/lua/5.1/?.so
```


&emsp;&emsp;注意，文件的扩展名是由路径定义的（例如，上例中使用的.so）。而在Windows中，此路径通常可以是这样的：

```lua
    .\?.dll;C:\Program Files\Lua501\dll\?.dll
```

&emsp;&emsp;当找到一个C程序库后，`require`就会通过`package.loadlib`来加载它，`loadlib`在前面章节中已讨论过。C程序库与Lua程序块是不同的，它没有定义一个单一的主函数，而是导出了几个C函数。具有良好行为的C程序库应该导出一个名为“`luaopen_<模块名>`”的函数。`require`会在链接完程序库后，尝试调用这个函数。将在后续章节中讨论如何编写C程序库。

&emsp;&emsp;一般通过模块的名称来使用它们。但有时必须将一个模块改名，以避免冲突。一种典型的情况是，在测试中需要加载同一模块的不同版本。对于一个Lua模块来说，其内部名称不是固定的，可以轻易地编辑它以改变其名称。但是却无法编辑一个二进制数据模块中`luaopen_*函数的名称`。为了允许这种重命名，`require`用到了一个小技巧：如果一个模块名中包含了连字符，`require`就会用连字符后的内容来创建`luaopen_*函数名`。例如，若一个模块名为`a-b`，`require`就认为它的`open`函数名为`luaopen_b`，而不是`luaopen_a-b`。因此，如果要使用的两个模块名都为`mod`，那么可以将其中一个重命名为`v1-mod`（或者`-mod`，或其他类似形式）。当调用`m1 = require "v1-mod"`时，`require`会找到改名后的文件`v1-mod`，并将其中的函数`luaopen_mod`作为`open`函数。

&emsp;&emsp;

####编写模块的基本方法

&emsp;&emsp;在Lua中创建一个模块最简单的方法是：创建一个`table`，并将所有需要导出的函数放入其中，最后返回这个`table`。以下代码演示这种方法。注意，将`inv`声明为程序块的局部变量，就是将其定义成一个私有的名称。

```lua
    complex = {}

    function complex.new(r, i) return {r=r, i=i} end

    -- 定义一个常量'i'
    complex.i = complex.new(0, 1)

    function complex.add(c1, c2)
        return complex.new(c1.r  c2.r, c1.i + c2.i)
    end

    function complex.sub(c1, c2)
        return complex.new(c1.r - c2.r, c1.i - c2.i)
    end

    function complex.mul(c1, c2)
        return complex.new(c1.r*c2.r - c1.i*c2.i,
                           c1.r*c2.i + c1.i*c2.r)
    end

    local function inv(c)
        local n = c.r^2 + c.i^2
        return complex.new(c.r/n, -c.i/n)
    end

    function complex.div(c1, c2)
        return complex.mul(c1, inv(c2))
    end

    return complex
```

&emsp;&emsp;上例中使用`table`编写模块时，没有提供与真正模块完全一致的功能性，首先，必须显式地将模块名放到每个函数定义中。其次，一个函数在调用同一模块中的另一个函数时，必须限定被调用函数的名称。可以使用一个固定的局部名称（例如`M`）来定义和调用模块内的函数，然后将这个局部名称赋予模块的最终名称。通过这种方法，可以将上例改写为：

```lua
    local M = {}
    complex = M 					-- 模块名

    M.i = {r=0, i=1}
    function M.new(r, i) return {r=r, i=i} end

    function M.add(c1, c2)
        return M.new(c1.r + c2.r, c1.i + c2.i)
    end
    <如前>
```

&emsp;&emsp;只要一个函数调用了同一模块中另一个函数（或者递归地调用自己），就仍需要一个前缀名称。但至少两个函数之间的连接不再需要依赖模块名，并且也只需在整个模块中的一处写出模块名。实际上，可以完全避免写模块名，因为`require`会将模块名作为参数传给模块：

```lua
    local modname = ...
    local M = {}
    _G[modname] = M

    M.i = {r=0, i=1}
    <如前>
```

&emsp;&emsp;经过这样的修改，若需要重命名一个模块，只需重命名并定义它的文件就可以了。

&emsp;&emsp;另一项小改进与结尾的`return`语句有关。若能将所有与模块相关的设置任务集中在模块开头，会更好。消除`return`语句的一种方法是，将模块`table`直接赋予`package.loaded`：

```lua
    local modname = ...
    local M = {}
    _G[modname] = M
    package.loaded[modname] = M
    <如前>
```

&emsp;&emsp;通过这样的赋值，就不需要在模块结尾返回`M`了。注意，如果一个模块无返回值的话，`require`就会返回`package.loaded[modname]`的当前值。

&emsp;&emsp;


