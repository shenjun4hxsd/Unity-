##AnimationClip.SampleAnimation 采样动画

**public void SampleAnimation(GameObject go, float time);**

|参数|说明|
|:--|:--|
|go|动画的游戏对象。|
|time|采样动画时间。|

描述 ：

在给定的时间采样动画，为任意动画的属性。

建议使用动画界面，而不是出于性能的考虑。这回在给定的时间采样动画。在剪辑任意动画的组件属性将被替换为采样值。大部分时间你可以使用Animation.Play替代。SampleAnimation用于当你需要在一个无序的方式帧之间跳跃，或基于某些特殊的输入。