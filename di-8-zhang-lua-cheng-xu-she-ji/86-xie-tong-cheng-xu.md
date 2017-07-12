##协同程序

&emsp;&emsp;协同程序与线程（thread）差不多，也就是一条执行序列，拥有自己独立的栈、局部变量和指令指针，同时又与其他协同程序共享全局变量和其他大部分东西。从概念上讲线程与协同程序的主要区别在于，一个具有多个线程的程序可以同时运行几个线程，而协同程序却需要彼此协作地运行。就是说，**一个具有多个协同程序的程序在任意时刻只能运行一个协同程序，并且正在运行的协同程序只会在其显式地要求挂起（suspend）时，它的执行才会暂停。**

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
    coroutine.resume( co )    -- co 2
    coroutine.resume( co )    -- co 3
    ...
    coroutine.resume( co )    -- co 10
    coroutine.resume( co )    -- 什么都不打印
```

&emsp;&emsp;在最后一次调用resume时，协同程序的内容已经执行完毕，并已经返回。因此，这时协同程序处于死亡状态。如果试图再次恢复它的执行，resume将返回false及一条错误消息：

```lua
    print(coroutine.resume( co ))    -- false cannot resume dead coroutine
```

&emsp;&emsp;请注意，resume是在保护模式中运行的。因此，如果在一个协同程序的执行中发生任何错误，Lua是不会显示错误消息的，而是将执行权返回给resume调用。

&emsp;&emsp;当一个协同程序A唤醒一个协同程序B时，协同程序A就处于一个特殊状态，既不是挂起状态（无法继续A的执行），也不是运行状态（是B在运行）。所以将这时的状态称为“正常”状态。

&emsp;&emsp;Lua的协同程序还具有一项有用的机制，就是可以通过一对resume-yield来交换数据。在第一次调用resume时，并没有对应的yield在等待它，因此所有传递给resume的额外参数都将视为协同程序主函数的参数：

```lua
    co = coroutine.create( function ( a, b, c )
	print( "co", a, b, c )
    end )
    coroutine.resume( co, 1, 2, 3 )    -- co 1 2 3
```

&emsp;&emsp;在resume调用返回的内容中，第一个值为true则表示没有错误，而后面所有的值都是对应yield传入的参数：

```lua
    co = coroutine.create( function ( a, b )
	coroutine.yield( a + b, a - b )
    end )

    print( coroutine.resume( co, 20, 10 ) )    -- true 30 10
```

&emsp;&emsp;与此对应的是，yield返回的额外值就是对应resume传入的参数：

```lua
    co = coroutine.create( function ( )
        print( "co", coroutine.yield( ) )
    end )
    
    print(coroutine.resume( co, "a" ))  -- true
    print(coroutine.resume( co, 4, 5, 6 )) -- co 4 5 6  -- true
    print(coroutine.resume( co, 4, 5)) -- false cannot resume dead coroutine
```

&emsp;&emsp;最后，当一个协同程序结束时，它的主函数所返回的值都将作为对应resume的返回值：

```lua
    co = coroutine.create( function ( )
	return 6, 7
    end )
    print( coroutine.resume( co ) )		-- true 6 7
```

###管道与过滤器

```lua
----------------------------------------- 管道与过滤器

    function receive( prod )
    	local status, value = coroutine.resume( prod )
    	return value
    end
    
    function  send( x )
    	coroutine.yield( x )
    end
    
    function producer( )
    	return coroutine.create( function ( )
    		while true do
    			local x = io.read()		-- 产生新值
    			send(x)
    		end
    	end )
    end
    
    function filter( prod )
    	return coroutine.create( function ( )
    		for line = 1, math.huge do
    			local x = recevie(prod)		-- 获取新值
    			x = string.format( "%5d %s", line, x )
    			send(x)						-- 将新值发送给消费者
    		end
    	end )
    end
    
    function consumer( prod )
    	while true do
    		local x = receive(prod)		-- 获取新值
    		io.write( x, "\n" )			-- 消费新值
    	end
    end
    
    -- 运行代码
    consumer(filter(producer()))
```

&emsp;&emsp;通常，coroutine.wrap比coroutine.create更易于使用。它提供了一个对于协同程序编程实际所需的功能，即一个可以唤醒协同程序的函数。但也缺乏灵活性。无法检查wrap所创建的协同程序的状态，此外，也无法检测出运行时的错误。
