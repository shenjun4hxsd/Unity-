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

&emsp;&emsp;下面是一个更高级的示例，展现了一个可以遍历当前输入文件中所有单词的迭代器 -- allwords。为了完成这样的遍历，需要保持两个值：当前行的内容（变量line）及在该行中所处的位置（变量pos）。有了这些信息，就可以不断产生下一个单词了。这个迭代器函数的主要部分就是string.find的调用。此调用会在当前行中，以当前位置作为起始来搜索一个单词。使用模式（pattern）‘%w+’来描述一个“单词”，它用于匹配一个或多个的文字／数字字符。如果string.find找到了一个单词，迭代器就会将当前位置更新为该单词之后的第一个字符，并返回该单词。否则，它就读取新的一行并反复这个搜索过程。若没有剩余的行，则返回nil，以此表示迭代的结束。