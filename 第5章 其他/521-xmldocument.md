##XMLDocument

####定义

XMLDocument 在C#中用于表示 XML 文档。可使用此类在文档中加载、验证、编辑、添加和放置 XML。

        命名空间:   System.Xml
        程序集:  System.Xml（System.Xml.dll 中）


####写入

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