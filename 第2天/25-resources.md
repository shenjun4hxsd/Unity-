##Resources 资源

Resources类允许你从指定的路径查找或访问资源。

所有资源必须在项目Assets文件内的任意Resources文件夹中，可以通过Resources.Load访问。允许有多个Resources文件夹，每次加载对象是会自动检查。

在Unity中通常不需要使用路径名来访问资源，相反你可以通过声明一个成员变量来暴露一个资源的引用，然后在检视面板中指定它。使用这个技巧的时候Unity可以在构建的时候自动计算哪个资源被使用。这从根本上最大限度地减少了实际用于游戏的资源的尺寸。当你放资源在“Resources”文件夹中时，所有在“Resources”文件夹中的资源都将被打包编译包含在游戏中。

另一个使用路径名的缺点是，缺乏可重用性，因为脚本对于使用的资源具有硬编码要求。另一方面使用暴露在检视面板中的资源引用，是自文档化的，对于使用脚本的用户来说也是立竿见影的。

然而，有些情况下按照名称比在检视面板中取回一个资源更方便，尤其是当在检视面板中指定引用是不方便的时候。例如你或许想从脚本创建一个游戏物体，为程序生成的网格赋值一个纹理。

有些已加载的资源，最明显的是纹理，即使在场景没有实例，也最占内存。当资源不再需要时，你可以使用Resources.UnloadUnusedAssets回收内存。


示例：

```csharp
    using UnityEngine;
    using System.Collections;
 
    public class ExampleClass : MonoBehaviour {
        void Start() {
            var go = GameObject.CreatePrimitive(PrimitiveType.Plane);
            var rend = go.GetComponent<Renderer>();
            rend.material.mainTexture = Resources.Load("glass") as Texture;
        }
    }
```


|静态方法|说明|
|:--|:--|
|FindObjectsOfTypeAll|返回所有type类型对象的一个列表。|
|**Load**|加载储存在Resources文件夹中path处的资源。|
|LoadAll|加载Resources文件夹中的path文件夹或者文件中的所有资源。|
|LoadAsync|异步加载Resources文件夹中的资源。|
|UnloadAsset|从内存卸载指定的资源。|
|UnloadUnusedAssets|卸载未使用的资源。|


---
###Resources.Load 加载

加载储存在Resources文件夹中path处的资源。

注意：路径名使用正斜杠“/”，如果使用反斜杠“\”会不正常运行。

如果发现，返回所在path处的资源，否则返回null。如果有type参数，只有type类型的物体将被返回。 Path相对于Resources文件夹，忽略扩展名。Resouces文件夹可以在Assets文件夹中的任何位置。

**示例1：**

```javascript
    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour {

        void Start() {
            GameObject go = GameObject.CreatePrimitive(PrimitiveType.Plane);
            Renderer rend = go.GetComponent<Renderer>();
            rend.material.mainTexture = Resources.Load("glass") as Texture;
        }
    }
```

**示例2：**

```javascript

    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour {

        void Start() {

            // GameObject instance = Instantiate(Resources.Load("enemy", typeof(GameObject))) as GameObject;
            GameObject instance = Instantiate(Resources.Load<GameObject>("enemy")) as GameObject;

        }
    }
```

