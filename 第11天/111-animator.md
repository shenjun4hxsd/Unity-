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





🔚



