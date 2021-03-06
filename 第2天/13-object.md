##Object 对象

UnityEngine.Object

|变量|说明|
|:--|:--|
|hideFlags|由用户保存在场景或可修改的对象应该被隐藏么？|
|name|对象的名称|


|方法|说明|
|:--|:--|
|GetInstanceID|返回对象的实例ID。|
|ToString|返回对象的名称。|

|静态方法|说明|
|:--|:--|
|**Destroy**|删除一个游戏对象、组件或资源|
|DestroyImmediate|立即销毁物体对象，强烈建议使用 Destroy 代替。因为它可以永久性地销毁资源，此函数只应在编写编辑器代码时使用|
|DontDestroyOnLoad|加载新场景的时候使目标对象不被自动销毁。|
|FindObjectOfType|返回Type类型第一个激活的加载的对象。（效率非常低，不建议使用）|
|FindObjectsOfType|返回Type类型的所有激活的加载的物体列表。（效率非常低，不建议使用）|
|**Instantiate**|克隆原始物体并返回克隆物体。|


|Operators 操作符|说明|
|:--|:--|
|bool|对象是否存在？|
|operator !=|比较如果两个对象不相同。|
|operator ==|比较如果两个物体相同。|

---
###示例：

**Object.Destroy**

会在 Update 之后，渲染之前执行销毁操作。

```csharp
    public static void Destroy(Object obj, float t = 0.0f);


    // 删除当前游戏对象
    Destroy (gameObject);

    // 删除当前这个脚本
    Destroy (this);

    // 删除当前游戏对象上的刚体组件
    Destroy (rigidbody);

    // 5秒后删除这个游戏对象
    Destroy (gameObject, 5);

```

---

**Object.Instance**
```csharp
    public static Object Instantiate(Object original);
    public static Object Instantiate(Object original, Transform parent);
    public static Object Instantiate(Object original, Transform parent, bool worldPositionStays);
    public static Object Instantiate(Object original, Vector3 position, Quaternion rotation);
    public static Object Instantiate(Object original, Vector3 position, Quaternion rotation, Transform parent);

参数：

original：需要被克隆的原始物体
position：新物体的位置
rotation：新物体的旋转
parent：指定新物体的父物体
worldPositionStays：当指定父物体以后仍保持原始的世界坐标

```

```csharp
    using UnityEngine; 

    public class InstantiateExample : MonoBehaviour
    {
        public GameObject prefab;

        void Start()
        {
            for (int i = 0; i < 10; i++)
                Instantiate(prefab, new Vector3(i * 2.0f, 0, 0), Quaternion.identity);
        }
    }
```
实例化也能直接克隆脚本实例，整个游戏物体层次将被克隆，并且返回被克隆脚本的实例


**使用泛型的方式：**

```csharp
    using UnityEngine; 

    public class InstantiateGenericsExample : MonoBehaviour
    {
        public Missile missile;

        void Start ()
        {
            Missile missileCopy = Instantiate<Missile>(missile);
        }
    }
```

