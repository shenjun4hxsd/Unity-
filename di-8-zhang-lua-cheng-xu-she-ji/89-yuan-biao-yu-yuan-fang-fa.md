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