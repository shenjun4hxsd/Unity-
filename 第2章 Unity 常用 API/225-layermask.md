##LayerMask 层蒙版

layermask允许你在检视面板中显示layermask弹出菜单。

|变量|说明|
|:--|:--|
|value|转换层蒙版的值为整数值。|

|静态方法|说明|
|:--|:--|
|GetMask|无论是内置的层名称集或是由用户在“标签和层管理器”中定义的层，返回与它们相等的层蒙版。|
|LayerToName|给定一个层数，返回内置的或在Tags and Layers中定义的层名称。|
|NameToLayer|给定一个层名称，返回内置的或在Tags and Layers manager中定义的层索引。|

###示例：

####LayerMask.value 值

转换层蒙版的值为整数值。

```javascript
    using UnityEngine;
    using System.Collections;
 
    public class ExampleClass : MonoBehaviour {
        public LayerMask mask = -1;
        void Update() {
            if (Physics.Raycast(transform.position, transform.forward, 100, mask.value))
                Debug.Log("Hit something");
        }
    }
```

---

####LayerMask.GetMask 获取蒙版

public static int **GetMask**(**params** **string[]** layerNames);
