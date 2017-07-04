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


&emsp;&emsp;可以在一个while循环中使用这个迭代器：

```lua
    t = {10, 20, 30}
    iter = values(t)		-- 创建迭代器
    while true do
        local element = iter()
        if element == nil then break end
        print(element)
    end
```

&emsp;&emsp;然而使用泛型for则更为简单。接下来会发现，它正是为这种迭代而设计的：

```lua
    t = {10, 20, 30}
    for element in values(t) do
        print(element)
    end
```

&emsp;&emsp;泛型for为一次迭代循环做了所有的簿记工作。它在内部保存了迭代器函数，因此不再需要iter变量。它在每次新迭代时调用迭代器，并在迭代器返回nil时结束循环。在下一节中将会看到泛型for能够做更多的事情。

&emsp;&emsp;下面是一个更高级的示例，展现了一个可以遍历当前输入文件中所有单词的迭代器 -- allwords。为了完成这样的遍历，需要保持两个值：当前行的内容（变量line）及在该行中所处的位置（变量pos）。有了这些信息，就可以不断产生下一个单词了。这个迭代器函数的主要部分就是string.find的调用。此调用会在当前行中，以当前位置作为起始来搜索一个单词。使用模式（pattern）‘`%w+`’来描述一个“单词”，它用于匹配一个或多个的文字／数字字符。如果string.find找到了一个单词，迭代器就会将当前位置更新为该单词之后的第一个字符，并返回该单词。否则，它就读取新的一行并反复这个搜索过程。若没有剩余的行，则返回nil，以此表示迭代的结束。

&emsp;&emsp;尽管迭代器本身具有复杂性，但allwords的使用还是很简明易懂的：

```lua
    for word in allwords() do
        print(word)
    end
```

&emsp;&emsp;对于迭代器而言，一种常见的情况就是：编写迭代器本身或许不太容易，但使用它们却是很容易的。这也不会成为一个大问题，因为通常使用Lua编程的最终用户不会去定义迭代器，而只是使用那些程序提供的迭代器。

```lua
    function allwords()
        local line = io.read()    -- 当前行
        local pos = 1             -- 一行中的当前位置
        return function()         -- 迭代器函数
            while line do         -- 若为有效的行内容就进入循环
                local s, e = string.find(line, "%w+", pos)
                if s then         -- 是否找到一个单词
                    pos = e + 1   -- 该单词的下一个位置
                    return string.sub(line, s, e)    -- 返回该单词
                else
                    line = io.read()    -- 没有找到单词，尝试下一行
                    pos = 1             -- 在第一个位置上重新开始
                end
            end
            return nil                  -- 没有其余行了，遍历结束
        end
    end
```

&emsp;&emsp;

####泛型for的语义

&emsp;&emsp;上述的那些迭代器都有一个缺点，就是需要为每个新的循环都创建一个新的closure。对于大多数情况而言，这或许不会有什么问题。例如在之前的allwords迭代器中，创建一个closure的代价相对于读取整个文件的代价而言，几乎可以忽略不计。但是，在另外一些情况下，这样的开销就不太容易接受了。因此，在这类情况中，希望能通过泛型for的自身来保存迭代器状态。在本节中会详细说明泛型for的这种保存状态的机制。

&emsp;&emsp;泛型for在循环过程内部保存了迭代器函数。实际上它保存着3个值：一个迭代器函数、一个恒定状态和一个控制变量。接下来将对此进行详细说明。

