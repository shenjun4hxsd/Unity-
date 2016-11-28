##Quaternion


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
