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