##数据文件与持久性

&emsp;&emsp;当涉及到数据文件的处理时，人们往往会认为写数据比读数据简单得多。当写一个文件时，对写的内容拥有完全的控制权。但是当读一个文件时，却无从得知会读到什么内容。一个强健的程序除了需要处理一个合法文件中所包含的所有类型的数据，还应能很好地处理坏损的文件。因此，编写一个强健的输入程序总是比较困难的。

&emsp;&emsp;在本章中，我们将看到如何使用Lua来避免程序中所有有关数据读取的代码，只需将数据按一种适当的格式书写就可以了。

&emsp;&emsp;

####数据文件

&emsp;&emsp;可以借由table构造式来定义一种文件格式。只需在写数据时做一点额外的工作，读取数据就会变得相当容易。这项技术也就是将数据作为Lua代码来输出，当运行这些代码时，程序也就读取了数据。而table的构造式可以使这些输出代码看上去更像是一个普通的数据文件。

&emsp;&emsp;下面通过一个示例来更清楚地理解这种做法。如果数据文件是一种预定义的格式，例如CSV（Comma-Separated Values，逗号分隔值）或XML，那么可以选择的做法很少。不过，如果是为了应用而创建数据文件的话，那么就可以使用Lua的构造式作为格式。在这种格式中，每条数据记录表示为一个Lua构造式。这样，原来以这种形式书写的数据文件：

```lua
    Donald E. Knuth, Literate Programming, CSLI, 1992
    Jon Bentley, More Programming Pearls, Addison-Wesley, 1990
```

&emsp;&emsp;现在可以改为：

```lua
    Entry{ "Donald E. Knuth",
           "Literate Programming",
           "CSLI",
           1992}
           
    Entry{ "Jon Bentley",
           "More Programming Pearls",
           "Addison-Wesley",
           1990}
```

&emsp;&emsp;记住，`Entry{<code>}`与`Entry({<code>})`是完全等价的，都是以一个table作为参数来调用函数Entry。因此，上面这段数据也是一个Lua程序。为了读取该文件，我们只需定义一个合适的Entry，然后运行此程序就可以了。例如，以下程序计算了数据文件中条目的数量：

```lua
    local count = 0
    function Entry(_) count = count + 1 end
    dofile("data")
    print("number of entries: " .. count)
```

&emsp;&emsp;下一个程序则可用于收集数据文件中所有作者的姓名，然后打印出这些姓名（不需要与文件中的次序相同）：

```lua
    local authors = {}            -- 作者姓名的集合
    function Entry(b) authors[b[1]] = true end
    dofile("data")
    for name in pairs(authors) do print(name) end
```

&emsp;&emsp;可以看到这些代码片段都采用了事件驱动的做法。Entry函数作为一个回调函数，在dofile时为数据文件中的每个条目所调用。

&emsp;&emsp;若文件不是非常大，可以使用名值对来表示每个字段：

```lua
    Entry{
        author = "Donald E. Knuth",
        title = "Literate Programming",
        publicsher = "CSLI",
        year = 1992
    }
    
    Entry{
        author = "Jon Bentley",
        title = "More Programming Pearls",
        publicsher = "Addison-Wesley",
        year = 1990
    }
```

&emsp;&emsp;这种格式就是“自描述的数据（self-describing data）”格式，其中每项数据都伴随一个表示其含义的简短描述。自描述的数据比CSV或其他紧缩格式更具可读性。当需要修改时，也易于手工编辑，可以在基本格式中作出一个细小的改动，而不需要同时改变数据文件。例如，如果要新增一个字段，只需修改读取程序中的一小块就可以了，内容就是当该字段不存在时提供一个默认值。