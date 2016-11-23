##MonoBehaviour

MonoBehaviour 是每个脚本派生自的基类。

注意:MonoBehavior对象(编辑器中)的复选框，只有在其有Start()，Awake()，Update()， FixedUpdate()和 OnGUI()函数时显示，没有这些函数时则隐藏。

| Message 消息 | 说明 |
|:--|:--|
|Awake|当该脚本实例被载入时 Awake 被调用。|
|Start|仅在当第一次脚本启用Update方法被调用之前调用。|
|FixedUpdate|当MonoBehaviour启用时，其 FixedUpdate 在每一帧被调用。|
|Update|当MonoBehaviour启用时，其Update在每一帧被调用。|
|LateUpdate|当Behaviour启用时，其LateUpdate在每一帧被调用。|
|OnDisable|当对象变为不可用或非激活状态时此函数被调用。|