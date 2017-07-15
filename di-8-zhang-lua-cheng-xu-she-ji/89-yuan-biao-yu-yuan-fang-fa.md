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

&emsp;&emsp;

####table访问的元方法

&emsp;&emsp;算术类和关系类元算符的元方法都为各种错误情况定义了行为，它们不会改变语言的常规行为。但是Lua还提供了一种可以改变table行为的方法。有两种可以改变的table行为：查询table及修改table中不存在的字段。

#####● __index元方法

当访问一个table中不存在的字段时，得到的结果为nil。这是对的，但并非完全正确。实际上，这些访问会促使解释器去查找一个叫__index的元方法。如果没有这个元方法，那么访问结果如前述的为nil。否则，就由这个元方法来提供最终结果。

&emsp;&emsp;下面将介绍一个有关继承的典型示例。假设要创建一些描述窗口的table，每个table中必须描述一些窗口参数，例如位置、大小及主题颜色等。所有这些参数都有默认值，因此希望在创建窗口对象时可以仅指定那些不同于默认值的参数。第一种方法是使用一个构造式，在其中填写那些不存在的字段。第二种方法是让新窗口从一个原型窗口处继承所有不存在的字段。首先，声明一个原型和一个构造函数，构造函数创建新的窗口，并使它们共享同一个元表：

```lua
    Window = {} 		-- 创建一个名字空间
    -- 使用默认值来创建一个原型
    Window.prototype = {x=0, y=0, width=100, height=100}
    Window.mt = {} 		-- 创建元表
    -- 声明构造函数
    function Window.new(o)
        setmetatable(o, Window.mt)
        return o
    end
```

&emsp;&emsp;现在，来定义`__index`元方法：

```lua
    Window.mt.__index = function(table, key)
        return Window.prototype[key]
    end
```

&emsp;&emsp;在这段代码之后，创建一个新窗口，并查询一个它没有的字段：

```lua
    w = Window.new{x=10, y=20}
    print(w.width)			--> 100
```

&emsp;&emsp;若Lua检测到`w`中没有某字段，但在其元表中却有一个`__index`字段，那么Lua就会以`w(table)`和“`width`”（不存在的key）来调用这个`__index`元方法。随后元方法用这个key来索引原型table，并返回结果。

&emsp;&emsp;在Lua中，将`__index`元方法用于继承是很普通的方法，因此Lua还提供了一种更便捷的方式来实现此功能。`__index`元方法不必一定是一个函数，它还可以是一个table。当它是一个函数时，Lua以table和不存在的key作为参数来调用该函数，这就如同上述内容。而当它是一个table时，Lua就以相同的方式来重新访问这个table。因此，前例中`__index`的声明可以简单地写为：

```lua
    Window.mt.__index = Window.prototype
```

&emsp;&emsp;现在，当Lua查找到元表的`__index`字段时，发现`__index`字段的值是一个table，那么Lua就会在Window.prototype中继续查找。也就是说，Lua会在这个table中重复这个访问过程，类似于执行这样的代码：

```lua
    Window.prototype["width"]
```

&emsp;&emsp;然后由这次访问给出想要的结果。

&emsp;&emsp;将一个table作为`__index`元方法是一种快捷的、实现单一继承的方式。虽然将函数作为`__index`来实现相同功能的开销较大，但函数更加灵活。可以通过函数来实现多重继承、缓存及其他一些功能。

&emsp;&emsp;如果不想在访问一个table时涉及到它的`__index`元方法，可以使用函数`rawget`。调用`rawget(t, i)`就是对table t进行了一个“原始的(raw)”访问，也就是一次不考虑元表的简单访问。一次原始访问并不会加速代码执行，但有时会用到它。

#####● __newindex元方法

&emsp;&emsp;**`__newindex`元方法与`__index`类似，不同之处在于前者用于table的更新，而后者用于table的查询**。当对一个table中不存在的索引赋值时，解释器就会查找`__newindex`元方法。如果有这个元方法，解释器就调用它，而不是执行赋值。如果这个元方法是一个table，解释器就在此table中执行赋值，而不是对原来的table。此外，还有一个原始函数允许绕过元方法：调用`rawset(t,k,v)`就可以不涉及任何元方法而直接设置table t中与key k相关联的value v。

&emsp;&emsp;组合使用`__index`和`__newindex`元方法就可以实现出Lua中的一些强大功能，例如，只读的table、具有默认值的table和面向对象编程中的继承。

&emsp;&emsp;

#####● 具有默认值的table

&emsp;&emsp;常规table中的任何字段默认都是nil。通过元表就可以很容易地修改这个默认值：

```lua
    function setDefault(t, d)
        local mt = {__index = function() return d end}
        setmetatable(t, mt)
    end

    tab = {x=10, y=20}
    print(tab.x, tab.z)			--> 10 nil
    setDefault(tab, 0)
    print(tab.x, tab.z)			--> 10 0
```

&emsp;&emsp;在调用`setDefault`后，任何对tab中存在字段的访问都将调用它的`__index`元方法，而这个元方法会返回0（这个元方法中d的值）。

&emsp;&emsp;`setDefault`函数为所有需要默认值的table创建了一个新的元表。如果准备创建很多需要默认值的table，这种方法的开销或许就比较大了。由于在元表中默认值d是与元方法关联在一起的，所以`setDefault`无法为所有table都使用同一个元表。若要让具有不同默认值的table都使用同一个元表，那么就需要将每个元表的默认值都存放到table本身中。可以使用额外的字段来保持默认值。如果不担心名字冲突的话，可以使用“`___`”这样的key作为这个额外的字段：

```lua
    local mt = {__index = function(t) return t.___ end}
    function setDefault(t, d)
        t.___ = d
        setmetatable(t, mt)
    end
```

&emsp;&emsp;如果担心名称冲突，那么要确保这个特殊key的唯一性也很容易。只需创建一个新的table，并用它作为key即可：

```lua
    local key = {} 			-- 唯一的key
    local mt = {__index = function(t) return t[key] end}
    function setDefault(t, d)
        t[key] = d
        setmetatable(t, mt)
    end
```

&emsp;&emsp;还有一种方法可以将table与其默认值关联起来：使用一个独立的table，它的key为各种table，value就是各种table的默认值。不过，为了正确地实现这种做法，我们还需要一种特殊性质的table，就是“弱引用table(Weak Table)”。在这里我们就不使用它了。将在后续章节中详细讨论。
