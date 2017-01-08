## XMLDocument

#### 一、定义

XMLDocument 在C\#中用于表示 XML 文档。  
可使用此类在文档中加载、验证、编辑、添加和放置 XML。

```
命名空间:   System.Xml
    程序集:  System.Xml（System.Xml.dll 中）
```

#### 二、写入

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

```
    subChild.SetAttribute("属性B", "子元素B");
    subChild.SetAttribute("属性A", "子元素A");
    //后添加的属性将显示在前面
```

**XmlDocument 创建XML 节点**

```
    CreateElement创建 xml元素对象，并通过AppendChild方法附加xml元素 为节点。

    CreateAttribute方法创建属性，通过AppendAttribute附加Attribute到节点的属性集合。
```

## 三、修改

**XmlDocument 读取XML**

```javascript
        xmlDoc.Load(@“d:\xml.xml”); 
        //读取一个外部XML
        XmlNode node = xmlDoc.SelectSingleNode(“root”);
         //查找单一的XML节点
        XmlNodeList nodeList = xmlDoc.SelectNodes("root/newChild");
        //将多个XML节点载入节点列表：
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
        // 查找ID为2的子节点，增加name节点(使用Xpath)
        XmlNode idnode = xmlDoc.SelectSingleNode("/root/newChild[id=2]");
        XmlNode id2Name = xmlDoc.CreateNode(XmlNodeType.Element, "name", null);
        id2Name.InnerText = "新元素二";
        idnode.AppendChild(id2Name);
```

**XPath**

```javascript
        // path为 root，子节点为id元素内容为2，名为newChild的节点；
        (“/root/newChild[id=2]”)

        // path为 root，属性为id属性值为2，名为newChild的节点；
        (“/root/newChild[@id=2]”);
```

**XmlDocument 为节点增加属性**

```javascript
//定义一个名为“修改”的XML属性对象modify
XmlAttribute modify = xmlDoc.CreateAttribute("修改");

//为idnode节点增加modify
idnode.Attributes.Append(modify);

//将idnode节点下名为“修改”的值修改为"完成";
idnode.Attributes["修改"].InnerText = "完成";

//将XMLNode 转换为 XMlElement 后操作；
((XmlElement)id1node).SetAttribute("Type", "1");
```

**XmlDocument 为节点删除属性**

```javascript
        XmlNode id1node = xmlDoc.SelectSingleNode("root/newChild[id=1]");
        XmlNodeList id1NodeChildNodes = id1node.ChildNodes;
        for (int i = 0; i < id1NodeChildNodes.Count; i++)
        {
                XmlElement tempE = (XmlElement)id1NodeChildNodes[i];
                tempE.RemoveAttribute("属性A");
        }
```

**XmlDocument 删除节点**

```javascript
        //父节点删除子节点
        node.RemoveChild(idnode);
        id2node.ParentNode.RemoveChild(id2node);
```

**XmlDocument 通过名字查找节点**

```javascript
        //查找某一节点下所有名为 (“newSubChild”)的节点，返回为XMLNODELIST;
        GetElementsByTagName("newSubChild")
```


---

###示例：

```javascript
	void OperateXml()
	{
		XmlDocument pDoc = new XmlDocument ();
		pDoc.Load (filepath);

		XmlElement pRoot = pDoc.DocumentElement;

		XmlElement pNode = pRoot.SelectSingleNode ("/books/book[name='水浒']") as XmlElement;

		XmlElement pPriceNode = pNode.GetElementsByTagName("price")[0] as XmlElement;
		Debug.Log (pPriceNode.InnerText);
		pPriceNode.InnerText = "100";


		XmlNodeList pList = pRoot.SelectNodes("/books/book[price>10]");
		Debug.Log ("count = " + pList.Count);
		if (pList != null && pList.Count > 0) {
			for (int i = pList.Count - 1; i >= 0; i--) {
				pList[i].ParentNode.RemoveChild(pList[i]);
			}
		}

		pDoc.Save (filepath);
	}
	
	bool AppendXml(XmlDocument pDoc, XmlElement pElement, ref Dictionary<string, string> pContent)
	{
		XmlElement pNode = pDoc.CreateElement ("book");

		if (pDoc == null || pElement == null)
			return false;

		if (pContent.Count <= 0)
			return false;

		string[] keys = new string[pContent.Count];

		pContent.Keys.CopyTo (keys, 0);

		for (int i = 0; i < keys.Length; i++) {
			XmlElement pTmp = pDoc.CreateElement(keys[i]);
			pTmp.InnerText = pContent[keys[i]];
			pNode.AppendChild(pTmp);
		}

		pElement.AppendChild (pNode);

		return true;
	}
```


---

###API

