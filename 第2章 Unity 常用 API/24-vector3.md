##Vector3 三维向量

结构体类型

表示 3D的 向量和点。

这个结构用于在 Unity 传递 3D 位置和方向。它包含向量运算的函数。



|静态变量|值|
|:--|:--|
|up|(0, 1, 0)|
|down|(0, -1, 0)|
|forward|(0, 0, 1)|
|back|(0, 0, -1)|
|left|(-1, 0, 0)|
|right|(1, 0, 0)|
|zero|(0, 0, 0)|
|one|(1, 1, 1)|

---

|变量|说明|
|:--|:--|
|magnitude|返回向量的长度（只读）。|
|**normalized**|返回该向量方向上的长度为1的向量（只读）。|
|sqrMagnitude|返回这个向量的长度的平方（只读）。|
|this[int]|使用[0], [1], [2]分别访问该向量的 x, y, z 元素。|
|x|向量的 x 元素|
|y|向量的 y 元素|
|z|向量的 z 元素|

---

|静态方法|说明|
|:--|:--|
|**Angle**| 返回两个向量之间的夹角。 |
|ClampMagnitude|返回的向量的长度，最大不超过 maxLength 所指示的长度。|
|**Distance**|返回两个点之间的距离。|
|**Cross**|计算两个向量的叉乘。|
|**Dot**|计算两个向量的点乘。|
|MoveTowards|当前的地点移向目标。|
|Max|返回一个由两个向量的最大元素组成的向量。|
|Min|返回一个由两个向量的最小元素组成的向量。|
|**Lerp**|两个点之间的线性插值。|
|LerpUnclamped|两个向量之间的线性插值。该插值t在小于0或大于1时的返回值不会被限制。|
|Normalize|使向量的长度为1。|
|Project|投影一个向量到另一个向量。|
|ProjectOnPlane|投影向量到一个平面上（由垂直到该平面的法线定义）。|
|Reflect|沿着法线反射向量。|
|SmoothDamp|随着时间的推移，逐渐改变一个向量朝向预期的目标。|
|RotateTowards|当前的向量转向目标。|
|OrthoNormalize|使向量规范化并且彼此相互垂直。|
|Scale|两个矢量组件对应相乘。|
|Slerp|在两个向量之间球形插值。|
|SlerpUnclamped|在两个向量之间球形插值。该插值t在小于0或大于1时的返回值不会被限制。|

---

###Vector3.ClampMagnitude
返回原向量的拷贝，并且它的模最大不超过maxLength所指示的长度。
也就是说，限制向量长度到一个特定的长度。

**示例：**
```javascript
    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour {
        public Vector3 centerPt;
        public float radius;
        void Update() {
            Vector3 movement = new Vector3(Input.GetAxis("Horizontal"), 0, Input.GetAxis("Vertical"));
            Vector3 newPos = transform.position + movement;
            Vector3 offset = newPos - centerPt;
            transform.position = centerPt + Vector3.ClampMagnitude(offset, radius);
        }
    }
```

---

###Vector3.Dot 点乘
两个向量的点乘积。
点积是一个浮点型的值，两个向量的长度相乘，然后乘以它们之间夹角的余弦值。
对于normalized向量，如果他们指向在完全相同的方向，Dot返回1。如果他们指向完全相反的方向，返回-1。对于其他的情况返回一个数（例如：如果是垂直的Dot返回0）。

**示例：**
```javascript
    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour {
        public Transform other;
        void Update() {
            if (other) {
                Vector3 forward = transform.TransformDirection(Vector3.forward);
                Vector3 toOther = other.position - transform.position;
                if (Vector3.Dot(forward, toOther) < 0)
                    print("The other transform is behind me!");
            
            }
        }
    }
```

---

###Vector3.Cross 叉乘
两个向量的交叉乘积。
两个向量的叉积乘积结果在垂直于两个输入向量的三分之一个向量。结果的大小等于两个输入相乘，然后乘以输入向量之间的角度的正弦值。你可以确定的方向的结果向量使用“左手法则”。

![](/assets/屏幕快照 2016-11-27 下午9.07.24.png)
**示例：**
```javascript
    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour {
        Vector3 GetNormal(Vector3 a, Vector3 b, Vector3 c) {
            Vector3 side1 = b - a;
            Vector3 side2 = c - a;
            return Vector3.Cross(side1, side2).normalized;
        }
    }
```

