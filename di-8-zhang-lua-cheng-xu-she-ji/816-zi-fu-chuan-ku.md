##字符串库

&emsp;&emsp;原始的Lua解释器操作字符串的能力是很有限的。一个程序只能创建字符串常量、连接字符串及获取字符串的长度。它无法提取子串或者检索字符串的内容。在Lua中真正的字符串操作能力来源于字符串库。

&emsp;&emsp;字符串库中的所有函数都导出在模块`string`中。在Lua5.1中，它还将这些函数导出作为`string`类型的方法。这样，假设要将一个字符串转换到大写形式，可以写`string.upper(s)`，也可以写`s:supper()`。但是为了避免与Lua5.0不兼容，在本书的大多数示例中将采用基于模块的写法。

&emsp;&emsp;

####基础字符串函数

&emsp;&emsp;字符串库中有一些函数非常简单。函数`string.len(s)`可返回字符串`s`的长度。函数`string.rep(s, n)`（或`s:rep(n)`）可返回字符串`s`重复`n`次的结果。例如，可以用`string.rep("a", 2^20)`来创建一个1MB的字符串。函数`string.lower(s)`可返回一份`s`的副本，其中所有的大写字母都被转换成小写形式，而其他字符则保持不变。`string.upper`与之相反，它将小写转换成大写。大小写转换函数有一个典型用途，假设要对一个字符串数组进行排序，并且不区分大小写，可以这样写：

```lua
    table.sort(a, function(a, b)
        return string.lower(a) < string.lower(b)
    end)
```

&emsp;&emsp;函数`string.upper`和`string.lower`都遵循当前的区域设置（local）。因此，如果在EuropeanLatin-1区域工作，那么表达式`string.upper("acao")`的结果就是“ACAO”。

&emsp;&emsp;函数`string.sub(s,i,j)`可以从字符串`s`中提取第`i`个到第`j`个字符。在Lua中，字符串的第一个字符的索引是1。还可以用负数索引，这样会从字符串的尾部开始计数，索引-1代表字符串的最后一个字符，-2代表倒数第二个字符，以此类推。这样，调用函数`string.sub(s,1,j)`或`s:sub(1,j)`，就可以得到字符串`s`中长度为`j`的前缀。调用`string.sub(s,j,-1)`或`s:sub(j)`，就可以得到字符串中从第`j`个字符开始的一个后缀。调用`string.sub(s,2,-2)`可以返回去掉字符串`s`的第一个和最后一个字符后的复制。

```lua
    s = "[in brackets]"
    print(string.sub(s, 2, -2)        --> in brackets
```

&emsp;&emsp;记住，Lua中的字符串是不可变的。和Lua中的所有其他函数一样，`string.sub`不会改变字符串的值，它只会返回一个新字符串。一种常见的错误是这么写：

```lua
    string.sub(s, 2, -2)      -- ❌
```

&emsp;&emsp;这里假定`s`的值就这样被改变了。如果要改变一个变量的值，就必须赋予它一个新的值：

```lua
    s = string.sub(s, 2, -2)
```

&emsp;&emsp;函数`string.char`和`string.byte`用于转换字符及其内部数值表示。`string.char`函数接受零个或多个整数，并将每个整数转换成对应的字符，然后返回一个由这些字符连接而成的字符串。`string.byte(s,i)`返回字符串`s`中第`i`个字符的内部数值表示，它的第二个参数是可选的，调用`string.byte(s)`可返回字符串s中第一个字符的内部数值表示。在下例中，假定字符是用ASCII表示的：

```lua
    print(string.char(97))        --> a
    i = 99;print(string.char(i, i+1, i+2))    --> cde
    print(string.byte("abc"))        --> 97
    print(string.byte("abc", 2))        --> 98
    print(string.byte("abc", -1))        --> 99
```

&emsp;&emsp;最后一行用了一个负数索引来访问字符串的最后一个字符。

&emsp;&emsp;在Lua5.1中，`string.byte`还可以接受可选的第三个参数。调用`string.byte(s,i,j)`可以返回索引`i`到`j`之间（包括`i`和`j`）的所有字符的内部表示值。

```lua
    print(string.byte("abc", 1, 2))        --> 97 98
```

