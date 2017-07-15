##环境

&emsp;&emsp;Lua将其所有的全局变量保存在一个常规的table中，这个table称为“环境（environment）”。这种组织结构的优点在于，其一，不需要再为全局变量创造一种新的数据结构，因此简化了Lua的内部实现。另一个优点是，可以像其他table一样操作这个table。为了便于实施这种操作，Lua将环境table自身保存在一个全局变量`_G`中。例如，以下代码打印了当前环境中所有全局变量的名称：

```lua
    for n in pairs(_G) do print(n) end
```