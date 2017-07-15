##元表（metatable）与元方法（meatmethod）

&emsp;&emsp;通常，Lua中的每个值都有一套预定义的操作集合。例如，可以将数字相加，可以连接字符串，还可以在table中插入一对key-value等。但是我们无法将两个table相加，无法对函数作比较，也无法调用一个字符串。

&emsp;&emsp;可以通过元表来修改一个值的行为，