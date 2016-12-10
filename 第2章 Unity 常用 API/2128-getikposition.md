##Animator.GetIKPosition 获取IK位置

**Vector3 GetIKPosition(AvatarIKGoal goal);**

**描述：**

获取IK目标的位置。

IK goal是指定身体部分的目标旋转和位置。Unity可以计算怎么移动该部分从开始点向目标（如，从动画获得的当前位置和旋转）。

此函数获取世界坐标指定目标的当前位置。

---

##AvatarIKGoal 骨骼IK目标

用于设置和获取IK权重，位置和旋转。请参考： Animator.SetIKPosition, Animator.SetIKPositionWeight, Animator.SetIKRotation and Animator.SetIKRotationWeight.


|枚举项|说明|
|:--|:--|
|LeftFoot|左脚。|
|RightFoot|右脚。|
|LeftHand|左手。|
|RightHand|右手。|

🔚





