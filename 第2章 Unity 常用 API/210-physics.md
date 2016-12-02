##Physics 物理

全局物理属性和辅助方法。

|静态变量|说明|
|:--|:--|
|AllLayers|选择所有层的层蒙版常量。|
|bounceThreshold|两个碰撞物体的相对速度如果在这个设定值之下，不会反弹。默认值2.。取值范围0到无穷大。必须为正数。|
|defaultContactOffset|新创建碰撞器的默认接触偏移值。|
|DefaultRaycastLayers|选择默认投射层的层蒙版常数。|
|**gravity**|场景中应用到所有刚体的重力。|
|IgnoreRaycastLayer|选择忽略层的层蒙版常量。|
|queriesHitTriggers|指定查询(raycasts, spherecasts, overlap tests, etc.)是否碰到触发器。|
|**sleepThreshold**|质量归一化的能量阈值，低于这个阈值的对象开始休眠。|
|solverIterationCount|任意刚体默认允许使用的求解迭代次数（默认为7）。必须为正数。|

|静态方法|说明|
|:--|:--|
|BoxCast|沿射线投射立方体，并返回碰到的信息。|
|BoxCastAll|就像Physics.BoxCast，但返回的是所有碰到的对象信息。|
|BoxCastNonAlloc|沿方向投射立方体，并储存到提供的缓冲器（RaycastHit）。返回碰到的储存在results缓冲器中的数量。|
|CapsuleCast|在场景中针对所有碰撞器投射一个胶囊，并返回碰到的细节信息。|
|CapsuleCastAll|像Physics.CapsuleCast，但这个函数将返回所有胶囊扫描交叉碰到的对象。|
|CapsuleCastNonAlloc|针对场景中所有的碰撞器投射一个胶囊，并返回缓冲器中碰到什么的信息。|
|CheckBox|检测给定盒是否与其他碰撞器重叠。|
|CheckCapsule|在世界坐标空间，检查胶囊体是否与任意碰撞器重叠。|
|CheckSphere|在世界坐标空间，由位置和半径定义的球与任意碰撞器重叠，如果返回true。|
|GetIgnoreLayerCollision|layer1和layer2的层的碰撞时否忽略？|
|IgnoreCollision|使碰撞检测系统忽略所有collider1和collider2碰撞。|
|IgnoreLayerCollision|使碰撞检测系统忽略layer1和layer2中的任意碰撞器的所有碰撞。|
|**Linecast**|在线的开始和结束位置之间，如果与任何碰撞器相交返回真。|
|OverlapBox|查找给定立方体内或与之接触的所有碰撞器。|
|OverlapBoxNonAlloc|查找给定立方体内或与之接触的所有碰撞器，并储存在缓冲器中。|
|**OverlapSphere**|返回球内或与之接触的所有碰撞器。|
|OverlapSphereNonAlloc|计算并储存该球内或与之接触的所有碰撞器到提供的缓冲器中。|
|**Raycast**|在场景中投下可与所有碰撞器碰撞的一条光线。注意：如果射线从碰撞体的内部或者背面打，Raycast不检测碰撞。|
|**RaycastAll**|在场景投射一条射线并返回所有碰撞。注意不保证顺序。|
|RaycastNonAlloc|在场景投射射线并储存碰到的对象到缓冲器中。|
|SphereCast|沿着射线方向投射一个球形，并返回更多的碰到的信息。|
|SphereCastAll|类似Physics.SphereCast，但是这个函数返回所有球形扫描到的碰撞信息。|
|SphereCastNonAlloc|沿direction方向投射球体，并储存在results缓冲器中。|


###示例：

####Physics.Raycast 射线投射

public static bool **Raycast**(**Vector3** origin, **Vector3** direction, **float** maxDistance = Mathf.Infinity, **int** layerMask = DefaultRaycastLayers, **QueryTriggerInteraction** queryTriggerInteraction = QueryTriggerInteraction.UseGlobal);

```javascript
    using UnityEngine;
    using System.Collections;
 
    public class ExampleClass : MonoBehaviour {
        void Update() {
            Vector3 fwd = transform.TransformDirection(Vector3.forward);
            if (Physics.Raycast(transform.position, fwd, 10))
                print("There is something in front of the object!");
 
        }
    }
```
注意：如果射线从碰撞体的内部或者背面打，Raycast不检测碰撞。如果你用脚本或动画移动碰撞器，需要至少一个在FixedUpdate执行，因为物理库能更新它的数据结构，因此一条射线在它的新位置之前打到碰撞器。

```javascript
    using UnityEngine;
    using System.Collections;
 
    public class ExampleClass : MonoBehaviour {
        void Update() {
            RaycastHit hit;
            if (Physics.Raycast(transform.position, -Vector3.up, out hit))
                float distanceToGround = hit.distance;
 
        }
    }
```


---

####Physics.CapsuleCast 胶囊投射

public static bool **CapsuleCast**(**Vector3** point1, **Vector3** point2, **float** radius, **Vector3** direction, out **RaycastHit** hitInfo, **float** maxDistance = Mathf.Infinity, **int** layerMask = DefaultRaycastLayers, **QueryTriggerInteraction** queryTriggerInteraction = QueryTriggerInteraction.UseGlobal);

|参数|说明|
|:--|:--|
|point1|球体开始中心点。|
|point2|球体结束的中心点。|
|radius|胶囊的半径。|
|direction|胶囊扫描的方向。|
|maxDistance|扫描的最大长度。|
|hitInfo|hitInfo包含更多的碰到碰撞器的信息。|
|layerMask|选择投射的层蒙版。|
|queryTriggerInteraction|指定是否查询碰到触发器。|

返回值：胶囊扫描到任意交叉碰撞器返回true，否则返回false。

胶囊是由radius半径与point1和point2位置的两个球形成胶囊的两端定义。返回胶囊沿direction方向碰到的第一个碰撞器。这通常用于投射不需足够的精度，因为你要找出一个特定大小的物体，如人物，能移动到某处而不在途中碰撞到任何东西。

注意，胶囊投射不检测胶囊重叠的碰撞器。如果从脚本或动画移动碰撞器，需要有至少一个在FixedUpdate中执行，使得物理库可以更新它的数据结构，在CapsuleCast碰到碰撞器之前是在它的新位置。

```javascript
    using UnityEngine;
    using System.Collections;
 
    public class ExampleClass : MonoBehaviour {
        void Update() {
            RaycastHit hit;
            CharacterController charContr = GetComponent<CharacterController>();
            Vector3 p1 = transform.position + charContr.center + Vector3.up * -charContr.height * 0.5F;
            Vector3 p2 = p1 + Vector3.up * charContr.height;
            float distanceToObstacle = 0;
 
            // Cast character controller shape 10 meters forward to see if it is about to hit anything.
            if (Physics.CapsuleCast(p1, p2, charContr.radius, transform.forward, out hit, 10))
                distanceToObstacle = hit.distance;
        }
    }
```

