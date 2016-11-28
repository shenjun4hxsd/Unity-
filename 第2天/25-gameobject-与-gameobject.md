###GameObject 游戏对象

Unity 场景里面所有实体的基类。

|变量|说明|
|:--|:--|
|activeInHierarchy|场景中的游戏对象是否激活？|
|activeSelf|该游戏对象的局部激活状态。（只读）|
|isStatic|如果一个游戏对象是静态仅在编辑器API指定。|
|layer|游戏对象所在的层，层的范围是在[0…31]之间。|
|scene|场景物体。|
|**tag**|这个游戏对象的标签。|
|**transform**|附加于这个游戏对象上的变换。(如果没有则为空)|

---

|方法|说明|
|:--|:--|
|**AddComponent**|添加一个名称为className的组件到游戏对象。|
|BroadcastMessage|对此游戏对象及其子对象的所有MonoBehaviour中调用名称为methodName的方法。|
|CompareTag|此游戏对象是否被标记为tag标签？|
|**GetComponent**|如果这个游戏对象附件了一个类型为type的组件，则返回该组件，否则为空。|
|GetComponentInChildren|返回此游戏对象或者它的所有子对象上(深度优先)的类型为type的组件。|
|GetComponentInParent|从父对象查找组件。|
|GetComponents|返回该游戏对象所有type类型的组件列表。|
|GetComponentsInChildren|返回此游戏对象与其子对象所有type类型的组件。|
|GetComponentsInParent|返回此游戏对象与其父对象所有type类型的组件。|
|SampleAnimation|用于任何动画剪辑在给定的时间采样动画。|
|SendMessage|在这个游戏物体上的所有MonoBehaviour上调用名称为methodName的方法。|
|SendMessageUpwards|在这个游戏物体及其祖先物体的所有MonoBehaviour中调用名称为methodName的方法。|
|**SetActive**|激活/停用此游戏对象。|

|静态方法|说明|
|:--|:--|
|**CreatePrimitive**|创建一个带有原型网格渲染器和适当的碰撞器的游戏对象。|
|**Find**|找到并返回一个名字为name的游戏物体。|
|FindGameObjectsWithTag|返回具体tag标签的激活的游戏对象列表，如果没有找到则为空。|
|**FindWithTag**|返回标记为tag的一个游戏对象，如果没有找到对象则为空。|


🔚




