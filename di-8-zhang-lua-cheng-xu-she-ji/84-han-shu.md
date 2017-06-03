##函数



####变长参数

Lua中的函数可以接受不同数量的实参。

```lua
    function add( ... )
	local s = 0
	for i, v in ipairs( ... ) do
		s = s + v
	end
	return s
	end
```

参数中的3个点（...）表示该函数可接受不同数量的实参。