XmlDocument类是.NET框架的DOC解析器。XmlDocument将XML视为树状结构，它装载XML文档，并在内存中构建该文档的树状结构。下面来看下XmlDocument提供了哪些功能。
 
一、属性：

```
Attributes　　　　　 获取一个 XmlAttributeCollection，它包含该节点的属性。 （继承自 XmlNode。） 
BaseURI 　　  　  　 获取当前节点的基 URI。 （重写 XmlNode..::.BaseURI。） 
ChildNodes 　　　　　获取节点的所有子节点。 （继承自 XmlNode。） 
DocumentElement 　   获取文档的根 XmlElement。 
DocumentType 　　　  获取包含 DOCTYPE 声明的节点。 
FirstChild 　　　　　获取节点的第一个子级。 （继承自 XmlNode。） 
HasChildNodes 　　　 获取一个值，该值指示节点是否有任何子节点。 （继承自 XmlNode。） 
Implementation 　　  获取当前文档的 XmlImplementation 对象。 
InnerText 　　　　　 获取或设置节点及其所有子节点的串联值。 （继承自 XmlNode。） 
InnerXml 　　　　　　获取或设置表示当前节点子级的标记。 （重写 XmlNode..::.InnerXml。） 
IsReadOnly 　　　　　获取一个值，该值指示当前节点是否是只读的。 （重写 XmlNode..::.IsReadOnly。） 
Item 　　　　　　　　已重载。 
LastChild 　　　　　 获取节点的最后一个子级。 （继承自 XmlNode。） 
LocalName 　　　　　 获取节点的本地名称。 （重写 XmlNode..::.LocalName。） 
Name 　　　　　　　　获取节点的限定名。 （重写 XmlNode..::.Name。） 
NamespaceURI 　　　  获取该节点的命名空间 URI。 （继承自 XmlNode。） 
NameTable 　　　　　 获取与此实现关联的 XmlNameTable。 
NextSibling 　　　　 获取紧接在该节点之后的节点。 （继承自 XmlNode。） 
NodeType 　　　　　  获取当前节点的类型。 （重写 XmlNode..::.NodeType。） 
OuterXml 　　　　　  获取表示此节点及其所有子节点的标记。 （继承自 XmlNode。） 
OwnerDocument 　　   获取当前节点所属的 XmlDocument。 （重写 XmlNode..::.OwnerDocument。） 
ParentNode 　　　　  已重载。 
Prefix 　　　　　　　获取或设置该节点的命名空间前缀。 （继承自 XmlNode。） 
PreserveWhitespace   获取或设置一个值，该值指示是否在元素内容中保留空白。 
PreviousSibling 　　 获取紧接在该节点之前的节点。 （继承自 XmlNode。） 
SchemaInfo 　　　　  返回节点的后架构验证信息集 (PSVI)。 （重写 XmlNode..::.SchemaInfo。） 
Schemas 　　　　　　 获取或设置与此 XmlDocument 关联的 XmlSchemaSet 对象。 
Value 　　　　　　　 获取或设置节点的值。 （继承自 XmlNode。） 
XmlResolver 　　　　 设置 XmlResolver 以用于解析外部资源。

```

二、方法

