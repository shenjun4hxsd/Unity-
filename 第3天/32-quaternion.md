##Quaternion
四元数用于表示旋转。

它们是紧凑的，不会出现万向节锁并且能够很容易被插值。Unity内使用Quaternion表示所有旋转。

他们基于复数并且直觉上不容易理解。你几乎不必要进入或者修改个人的四元数组件(x,y,z,w)；


###Quaternion.Slerp 球形插值
球形插值，通过t值from向to之间插值。参数取值范围[0,1]。

**实例：**
```javascript
    using UnityEngine;
    using System.Collections;
 
    public class ExampleClass : MonoBehaviour {
        public Transform from;
        public Transform to;
        public float speed = 0.1F;

        void Update() {
            transform.rotation = Quaternion.Slerp(from.rotation, to.rotation, Time.time * speed);
        }
    }
```


---
###Quaternion.LookRotation 注视旋转
创建一个旋转，沿着forward（z轴）并且头部沿着upwards（y轴）的约束注视。也就是建立一个旋转，使z轴朝向view y轴朝向up。

返回计算四元数。如果用于定向的变换，Z轴将会被对准前方并且如果这些向量正交，Y轴向前。如果forward方向是0，记录一个错误。

**实例：**

```javascript
    using UnityEngine;
    using System.Collections;
 
    public class ExampleClass : MonoBehaviour {
        public Transform target;
        void Update() {
            Vector3 relativePos = target.position - transform.position;
            Quaternion rotation = Quaternion.LookRotation(relativePos);
            transform.rotation = rotation;
        }
    }
```

---
###四元数

    因为矩阵的缺点我们需要使用四元数
        1、矩阵的冗余太多
        2、矩阵多次迭代后会造成万向锁问题
        3、矩阵的行列数总是影响向量的维度

    四元素特点
        1、四元数数据量小，只有四个元素
        2、四元数可以利用向量叉乘来计算，省去了矩阵乘法计算
        3、他会一直保持向量的维度是4维

    四元数组合
        [ w, v ]
        [w , (x, y, z) ]
        w是实数， v是三个虚数的组合

        计算
            参照复数四则运算
            使我们的虚数部分变成负数：就是共轭
            q＊ ＝ ［w, (-x, -y, -z)］

            求模
                ||p|| = sqrt( a*a + b *b );
                ||q|| = sqrt( pow(cos(a /2), 2) + pow(sin(a/2), 2) )
            求逆
                q逆 = q* / ||q||

            将逆和向量叉乘得到我们四元数旋转后的新坐标点

    注意事项
        四元数也是线性的，千万不能同时旋转


---

###API

|静态变量|说明|
|:--|:--|
|identity|同一性旋转（只读）。|


|变量|说明|
|:--|:--|
|eulerAngles|返回表示旋转的欧拉角度。|
|this[int]|分别使用 [0]、[1]、 [2]、 [3]，访问x、y、z、w组件。|
|w|四元数的W组件。不要直接修改这个，除非你很了解四元数。|
|x|四元数的X组件。不要直接修改这个，除非你很了解四元数。|
|y|四元数的Y组件。不要直接修改这个，除非你很了解四元数。|
|z|四元数的Z组件。不要直接修改这个，除非你很了解四元数。|

|公有方法|说明|
|:--|:--|
|Set|设置存在的四元素的x, y, z 和w 组件。|
|SetFromToRotation|创建一个从fromDirection到toDirection的旋转。|
|SetLookRotation|创建一个旋转，沿着forward（z轴）并且头部沿着up（y轴）的约束注视。也就是建立一个旋转，使z轴朝向view y轴朝向up。|
|ToAngleAxis|转换一个旋转用“角-轴”表示。|

|静态方法|说明|
|:--|:--|
|Angle|返回a和b两者之间的角度。|
|AngleAxis|绕axis轴旋转angle，创建一个旋转。|
|Dot|两个旋转之间的点乘。|
|Euler|返回一个旋转角度，绕z轴旋转z度，绕x轴旋转x度，绕y轴旋转y度（像这样的顺序）。|
|FromToRotation|从fromDirection到toDirection创建一个旋转。|
|Inverse|返回反向的旋转。|
|Lerp|通过t值from向to之间插值，并且规范化结果。|
|**LookRotation**|创建一个旋转，沿着forward（z轴）并且头部沿着upwards（y轴）的约束注视。也就是建立一个旋转，使z轴朝向view y轴朝向up。|
|RotateTowards|旋转一个角度从from向to。|
|**Slerp**|球形插值，通过t值from向to之间插值。参数取值范围[0,1]。|
|SlerpUnclamped|球形插值，通过t值from向to之间插值。该参数t是不在区间内。|

|Operators 运算符|说明|
|:--|:--|
|operator !=|判断两个四元数是否不同？|
|operator *|获取lhs 的旋转状态并应用rhs的旋转。|
|operator ==|判断两个四元数是否相等？|


🔚