&emsp;&emsp;`j`的默认值是`i`，因此在调用该函数时若不指定这个参数，那么就只返回第`i`个字符的值，这就与Lua5.0一样了。还有一种习惯写法是`{s:byte(1,-1)}`，这种写法会创建一个`table`，其中包含了`s`中所有字符的编码。有了这个`table`，就可以调用`string.char(unpack(t))`来重建原字符串。但是，由于Lua限制了一个函数的返回值数量，因此这项技术无法应用于较长的字符串。

&emsp;&emsp;函数string.format是用于格式化字符串的利器，经常用在输出上。它会根据第一个参数的描述，返回后续其他参数的格式化版本，这第一个参数也称为“格式化字符串”。编写格式化字符串的规则，与标准C语言中printf等函数的规则基本相同：它由常规文本和指示（directive）组成，这些指示控制了每个参数应放到格式化结果的什么位置，及如何放入它们。一个指示由字符‘%’加上一个字母组成，这些字母指定了如何格式化参数，例如‘d’用于十进制数、‘x’用于十六进制数、‘o’用于八进制数、‘f’用于浮点数和‘s’用于字符串等。在字符‘%’和字母之间可以再指定一些其他选项，用于控制格式的细节。例如，指定一个浮点数中有几个十进制数字。

```lua
    print(string.format("pi = %.4f", math.pi))        --> pi = 3.1416
    d = 5; m = 11; y = 1990
    print(string.format("%02d/%02d/%04d", d, m, y))        --> 05/11/1990
    tag, title = "h1", "a title"
    print(string.format("<%s>%s<%s>", tag, title, tag))        --> <h1>a title<h1>
```

&emsp;&emsp;在第一次打印中，%.4f表示一个浮点数的小数点后有4位数字。在第二次打印中，%02d表示一个十进制数字至少有两个数字，如不足两个数字，则用0补足；而指示%2d则表示用空格来补足。关于这些指示的完整描述可以参数Lua参考手册。或者查阅C语言手册，因为Lua是通过调用C标准库来完成实际的工作。

&emsp;&emsp;

####模式匹配（pattern-matching）函数

&emsp;&emsp;字符串库中最强大的函数是`find`、`match`、`gsub`（global substitution，全局替换）和`gmatch`（global match，全局匹配），它们都是基于“模式（pattern）”的。

&emsp;&emsp;不同于其他脚本语言，Lua既没有使用POSIX（regexp），也没有使用Perl正则表达式来进行模式匹配。其原因主要是考虑到Lua的大小。一个典型的POSIX正则表达式实现需要超过4000行代码，这相当于所有Lua标准库加在一起的大小。而相比之下，Lua采用的模式匹配实现的代码只有500行不到。当然，Lua的模式匹配所能达到的功能不及完整的POSIX实现。但是，Lua的模式匹配仍是一个强大的工具，并且它还具有一些特性，能在进行某些匹配时，比标准POSIX实现更为方便。

&emsp;&emsp;

#####&emsp;&emsp;● string.find函数

&emsp;&emsp;`string.find`函数用于在一个给定的目标字符串中搜索一个模式。最简单的模式就是一个单词，它只会匹配与自己完全相同的拷贝。例如，模式“`hello`”会搜索目标字符串中的子串“`hello`”。当`find`找到一个模式后，它会返回两个值：匹配到的起始索引和结尾索引。如果没有找到任何匹配，它就返回`nil`。

```lua
    s = "hello world"
    i, j = string.find(s, "hello")
    print(i, j)            --> 1 5
    print(string.sub(s, i, j))    --> hello
    print(string.fin(s, "world")    --> 7 11
    i, j = string.find(s, "l")
    print(i, j)            --> 3 3
    print(string.find(s, "llll"))        --> nil
```


&emsp;&emsp;如果匹配成功，就可以用`string.find`的返回值来调用`string.sub`，以此提取出目标字符串中匹配于该模式的那部分子串。

&emsp;&emsp;`string.find`函数还具有一个可选的第三个参数，它是一个索引，告诉函数应从目标字符串的哪个位置开始搜索。当处理所有与给定模式相匹配的部分时，这个参数就很有用。可以重复搜索新的匹配，且每次搜索都从上一次找到的位置开始。下面这个示例用字符串中所有换行符的位置创建了一个`table`：

```lua
    local t = {}		-- 存储索引的table
    local i = 0
    while true do
        i = string.find(s, "\n", i+1)		-- 查找下一个换行符
        if i == nil then break end
        t[#t + 1] = i
    end
```

