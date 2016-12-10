##AnimationClip.SetCurve 设置曲线

**public void SetCurve(string relativePath, Type type, string propertyName, AnimationCurve curve);**

|参数|说明|
|:--|:--|
|relativePath|应用给该曲线的游戏物体的路径。relativePath被格式化类似路径，如“root/spine/leftArm”。如果relativePath为空，表示动画剪辑附加的游戏物体。|
|type|被动画的组件的类类型|
|propertyName|被动画的属性的名字或路径|
|curve|动画曲线|

描述 ：

指定动画曲线来动画特定的属性。

如果曲线为null，曲线将被移除，如果曲线属性已经存在，曲线将被替换。

通常的名称是: “localPosition.x”, “localPosition.y”, “localPosition.z”, “localRotation.x”, “localRotation.y”, “localRotation.z”, “localRotation.w” “localScale.x”, “localScale.y”, “localScale.z”.


```javascript
using UnityEngine;
using System.Collections;
 
[RequireComponent(typeof(Animation))]
public class ExampleClass : MonoBehaviour {
    public Animation anim;
    void Start() {
        anim = GetComponent<Animation>();
        AnimationCurve curve = AnimationCurve.Linear(0, 1, 2, 3);
        AnimationClip clip = new AnimationClip();
        clip.legacy = true;
        clip.SetCurve("", typeof(Transform), "localPosition.x", curve);
        anim.AddClip(clip, "test");
        anim.Play("test");
    }
}
```

---

Material材质属性可以使用shader输出的属性名称制作动画。通常使用的名称是： “_MainTex”, “_BumpMap”, “_Color”, “_SpecColor”, “_Emission”。如何动画化不同材质属性类型：

Float属性： “PropertyName” 
Vector4 属性： “PropertyName.x”, “PropertyName.y”, “PropertyName.z”, “PropertyName.w” 
Color 属性： “PropertyName.r”, “PropertyName.g”, “PropertyName.b”, “PropertyName.a” 
UV 旋转属性：“PropertyName.rotation” UV 偏移和缩放： “PropertyName.offset.x”, “PropertyName.offset.y”, “PropertyName.scale.x”, “PropertyName.scale.y” 
对于在同一renderer的多个索引材质，你能想这样添加前缀：“[1]._MainTex.offset.y”