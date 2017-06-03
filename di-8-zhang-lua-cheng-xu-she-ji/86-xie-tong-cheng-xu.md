##协同程序

&emsp;&emsp;协同程序与线程（thread）差不多，也就是一条执行序列，拥有自己独立的栈、局部变量和指令指针，同时又与其他协同程序共享全局变量和其他大部分东西。从概念上讲线程与协同程序的主要区别在于，一个具有多个线程的程序可以同时运行几个线程，而协同程序却需要彼此协作地运行。就是说，一个具有多个协同程序的程序在任意时刻只能运行一个协同程序，并且正在运行的协同程序只会在其显式地要求挂起（suspend）时，它的执行才会暂停。

###协同程序基础

&emsp;&emsp;Lua将所有关于协同程序的函数放置在一个名为“coroutine”的table中。

&emsp;&emsp;函数create用于创建新的协同程序，它只有一个参数，就是一个匿名函数。该函数的代码就是协同程序所需执行的内容。create会返回一个thread类型的值，用以表示新的协同程序。

```lua
    co = coroutine.create( function () print( "hi" ) end )
    print( co )    -- thread: 0x7fe2f1506218
```

&emsp;&emsp;一个协同程序可以处于4种不同的状态：挂起（suspended）、运行（running）、死亡（dead）和正常（normal）。当创建一个协同程序时，它处于挂起状态。也就是说，协同程序不会在创建它时自动执行其内容。可以通过函数status来检查协同程序的状态：

```lua
    print( coroutine.status( co ) )    -- suspended
```

&emsp;&emsp;函数coroutine.resume用于启动或再次启动一个协同程序的执行，并将其状态由挂起改为运行：

```lua
    coroutine.resume( co )             -- hi
```

&emsp;&emsp;在本例中，协同程序的内容只是简单地打印了“hi”后便终止了，然后它就处于死亡状态，也就再也无法返回了：

```lua
    print( coroutine.status( co ) )    -- dead
```

&emsp;&emsp;到目前为止，协同程序看上去还只是像一种复杂的函数调用方法。其实协同程序的真正强大之处在于函数yield的使用上，该函数可以让一个运行中的协同程序挂起，而之后可以再恢复它的运行。

```lua
    co = coroutine.create( function ( )
	for i = 1, 10 do
		print( "co", i )
		coroutine.yield( )
	end
    end )
```

&emsp;&emsp;现在当唤醒这个协同程序时，它就会开始执行，直到第一个yield：

```lua
    coroutine.resume( co )    -- co 1
```

&emsp;&emsp;如果此时检查其状态，会发现协同程序处于挂起状态，因此可以再次恢复其运行：

```lua
    print( coroutine.status( co ) )    -- suspended
```

&emsp;&emsp;从协同程序的角度看，所有在它挂起时发生的活动都发生在yield调用中。当恢复协同程序的执行时，对于yield的调用才最终返回。然后协同程序继续它的执行，直到下一个yield调用或执行结束：

```lua

```
