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