##元表（metatable）与元方法（meatmethod）

&emsp;&emsp;通常，Lua中的每个值都有一套预定义的操作集合。例如，可以将数字相加，可以连接字符串，还可以在table中插入一对key-value等。但是我们无法将两个table相加，无法对函数作比较，也无法调用一个字符串。

&emsp;&emsp;可以通过元表来修改一个值的行为，使其在面对一个非预定义的操作时执行一个指定的操作。例如，假设a和b都是table，通过元表可以定义如何计算表达式a+b。当Lua试图将两个table相加时，它会先检查两者之一是否有元表，然后检查该元表中是否有一个叫__add的字段。如果Lua找到了该字段，就调用该字段对应的值。这个值也就是所谓的“元方法”，它应该是一个函数，在本例中，这个函数用于计算table的和。

&emsp;&emsp;**Lua中的每个值都有一个元表**。table和userdata可以有各自独立的元表，而其他类型的值则共享其类型所属的单一元表。**Lua在创建新的table时不会创建元表**：

```lua
    t = {}
    print(getmetatable(t))        --> nil
```

&emsp;&emsp;可以使用setmetatable来设置或修改任何table的元表：

```lua
    t1 = {}
    setmetatable(t, t1)
    assert(getmetatable(t) == t1)
```

&emsp;&emsp;任何table都可以作为任何值的元表，而一组相关的table也可以共享一个通用的元表，此元表描述了它们的共同的行为。一个table甚至可以作为它自己的元表，用于描述其特有的行为。总之，任何搭配形式都是合法的。

&emsp;&emsp;**在Lua代码中，只能设置table的元表**。若要设置其他类型的值的元表，则必须通过C代码来完成。在第20章中，将会看到标准的字符串程序库为所有的字符串都设置了一个元表，而其他类型在默认情况中都没有元表。

```lua
    print(getmetatable("hi")         --> table:0x80772e0
    print(getmetatable(10))          --> nil
```

&emsp;&emsp;

####算术类的元方法

&emsp;&emsp;在本节中，会引入一个简单的示例，以说明如何使用元表。假设用table来表示集合，并且有一些函数用来计算集合的并集和交集等。为了保持名称空间的整齐，则将这些函数存入一个名为Set的table中。

```lua
    Set = {}
    
    -- 根据参数列表中的值创建一个新的集合
    function Set.new(l)
        local set = {}
        for _, v in ipairs(l) do set[v] = true end
        return set
    end
    
    function Set.union(a, b)
        local res = Set.new{}
        for k in pairs(a) do res[k] = true end
        for k in pairs(b) do res[k] = true end
        return res
    end
    
    function Set.intersection(a, b)
        local res = Set.new{}
        for k in pairs(a) do
            res[k] = b[k]
        end
        return res
    end
```

&emsp;&emsp;为了帮助检查此示例，还定义了一个用于打印集合的函数：

```lua
    function Set.tostring(set)
        local l = {}        -- 用于存放集合中所有元素的列表
        for e in pairs(set) do
            l[#l + 1] = e
        end
        return "{" .. table.concat(l, ", " .. "}"
    end
    
    function Set.print(s)
        print(Set.tostring(s))
    end
```

&emsp;&emsp;假设使用加号（`+`）来计算两个集合的并集，那么就需要让所有用于表示集合的table共享一个元表，并且在该元表中定义如何执行一个加法操作。第一步是创建一个常规的table，准备用作集合的元表：

```lua
    local mt = {}        -- 集合的元表
```

&emsp;&emsp;下一步是修改Set.new函数。这个函数是用于创建集合的，在新版本中只加了一行，即将mt设置为当前所创建table的元表：

```lua
    function Set.new(l)        -- 第2版
        local set = {}
        setmetatable(set, mt)
        for _, v in ipairs(l) do set[v] = true end
        return set
    end
```

&emsp;&emsp;在此之后，所有由Set.new创建的集合都具有一个相同的元表：

```lua
    s1 = Set.new{10, 20, 30, 50}
    s2 = Set.new{30, 1}
    print(getmetatable(s1))    --> table: 00672B60
    print(getmetatable(s2))    --> table: 00672B60
```

&emsp;&emsp;最后，将元方法加入元表中。在本例中，这个元方法就是用于描述如何完成加法的`__add`字段。

```lua
    mt.__add = Set.union
```

&emsp;&emsp;此后只要Lua试图将两个集合相加，它就会调用Set.union函数，并将两个操作数作为参数传入。可以使用加号来求集合的并集：

```lua
    s3 = s1 + s2
    Set.print(s3)        -->  {1, 10, 20, 30, 50}
```

&emsp;&emsp;类似地，还可以使用乘号来求集合的交集：

```lua
    mt.__mul = Set.intersection
    Set.print((s1 + s2)*s1)    --> {10, 20, 30, 50}
```

&emsp;&emsp;在元表中，每种算术操作符都有对应的字段名。除了上述的`__add`和`__mul`外，还有`__sub`（减法）、`__div`（除法）、`__unm`（相反数）、`__mod`（取模）和`__pow`（乘幂）。