---

###Vector3.Lerp 线性差值
两个向量之间的线性插值。
按照分数t在from到to之间插值。这是最常用的寻找一点沿一条线的两个端点之间一些分数的方式（例如，在那些点之间逐渐移动一个对象）。这分数是在范围[ 0…1]。t是夹在 [0…1]之间，当t = 0时，返回from，当t = 1时，返回to。当t = 0.5 返回from和to的中间点。

**示例：**
```javascript
    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour {

        public Transform startMarker;
        public Transform endMarker;
        public float speed = 1.0F;
        private float startTime;
        private float journeyLength;

        void Start() {
            startTime = Time.time;
            journeyLength = Vector3.Distance(startMarker.position, endMarker.position);
        }

        void Update() {
            float distCovered = (Time.time - startTime) * speed;
            float fracJourney = distCovered / journeyLength;
            transform.position = Vector3.Lerp(startMarker.position, endMarker.position, fracJourney);
        }
    }
```

---

###Vector3.Slerp 球形差值
两个向量之间的弧形插值
通过t数值在from和to之间插值。这与线性内插之间的不同（即， “线性插值” ）是该向量被视为方向而不是空间中的点。返回的向量的方向是由角内插，其大小是from和to的幅度之间进行内插。

**示例：**

```javascript
    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour {
        public Transform sunrise;
        public Transform sunset;
        public float journeyTime = 1.0F;
        private float startTime;

        void Start() {
            startTime = Time.time;
        }

        void Update() {
            Vector3 center = (sunrise.position + sunset.position) * 0.5F;
            center -= new Vector3(0, 1, 0);
            Vector3 riseRelCenter = sunrise.position - center;
            Vector3 setRelCenter = sunset.position - center;
            float fracComplete = (Time.time - startTime) / journeyTime;
            transform.position = Vector3.Slerp(riseRelCenter, setRelCenter, fracComplete);
            transform.position += center;
        }
    }
```

---

###Vector3.MoveTowards
当前的地点移向目标。
这个函数的返回值是一个点maxdistancedelta单位更接近于目标/点沿着当前的和目标之间的线。如果目标是比maxdistancedelta /然后返回值将等于目标接近（即移动不会超过目标）。maxdistancedelta负值可以用来从目标推开该向量。

**示例：**

```javascript
    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour {
        public Transform target;
        public float speed;

        void Update() {
            float step = speed * Time.deltaTime;
            transform.position = Vector3.MoveTowards(transform.position, target.position, step);
        }
    }
```

---
###Vector3.SmoothDamp
随着时间的推移，逐渐改变一个向量朝向预期的目标。
向量由一些像弹簧阻尼器函数平滑，这将永远不会超过。最常见的用途是相机平滑跟随。

**示例：**

```javascript
    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour {
        public Transform target;
        public float smoothTime = 0.3F;
        private Vector3 velocity = Vector3.zero;

        void Update() {
            Vector3 targetPosition = target.TransformPoint(new Vector3(0, 5, -10));
            transform.position = Vector3.SmoothDamp(transform.position, targetPosition, ref velocity, smoothTime);
        }
    }
```

---

###Vector3.RotateTowards 转向
当前的向量转向目标。

这个函数类似于MoveTowards除了将向量被视为一个方向，而不是一个位置上。当前向量将被旋转朝向目标方向由maxRadiansDelta的角度，虽然会恰好落在目标，而不是超过。如果当前的大小和目标的是不同的，那么结果的幅度将被线性地旋转过程中进行插值。如果一个负值用于maxRadiansDelta ，向量会转离目标/直到它指向完全相反的方向，然后停止。

**示例：**

```javascript
    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour {
        public Transform target;
        public float speed;

        void Update() {
            Vector3 targetDir = target.position - transform.position;
            float step = speed * Time.deltaTime;
            Vector3 newDir = Vector3.RotateTowards(transform.forward, targetDir, step, 0.0F);
            Debug.DrawRay(transform.position, newDir, Color.red);
            transform.rotation = Quaternion.LookRotation(newDir);
        }
    }
```

🔚
