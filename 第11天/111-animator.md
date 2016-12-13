##Animator

**新动画系统**

    Rig标签：
        Generic
        Humanoid

    必须给它传递Avatar才可以工作。


**支持 Avatar **
```
    Avatar : Unity提供的简化的基本骨骼信息，与模型的骨骼信息匹配，匹配成功后，会生成一个Avatar文件。

    可以实现动画的重定向。
    可以借助AvatarMask文件的支持实现动画分层融合显示。
    可以实现Humanoid中的骨骼IK（反向运动学）。
    可以通过参数传递切换动画状态。
```

**Generic（通用动画）：**
```
    可以使用可视化编辑的动画状态机。
    缺少动画层显示控制、骨骼的动画遮罩、骨骼的IK。
```

**Humanoid（人型动画）：**
```
    要有CS骨骼，而且能形成TPos。
    需要Avatar支持。

    可以使用可视化编辑的动画状态机。
    具有动画层次控制、可以使用骨骼遮罩实现不同部位的动画、实现骨骼IK运动。
```

**RuntimeAnimationController**
```
    用来记录状态机。
    在记录状态机时他也记录了状态机切换使用的参数。

    float
    int
    bool
    Trigger(会自动复位的bool)

    继承自UnityEngine.Object，所以他需要使用数据装载函数来实现创建。
    注意：它其实就是一个文件的载体，什么功能都没有。
```

####面板属性

|属性|说明|
|:--|:--|
|Controller|需要AnimatorControll文件。|
|Avatar|和AnimatiorController文件有关的Avatar文件|
|Apply Root Motion|根节点的运动变换其实就是整个物体运动变换通过 Y 轴垂直在水平面上的一个投影。根节点的运动变换在动画的每一帧中都会进行计算。计算出来的根节点变换结果都会应用在播放动画的对象上，让该对象按照根节点的运动变换进行移动。RootMotion 就是作用于动画物体在 X 轴和 Z 轴上的位移的，而且这个位移是根据实际播放的动画中每一帧物体的位移在 X 和 Z 轴上投影计算出来的。|
|Update Mode|设定动画播放时代更新状态，如果没有特殊需要的我们使用Normal。|
|&emsp;&emsp;Normal|和引擎渲染保持同步帧。|
|&emsp;&emsp;Animate Physic|使用物理动画的播放帧。|
|&emsp;&emsp;Unscaled Time|使用原始帧率播放。|
|Culling Mode|动画在什么状态下播放。|
|&emsp;&emsp;Based On Renderer|当摄像机看见时才播放|
|&emsp;&emsp;Always Animate|不管什么状态都播放动画。|



 Animation Import Settings 中『Animations』Tab 页中各项设置的作用。

