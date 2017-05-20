##XML示例

```csharp
    /*
     * Author: shenjun
     */
    
    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;
    
    using UnityEditor;
    
    using System.Xml;
    
    public class XML_Operation : MonoBehaviour
    {
    
        string fileName = "Items.xml";
    
        public string FilePath { get { return Application.streamingAssetsPath + "/" + fileName; } }
    
        public List<PackageItem> equips = new List<PackageItem>();
        public List<PackageItem> items = new List<PackageItem>();
    
        #region ContextMenuItem用法
    
        [ContextMenuItem("ResetXML", "ResetXML")]   // 给该字符串字段添加一个单独的右键菜单 并设置该菜单功能所调用的方法
        [Multiline(4)]   // 使字符串在检视面板显示成几行
        public string SS = "";
    
        void ResetXML()
        {
            SS = "";
        }
    
        #endregion
    
        #region Debug
    
        [ContextMenuItem("Reset", "ResetEquipsLoaded")]
        public List<PackageItem> equipsLoaded = new List<PackageItem>();
    
        [ContextMenuItem("Reset", "ResetItemsLoaded")]
        public List<PackageItem> itemsLoaded = new List<PackageItem>();
    
        void ResetEquipsLoaded()
        {
            equipsLoaded.Clear();
        }
    
        void ResetItemsLoaded()
        {
            itemsLoaded.Clear();
        }
    
        void AppendTest()
        {
            // 1. 使用绝对路径的XPath
            //        bool flag = AppendXML("/道具列表/装备", new PackageItem(){ id = "4", name = "铁剑", price = 500 });
    
            // 2. 开头是两个斜线表示文件中所有符合模式的元素都会被选出来，即使处于树中不同的层级也会被选出来。
            //        bool flag = AppendXML("//装备", new PackageItem(){ id = "4", name = "铁剑", price = 500 });
    
            // 3. 通配符
            //        bool flag = AppendXML("/*/装备", new PackageItem(){ id = "4", name = "铁剑", price = 500 });
    
            // 4. 使用方括号可以选择分支 下标从1开始 选择第一个节点
            //        bool flag = AppendXML("/道具列表/装备[1]", new PackageItem(){ id = "4", name = "铁剑", price = 500 });
    
            // 5. 选择最后一个节点
            //        bool flag = AppendXML("/道具列表/装备[last()]", new PackageItem(){ id = "4", name = "铁剑", price = 500 });
    
            // 6. 选择所有带有 倚天剑 元素并满足一定层次结构的装备节点
            //        bool flag = AppendXML("/道具列表/装备[倚天剑]", new PackageItem(){ id = "4", name = "铁剑", price = 500 });
    
            // 7. 选取 价格=1000的节点
            //        bool flag = AppendXML("/道具列表/装备/青铜剑[价格=1000]", new PackageItem(){ id = "4", name = "铁剑", price = 500 });
    
            // 8. 选取 符合一定层次结构的 装备和道具 节点
            //        bool flag = AppendXML("/道具列表/装备 | /道具列表/道具", new PackageItem(){ id = "4", name = "铁剑", price = 500 });
    
            // 9. 除了选择元素之外，也可以选择属性 属性都是以 @开头 这里表示选取所有id属性
            //        bool flag = AppendXML("//@id", new PackageItem(){ id = "4", name = "铁剑", price = 500 });
    
            // 10. 选取所有道具节点下 具有id属性的节点
            //        bool flag = AppendXML("//道具/*[@id]", new PackageItem(){ id = "4", name = "铁剑", price = 500 });
    
            // 11. 选取所有道具节点下 具有id属性 并且id属性等于12的节点
            //        bool flag = AppendXML("//道具/*[@id=12]", new PackageItem(){ id = "4", name = "铁剑", price = 500 });
    
            //        bool flag = AppendXML("大还丹", new PackageItem(){ id = "4", name = "铁剑", price = 500 }, true);
            //        if (!flag)
            //            Debug.Log("插入失败");
        }
    
        void RemoveTest()
        {
            //        bool flag = RemoveXML("//*[@id=12]");
    
            //        bool flag = RemoveXML("/道具列表/装备/倚天剑");
    
            //        bool flag = RemoveXML("/道具列表/装备/*[价格=1000]");
    
            //        bool flag = RemoveXML("屠龙刀", true);
            //        if (!flag)
            //            Debug.Log("删除失败");
        }
    
        void ModifyTest()
        {
            bool flag = ModifyXML("//*[@id=12]/价格", "2000");
    
            if(!flag)
            {
                Debug.Log("修改失败");
            }
        }
    
        void GetXMLNodeTest()
        {
            List<PackageItem> items;
    //        items = GetXMLNode("//*[@id=11]");
    
            items = GetXMLNode("青铜剑", true);
            foreach (var item in items)
            {
                Debug.Log("Name :" + item.name);
                Debug.Log("Id : " + item.id);
                Debug.Log("Price : " + item.price);
            }
        }
    
        #endregion
    
    
        void Start()
        {
    //        AppendTest();
    //        RemoveTest();
    //        ModifyTest();
            GetXMLNodeTest();
        }
    
    
    
        List<PackageItem> GetXMLNode(string xpath, bool byTag = false)
        {
            if (!System.IO.File.Exists(FilePath)) return null;
    
            List<PackageItem> nodesList = new List<PackageItem>();
            XmlDocument xmlDoc = new XmlDocument();
            xmlDoc.Load(FilePath);
            XmlNodeList nodes;
            if(byTag)
            {
                nodes = xmlDoc.GetElementsByTagName(xpath);
            }
            else
            {
                nodes = xmlDoc.SelectNodes(xpath);
            }
            foreach (XmlElement n in nodes)
            {
                PackageItem item = new PackageItem();
                item.name = n.Name;
                if(n.HasAttribute("id"))
                {
                    item.id = n.GetAttribute("id");
                }
                if(n.HasChildNodes)
                {
                    int.TryParse(n.FirstChild.InnerText, out item.price);
                }
                nodesList.Add(item);
            }
            return nodesList;
        }
    
        [ContextMenu("生成道具XML文件")]
        void CreateXML()
        {
            if (System.IO.File.Exists(FilePath))
            {
                System.IO.File.Delete(FilePath);
            }
    
            XmlDocument xmlDoc = new XmlDocument();
            XmlDeclaration xmlDec = xmlDoc.CreateXmlDeclaration("1.0", "UTF-8", null);
            xmlDoc.AppendChild(xmlDec);
    
            // 只允许有一个根节点
            XmlElement root = xmlDoc.CreateElement("道具列表");
    
    
            if (equips.Count > 0)
            {
                XmlElement xmlEquip = xmlDoc.CreateElement("装备");
    
                foreach (var item in equips)
                {
                    XmlElement name = xmlDoc.CreateElement(item.name);
                    name.SetAttribute("id", item.id);
    
                    XmlElement price = xmlDoc.CreateElement("价格");
                    price.InnerText = item.price.ToString();
    
    
                    xmlEquip.AppendChild(name);
                    name.AppendChild(price);
                }
    
                root.AppendChild(xmlEquip);
            }
    
            if (items.Count > 0)
            {
                XmlElement xmlItem = xmlDoc.CreateElement("道具");
                foreach (var item in items)
                {
    
                    XmlElement name = xmlDoc.CreateElement(item.name);
                    name.SetAttribute("id", item.id);
    
                    XmlElement price = xmlDoc.CreateElement("价格");
                    price.InnerText = item.price.ToString();
    
                    xmlItem.AppendChild(name);
                    name.AppendChild(price);
                }
    
                root.AppendChild(xmlItem);
            }
    
            xmlDoc.AppendChild(root);
            xmlDoc.Save(FilePath);
    
            #if UNITY_EDITOR
            AssetDatabase.Refresh();
            #endif
        }
    
        [ContextMenu("载入XML文件")]
        void LoadXML()
        {
            equipsLoaded.Clear();
            itemsLoaded.Clear();
    
            XmlDocument xmlDoc = new XmlDocument();
            xmlDoc.Load(FilePath);
    
    //        XmlElement root = xmlDoc.DocumentElement;
    //        Debug.Log("Root : " + root.Name);
    
            // XPath的开头是一个斜线代表这是绝对路径，可以选出所有路径符合这个模式的元素。
            XmlNode node = xmlDoc.SelectSingleNode("/道具列表/装备");
            XmlNodeList pNodeList = node.ChildNodes;
            foreach (var item in pNodeList)
            {
                XmlElement element = item as XmlElement;
                if (element != null)
                {
                    PackageItem equip = new PackageItem();
                    equip.name = element.Name;
                    equip.id = element.GetAttribute("id");
                    equip.price = int.Parse(element.InnerText);
                    equipsLoaded.Add(equip);
                }
            }
    
            node = xmlDoc.SelectSingleNode("/道具列表/道具");
            pNodeList = node.ChildNodes;
            foreach (var item in pNodeList)
            {
                XmlElement element = item as XmlElement;
                if (element != null)
                {
                    PackageItem equip = new PackageItem();
                    equip.name = element.Name;
                    equip.id = element.GetAttribute("id");
                    equip.price = int.Parse(element.InnerText);
                    itemsLoaded.Add(equip);
                }
            }
        }
    
        bool AppendXML(string xpath, PackageItem item, bool byTag = false)
        {
            if (!System.IO.File.Exists(FilePath))
                return false;
    
            XmlDocument xmlDoc = new XmlDocument();
            xmlDoc.Load(FilePath);
    
            XmlNodeList nodes;
            if (byTag)
            {
                nodes = xmlDoc.GetElementsByTagName(xpath);
            }
            else
            {
                nodes = xmlDoc.SelectNodes(xpath);
            }
    
            if (nodes == null || nodes.Count == 0)
                return false;
    
            // 测试第9点 获取所有满足条件的属性
    //        foreach (XmlAttribute n in nodes) 
    //        {
    //            Debug.Log(n.Value);
    //            Debug.Log(n.OwnerElement.Name);
    //        }
    
            foreach (XmlElement node in nodes)
            {
                XmlElement name = xmlDoc.CreateElement(item.name);
                name.SetAttribute("id", item.id);
                XmlElement price = xmlDoc.CreateElement("价格");
                price.InnerText = item.price.ToString();
                name.AppendChild(price);
    
                node.AppendChild(name);
            }
           
            xmlDoc.Save(FilePath);  // 保存
            return true;
        }
    
        bool RemoveXML(string xpath, bool byTag = false)
        {
            if (!System.IO.File.Exists(FilePath))
                return false;
    
            XmlDocument xmlDoc = new XmlDocument();
            xmlDoc.Load(FilePath);
    
            XmlNodeList nodes;
            if (byTag)
            {
                nodes = xmlDoc.GetElementsByTagName(xpath);
            }
            else
            {
                nodes = xmlDoc.SelectNodes(xpath);
            }
    
            if (nodes == null || nodes.Count == 0)
                return false;
    
            for (int i = 0; i < nodes.Count; i++)
            {
                Debug.Log("删除节点：" + nodes[i].Name);
                nodes[i].ParentNode.RemoveChild(nodes[i]);
            }
    
            xmlDoc.Save(FilePath);
            return true;
        }
    
        bool ModifyXML(string xpath, string value)
        {
            if (!System.IO.File.Exists(FilePath)) return false;
    
            XmlDocument xmlDoc = new XmlDocument();
            xmlDoc.Load(FilePath);
    
            XmlNodeList nodes = xmlDoc.SelectNodes(xpath);
    
            if (nodes == null || nodes.Count == 0) return false;
    
            foreach (XmlElement n in nodes)
            {
                n.InnerText = value;
            }
    
            xmlDoc.Save(FilePath);
            return true;
        }
      
    }
    
    [System.Serializable]
    public struct PackageItem
    {
        public string id;
        public string name;
        public int price;
    }
    
```