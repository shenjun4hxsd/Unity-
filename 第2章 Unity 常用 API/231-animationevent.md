##AnimationEvent 动画事件

描述：

AnimationEvent类似于SendMessage让你调用一个脚本函数，这个脚本是动画播放的一部分

动画事件函数支持0参数或一个参数，参数可以是浮点型，字符串，object或AnimationEvent。


|变量|说明|
|:--|:--|
|animationState|该事件引发的动画状态（只读）。|
|animatorClipInfo|与此事件有关的动画器剪辑信息（只读）。|
|animatorStateInfo|有关该事件的动画器状态信息（只读）。|
|floatParameter|浮点型参数，储存在该事件中并发送给函数。|
|functionName|被调用的函数的名称。|
|intParameter|整数型参数，储存在事件中并发送给函数。|
|isFiredByAnimator|如果该动画事件已经由Animator组件触发，返回true。|
|isFiredByLegacy|如果该动画事件已经由Animation组件触发，返回true。|
|messageOptions|函数调用选项。|
|objectReferenceParameter|储存在事件中的引用对象参数，并发送给函数。|
|stringParameter|储存在该事件中的字符串参数，并发送给函数。|
|time|引发该事件的时间点。|