|选项|说明|
|:--|:--|
|Import Animation|勾选这个才可以导入动画到 Unity 工程中。|
|Bake Animations|这个选项只在使用 Humanoid 动画并且使用到了 IK 特性的时候才可用。|
|Anim.Compression|这个是关于动画压缩选项的，默认会选择 Keyframe Reduction 这个是『压缩关键帧』，就是 Unity 会自行重采样动画的关键帧，还有两个选项『Off 和 Optimal』，一个是关闭动画压缩，一个是最优化压缩（应该是压缩效率最高，动画效果失真度可能也较高）。|
||选择了 Keyframe Reduction 或者 Optimal 压缩选项，就会有三个用于控制压缩选项的系数配置， Rotation Error，Position Error 和 Scale Error，这个三个参数默认都是 0.5，越小呢精度就越高也就是说动画的失真度越小。|
|Clips|这个下面列出了这个 FBX 文件下包含的所有动画，我们在默认的动画文件基础上新建和删除动画片段 (Animation Clip)，当然每个动画片段都是可以指定起始帧和结束帧的； 以下的设置都是针对单个动画片段|
|Loop Time|勾选这个选项之后，如果 Animator 处于播放这个动画状态时，在播放完第一遍这个动画片段之后，会自动循环从起始帧再次开始播放动画，如此循环往复。如果我们不勾选这个选项，例如 Animator 一直处于播放这个动画的状态，那么动画会定格在动画的结束帧，直到我们通过 Animator 切换这个 Animator 状态机的状态，切换到其他的动画|
|&emsp;&emsp;Loop Pose|用于控制动画循环播放时，从结束帧切换到起始帧时，动画的动作可以无缝的衔接上。|
|&emsp;&emsp;Cycle Offset|用于控制循环的时候起始帧偏移。|
|Root Transform Rotation|根节点的旋转信息。|
|&emsp;&emsp;Bake Into Pose|勾选后会将根节点每一帧的旋转方向信息烘焙到动画的骨骼运动中，在整个动画播放的过程中，根节点的旋转信息就不会在通过 Root Motion 作用到播放该动画的 GameObject 上了，这就意味着这个动画播放的过程中，该物体的 Transform 中的 Rotation 值不会因为动画中物体做了任何旋转而发生改变，而是会保持一个恒定的值，和该动画播放之前的旋转值保持一致。|
|&emsp;&emsp;Based Upon (at Start) 或者 Based Upon|根节点旋转的参考基准，有两个选项『Original 和 Root Node Rotation』这两个分别指的是动画文件中指定的旋转值和根节点旋转信息，其实我更愿意将 Original 理解为动画中原点的旋转值，因为在整个动画播放的过程中，所有骨骼肯定都会有旋转和位移的变换，但是动画的原点其实一定都是确定的，这样理解感觉更简单也更形象一些，勾选了 Bake Into Pose 之后，就会变成 Based Upon 而不勾选 Bake Into Pose 就会保持为 Based Upon (at Start)。|
|&emsp;&emsp;Offset|旋转角度与参考基准的偏移（以度为单位）。|
|Root Transform Position(Y)|根节点位移信息（Y 轴）。|
|&emsp;&emsp;Bake Into Pose|勾选后会将根节点每一帧在垂直 Y 轴方向上的运动信息烘焙到动画的骨骼运动中，在整个动画播放的过程中，根节点在 Y 轴方向的所有位移信息不会通过 Root Motion 作用到播放该动画的 GameObject 上，这就意味着我们在场景中看到物体在 Y 轴上有位移，例如向上或者向下移动，但是该物体的 Transform 中的 Position 信息不会发生改变，会跟动画播放之前的 Position 信息保持一致。|
|&emsp;&emsp;Based Upon 或者 Based Upon (at Start)|这个貌似有点不一样哦，在选中 Bake Into Pose 之后会变成 Based Upon (at Start)，不勾选的时候是 Based Upon，不过这个就能理解了。不烘焙的话，那么 Root Motion 中 Y 轴的变化就依赖于选择的『Original 或者 Root Node Position』的 Y 轴位移变化，如果选择烘焙的话，那么就以这个动画的起始帧的 Y 轴作为整个动画 Root Motion 的 Y 轴位移，在整个动画播放的过程中，Y 轴的位移都是恒定不变的。|
|&emsp;&emsp;Offset|垂直方向上的偏移。|
|Root Transform Position(XZ)|根节点位移信息（水平面，XZ 轴）|
|&emsp;&emsp;Bake Into Pose|勾选后会将根节点每一帧在水平面（X 和 Z 轴）方向上的运动信息烘焙到动画的骨骼运动中，在整个动画播放的过程中，根节点在 X 和 Z 轴方向的所有位移信息不会通过 Root Motion 作用到播放该动画的 GameObject 上，这就意味着我们在场景中看到物体在水平面上移动，但是该物体的 Transform 中的 Position 信息不会发生改变，会跟动画播放之前的 Position 信息保持一致，假如动画中物体会向前移动 3 米，我们会看到物体在整个动画播放过程中确实在向前移动，播放到最后一帧时确实向前移动了 3 米，但是当这个动画播放完毕之后，切换到任何其他的动画时，物体会直接闪回这个动画播放前物体所在的位置，所以通常我们需要保留动作位移的动画都不会勾选这个选项。那这个选项有神马用捏？例如某些待机动画，我们其实希望物体只是做一个待机动作，但是实际上不想让物体在水平方向上有位移，这个时候就可以勾选这个选项了，到时候看起来物体就像是钉在水平面上了。|
|Mask|这个掩码主要是用于控制动画播放过程中，各个骨骼之间的运动变换的。|
|&emsp;&emsp;Definition|可以选择从动画文件创建也可以选择使用其他动画文件中已经创建好的配置。|
|&emsp;&emsp;Transform|这个就是动画文件中所有骨骼的层级关系，可以选择勾选那些需要应用动画中运动变换的骨骼。|
|Curves|这个主要用于设置某些跟动画相关的参数用，例如控制整个动画播放过程中的速度参数之类的，在动画播放的过程中可以通过 Animator.GetFloat(ParamName) 函数来读取曲线的值，曲线的 X 轴为动画的时间轴，Y 轴为曲线的值，曲线可以通过曲线编辑器进行增加关键点，调整曲线斜率进行编辑，读取时默认会根据当前动画播放的进度作为 X 轴的值进行读取，一个动画片段可以有多个曲线。|
|Events|这个是用于在动画播放的过程中触发事件的，例如整个动画中有起跳和落地两个事件需要在准确的时间点触发并通知到游戏中其他的对象，那么就可以在 Events 时间轴上新增事件通知，设置好触发的方法名称和参数，在播放该动画的 GameObject 上确保有某个脚本中有与该事件通知的方法签名一致的方法就好了，当动画播放到触发通知时间时，就会向 GameObject 广播该时间通知，脚本中方法签名一致的方法就会被回调了，那我们就可以做我们需要做的事情了。|



🔚



