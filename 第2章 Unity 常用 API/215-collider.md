##Collider 碰撞器

所有碰撞器的基类。

如果一个对象带有碰撞器，需要在游戏中移动，那么你应该添加个Rigidbody组件给它。如果你不想该对象与其他对象有交互，刚体可以设置为运动学刚体（kinematic）。


|变量|说明|
|:--|:--|
|attachedRigidbody|碰撞器附加的刚体。碰撞器会自动连接到这个刚体或它的任意父对象刚体。|
|bounds|碰撞器框的对象。注意，如果碰撞器被禁用或该游戏对象没激活，边界盒值将返回空。|
|contactOffset|该碰撞器的触点偏移值。那些碰撞器间的距离比它们的contactOffset值之和小将产生接触。该触点偏移值必须为正。触点偏移允许碰撞检测系统预判接触，即使对象稍微分开点。|
|enabled|启用碰撞器将会碰撞其他碰撞器，禁用碰撞器就不会碰撞其他碰撞器。|
|isTrigger|碰撞器是一个触发器？|
|material|用于碰撞器的物理材质。|
|sharedMaterial|该碰撞器的共享的物理材质。修改材质将改变所有碰撞器使用该材质的表面属性，大多数情况下你可以修改Collider.material替代。|


|公共方法|说明|
|:--|:--|
|ClosestPointOnBounds|到碰撞器的框最近的点。|
|Raycast|投射一条射线（Ray），它忽略所有碰撞器，除了这个。|

|Messsage 消息|说明|
|:--|:--|
|OnCollisionEnter|当该collider/rigidbody开始碰到另一个rigidbody/collider时OnCollisionEnter被调用。|
|OnCollisionExit|当 collider/rigidbody停止碰撞另一个 rigidbody/collider时，OnCollisionExit被调用。|
|OnCollisionStay|每当该collider/rigidbody碰到其他rigidbody/collider，将在每帧调用OnCollisionStay。|
|OnTriggerEnter|当碰撞器进入触发器时OnTriggerEnter被调用。|
|OnTriggerExit|当该碰撞器停止接触触发器时，OnTriggerExit被调用。也就是说，当碰撞器离开触发器时，调用OnTriggerExit。|
|OnTriggerStay|每当碰撞器从进入触发器，几乎每帧调用OnTriggerStay。|

###示例：
**Collider.ClosestPointOnBounds 在框上最近的点**

到碰撞器的框最近的点。

当应用于爆炸伤害，这能用于计算伤害点数。

```csharp
    using UnityEngine;
    using System.Collections;
 
    public class ExampleClass : MonoBehaviour {
        public float hitPoints = 100.0F;
        public Collider coll;

        void Start() {
            coll = GetComponent<Collider>();
        }

        void ApplyHitPoints(Vector3 explosionPos, float radius) {
            // The distance from the explosion position to the surface of the collider.
            Vector3 closestPoint = coll.ClosestPointOnBounds(explosionPos);
            float distance = Vector3.Distance(closestPoint, explosionPos);
 
            // The damage should decrease with distance from the explosion.
            float damage = 1.0F - Mathf.Clamp01(distance / radius);
            hitPoints -= damage * 10.0F;
        }
    }
```

---

**Collider.Raycast 射线投射**

public bool Raycast(Ray ray, out RaycastHit hitInfo, float maxDistance);

|Parameters|参数|
|:--|:--|
|ray|The starting point and direction of the ray.射线的开始点和方向。|
|hitInfo|If true is returned, hitInfo will contain more information about where the collider was hit (See Also: RaycastHit).\\如果返回真，hitInfo将包含更多关于碰撞器碰到哪里的信息（参见RaycastHit）|
|distance|The length of the ray.\\射线的长度。|


返回布尔(bool)值，当射线和任何碰撞器相交时，返回true，否则为false。也就是说，当射线碰触到任何碰撞器时返回真，否则返回假。

```csharp
    using UnityEngine;
    using System.Collections;
 
    public class ExampleClass : MonoBehaviour {
        public Collider coll;
        void Start() {
            coll = GetComponent<Collider>();
        }
        void Update() {
            if (Input.GetMouseButtonDown(0)) {
                Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
                RaycastHit hit;
                // 投射一条射线（Ray），它忽略所有碰撞器，除了这个。
                if (coll.Raycast(ray, out hit, 100.0F))
                    transform.position = ray.GetPoint(100.0F);
            }
        }
    }
```