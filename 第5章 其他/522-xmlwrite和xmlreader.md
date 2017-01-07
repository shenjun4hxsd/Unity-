##XMLWrite 和 XMLReader

####定义

XMLWrite表示一个写入器，该写入器提供一种快速、非缓存和只进方式以生成包含 XML 数据的流或文件。

XMLReader表示提供对 XML 数据进行快速、非缓存、只进访问的读取器。

```
    命名空间:
        System.Xml
    程序集:
        System.Xml（System.Xml.dll 中）
```     
        

####XMLWrite

XMLWrite 创建 XML 声明

```javascritp
    using System.Xml;//引入命名空间
    XmlTextWriter textWriter = new XmlTextWriter(@"d:\xmlRead.xml",Encoding.UTF8);
    textWriter.WriteStartDocument();
```