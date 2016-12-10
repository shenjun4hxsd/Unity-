##Animation.Sample 采样

描述 ：

在当前状态对动画进行采样。

当你明确想设置一些动画状态并且对它取样一次的时候有用。

```javascript
using UnityEngine;
using System.Collections;
 
public class ExampleClass : MonoBehaviour {
    public Animation anim;
    void Start() {
        anim = GetComponent<Animation>();
        anim["MyClip"].time = 2.0F;
        anim["MyClip"].enabled = true;
        anim.Sample();
        anim["MyClip"].enabled = false;
    }
}
```