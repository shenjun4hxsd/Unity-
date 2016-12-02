##RaycastHit 射线投射碰撞信息

该结构用来获取射线投射返回的信息。

|变量|说明|
|:--|:--|
|barycentricCoordinate|碰到的三角形的重心坐标。|
|**collider**|碰到的碰撞器。|
|**distance**|从射线的原点到触碰点的距离。|
|lightmapCoord|在触碰点的UV光照贴图的坐标。|
|normal|射线触碰表面的法线。|
|point|在世界坐标空间，射线碰到碰撞器的接触点。|
|rigidbody|碰到的该碰撞器上的刚体。如果碰撞器上没有附加刚体，那么返回null。|
|textureCoord|在触碰点的UV纹理坐标。|
|textureCoord2|在接触点处的第二套UV纹理坐标。|
|transform|碰到的该刚体或碰撞器的变换。|
|triangleIndex|碰到的三角形的索引。|