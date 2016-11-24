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
|Destroy|删除一个游戏对象、组件或资源|
|DestroyImmediate|立即销毁物体obj，强烈建议使用Destroy代替。|
|DontDestroyOnLoad|加载新场景的时候使目标对象不被自动销毁。|
|FindObjectOfType|返回Type类型第一个激活的加载的对象。|
|FindObjectsOfType|返回Type类型的所有激活的加载的物体列表。|
|Instantiate|克隆原始物体并返回克隆物体。|


|Operators 操作符|说明|
|:--|:--|
|bool|对象是否存在？|
|operator !=|比较如果两个对象不相同。|
|operator ==|比较如果两个物体相同。|

