##Animation.PlayQueued 播放队列

**public AnimationState PlayQueued(string animation, QueueMode queue = QueueMode.CompleteOthers, PlayMode mode = PlayMode.StopSameLayer);**

描述 ：

在前一个动画播放完成之后直接播放下一个动画。

比如你可能会一个接一个播放一个特殊序列的动画。

动画状态在播放之前会复制自己，因此你可以在相同的动画之间进行淡入淡出。这个可以用于覆盖两个相同的动画。比如你可能有一个剑挥舞的动画，玩家连续快速削两次。你可以倒播这个动画然后从头再播放但是你会看到动画之间有跳越现象。