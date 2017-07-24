##用户自定义类型

&emsp;&emsp;本章将介绍如何用C语言编写新的类型来扩展Lua。下面将从一个小示例入手，使用元表和其他机制来扩展它。

&emsp;&emsp;这个示例实现了一种很简单的类型--布尔数组。选用这个示例是因为它不涉及到复杂的算法，从而可以使读者专注于API的问题。不过，这个示例本身还是具有实用价值的。当然，可以在Lua中用table来实现布尔数组。但C语言实现可以将每个布尔值存储在一个bit中，从而将内存用量减少到不足table方法的3%。

&emsp;&emsp;这个实现需要以下定义：

```lua
    #include <limits.h>
    
    #define BITS_PER_WORD (CHAR_BIT* sizeof(unsigned int))
    #define I_WORD(i)    ((unsigned int)(i)/BITS_PER_WORD)
    #define I_BIT(i)    (1 << ((unsigned int)(i) % BITS_PER_WORD))
```

&emsp;&emsp;`BITS_PER_WORD`是一个无符号整型的`bit`数量。宏`I_WORD`根据给定的索引来计算对应的`bit`位所存放的`word`（字），`I_BIT`计算出一个掩码，用于访问这个`word`中的正确`bit`。

&emsp;&emsp;可以使用以下结构来表示数组：

```lua
    typedef struct NumArray {
        int size;
        unsigned int values[1];    /* 可变部分 */
    } NumArray
```

&emsp;&emsp;这里。由于C89标准不允许分配0长度的数组，所以声明了数组values需要有一个元素来作为占位符。接下来会在分配数组时定义实际的大小。下面这个表达式可以计算出具有n个元素的数组大小：

```lua
    sizeof(NumArray) + I_WORD(n - 1)*sizeof(unsigned int)
```

&emsp;&emsp;注意，这里无须对I_WORD加1，因为原来的结构中已经包含了一个元素的空间。

&emsp;&emsp;

####userdata

&emsp;&emsp;首先要面临的问题是如何在Lua中表示这个NumArray结构。Lua为此提供了一种基本类型`userdata`。`userdata`提供了一块原始的内存区域，可以用来存储任何东西。并且，在Lua中`userdata`没有任何预定义的操作。

&emsp;&emsp;函数`lua_newuserdata`会根据指定的大小分配一块内存，并将对应的userdata压入栈中，最后返回这个内存块的地址：

```lua
    void *lua_newuserdata(lua_State *L, size_t size);
```