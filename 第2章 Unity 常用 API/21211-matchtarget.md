##Animator.MatchTarget 匹配目标

**void MatchTarget(Vector3 matchPosition, Quaternion matchRotation, AvatarTarget targetBodyPart, MatchTargetWeightMask weightMask, float startNormalizedTime, float targetNormalizedTime = 1);**


|参数|说明|
|:--|:--|
|matchPosition|我们想要身体部件到达的位置。|
|matchRotation|我们想要身体部件到达的旋转。|
|targetBodyPart|匹配所涉及的身体部件。|
|weightMask|匹配位置和旋转包含的权重。|
|startNormalizedTime|此动画剪辑的开始时间（0为剪辑开始，1为结束）。|
|targetNormalizedTime|此动画剪辑的开始时间（0为剪辑开始，1为结束），值大于1时表示若干循环后，设置触发匹配。如：2.3表示在第二次循环后的30%。|


**描述：**

自动调节该对象的位置和旋转，因此，在当前状态是在指定的进度时，AvatarTarget达到matchPosition。

目标匹配仅工作于基本层（索引为0）。




