##面向对象编程

&emsp;&emsp;Lua中的`table`就是一种对象，这句话可以从3个方面来证实。首先，`table`与对象一样可以拥有状态。其次，`table`也与对象一样拥有一个独立于其值的标识（一个`self`）。例如，两个具有相同值的对象（`table`）是两个不同的对象。最后，`table`与对象一样具有独立于创建者和创建地的生命周期。

&emsp;&emsp;对象有其自己的操作。同样`table`也有这样的操作：

```lua
    Account = {balance = 0}
    function Account.withdraw(v)
        Account.balance = Account.balance - v
    end
```