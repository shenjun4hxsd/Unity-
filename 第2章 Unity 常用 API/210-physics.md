##Physics 物理

全局物理属性和辅助方法。

|静态变量|说明|
|:--|:--|
|AllLayers|选择所有层的层蒙版常量。|
|bounceThreshold|两个碰撞物体的相对速度如果在这个设定值之下，不会反弹。默认值2.。取值范围0到无穷大。必须为正数。|
|defaultContactOffset|新创建碰撞器的默认接触偏移值。|
|DefaultRaycastLayers|选择默认投射层的层蒙版常数。|
|gravity|场景中应用到所有刚体的重力。|
|IgnoreRaycastLayer|选择忽略层的层蒙版常量。|
|queriesHitTriggers|指定查询(raycasts, spherecasts, overlap tests, etc.)是否碰到触发器。|
|sleepThreshold|质量归一化的能量阈值，低于这个阈值的对象开始休眠。|
|solverIterationCount|默认的求解迭代次数允许使用任意刚体（默认为7）。必须为正数。|

|静态方法|说明|
|:--|:--|
|BoxCast|沿射线投射立方体，并返回碰到的信息。|
|BoxCastAll|就像Physics.BoxCast，但返回的是所有碰到的对象信息。|
|BoxCastNonAlloc|沿方向投射立方体，并储存到提供的缓冲器。|
|CapsuleCast|在场景中针对所有碰撞器投射一个胶囊，并返回碰到的细节信息。|
|CapsuleCastAll|像Physics.CapsuleCast，但这个函数将返回所有胶囊扫描交叉碰到的对象。|
|CapsuleCastNonAlloc|针对场景中所有的碰撞器投射一个胶囊，并返回缓冲器中碰到什么的信息。|