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

&emsp;&emsp;在元表中，每种算术操作符都有对应的字段名。除了上述的`__add`和`__mul`外，还有`__sub`（减法）、`__div`（除法）、`__unm`（相反数）、`__mod`（取模）和`__pow`（乘幂）。此外，还可以定义`__concat`字段，用于描述连接操作符的行为。

&emsp;&emsp;当两个集合相加时，可以使用任意一个集合的元表。然而，当一个表达式中混合了具有不同元表的值时，例如：

```lua
    s = Set.new{1, 2, 3}
    s = s + 8
```

&emsp;&emsp;Lua会按照如下步骤来查找元表：如果第一个值有元表，并且元表中有`__add`字段，那么Lua就以这个字段为元方法，而与第二个值无关；反之，如果第二个值有元表并含有`__add`字段，Lua就以此字段为元方法；如果两个值都没有元方法，Lua就引发一个错误。因此，上例会调用Set.union，而表达式10+s和“hello”+s也是一样的。

&emsp;&emsp;Lua可以包含这些混合类型，但实现需要注意如果执行了s=s+8，那么在Set.union内部就会发生错误：

```lua
    bad argument $1 to 'pairs' (table expected, got number)
```

&emsp;&emsp;如果想要得到更清楚的错误消息，则必须在实际操作前显式地检查操作数的类型：

```lua
    function Set.union(a, b)
        if getmetatable(a) ~= mt or getmetatable(b) ~= mt then
            error("attempt to 'add' a set with a non-set value", 2)
        end
        <与前例相同的内容>
```

&emsp;&emsp;注意，error的第二个参数（上例中的2）用于指示哪个函数调用造成了该错误消息。

&emsp;&emsp;

####关系类的元方法

&emsp;&emsp;元表还可以指定关系操作符的含义，元方法为`__eq`（等于）、`__lt`（小于）和`__le`（小于等于）。而其他3个关系操作符则没有单独的元方法，Lua会将`a~=b`转化为`not(a==b)`，将`a>b`转化为`b<a`，将`a>=b`转化为`b<=a`。


&emsp;&emsp;在Lua4.0之前，所有的顺序操作符都被转化为一种操作符（小于），例如，`a<=b`转化为`not(b<a)`。不过，这种转化遇到“部分有序（partial order）”就会发生错误。所谓“部分有序”是指，对于一种类型而言，并不是所有的值都能排序的。例如，大多数计算机中的浮点数就不是完全可以排序的。因为存在着一种叫“Not a Number(NaN)”的值。IEEE754是一份当前所有浮点数硬件都采用的事实标准，其中将NaN视为一种未定义的值，例如0/0的结果就是NaN。标准规定了任何涉及NaN的比较都应返回false（假）。这意味着`NaN<=x`永远为假，但是`x<NaN`也为假。因此，前面提到的将`a<=b`转化为`not(b<a)`就不合法了。

&emsp;&emsp;在上面的集合示例中，也存在着类似的问题。在集合操作中`<=`通常表示集合间的包含关系：`a<=b`通常意味着a是b的一个子集。根据这样的表示，仍有可能得到`a<=b`和`b<a`同时为假的情况。因此需要分别为`__le`（小于等于）和`__lt`（小于）提供实现：

```lua
mt.__le = function(a, b)		-- 集合包含
              for k in pairs(a) do
                  if not b[k] then return false end
              end
              return true
           end

    mt.__lt = function(a, b)
                  return a<=b and not (b<=a)
              end
```

&emsp;&emsp;最后，还可以定义集合的相等性判断：

```lua
    mt.__eq = function(a, b)
                  return a <= b and b <= a
              end
```

&emsp;&emsp;有了这些定义后，就可以比较集合了：

```lua
    s1 = Set.new{2, 4}
    s2 = Set.new{4, 10, 2}
    print(s1 <= s2)			-- true
    print(s1 < s2)			-- true
    print(s1 >= s1)			-- true
    print(s1 > s1)			-- false
    print(s1 == s2 * s1)	-- true
```

&emsp;&emsp;与算术类的元方法不同的是，关系类的元方法不能应用于混合的类型。对于混合类型而言，关系类元方法的行为就模拟这些操作符在Lua中普通的行为。如果试图将一个字符串与一个数字作顺序性比较，Lua会引发一个错误。同样，如果试图比较两个具有不同元方法的对象，Lua也会引发一个错误。

&emsp;&emsp;等于比较永远不会引发错误。但是如果两个对象拥有不同的元方法，那么等于操作不会调用任何一个元方法，而是直接返回false。这种行为模拟了Lua的普通行为。在Lua的普通行为中，字符串总是不等于数字的，与它们的值无关。另外，只有当两个比较对象共享一个元方法时，Lua才调用这个等于比较的元方法。

&emsp;&emsp;


####库定义的元方法

&emsp;&emsp;各种程序库在元表中定义它们自己的字段是很普通的方法。到目前为止介绍的所有元方法都只针对于Lua的核心，也就是一个虚拟机（virtual machine）。它会检测一个操作中的值是否有元表，这些元表中是否定义了关于此操作的元方法。从另一方面说，由于元表也是一种常规的table，所以任何人、任何函数都可以使用它们。

&emsp;&emsp;函数`tostring`就是一个典型的实例。在前面已介绍过`tostring`了，它能将各种类型的值表示为一种简单的文本格式：

```lua
    print({})			--> table: 0x8062ac0
```

&emsp;&emsp;函数`print`总是调用`tostring`来格式化其输出。当格式化任意值时，`tostring`会检查该值是否有一个`__tostring`的元方法。如果有这个元方法，`tostring`就用该值作为参数来调用这个元方法。接下来由这个元方法完成实现的工作，它返回的结果也就是`tostring`的结果。

&emsp;&emsp;在集合的示例中，已定义了一个将集合表示为字符串的函数。接下来要做的就是设置元表的`__tostring`字段：

```lua
    mt.__tostring = Set.tostring
```

&emsp;&emsp;此后只要调用`print`来打印集合，`print`就会调用`tostring`函数，进而调用到`Set.tostring`：

```lua
    s1 = Set.new{10, 4, 5}
    print(s1)				--> {4, 5, 10}
```

&emsp;&emsp;函数`setmetatable`和`getmetatable`也会用到元表中的一个字段，用于保护元表。假设想要保护集合的元表，使用户既不能看也不能修改集合的元表。那么就需要用到字段`__metatable`。当设置了该字段时，`getmetatable`就会返回这个字段的值，而`setmetatable`则会引发一个错误：

```lua
    mt.__metatable = "not your business"

    s1 = Set.new{}
    print(getmetatable(s1))		--> not your business
    setmetatable(s1, {})
    stdin:1: cannot change protected metatable
```