&emsp;&emsp;接下来将介绍一种更简单的方法来编写这个循环，其中用到了`string.gmatch`迭代器。

&emsp;&emsp;

#####&emsp;&emsp;● string.match函数

&emsp;&emsp;从某种意义上说，函数`string.match`与`string.find`非常相似，它也是用于在一个字符串中搜索一种模式。不同之处在于，`string.match`返回的是目标字符串中与模式相匹配的那部分子串，而非该模式所在的位置。

```lua
    print(string.match("hello world", "hello"))		--> hello
```

&emsp;&emsp;对于固定的模式，例如“`hello`”，使用这个函数就没有什么意义了。但当使用变量模式（`Variable Pattern`）时，它的特性就显现出来了，如下示例：

```lua
    date = "Today is 17/7/1990"
    d = string.match(date, "%d+/%d+/%d+")
    print(d)		--> 17/7/1990
```

&emsp;&emsp;在后面将会讨论模式“`%d+/%d+/%d+`”的含义及`string.match`的高级用法。

&emsp;&emsp;

#####&emsp;&emsp;● string.gsub函数

&emsp;&emsp;`string.gsub`有3个参数：目标字符串、模式和替换字符串。它的基本用法是将目标字符串中所有出现模式的地方替换为替换字符串（最后一个参数）：

```lua
    s = string.gsub("Lua is cute", "cute", "great")
    print(s)			--> Lua s great
    s = string.gsub("all lii", "l", "x")
    print(s)			--> axx xii
    s = string.gsub("Lua is great", "Sol", "Son")
    print(s)			--> Lua is great
```

&emsp;&emsp;另外还有可选的第四个参数，可以限制替换的次数：

```lua
    s = string.gsub("all lii", "l", "x", 1)
    print(s)			--> axl lii
    s = string.gsub("all lii", "l", "x", 2)
    print(s)			--> axx lii
```

&emsp;&emsp;函数`string.gsub`还有另一个结果，即实际替换的次数。例如，以下代码就是一种统计字符串中空格数量的简单方法：

```lua
    count = select(2, string.gsub(str, " ", " "))
```

&emsp;&emsp;

#####&emsp;&emsp;● string.gmatch函数


&emsp;&emsp;`string.gmatch`会返回一个函数，通过这个函数可以遍历到一个字符串中所有出现指定模式的地方。例如，以下示例找出了给定字符串`s`中所有的单词：

```lua
    words = {}
    for w in string.gmatch(s, "%a+") do
        words[#words + 1] = w
    end
```

&emsp;&emsp;其中模式“`%a+`”表示匹配一个或多个字母字符的序列（也就是单词）。在本例中，`for`循环会遍历目标字符串中所有的单词，并且将它们储存到列表`words`中。

&emsp;&emsp;通过`gmatch`和`gsub`可以模拟出Lua中的`require`在寻找模块时所用的搜索策略，如下：

```lua
    function search(modname, path)
        modname = string.gsub(modname, "%.", "/")
        for c in string.gmatch(path, "[^;]+") do
            local fname = string.gsub(c, "?", modname)
            local f = io.open(fname)
            if f then
                f:close()
                return fname
            end
        end
        return nil		-- 未找到
    end
```

&emsp;&emsp;首先，用点符号来替换所有的目录分隔符，示例假设目录分隔符未‘`/`’。接下去我们就会看到，在模式中“点”具有特殊的含义，因此若要表示一个点必须写为“`%.`”。其次，函数遍历路径中的所有组成部分，这里所谓的组成部分是指那些不包含分号的最长子串。对于每个组成部分，都用模块名来替换其中的问号，以此获得最终的文件名。最后，检查该文件是否存在。如果存在，则关闭该文件，并返回它的名称。

&emsp;&emsp;

#####&emsp;&emsp;● 模式

&emsp;&emsp;可以用字符分类（character class）创建更多有用的模式。字符分类就是模式中的一项，可以与一个特定集合中的任意字符相匹配。例如，分类`%d`可匹配任意数字。如下例可以用模式“`%d%d/%d%d/%d%d%d%d`”搜索符合“`dd/mm/yyyy`”格式的日期：

