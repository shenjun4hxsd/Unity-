##XMLWrite 和 XMLReader

####一、定义

XMLWrite表示一个写入器，该写入器提供一种快速、非缓存和只进方式以生成包含 XML 数据的流或文件。

XMLReader表示提供对 XML 数据进行快速、非缓存、只进访问的读取器。

```
    命名空间:
        System.Xml
    程序集:
        System.Xml（System.Xml.dll 中）
```     
        

####二、XMLWrite

**XMLWrite 创建 XML 声明**

```javascript
    //引入命名空间
    using System.Xml;
    
    XmlTextWriter textWriter = new XmlTextWriter(@"d:\xmlRead.xml",Encoding.UTF8);
    textWriter.WriteStartDocument();
```

**XMLWrite 创建节点**

```javascript
    textWriter.WriteStartElement(“root"); 
    textWriter.WriteStartElement(“Child");
    // 为元素加入属性
    textWriter.WriteElementString(“SubChild”, “这是元素内容");
    textWriter.WriteEndElement();
    textWriter.WriteEndElement();
```

**XMLWrite为元素加入属性**

```javascript
    textWriter.WriteStartElement("Child");
    textWriter.WriteAttributeString("属性A","A");
    textWriter.WriteAttributeString("属性B","B");
    textWriter.WriteEndElement();
```

**XMLWrite写入注释**

```javascript
 textWriter.WriteComment(“注释内容"); textWriter.WriteCData(“<strong>CData 内容</strong>");
```

**XMLWrite创建XML 要点**

```javascript
    XMLWrite 子节点 和属性 必须在WriteEndElement()方法前。
    开始写入，调用WriteStartDocument方法。
    写入结束，调用WriteEndDocument方法，和Close方法，关闭流。
```

##三、XMLReader

**XmlReader读取XML**

```javascript
    //读取一个外部XML
    XmlTextReader textReader = new XmlTextReader(@"d:\xmlRead.xml");
```

**XmlReader读取节点信息**

```javascript
    while (textReader.Read())
    {
        if (textReader.NodeType == XmlNodeType.Element)
        {
            Console.WriteLine("Name:" + textReader.Name);
            Console.WriteLine("Value:" + textReader.ReadString());
        }
    }
```

**XmlReader读取属性**

```javascript
	 textReader.GetAttribute(“属性A”));//方式一：取得当前Reader元素的指定名称的属性值；             for (int i = 0; i < textReader.AttributeCount; i++)             {              textReader.MoveToAttribute(i);              Console.WriteLine("AttributeName:" + textReader.Name);              Console.WriteLine("AttributeValue:" + textReader.Value);               }//方式二，获取当前Reader元素的属性，将Reader移动到当前属性；
```