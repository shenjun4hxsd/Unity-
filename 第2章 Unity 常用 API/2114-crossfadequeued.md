##Animation.CrossFadeQueued

**public AnimationState CrossFadeQueued(string animation, float fadeLength = 0.3F, QueueMode queue = QueueMode.CompleteOthers, PlayMode mode = PlayMode.StopSameLayer);**

描述 ：

在前一个动画播放完成之后淡入淡出下一个动画。

例如你可以播放一个特定的动画序列。

动画在播放前复制自身，因此你可以在相同的动画间渐变，这可用来重叠两个相同的动画。例如你可能有一个挥剑的动画，玩家快速挥动了2次，你可以回放这个动画并从开始播放它，但会跳帧。


**queue modes：** 

如果queue为QueueMode.CompleteOthers这个动画只在所有其他动画都停止播放时才开始。
如果queue为QueueMode.PlayNow这个动画将以一个复制的动画状态立即开始播放。