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

&emsp;&emsp;调用时可写为：

```lua
    a:withdraw(100.00)
```

&emsp;&emsp;冒号的作用是在一个方法定义中添加一个额外的隐藏参数，以及在一个方法调用中添加一个额外的实参。冒号只是一种语法便利，并没有引入任何新的东西。例如，用点语法来定义一个函数，并用冒号语法调用它。反之，只要能正确地处理那个额外参数即可：

```lua
    Account = {balance=0, 
               withdraw=function(self, v)
                            self.balance = self.balance - v
                        end
              }
              
    function Account:deposit(v)
        self.balance = self.balance + v
    end
    
    Account.deposit(Account, 200.00)
    Account:withdraw(100.00)
```

&emsp;&emsp;现在的对象已有一个标识、一个状态和状态之上的操作。不过还缺乏一个类（class）系统、继承和私密性（privacy）。首先解决第一个问题，如何创建多个具有类似行为的对象？更准确地说，如何创建多个account账户对象？

&emsp;&emsp;

####类

&emsp;&emsp;一个类就像是一个创建对象的模具。有些面向对象语言提供了类的概念，在这些语言中每个对象都是某个特定类的实例。Lua则没有类的概念，每个对象只能自定义行为和形态。不过，要在Lua中模拟类也并不困难，可以参照一些基于原型的语言，例如`Self`和`NewtonScript`。在这些语言中，对象是没有“类型”的（objects have no classes）。而是每个对象都有一个原型（prototype）。原型也是一种常规的对象，当其他对象（类的实例）遇到一个未知操作时
，原型会先查找它。在这种语言中要表示一个类，只需创建一个专用作其他对象（类的实例）的原型。类和原型都是一种组织对象间共享行为的方式。

&emsp;&emsp;在Lua中实现原型很简单，使用后续章节所述的继承即可。更准确地说，如果有两个对象a和b，要让b作为a的一个原型，只需输入如下语句：

```lua
    setmetatable(a, {__index = b})
```

&emsp;&emsp;在此之后，a就会在b中查找所有它没有的操作。若将b称为是对象a的类，只不过是术语上的一个变化。

&emsp;&emsp;回到先前银行账号的示例。为了创建更多与`Account`行为类似的账号，可以让这些新对象从`Account`行为中继承这些操作。具体做法就是使用`__index`元方法。可以应用一项小优化，则无须创建一个额外的`table`作为账户对象的元表。而是使用`Account table`自身作为元表：

```lua
    function Account:new(o)
        o = o or {}        -- 如果用户没有提供table，则创建一个
        setmetatable(o, self)
        self.__index = self
        return o
    end
```

