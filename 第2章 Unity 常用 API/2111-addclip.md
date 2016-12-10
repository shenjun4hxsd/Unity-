##Animation.AddClip 添加剪辑

**public void AddClip(AnimationClip clip, string newName);**

描述 ： 添加一个指定名称的动画剪辑。

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


**public void AddClip(AnimationClip clip, string newName, int firstFrame, int lastFrame, bool addLoopFrame = false);**

**参数 ： addLoopFrame**

是否应该在最后插入一个额外的帧匹配第一帧？如果你在制作一个循环的动画，那么可以打开这个选项。

描述 : 在firstFrame和lastFrame之间添加动画剪辑，新的动画剪辑也会被添加到名称为newName的动画中。如果已存在具有相同名称的剪辑，会被替换为新剪辑。

```javascript
using UnityEngine;
using System.Collections;
 
public class ExampleClass : MonoBehaviour {
    public Animation anim;
    void Start() {
        anim = GetComponent<Animation>();
        anim.AddClip(anim.clip, "shoot", 0, 10);
        anim.AddClip(anim.clip, "walk", 11, 20, true);
        anim.AddClip(anim.clip, "idle", 21, 30, true);
    }
}
```