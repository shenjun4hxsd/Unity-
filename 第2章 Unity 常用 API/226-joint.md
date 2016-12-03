##Joint（关节）

Joint是所有关节的基类。

|变量|说明|
|:--|:--|
|anchor|围绕限制的关节运动的锚点的位置。|
|autoConfigureConnectedAnchor|接触锚点被自动配置？|
|axis|围绕限制的身体的方向的轴。|
|breakForce|该力应用于关节的拉伸。|
|breakTorque|该转矩应用于该关节的拉伸。|
|connectedAnchor|相对于连接刚体的锚点位置。|
|connectedBody|参考该关节连接的其他刚体。|
|enableCollision|启用连接的身体和关节之间的碰撞。|
|enablePreprocessing|切换该关节的预处理。|


|Message 消息|说明|
|:--|:--|
|OnJointBreak|当附加到相同游戏物体上的关节被断开时调用。|


**示例：**

当一个力比关节的断开力高时，该关节将断开。当关节断开时，OnJointBreak将被调用，并且应用到关节的断开力将被传入，OnJointBreak之后这个关节自动从游戏物体移除。

注意：OnJointBreak 通过unity被调用，他不是一个函数，你可以从关节中调用。

```javascript
using UnityEngine;
    using System.Collections;
 
    public class ExampleClass : MonoBehaviour {
        void OnJointBreak(float breakForce) {
            Debug.Log("A joint has just been broken!, force: " + breakForce);
        }
    }
```
