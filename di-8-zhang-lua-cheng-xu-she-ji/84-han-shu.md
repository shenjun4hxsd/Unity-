##函数



####变长参数

```lua
    function add( ... )
	local s = 0
	for i, v in ipairs( ... ) do
		s = s + v
	end
	return s
	end
```