```lua
    s = "Deadline is 30/05/1999, firm"
    date = "%d%d/%d%d/%d%d%d%d"
    print(string.sub(s, string.find(s, data)))		--> 30/05/1999
```

&emsp;&emsp;下表列出了所有的字符分类：

|||
|:--|:--|
|.|所有字符|
|%a|字母|
|%c|控制字符|
|%d|数字|
|%l|小写字母|
|%p|标点符号|
|%s|空白字符|
|%u|大写字母|
|%w|字母和数字字符|
|%x|十六进制数字|
|%z|内部表示为0的字符|


&emsp;&emsp;这些分类的大写形式表示它们的补集，例如，“`%A`”表示所有非字母字符：

```lua
    print(string.gsub("hello, up-down!", "%A", "."))
    --> hello..up.down.4
```

&emsp;&emsp;注意，打印出的“4”不是结果字符串的一部分，而是`gsub`的第二个结果，即替换的次数。在后续打印`gsub`结果的示例里，将忽略这个计数结果。

&emsp;&emsp;在模式中还有一些字符被称为“魔法字符”，它们具有特殊的含义。这些魔法字符有：

```lua
    ( ) . % + - * ? [ ] ^ $
```

&emsp;&emsp;字符‘`%`’作为这些魔法字符的转义符。因此“`%.`”表示匹配一个点，“`%%`”表示匹配字符‘`%`’本身。不仅可以将‘`%`’用于魔法字符，还可以用于其他所有非字母和数字的字符。当不确定某个字符是否需要转义时，应该前置一个转义符。

&emsp;&emsp;对于Lua来说，模式就是普通的字符串，并像其他字符串一样遵循相同的规则。只有模式函数才会解释它们，此时才会将‘`%`’当作转义符来处理。在模式中放入一个引号的方法，与在普通字符串中放入引号的方法相同。也就是说，需要用Lua的转义符‘`\`’来对引号进行转义。

&emsp;&emsp;在一对方括号内将不同的字符分类或单个字符组合起来，即可创建出属于用户自己的字符分类，这种新的字符分类叫做“`字符集（char-set）`”。例如，字符集“`[%w_]`”表示同时匹配字母、数字和下划线；字符集“`[01]`”表示匹配二进制数字；字符集“`[%[%]]`”表示匹配方括号本身。若要统计一段文本中元音的数量，可以这么写：

```lua
    nvow = select(2, string.gsub(text, "[AEIOUaeiou]", ""))
```

&emsp;&emsp;在字符集中包含一段字符范围的做法是写出字符范围的第一个字符和最后一个字符，并用横线连接它们。这个方法用的很少，因为大多数常用的字符范围都已预定义好了，例如“`[0-9]`”即为“`%d`”，“`0-9a-fA-F`”则为“`%x`”。不过，如果需要查找一个八进制数字，那么可以写“`[0-7]`”而不是“`[01234567]`”。在一个字符集前加一个‘`^`’，就可以得到这个字符集的补集，像上例模式“`[^0-7]`”表示所有非八进制数字的字符，而模式“`[^\n]`”则表示除了换行符以外的其他字符。对于简单的分类，使用其大写形式也可以得到其补集，“`%S`”显然要比“`[^%s]`”简单。

&emsp;&emsp;字符分类使用与Lua相同的区域设置。因此，“`[a-z]`”可能并不等于“`%l`”。在某些区域设置中，后者可能会包括像“`ç`”和“`ã`”这样的字母。一般情况下，选用后者，因为它更简单、更具移植性、也更高效。

&emsp;&emsp;还可以通过修饰符来描述模式中的重复部分和可选部分。Lua的模式提供4种修饰符：

|||
|:--|:--|
|+|重复1次或多次|
|*|重复0次或多次|
|-|也是重复0次或多次|
|?|可选部分（出现0或1次）|

&emsp;&emsp;“`+`”修饰符可匹配属于字符分类的一个或多个字符。它总是获取与模式相匹配的最长序列，例如，模式“`%a+`”表示一个或多个字母，即单词：

```lua
    print(string.gsub("one, and two; and three", "%a+", "word"))
    --> word, word word; word word
```

&emsp;&emsp;模式“`%d+`”匹配一个或多个数字（一个整数）：

```lua
    print(string.match("the number 1298 is evn", "%d+"))	--> 1298
