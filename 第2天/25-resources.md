##Resources 资源

Resources类允许你从指定的路径查找或访问资源。

在编辑器，Resources.FindObjectsOfTypeAll能用来定位资源和场景对象。

所有资源必须在项目Assets文件内的任意Resources文件夹中，可以通过Resources.Load访问。允许有多个Resources文件夹，每次加载对象是会自动检查。

在Unity中通常不需要使用路径名来访问资源，相反你可以通过声明一个成员变量来暴露一个资源的引用，然后在检视面板中指定它。使用这个技巧的时候Unity可以在构建的时候自动计算哪个资源被使用。这从根本上最大限度地减少了实际用于游戏的资源的尺寸。当你放资源在“Resources”文件夹中时，所有在“Resources”文件夹中的资源都将被打包编译包含在游戏中。

另一个使用路径名的缺点是，缺乏可重用性，因为脚本对于使用的资源具有硬编码要求。另一方面使用暴露在检视面板中的资源引用，是自文档化的，对于使用脚本的用户来说也是立竿见影的。

然而，有些情况下按照名称比在检视面板中取回一个资源更方便，尤其是当在检视面板中指定引用是不方便的时候。例如你或许想从脚本创建一个游戏物体，为程序生成的网格赋值一个纹理。

有些已加载的资源，最明显的是纹理，即使在场景没有实例，也最占内存。当资源不再需要时，你可以使用Resources.UnloadUnusedAssets回收内存。


示例：

```javascript
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