GameObject 游戏对象

Unity 场景里面所有实体的基类。

|变量|说明|
|:--|:--|
|activeInHierarchy|场景中的游戏对象是否激活？|
|activeSelf|该游戏对象的局部激活状态。（只读）|
|isStatic|如果一个游戏对象是静态仅在编辑器API指定。|
|layer|游戏对象所在的层，层的范围是在[0…31]之间。|
|scene|场景物体。|
|tag|这个游戏对象的标签。|
|transform|附加于这个游戏对象上的变换。(如果没有则为空)|

|方法|说明|
|:--|:--|




AddComponent

Adds a component class named /className/ to the game object.

添加一个名称为className的组件到游戏对象。

BroadcastMessage

Calls the method named /methodName/ on every MonoBehaviour in this game object or any of its children.

对此游戏对象及其子对象的所有MonoBehaviour中调用名称为methodName的方法。

CompareTag

Is this game object tagged with /tag/?

此游戏对象是否被标记为tag标签？

GetComponent

Returns the component of Type /type/ if the game object has one attached, null if it doesn't.

如果这个游戏对象附件了一个类型为type的组件，则返回该组件，否则为空。

GetComponentInChildren

Returns the component of Type /type/ in the GameObject or any of its children using depth first search.

返回此游戏对象或者它的所有子对象上(深度优先)的类型为type的组件。

GetComponentInParent

Finds component in the parent.

从父对象查找组件。

GetComponents

Returns all components of Type /type/ in the GameObject.

返回该游戏对象所有type类型的组件列表。

GetComponentsInChildren

Returns all components of Type /type/ in the GameObject or any of its children.

返回此游戏对象与其子对象所有type类型的组件。

GetComponentsInParent

Returns all components of Type /type/ in the GameObject or any of its parents.

返回此游戏对象与其父对象所有type类型的组件。

SampleAnimation

Samples an animation at a given time for any animated properties.

用于任何动画剪辑在给定的时间采样动画。

SendMessage

Calls the method named /methodName/ on every MonoBehaviour in this game object.

在这个游戏物体上的所有MonoBehaviour上调用名称为methodName的方法。

SendMessageUpwards

Calls the method named /methodName/ on every MonoBehaviour in this game object and on every ancestor of the behaviour.

在这个游戏物体及其祖先物体的所有MonoBehaviour中调用名称为methodName的方法。

SetActive

Activates/Deactivates the GameObject.

激活/停用此游戏对象。


