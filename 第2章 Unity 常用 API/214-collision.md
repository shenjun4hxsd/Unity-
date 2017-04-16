##Collision 碰撞

描述碰撞

碰撞信息被传递给Collider.OnCollisionEnter, Collider.OnCollisionStay和Collider.OnCollisionExit这些事件。

|变量|说明|
|:--|:--|
|collider|被碰撞的那个碰撞器(只读)。|
|contacts|物理引擎生成的碰撞点集。|
|gameObject|被碰撞的碰撞器所在的游戏对象(只读)。|
|impulse|应用在接触解决的碰撞的总的推动力。|
|relativeVelocity|两个碰撞的对象之间的相对线性速度(只读)。|
|rigidbody|被碰撞的刚体组件(只读)。当这个对象碰撞到一个没有被附加刚体组件的碰撞器时返回null。|
|transform|我们碰撞的对象的变换(只读)。|


###示例：

**Collision.contacts 接触点**

物理引擎生成的碰撞点集。

```csharp
    using UnityEngine;
    using System.Collections;
 
    public class ExampleClass : MonoBehaviour {
        void OnCollisionStay(Collision collision) {
            foreach (ContactPoint contact in collision.contacts) {
                print(contact.thisCollider.name + " hit " + contact.otherCollider.name);
                Debug.DrawRay(contact.point, contact.normal, Color.white);
            }
        }
    }
```

```csharp
    using UnityEngine;
    using System.Collections;
 
    public class ExampleClass : MonoBehaviour {
        public Transform explosionPrefab;
        void OnCollisionEnter(Collision collision) {
            ContactPoint contact = collision.contacts[0];
            Quaternion rot = Quaternion.FromToRotation(Vector3.up, contact.normal);
            Vector3 pos = contact.point;
            Instantiate(explosionPrefab, pos, rot);
            Destroy(gameObject);
        }
    }
```


###ContactPoint 接触点

描述碰撞发生位置的一个接触点。

碰撞点被存储在碰撞对象中。参见:Collision

|变量|说明|
|:--|:--|
|normal|接触点的法向量。|
|otherCollider|其他相关联的碰撞器。|
|point|接触的点。|
|thisCollider|第一个相关联的碰撞器。|


🔚
