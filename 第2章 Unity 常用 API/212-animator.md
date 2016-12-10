##Animator 动画器

控制Mecanim动画系统的接口。


|变量|说明|
|:--|:--|
|angularVelocity|获得上一帧计算的Avatar角速度。|
|applyRootMotion|是否应用根运动？|
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
|isHuman|如果当前的绑定是人形的，返回真；如果是generic（普通）的则返回假。|
|isMatchingTarget|如果自动匹配是激活的。|
|isOptimizable|如果当前的绑定是AnimatorUtility.OptimizeTransformHierarchy优化的。|
|layerCount|该动画器控制器的层数。|
|leftFeetBottomHeight|获得左脚底的高度。|
|linearVelocityBlending|当linearVelocityBlending 设置为true，根运动速度和角速度将线性混合。|
|parameters|用于动画器只读访问动画器控制器参数。|
|pivotPosition|获得当前轴心点的位置 。|
|pivotWeight|获得轴心点的权重。|
|playbackTime|在记录缓冲区设置播放位置。|
|recorderMode|获取动画器录制的模式。|
|recorderStartTime|缓冲区第一帧的开始时间，相对于StartRecording被调用时的帧。|