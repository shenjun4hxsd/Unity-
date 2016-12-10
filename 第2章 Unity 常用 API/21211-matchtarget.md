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