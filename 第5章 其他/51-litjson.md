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

JSON是一个简单的，但功能强大的序列化数据格式。它定义了简单的类型，如布尔，数（int和float）和字符串，和几个数据结构：List和Dictionnary。

LitJson是用C #编写的，它的目的是要小，快速，易用。它使用了Mono框架。

####语法
JSON 语法是JavaScript对象表示法语法的子集。

        • 数据在 名称／值（键-值） 对中
        • 数据由逗号分隔
        • 花括号保存对象
        • 方括号保存数组
        
值的类型可以是:

```csharp
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

        ｛ "firstName":"Shen", "lastName":"Jun" ｝


        • 名称和值之间用冒号分隔
        • 多个变量之间用逗号分隔
        • 结尾不需要添加分号

JSON 对象

```csharp
        {
                "china": {
                        "hangzhou":{"item":"1"},
                        "shanghai":{"item":"2"},
                        "chengdu":{"item":"3"}
                },
                "America": {
                        "aa":{"item":"1"},
                        "bb":{"item":"2"}
                }
        }
```

JSON 数组

        JSON数组在方括号中书写，数组可包含多个对象：

```csharp
{
    "people": [
             { "firstName" : "Brett", "lastName" : "McLaughlin" },
             { "firstName" : "Jason", "lastName" : "Hunter" },
             { "firstName" : "Elliotte", "lastName" : "Harold" }
     ]
}
```

####归纳小结

1、对象是属性、值对的集合。一个对象的开始于“{”，结束于“}”。每一个属性名和值间用“:”提示，属性间用“,”分隔。

2、数组是有顺序的值的集合。一个数组开始于"["，结束于"]"，值之间用","分隔。

3、值可以是引号里的字符串、数字、true、false、null，也可以是对象或数组。这些结构都能嵌套。

4、字符串和数字的定义和C或Java基本一致。

5、JSON 提供了一种优秀的面向对象的方法，以便将元数据缓存到客户机上。


---

####将 JSON 数据赋值给变量

例如，可以创建一个新的字符串变量，然后将 JSON 格式的数据字符串直接赋值给它：

```csharp
        string str = @"
        {
        ""Name"":""shenjun"",
        ""Age"": 40,
        ""Birthday"" : ""1986-11-21"",
        ""Thumbnail"":
        [
                {
                ""Url"": ""http://xuanyusong.com"",
                ""Height"": 256,
                ""Width"":  ""200""
                },
                {
                ""Url"": ""http://baidu.com"",
                ""Height"": 1024,
                ""Width"": ""500""
                }
        ]
        }";
```

####访问数据

使用JsonData，JsonMapper来处理Json的编码和解析

```csharp
        JsonData jd = JsonMapper.ToObject(str);
                Debug.Log("name = " + (string)jd["Name"]);
        Debug.Log("Age = " + (int)jd["Age"]);
        Debug.Log("Birthday = " + (string)jd["Birthday"]);
        JsonData jdItems = jd["Thumbnail"];
        for (int i = 0; i < jdItems.Count; i++)
        {
                Debug.Log("URL = " + jdItems[i]["Url"]);
                Debug.Log("Height = " + (int)jdItems[i]["Height"]);
                Debug.Log("Width = " + jdItems[i]["Width"]);
        }
```

```csharp
	void StreamWriteJson<T>(string path, T o)
	{
		using (FileStream fsWrite = File.Open (path, FileMode.OpenOrCreate)) {
			string json = JsonMapper.ToJson (o);
			byte[] buffer = System.Text.Encoding.UTF8.GetBytes (json);
			fsWrite.Write (buffer, 0, buffer.Length);
		}
	}

	T StreamReadJson<T>(string path)
	{
		using (FileStream fsReader = File.Open (path, FileMode.Open, FileAccess.Read)) {
			byte[] buffer = new byte[1024];
			int length;
			System.Text.StringBuilder sb = new System.Text.StringBuilder ();
			while ((length = fsReader.Read (buffer, 0, buffer.Length)) > 0) {
				string result = System.Text.Encoding.UTF8.GetString (buffer, 0, length);
				sb.Append (result);
			}
			string s = sb.ToString();
			T o = JsonMapper.ToObject<T> (s);
			return o;
		}
	}
```

**使用JsonWriter原始方式生成Json**

```csharp
        将数组转换成json:["one","two","three","four"]
        
        JsonWriter writer = new JsonWriter();
        writer.WriteArrayStart();
        writer.Write("one");
        writer.Write("two");
        writer.Write("three");
        writer.Write("four");
        writer.WriteArrayEnd();
```

**将复合对象转换成json字符串**

```csharp
{"book":{"title":"android game!","author":"pei","bookdetail":{"pages":429,"about":null}}}JsonWriter writer2 = new JsonWriter();writer2.WriteObjectStart();        writer2.WritePropertyName("book");         writer2.WriteObjectStart();        writer2.WritePropertyName("title");        writer2.Write("android game!");        writer2.WritePropertyName("author");writer2.Write("pei");        writer2.WritePropertyName("bookdetail");writer2.WriteObjectStart();        writer2.WritePropertyName("pages");        writer2.Write(429);        writer2.WritePropertyName("about");        writer2.Write(null);        writer2.WriteObjectEnd();         writer2.WriteObjectEnd();        writer2.WriteObjectEnd();       Debug.Log(writer2.ToString());

```

####文件的读写路径

        Resources.Load	在移动平台只读
        
        Application.dataPath	在移动平台没有读写权限
        
        Application.streamingAssetsPath	移动平台只读
        
        Application.temporaryCachePath	可读写
        
        Application.persistentDataPath	可读写
        
        

####JSON与XML

类似XML，比XML更小、更快、更易解析。

        没有结束标签
        更短
        读写速度更快
        使用数组
        不使用保留字
        
        
🔚


 