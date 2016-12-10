##Animation.PlayQueued 播放队列

**public AnimationState PlayQueued(string animation, QueueMode queue = QueueMode.CompleteOthers, PlayMode mode = PlayMode.StopSameLayer);**

描述 ：

在前一个动画播放完成之后直接播放下一个动画。

比如你可能会一个接一个播放一个特殊序列的动画。

动画状态在播放之前会复制自己，因此你可以在相同的动画之间进行淡入淡出。这个可以用于覆盖两个相同的动画。比如你可能有一个剑挥舞的动画，玩家连续快速削两次。你可以倒播这个动画然后从头再播放但是你会看到动画之间有跳越现象。


**下面几个模式可用：**
 
如果queue是QueueMode.CompleteOthers，当所有其他动画停止播放，这个动画才会开始。 
如果queue是QueueMode.PlayNow，这个动画将在一个重复的动画状态下立即开始播放。

当动画完成播放之后，它会自动清理。在一个动画状态结束之后使用它，将会导致一个异常。

```javascript
using UnityEngine;
using System.Collections;
 
public class ExampleClass : MonoBehaviour {
    public Animation anim;
    void Start() {
        anim = GetComponent<Animation>();
    }
    void Update() {
        if (Input.GetButtonDown("Fire1"))
            anim.PlayQueued("shoot", QueueMode.PlayNow);
 
    }
}
```