```

&emsp;&emsp;修饰符“`*`”类似于“`+`”，但它还接受出现0次的情况。一种典型的用途是匹配一个模式不同部分之间的空格。例如，匹配像“`()`”或“`（）`”这样的一对空圆括号，可以使用模式“`%(%s*%)`”。其中“`%s*`”可匹配0个或多个空格。另一个示例是用模式“`[_%a][_%w]*`”匹配Lua程序中的标识符，标识符是一个由字母或下划线开始，并伴随0个或多个下划线、字母或数字的序列。

&emsp;&emsp;修饰符‘`-`’和‘`*`’一样，也是用于匹配属于字符分类的0个或多个字符的。不过，它会匹配最短的子串。虽然有时候“`*`”和“`-`”没什么差别，但通常它们所表现出来的结果却是截然不同的。例如，当试图用模式“`[_%a][_%w]-`”来查找一个标识符时，只会找到第一个字母，因为“`[_%w]-`”总是匹配空串。又如，假设要查找一个C程序中的注释，通常会首先尝试“`/%*.*%*/`”，然后由于“`.*`”会尽可能地扩展，因此程序中的第一个“`/*`”只会与最后一个“`*/`”相匹配：

```lua
    test = "int x; /* x */ int y; /* y */"
    print(string.gsub(test, "/%*.*%*/", "<COMMENT>"))
    --> int x; <COMMENT>
```

&emsp;&emsp;若使用“`.-`”模式，则会以尽可能少的扩展来找到第一个“`*/`”。这样就能得到想要的结果了：

```lua
    test = "int x; /* x */ int y; /* y */"
    print(string.gsub(test, "/%*.-%*/", "<COMMENT>"))
    --> int x; <COMMENT> int y; <COMMENT>
```

&emsp;&emsp;最后一个修饰符“`?`”可用于匹配一个可选的字符。例如，要在一段文本中寻找一个整数，而这个整数可以包括一个可选的正负号。那么使用模式“`[+-]?%d+`”就可以完成这项任务，它可以匹配像“`-12`”、“`23`”、“`+1009`”这样的数字，而“`[+-]`”是一个匹配‘`+`’和‘`-`’号的字符分类，后面的‘`?`’说明这个符号是可选的。

&emsp;&emsp;与其他系统不同的是，Lua中的修饰符只能应用于一个字符分类，无法对一组分类进行修饰。例如，无法写出匹配一个可选单词的模式。在本章的最后会介绍一些高级技术，可以使用它们来绕开这条限制。

&emsp;&emsp;如果模式以一个‘`^`’起始，那么它只会匹配目标字符串的开头部分。类似地，如果模式以‘`$`’结尾，那么它只会匹配目标字符串的结尾部分。这些标记可用于限定一个模式的查找。例如，下面这行测试：

```lua
    if string.find(s, "^%d") then ...
```

&emsp;&emsp;可检查字符串`s`是否以一个数字开头，而以下测试：

```lua
    if string.find(s, "^[+-]?%d+$") then ...
```

&emsp;&emsp;则可检查这个字符串是否表示一个整数，并且没有多余的前导字符和结尾字符。

&emsp;&emsp;在模式中，还可以使用“`%b`”，用于匹配成对的字符。它的写法是“`%b<x><y>`”，其中`<x>`和`<y>`是两个不同的字符，`<x>`作为一个起始字符，`<y>`作为一个结束字符。例如，模式“`%b()`”可匹配以‘`(`’开始，并以‘`)`’结束的子串：

```lua
    s = "a (enclosed (in) parentheses) line"
    print(string.gsub(s, "%b()", ""))		--> a line
```

&emsp;&emsp;这种模式的典型用法包括“`%b()`”、“`%b[]`”、“`%b{}`”和“`%b<>`”，但实际上可以用任何字符作为分隔符。

&emsp;&emsp;

#####&emsp;&emsp;● 捕获（capture）

&emsp;&emsp;捕获功能可根据一个模式从目标字符串中抽出匹配于该模式的内容。在指定捕获时，应将模式中需要捕获的部分写到一对圆括号内。

&emsp;&emsp;对于具有捕获的模式，函数`string.match`会将所有捕获到的值作为单独的结果返回。即它会将目标字符串切成多个捕获到的部分：

```lua
    pair = "name = Anna"
    key, value = string.match(pair, "(%a+)%s*=%s(%a+)")
    print(key, value)		--> name Anna
```