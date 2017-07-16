##面向对象编程

&emsp;&emsp;Lua中的`table`就是一种对象，这句话可以从3个方面来证实。首先，`table`与对象一样可以拥有状态。其次，`table`也与对象一样拥有一个独立于其值的标识（一个`self`）。例如，两个具有相同值的对象（`table`）是两个不同的对象。最后，`table`与对象一样具有独立于创建者和创建地的生命周期。

&emsp;&emsp;对象有其自己的操作。同样`table`也有这样的操作：

```lua
    Account = {balance = 0}
    function Account.withdraw(v)
        Account.balance = Account.balance - v
    end
```

&emsp;&emsp;上面的代码创建了一个新函数，并将该函数存入`Account`对象的`withdraw`字段中。则可进行如下调用：

```lua
    Account.withdraw(100.00)
```

&emsp;&emsp;这种函数就是所谓的“方法（Method）”。不过，在函数中使用全局名称`Account`是一个不好的编程习惯。因为这个函数只能针对特定对对象工作，并且，这个特定对象还必须存储在特定的全局变量中。如果改变了对象的名称，`withdraw`就再也不能工作了：

```lua
    a = Account;Account = nil
    a.withdraw(100.00)        -- 错误！❌
```

&emsp;&emsp;这种行为违反了前面提到的对象特性，即对象拥有独立的生命周期。

&emsp;&emsp;有一种灵活的方法，即指定一项操作所作用的“接受者”。因此需要一个额外的参数来表示该接受者。这个参数通常称为`self`或`this`：

```lua
    function Account.withdraw(self, v)
        self.balance = self.balance - v
    end
```

&emsp;&emsp;此时当调用该方法时，必须指定其作用的对象：

```lua
    a1 = Account; Account = nil
    ...
    a1.withdraw(a1, 100.00)        -- OK
```

&emsp;&emsp;通过对`self`参数的使用，还可以针对多个对象使用同样的方法：

```lua
    a2 = {balance=0, withdraw=Account.withdraw}
    ...
    a2.withdraw(a2, 260.00)
```

&emsp;&emsp;使用`self`参数是所有面向对象语言的一个核心。大多数面向对象语言都能对程序员隐藏部分`self`参数，从而使得程序员不必显式地声明这个参数。Lua只需使用冒号，则能隐藏该参数。即可将上例重写为：

```lua
    function Account:withdraw(v)
        self.balance = self.balance - v
    end
```