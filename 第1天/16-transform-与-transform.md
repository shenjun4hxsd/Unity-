##Transform 与 transform 区别

---

### Transform 变换
描述：对象的位置、旋转和缩放

场景中的每一个对象都有一个Transform组件。用于储存并操控物体的位置、旋转和缩放。每一个 Transform 可以有一个父级，允许你分层次应用位置、旋转和缩放。可以在Hierarchy 面板查看层次关系。他们也支持计数器（enumerator），因此你可以使用循环遍历子对象。

```csharp
    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour {
 
        void Example() {
            foreach (Transform child in transform) {
                child.position += Vector3.up * 10.0F;
            }
        }
    }
```

就是类与对象的区别

Transform 组件 ：每个游戏物体默认带有的组件，有且只有一个，不能手动添加。

transform ：当前游戏物体的 Transform 组件对象。


🔚