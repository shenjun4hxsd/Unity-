##MonoBehaviour

MonoBehaviour 是默认创建出来的每个脚本派生自的基类。

注意：MonoBehavior对象（编辑器中）的复选框，只有在其有 `Start()`，`Awake()`，`Update()`， `FixedUpdate()` 和 `OnGUI()` 函数时显示，没有这些函数时则隐藏。

| Message 消息 | 说明 |
|:--|:--|
|Awake|当该脚本实例被载入时 Awake 被调用。|
|Start|仅在当第一次脚本启用Update方法被调用之前调用。|
|FixedUpdate|当MonoBehaviour启用时，其 FixedUpdate 在每一帧被调用。|
|Update|当MonoBehaviour启用时，其Update在每一帧被调用。|
|LateUpdate|当Behaviour启用时，其LateUpdate在每一帧被调用。|
|OnDisable|当对象变为不可用或非激活状态时此函数被调用。|
|OnDestroy|当MonoBehaviour将被销毁时，这个函数被调用。|



继承的成员

|变量|说明|
|:--|:--|
|name|游戏对象的名字|
|enabled|为 true 表示激活当前脚本，为 false 表示当前脚本失活。|
|gameObject|当前游戏对象。|
|transform|当前游戏对象的 Transform 组件。|
|tag|当前游戏对象的标签。|
