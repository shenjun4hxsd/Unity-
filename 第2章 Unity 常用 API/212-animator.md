##Animator 动画器

控制Mecanim动画系统的接口。


|变量|说明|
|:--|:--|
|angularVelocity|获得上一帧计算的Avatar角速度。|
|**applyRootMotion**|是否应用根运动？|
|avatar|获取或设置当前的Avatar。|
|bodyPosition|身体质量中心的位置。|
|bodyRotation|该身体质量中心的旋转。|
|cullingMode|控制动画器组件的剔除模式。|
|deltaPosition|获取上一帧Avatar的增量位置。|
|deltaRotation|获取上一帧Avatar的增量旋转。|
|feetPivotActive|混合身体质量中心与脚质量中心的轴心点。0%表示混合轴心点在身体的质量中心；100%表示混合轴心点在脚的轴心点；|
|gravityWeight|基于当前播放的动画的当前重力。|
|hasRootMotion|如果当前绑定已经是根运动，返回真。|
|hasTransformHierarchy|如果该对象已经是变换层级，返回真。|
|humanScale|返回当前人形绑定Avatar的缩放，（如果绑定是generic，默认为1）|
|**isHuman**|如果当前的绑定是人形的，返回真；如果是generic（普通）的则返回假。|
|isMatchingTarget|如果自动匹配是激活的。|
|isOptimizable|如果当前的绑定是AnimatorUtility.OptimizeTransformHierarchy优化的。|
|**layerCount**|该动画器控制器的层数。|
|leftFeetBottomHeight|获得左脚底的高度。|
|linearVelocityBlending|当linearVelocityBlending 设置为true，根运动速度和角速度将线性混合。|
|parameters|用于动画器只读访问动画器控制器参数。|
|pivotPosition|获得当前轴心点的位置 。|
|pivotWeight|获得轴心点的权重。|
|playbackTime|在记录缓冲区设置播放位置。|
|recorderMode|获取动画器录制的模式。|
|recorderStartTime|缓冲区第一帧的开始时间，相对于StartRecording被调用时的帧。|
|recorderStopTime|记录剪辑的结束时间，相对于StartRecording被调用的帧。|
|rightFeetBottomHeight|获取右脚底的高度。|
|rootPosition|根位置，该游戏对象的位置。|
|rootRotation|根旋转，该游戏对象的旋转角度。|
|**runtimeAnimatorController**|AnimatorController的运行时代表，用来控制Animator。|
|**speed**|动画器的播放速度。1表示正常播放速度。|
|stabilizeFeet|过渡和混合时，脚自动稳定。|
|targetPosition|返回由SetTarget(AvatarTarget targetIndex, float targetNormalizedTime))指定的目标的位置。|
|targetRotation|返回由SetTarget(AvatarTarget targetIndex, float targetNormalizedTime))指定的目标的旋转角度。|
|updateMode|该动画器的指定更新模式。|
|velocity|获得上帧计算的Avatar速度。|

&emsp;

|函数|说明|
|:--|:--|
|ApplyBuiltinRootMotion|应用默认的根运动。|
|**CrossFade**|在当前状态和目标状态之间创建一个动态过渡。|
|CrossFadeInFixedTime|与animator.crossfade相同，但在目标状态的持续时间和偏移是在固定的时间。|
|GetAnimatorTransitionInfo|获取指定AnimatorController层的过渡信息。仅在过渡持续期间有效。|
|GetBehaviour|返回匹配类型T或派生自T的首个状态机行为，如果没有找到返回null。|
|GetBehaviours|返回匹配类型T或派生自T的所有状态机行为，如果没有找到返回null。|
|GetBoneTransform|返回人形骨骼ID映射的变换。|
|**GetBool**|获取布尔参数的值。对于Trigger类型参数他也可以获取回来|
|GetCurrentAnimationClipState|获取当前状态播放的当前动画信息列表。|
|**GetCurrentAnimatorStateInfo**|获取指定的AnimatorController层的当前状态信息。|
|**GetFloat**|获取一个浮点数参数的值。|
|GetIKHintPosition|获取世界坐标当前IK hint的位置。|
|GetIKHintPositionWeight|获取IK Hint的过渡的权重。 (0表示IK之前的原始动画，1表示在Hint)。|
|GetIKPosition|获取IK goal的位置。|
|GetIKPositionWeight|获取IK goal的过渡权重(0表示IK之前的原始动画，1表示在goal)。|
|GetIKRotation|获取IK goal的旋转。|
|GetIKRotationWeight|获取IK goal的旋转权重(0表示IK之前的旋转，1表示IK goal的旋转)。|
|**GetInteger**|获取整数参数的值。|
|**GetLayerName**|获取该层的名称。|
|**GetLayerWeight**|获取该层的当前权重。|
|GetNextAnimationClipState|获取当前播放动画信息列表的下个状态。仅在过渡期间有效。|
|GetNextAnimatorStateInfo|获取指定的AnimatorController层上的下个状态信息。仅在过渡期间有效。|
|HasState|如果在动画器控制器是当前动画状态返回true 。|
|InterruptMatchTarget|中断目标自动匹配。|
|**IsInTransition**|指定的AnimatorController层是否在过渡。|
|IsParameterControlledByCurve|如果参数是由动画的附加曲线控制，返回真。|
|MatchTarget|自动调节该对象的位置和旋转，因此，在当前状态是在指定的进度时，AvatarTarget达到matchPosition。|
|**Play**|播放一个状态。|
|PlayInFixedTime|与animator.crossfade相同，但在目标状态的持续时间和偏移是在固定的时间。|
|Rebind|重新绑定动画器的所有动画的属性和网格数据。这个函数用于当你手动通过脚本修改对象的层级时使用，像合并网格或交换一个完整的变换层级。|
|ResetTrigger|重设该触发参数为假。|
|**SetBool**|设置一个布尔参数的值。|
|**SetFloat**|设置一个浮点数的值。|
|SetIKHintPosition|设置一个IK hint的位置。|
|SetIKHintPositionWeight|设置IK hint过渡权重（0表示IK之前的原始动画，1表示在hint）|
|SetIKPosition|设置一个IK goal的位置。|
|SetIKPositionWeight|设置IK goal的过渡权重（0表示IK之前的原始动画，1表示在goal）。|
|SetIKRotation|设置IK goal的旋转。|
|SetIKRotationWeight|设置IK goal的旋转权重（0表示IK之前的旋转，1表示IKgoal旋转）。|
|**SetInteger**|设置一个整数参数的值。|
|**SetLayerWeight**|设置该层的当前权重。|
|SetLookAtPosition|设置注视的位置。|
|SetLookAtWeight|设置注视的权重。|
|SetTarget|设置AvatarTarget和targetNormalizedTime 用于当前状态。|
|**SetTrigger**|设置一个要激活的触发器参数。|
|StartPlayback|在播放模式设置动画。|
|StartRecording|设置动画器的记录模式并分配frameCount大小的圆形缓冲区。|
|StopPlayback|停止动画器播放模式。当播放停止时，Avatar恢复从游戏逻辑获得控制权。|
|StopRecording|停止动画器记录模式。|
|Update|基于deltaTime计算动画器。|

|静态函数|说明|
|:--|:--|
|**StringToHash**|从字符串生成一个参数ID。|


🔚
