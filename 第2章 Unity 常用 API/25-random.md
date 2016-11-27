##Random 随机数

产生随机数的类。

|静态变量|说明|
|:--|:--|
|insideUnitCircle|随机返回一个半径为1的圆内的点（只读）。|
|insideUnitSphere|随机返回一个半径为1的球体内的点（只读）。|
|onUnitSphere|随机返回一个单位球体表面上的点（只读）。|
|rotation|随机返回一个（用四元数表示的）角度（只读）。|
|rotationUniform|随机返回一个均匀分布的旋转角度（只读） 。|
|seed|给随机数发生器设置种子。|
|state|获取/设置随机数发生器的完全内部状态。|
|value|返回一个0.0（包括）到1.0（包括）之间的随机数（只读）。|

|静态方法|说明|
|:--|:--|
|Range|在最大值（包括）和最小值（包括）范围内返回一个随机数（只读）|

###示例：
```javascript

    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour
    {
        public GameObject prefab;

        // Instantiate the prefab somewhere between -10.0 and 10.0 on the x-z plane
        void Start()
        {
            Vector3 position = new Vector3(Random.Range(-10.0f, 10.0f), 0, Random.Range(-10.0f, 10.0f));
            Instantiate(prefab, position, Quaternion.identity);
        }
    }
```



🔚