```
AppendChild 　　　　　　　　　　　  将指定的节点添加到该节点的子节点列表的末尾。 （继承自 XmlNode。） 
Clone 　　　　　　　　　　　　　　  创建此节点的一个副本。 （继承自 XmlNode。） 
CloneNode 　　　　　　　　　　　　  创建此节点的一个副本。 （重写 XmlNode..::.CloneNode(Boolean)。） 
CreateAttribute 　　　　　　　　　　已重载。 创建具有指定名称的 XmlAttribute。 
CreateCDataSection 　　　　　　　　 创建包含指定数据的 XmlCDataSection。 
CreateComment　　　　 　　　　　　  创建包含指定数据的 XmlComment。 
CreateDefaultAttribute 　　　　　　 创建具有指定前缀、本地名称和命名空间 URI 的默认属性。 
CreateDocumentFragment 　　　　     创建 XmlDocumentFragment。 
CreateDocumentType 　　　　　　     返回新的 XmlDocumentType 对象。 
CreateElement 　　　　　　　　　　  已重载。 创建 XmlElement。 
CreateEntityReference 　　　　　　  创建具有指定名称的 XmlEntityReference。 
CreateNavigator 　　　　　　　　　　已重载。 创建一个用于导航此文档的新 XPathNavigator 对象。 
CreateNode　　　　　　　　　　　　  已重载。 创建 XmlNode。 
CreateProcessingInstruction 　　　　创建一个具有指定名称和数据的 XmlProcessingInstruction。 
CreateSignificantWhitespace 　　　　创建一个 XmlSignificantWhitespace 节点。 
CreateTextNode 　　　　　　　　　 　创建具有指定文本的 XmlText。 
CreateWhitespace 　　　　　　　　 　创建一个 XmlWhitespace 节点。 
CreateXmlDeclaration 　　　　　　　 创建一个具有指定值的 XmlDeclaration 节点。 
GetElementById 　　　　　　　　　 　获取具有指定 ID 的 XmlElement。 
GetElementsByTagName 　　　　　     已重载。 返回一个 XmlNodeList，它包含与指定名称匹配的所有子代元素的列表。 
GetEnumerator 　　　　　　　　　 　 提供对 XmlNode 中节点上“for each”样式迭代的支持。 （继承自 XmlNode。） 
GetHashCode 　　　　　　　　　　    用作特定类型的哈希函数。 （继承自 Object。） 
GetNamespaceOfPrefix 　　　　　　   查找当前节点范围内离给定的前缀最近的 xmlns 声明，并返回声明中的命名空间 URI。 （继承自 XmlNode。） 
GetPrefixOfNamespace 　　　　　　   查找当前节点范围内离给定的命名空间 URI 最近的 xmlns 声明，并返回声明中定义的前缀。 （继承自 XmlNode。） 
ImportNode 　　　　　　　　　　　 　将节点从另一个文档导入到当前文档。 
InsertAfter 　　　　　　　　　　　  将指定的节点紧接着插入指定的引用节点之后。 （继承自 XmlNode。） 
InsertBefore 　　　　　　　　　 　　将指定的节点紧接着插入指定的引用节点之前。 （继承自 XmlNode。） 
Load 　　　　　　　　　　　　　 　  已重载。 从 Stream、URL、TextReader 或 XmlReader 加载指定的 XML 数据。 
LoadXml 　　　　　　　　　　　　　  从指定的字符串加载 XML 文档。 
Normalize 　　　　　　　　　　　　　将此 XmlNode 下子树完全深度中的所有 XmlText 节点都转换成“正常”形式，在这种形式中只有标记（即标记、注释、处理指令、　　　　　　　　　　　　　　　　　　　　CDATA 节和实体引用）分隔 XmlText 节点，也就是说，没有相邻的 XmlText 节点。 （继承自 XmlNode。） 
PrependChild 　　　　　　　　　　　 将指定的节点添加到该节点的子节点列表的开头。 （继承自 XmlNode。） 
ReadNode 　　　　　　　　　 　　　  根据 XmlReader 中的信息创建一个 XmlNode 对象。读取器必须定位在节点或属性上。 
RemoveAll 　　　　　　　　　　　　  移除当前节点的所有子节点和/或属性。 （继承自 XmlNode。） 
RemoveChild 　　　　　　　　 　　　 移除指定的子节点。 （继承自 XmlNode。） 
ReplaceChild 　　　　　　　　　　　 用 newChild 节点替换子节点 oldChild。 （继承自 XmlNode。） 
Save 　　　　　　　　　　　　　　　 已重载。 将 XML 文档保存到指定的位置。 
SelectNodes 　　　　　　　　　　　  已重载。 
SelectSingleNode 　　　　　　　　　 已重载。 
Supports 　　　　　　　　　　　　　 测试 DOM 实现是否实现特定的功能。 （继承自 XmlNode。） 
Validate 　　　　　　　　　　　　　 已重载。 验证 XmlDocument 是不是 Schemas 属性中包含的 XML 架构定义语言 (XSD) 架构。 
WriteContentTo 　　　　　　　　 　　将 XmlDocument 节点的所有子级保存到指定的 XmlWriter 中。 （重写 XmlNode..::.WriteContentTo(XmlWriter)。） 
WriteTo　　　　　　　　　　　　　　 将 XmlDocument 节点保存到指定的 XmlWriter。 （重写 XmlNode..::.WriteTo(XmlWriter)。） 
```

三、事件

```
NodeChanged 　　　　　　 当属于该文档的节点的 Value 已被更改时发生。 
NodeChanging 　　　　　　当属于该文档的节点的 Value 将被更改时发生。 
NodeInserted 　　　　　　当属于该文档的节点已被插入另一个节点时发生。 
NodeInserting 　　　　　 当属于该文档的节点将被插入另一个节点时发生。 
NodeRemoved 　　　　　　 当属于该文档的节点已被从其父级移除时发生。 
NodeRemoving 　　　　　　当属于该文档的节点将被从文档中移除时发生。
```

助记属性：

```
PreviousSibling　　上一个兄弟节点
NextSibling　　　　下一个兄弟节点
FirstChild　　　　 第一个子节点
LastChild　　　　　最后一个子节点
ChildNodes　　　 　子节点集合
ParentNode　　　   父节点
```
