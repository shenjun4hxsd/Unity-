##类型与值

Lua是一种动态类型的语言。在语言中没有类型定义的语法，每个值都“携带”了它自身的类型信息。


函数type可根据一个值返回其类型名称。type函数总是返回一个字符串

        print(type("Hello wold"))        -- string
        print(type(10.4*3))              -- number
        print(type(print))               -- function
        print(type(type))                -- function
        print(type(true))                -- boolean
        print(type(nil))                 -- nil
        print(type(type(X)))             -- string