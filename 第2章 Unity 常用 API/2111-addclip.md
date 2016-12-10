##Animation.AddClip 添加剪辑

**public void AddClip(AnimationClip clip, string newName);**

添加一个指定名称的动画剪辑。

```javascript
using UnityEngine;
using System.Collections;
 
public class ExampleClass : MonoBehaviour {
    public AnimationClip walkClip;
    public Animation anim;
    void Start() {
        anim = GetComponent<Animation>();
        anim.AddClip(walkClip, "walk");
    }
}
```