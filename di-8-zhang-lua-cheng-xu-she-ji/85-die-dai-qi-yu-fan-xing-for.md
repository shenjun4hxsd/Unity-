##迭代器与范型for

&emsp;&emsp;本张将介绍如何编写适用于泛型for的迭代器（Iterator）。先从简单的迭代器入手，然后将学习如何利用泛型for的各种能力来编写更简单、更有效的迭代器。

&emsp;&emsp;

####迭代器与closure

&emsp;&emsp;所谓“迭代器”就是一种可以遍历一种集合中所有元素的机制。在Lua中，通常将迭代器表示为函数。每调用一次函数，即返回集合中的“下一个”元素。

&emsp;&emsp;


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