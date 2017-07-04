##迭代器与范型for

&emsp;&emsp;本张将介绍如何编写适用于泛型for的迭代器（Iterator）。先从简单的迭代器入手，然后将学习如何利用泛型for的各种能力来编写更简单、更有效的迭代器。

&emsp;&emsp;

####迭代器与closure

&emsp;&emsp;所谓“迭代器”就是一种可以遍历一种集合中所有元素的机制。在Lua中，通常将迭代器表示为函数。每调用一次函数，即返回集合中的“下一个”元素。

&emsp;&emsp;每个迭代器都需要在每次成功调用之间保持一些状态，这样才能知道它所在的位置及如何步进到下一个位置。closure对于这类任务提供了极佳的支持，一个closure就是一种可以访问其外部嵌套环境中的局部变量的函数。对于closure而言，这些变量就可用于在成功调用之间保持状态值，从而使closure可以记住它在一次遍历中所在的位置。当然，为了创建一个新的closure，还必须创建它的这些“非局部的变量”。因此一个closure结构通常涉及到两个函数：closure本身和一个用于创建该closure的工厂函数。


```lua
	function values( t )
		local i = 0;
		return function( )
			i = i + 1;
			return t[i];
		end
	end
	
	t = { 10, 20, 30 }
	iter = values(t)
	while true do
		local element = iter();
		if element == nil then break end
		print( element )
	end
	
	for element in values(t) do
		print( element )
	end
```

&emsp;&emsp;作为示例，来为列表编写一个简单的迭代器。与ipairs不同的是该迭代器并不是返回每个元素的索引，而是返回元素的值：

```lua
    function values(t)
        local i = 0
        return function() i = i + 1; return t[i] end
    end
```

&emsp;&emsp;在本例中，values就是一个工厂。每当调用这个工厂时，它就创建一个新的closure（即迭代器本身）。这个closure将它的状态保存在其外部变量t和i中。每当调用这个迭代器时，它就从列表t中返回下一个值。直到最后一个元素返回后，迭代器就会返回nil，以此表示迭代的结束。