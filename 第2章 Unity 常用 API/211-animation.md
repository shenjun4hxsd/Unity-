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
