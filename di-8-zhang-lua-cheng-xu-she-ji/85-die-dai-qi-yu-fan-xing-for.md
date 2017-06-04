##迭代器与范型for

&emsp;&emsp;所谓“迭代器”就是一种可以遍历一种集合中所有元素的机制。


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