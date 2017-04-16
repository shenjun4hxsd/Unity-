##Animation.CrossFade 淡入淡出

**public void CrossFade(string animation, float fadeLength = 0.3F, PlayMode mode = PlayMode.StopSameLayer);**

描述 ： 

● 在一定时间内淡入名称为name的动画并且淡出其他动画。

● 如果模式是PlayMode.StopSameLayer，在同一层的动画将在动画淡入的时候淡出。如果模式是PlayMode.StopAll，所有动画将在淡入的时候淡出。

● 如果动画没有被设置成循环，它将停止并且在播放完成之后倒带至开始。

```csharp
    using UnityEngine;
    using System.Collections;
     
    public class ExampleClass : MonoBehaviour {
        public Animation anim;
        void Start() {
            anim = GetComponent<Animation>();
        }
        void Update() {
            if (Mathf.Abs(Input.GetAxis("Vertical")) > 0.1F)
                anim.CrossFade("Run");
            else
                anim.CrossFade("Idle");
        }
    }
```

🔚