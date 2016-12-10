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