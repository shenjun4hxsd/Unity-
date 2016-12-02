##Collider 碰撞器

所有碰撞器的基类。

如果一个对象带有碰撞器，需要在游戏中移动，那么你应该添加个Rigidbody组件给它。如果你不想该对象与其他对象有交互，刚体可以设置为运动学刚体（kinematic）。


|变量|说明|
|:--|:--|
|attachedRigidbody|碰撞器附加的刚体。|
|bounds|碰撞器框的对象。|
|contactOffset|该碰撞器的触点偏移值。|
|enabled|启用碰撞器将会碰撞其他碰撞器，禁用碰撞器就不会碰撞其他碰撞器。|
|isTrigger|碰撞器是一个触发器？|
|material|用于碰撞器的物理材质。|
|sharedMaterial|该碰撞器的共享的物理材质。|


|公共方法|说明|
|:--|:--|
|ClosestPointOnBounds|到碰撞器的框最近的点。|
|Raycast|投射一条射线（Ray），它忽略所有碰撞器，除了这个。|

