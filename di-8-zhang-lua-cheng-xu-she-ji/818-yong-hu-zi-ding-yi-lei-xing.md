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

&emsp;&emsp;如果由于某些原因，需要通过其他机制来分配内存。那么可以创建只有一个指针大小的userdata，然后将指向真正内存块的指针存入其中。在下一章中就有这样的例子。

&emsp;&emsp;以下函数就用lua_newuserdata创建了一个新的布尔数组：

```lua
    static int newarray(lua_State *L) {
        int i, n;
        size_t nbytes;
        NumArray *a;
        
        n = luaL_checkint(L, 1);
        luaL_argcheck(L, n >= 1, 1, "invalid size")
        nbytes = sizeof(NumArray) + I_WORD(n - 1)*sizeof(unsigned int);
        a = (NumArray *)lua_newuserdata(L, nbytes);
        
        a->size = n;
        for(i = 0; i <= I_WORD(n-1); i++)
            a->values[i] = 0;        /* 初始化数组 */
        return 1;        /* 新的userdata已在栈上 */
    }
```

&emsp;&emsp;其中，宏`luaL_checkint`只是在调用`luaL_checkinteger`后进行了一个类型转换。只要在Lua中注册好newarray，就可以通过语句a=array.new(1000)来创建一个新数组。

&emsp;&emsp;可以通过这样的调用array.set(array, index, value)，在数组中存储元素。后面的内容会介绍如何使用元表来实现更传统的语法array[index]=value。无论哪种写法，底层函数都是相同的。

&emsp;&emsp;下面将遵循Lua惯例，假设索引从1开始。

```lua
    static int setarray(lua_State *L)
        NumArray *a = (NumArray *)lua_touserdata(L, 1);
        int index = luaL_checkint(L, 2) - 1;
        luaL_checkany(L, 3);
        
        luaL_argcheck(L, a != NULL, 1, "'array' expected");
        
        luaL_argcheck(L, 0 <= index && index < a->size, 2, "index out of range");
        
        if(lua_toboolean(L, 3))
            a->values[I_WORD(index)] |= I_BIT(index);    /* 设置bit */
        else
            a->values[I_WORD(index)] &= ~I_BIT(index);    /* 重置bit */
        return 0;
```

&emsp;&emsp;由于Lua中任何值都可以转换为布尔，所以这里对第3个参数使用luaL_checkany，它只确保了在这个参数位置上有一个值。如果用错误的参数调用了setarray，就会得到这样的错误消息：

```lua
    array.set(0, 11, 0)
    --> stdin:1: bad argument #1 to 'set' ('array' expected)
    array.set(a, 0)
    --> stdin:1: bad argument #3 to 'set' (value expected)
```

&emsp;&emsp;下一个函数用于检索元素：

```lua
    static int getarray(lua_State *L) {
        NumArray *a = (NumArray *)lua_touserdata(L, 1);
        int index = luaL_checkint(L, 2) - 1;
        
        luaL_argcheck(L, a != NULL, 1, "'array' expected");
        
        lua_pushboolean(L, a->values[I_WORD(index)] & I_BIT(index));
        return 1;
    }
```

&emsp;&emsp;下面还定义了一个函数用于检索一个数组的大小：

```lua
    static int getsize(lua_State *L) {
        NumArray *a = (NumArray *)lua_touserdata(L, 1);
        luaL_argcheck(L, a != NULL, 1, "'array' expected");
        lua_pushinteger(L, a->size);
        return 1;
    }
```

&emsp;&emsp;最后，需要一些代码来初始化这个库：

```lua
    static const struct luaL_Reg arraylib [] = {
        {"new", newarray},
        {"set", setarray},
        {"get", getarray},
        {"size", getsize},
        {NULL, NULL}
    }
```