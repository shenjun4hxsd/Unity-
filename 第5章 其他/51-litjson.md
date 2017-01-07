##JSON(JavaScript Object Notation)

####存储和交换文本信息的语法。

        是一种轻量级的数据交换格式。
        采用完全独立于语言的文本格式。
        使用了类似于C语言家族的习惯。
        具有自我描述性，更易理解

####JavaScript对象表示法

Json：是来源于JavaScript中的序列化方式，他可以很好的将我们的数组，对象等信息存储成一个文本文件，而且可以反序列化将数据恢复到我们的对象或者是数组当中去。我们利用它这样的特性可以实现游戏或者虚拟现实数据的记录，将这些记录我们可以存储在硬盘上，方便下一次启动程序后可以读回来。


>可以在 http://JSON.org 了解关于JSON的更多信息。


---

##LitJson

LitJSON is a small and fast library for handling data in the JSON format. It is written in C# and is compatible with all .Net languages.

JSON是一个简单的，但功能强大的序列化数据格式。它定义了简单的类型，如布尔，数（int和float）和字符串，和几个数据结构：list和dictionnary。

litjson是用C #编写的，它的目的是要小，快速，易用。它使用了Mono框架。

####语法
JSON 语法是JavaScript对象表示法语法的子集。

        数据在 名称／值（键-值） 对中
        数据由逗号分隔
        花括号保存对象
        方括号保存数据
        
值的类型可以是:

```javascript
        public enum JsonType
        {
                None,
                Object,
                Array,
                String,
                Int,
                Long,
                Double,
                Boolean
        }
```

####举例：

JSON 对象在花括号中书写，对象可以包含多个名称／值对：

        ｛”firstName”：”Shen”, ”lastName”：”Jun”｝














