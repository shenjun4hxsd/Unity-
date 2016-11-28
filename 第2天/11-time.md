##Time 时间

获取时间信息的接口。

帧率：每秒钟绘制的次数，次数越高表现的越精细。


|静态变量||
|:--|:--|
|captureFramerate|减慢游戏播放时间以允许在帧之间保存屏幕图像。|
|**deltaTime**|以秒计算，完成最后一帧的时间（只读）。|
|fixedDeltaTime|以秒计间隔，在物理和其他固定帧速率进行更新（像MonoBehaviour的FixedUpdate）。|
|fixedTime|最近的FixedUpdate已开始的时间（只读）。这是以秒计自游戏开始的时间。|
|**frameCount**|已经传递帧的总数（只读）。|
|maximumDeltaTime|一帧能获得的最大时间。物理和其他固定帧速率更新（像FixedUpdate）。|
|realtimeSinceStartup|以秒计，自游戏开始的真实时间（只读）。|
|smoothDeltaTime|经平滑的Time.deltaTime时间（只读）。|
|**time**|以秒计算，从游戏开始的时间（只读）。也就是说，从游戏开始到到现在所用的时间。|
|**timeScale**|时间的缩放。这可以用于减慢运动效果。|
|timeSinceLevelLoad|以秒计算，从最后的关卡已经加载完的时间（只读）。|
|unscaledDeltaTime|无关timeScale，以秒计算，要完成最后一帧的时间（只读）。|
|unscaledTime|无关timeScale，以秒计算，从游戏开始的时间（只读）。|

###Time.deltaTime 增量时间

**示例：**
```javascript
    using UnityEngine;
    using System.Collections;
 
    public class ExampleClass : MonoBehaviour {
        void Update() {
            float translation = Time.deltaTime * 10;
            transform.Translate(0, 0, translation);
        }
    }
```

🔚

