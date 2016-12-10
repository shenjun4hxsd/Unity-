##Animation 动画

animation组件用于播放动画。

你能指定动画剪辑到动画组件并从脚本控制播放。该动画系统是基于权重并支持动画融合、添加动画、动画混合、层并完全控制播放的各个方面。

AnimationState可以用于改变动画的层、修改播放速度以及直接控制融合与混合。

动画也支持枚举，所以你可以像这样在AnimationStates之间循环：

```javascript
using UnityEngine;
    using System.Collections;
 
    public class ExampleClass : MonoBehaviour {
        public Animation anim;
        void Start() {
            anim = GetComponent<Animation>();
            foreach (AnimationState state in anim) {
                state.speed = 0.5F;
            }
        }
    }
```


|变量|说明|
|:--|:--|
|animatePhysics|如果打开这个选项，动画会在物理循环过程中被执行。这个选项只有在结合运动学刚体的时候才有用。|
|clip|默认的动画。|
|cullingType|控制动画组件的剔除类型。|
|isPlaying|有任意动画正播放？|
|localBounds|在本地坐标空间这个动画的动画组件的AABB。|
|playAutomatically|在启动的时候是否自动播放默认动画剪辑（Animation.clip）。|
|this[string]|返回指定名称的动画状态。|
|wrapMode|动画剪辑播放完成之后，应该如何操作？|


|公共函数|说明|
|:--|:--|
|AddClip|添加一个指定名称的动画剪辑。|
|Blend|在接下来的几秒内混合命名为animation的动画到targetWeight。|
|CrossFade|在一定时间内淡入名称为name的动画并且淡出其他动画。|
|CrossFadeQueued|在前一个动画播放完成之后淡入淡出下一个动画。|
|GetClipCount|取得该动画的动画剪辑数量。|
|IsPlaying|名为name的动画正在播放吗？|
|Play|不带有任何混合的播放动画。|
|PlayQueued|在前一个动画播放完成之后直接播放下一个动画。|
|RemoveClip|从动画列表移除剪辑。|
|Rewind|倒回名称为name的动画。|
|Sample|在当前状态对动画进行采样。|
|Stop|停止所有当前Animation正在播放的动画。|












