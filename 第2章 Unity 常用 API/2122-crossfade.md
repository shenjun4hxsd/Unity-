##Animator.CrossFade 淡入淡出

**void CrossFade(string stateName, float transitionDuration, int layer = -1, float normalizedTime = float.NegativeInfinity);**

**void CrossFade(int stateNameHash, float transitionDuration, int layer = -1, float normalizedTime = float.NegativeInfinity); **


|参数|说明|
|:--|:--|
|stateName|目标状态的名字。|
|transitionDuration|过渡的持续时间。该值是源状态的归一化时间。|
|normalizedTime|当前目标状态的开始时间，该值是源状态的归一化时间，值在0~1之间。|
|layer|包含目标状态的层索引。|
|stateNameHash|目标状态的状态哈希。|