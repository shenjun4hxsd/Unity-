##XMLDocument

####一、定义

XMLDocument 在C#中用于表示 XML 文档。可使用此类在文档中加载、验证、编辑、添加和放置 XML。

        命名空间:   System.Xml
        程序集:  System.Xml（System.Xml.dll 中）


####二、写入

**XmlDocument 创建 XML 声明**

```javascript
        using System.Xml;//引入命名空间

        XmlDocument xmldoc ;//声明 XML文档 对象
        XmlDeclaration xmldecl;//声明 XML声明 对象
        
        xmldoc = new XmlDocument ( ) ;
        xmldecl = xmldoc.CreateXmlDeclaration("1.0",“UTF-8",null);
        xmldoc.AppendChild (xmldecl);//xmldoc 加入 xmldecl定义的声明
```

**XmlDocument 创建根节点**

```javascript
        XmlElement xmlelem;//声明 XML元素对象
        
        xmlelem = xmldoc.CreateElement ( "" , “root" , "" ) ;
        xmldoc.AppendChild (xmlelem) ;
        xmldoc.Save ("xml.xml") ;
```

**XmlDocument 加入XML元素**

```javascript
        for (int i = 0; i < 3; i++)
        {
                XmlElement child = xmldoc.CreateElement("child");
                XmlElement subChild = xmldoc.CreateElement("subChild");
                subChild.InnerText = "这是元素内容";
                child.AppendChild(subChild);
                xmlelem.AppendChild(child);
        }
```

**XmlDocument 为元素加入属性**

        subChild.SetAttribute("属性B", "子元素B");
        subChild.SetAttribute("属性A", "子元素A");
        //后添加的属性将显示在前面

**XmlDocument 创建XML 节点**

        CreateElement创建 xml元素对象，并通过AppendChild方法附加xml元素 为节点。
        CreateAttribute方法创建属性，通过AppendAttribute附加Attribute到节点的属性集合。


##三、修改

**XmlDocument 读取XML**

```javascript
        xmlDoc.Load(@“d:\xml.xml”); //读取一个外部XML
        XmlNode node = xmlDoc.SelectSingleNode(“root”); //查找单一的XML节点
        XmlNodeList nodeList = xmlDoc.SelectNodes("root/newChild");//将多个XML节点载入节点列表：
```


**XmlDocument 使用XmlReader忽略文档里面的注释**

```javascript
        XmlDocument xmlDoc = new XmlDocument();
        XmlReaderSettings settings = new XmlReaderSettings();
        settings.IgnoreComments = true;//忽略文档里面的注释
        XmlReader reader = XmlReader.Create(@“xml.xml", settings);
        xmlDoc.Load(reader);
        reader.Close();
```

**XmlDocument 为NodeList增加元素**

```javascript
        // 为列表中所有节点加入ID子节点： 
        for (int i = 0; i < nodeList.Count; i++)
        {
                XmlNode id = xmlDoc.CreateNode(XmlNodeType.Element, "id", null);
                id.InnerText = i.ToString();
                nodeList[i].AppendChild(id);
        }
```

**XmlDocument 使用Xpath查找节点**

```javascript
查找ID为2的子节点，增加name节点(使用Xpath)XmlNode idnode = xmlDoc.SelectSingleNode("root/newChild[id=2]");XmlNode id2Name = xmlDoc.CreateNode(XmlNodeType.Element, "name", null);id2Name.InnerText = "新元素二";idnode.AppendChild(id2Name);
```









