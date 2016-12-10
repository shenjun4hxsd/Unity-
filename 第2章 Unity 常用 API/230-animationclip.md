##AnimationClip 动画剪辑

描述 ：

储存基于关键帧的动画。

AnimationClip是用于Animation来播放动画。

|变量|说明|
|:--|:--|
|events|用于该剪辑的动画事件。|
|frameRate|被取样关键帧的帧速率。（只读）|
|humanMotion|如果该动画包含的曲线是人类骨骼，返回true。|
|legacy|如果动画剪辑被使用于旧版Animation组件，返回true。|
|length|动画长度，单位秒。（只读）|
|localBounds|动画组件在本地坐标空间这个动画剪辑的AABB也被附加。|
|wrapMode|在动画状态设置使用的默认循环模式。|


|公共函数|说明|
|:--|:--|
|AddEvent|给剪辑添加动画事件。|
|ClearCurves|从该剪辑清除所有曲线。|
|EnsureQuaternionContinuity|为了保证更好的四元数插值，你设置完成动画曲线后调用这个函数。该函数重新调整四元数键来确保最短路径插补。|
|SampleAnimation|在给定的时间采样动画，为任意动画的属性。|
|SetCurve|指定动画曲线来动画特定的属性。|