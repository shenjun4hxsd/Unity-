##AnimationState

描述：

AnimationState 完全控制动画混合。

在大多数情况下动画界面是足够易于使用。如果您需要完全控制动画融合的任何过程，请使用 AnimationState。

AnimationState 接口允许您在的任何动画播放时修改速度、 权值、 时间和层。您还可以设置动画混合和 wrapMode。

|参数|说明|
|:--|:--|
|blendMode|使用哪种混合模式?|
|clip|此动画状态正在播放的剪辑。|
|enabled|启用/禁用动画。|
|length|动画剪辑的长度以秒为单位。|
|name|动画的名字。|
|normalizedSpeed|规范化播放速度。|
|normalizedTime|规范化动画时间。|
|speed|加快动画的播放速度，1 为正常播放速度。|
|time|当前动画时间。|
|weight|动画权重。|
|wrapMode|动画的循环模式。|

|公有方法|说明|
|:--|:--|
|AddMixingTransform|添加一个动画的Transform。这使你减少创建动画必须的数量。|
|RemoveMixingTransform|移除一个动画的Transform。|



示例：

```javascript
using UnityEngine;
using System.Collections;
 
public class ExampleScript : MonoBehaviour
{
    public Animation anim;
 
    void Start()
    {
        // Adds a mixing transform using a path instead
        Transform mixTransform = transform.Find("root/upper_body/left_shoulder");
 
        // Add mixing transform
        anim["wave_hand"].AddMixingTransform(mixTransform);
    }
}
```

🔚


