##Animation

####&emsp;&emsp;简介
```
    主要是操作帧序列的动画系统
    Animation控制的是旧动画系统
    Animation就是一个组件
        旧动画系统的组件
        属于新动画系统的子集
        Rig标签下选择Legacy
```

####&emsp;&emsp;面板参数

|参数|说明|
|:--|:--|
|Animation|默认播放的动画片段|
|Animations|所有可以播放的动画片段|
|Play Automaticlly|勾选以后，游戏开始会自动播放Animation中指定的动画片段|
|Animate Physics|是有参与物理运动|
|Cull Type|什么时候渲染|
|&emsp;&emsp;Allways|总是渲染|
|&emsp;&emsp;Based On Renderers|摄像机能看到的时候渲染|


####&emsp;&emsp;辅助类

&emsp;&emsp;&emsp;&emsp;**AnimationClip**
```
        提供Aniamtion组件动画的基础类
        他也有自己辅助函数
             AnimationEvent（就是当前动画切片提供我们的动画事件）

```

&emsp;&emsp;&emsp;&emsp;**AnimationState**
```
        用于监视当前Animation播放的动画切片的状态

        功能
            name（动画片段的名字）
            clip
                动画片段
                AnimationEvent pEvent = new AnimationEvent();
                pEvent.name = "Fun";
                pEvent.time = 0.5f;
            time（返回当前动作切片被播放了多久，单位是秒）
                time/length（计算出动画片段播放了几遍）
            normalizedTime
                返回当前动作切片被播放了多久
                单位是总时间的百分比
            length
                返回当前切片的总时间长度，单位是秒
            speed
                可以获取或者设置当前切片的播放速度
                > 1（加速播放）
                0～1（减速播放）
            normalizedSpeed
                可以获取或者设置当前切片的播放速度（百分比制）
            wrapMode
                获取或设置当前切片的循环状态
                    Default
                    Once（只播放一次）
                    Loop（循环播放）
                    PingPang
                    ClampForver（播放到最后一帧的时候，会循环播放最后一帧）
```

####属性
```
    clip
        用于确定默认的播放动作切片
        注意：这个切片必须存在于Animation组件的动画切片列表中
    animatePhysics
        如果为真表示可以支持当前动画切片的物理动作
        注意：美术必须能做到利用根骨骼驱动模型才有用
    cullingType
        用于确定动画的播放特性
            可以在任何状态下都播放动画
            可以在摄像机看见他的时候才播放动画
    playAutomatically
        如果为真表示Awake执行后就会自动开发播放clip属性的动画
    wrapMode
        全局设定所有动画切片播放的循环特性
        在代码中他返回的是当前正在播放的动画切片的循环特性
    localBounds
        返回本地每个骨骼的大小
    isPlaying
        返回当前是否在播放我们的动画
```

####方法
```
    索引
        通过动画切片的名字返回这个动画切片的AnimationState
        可以使用这个方法来监视我们的动画播放情况
    AddClip
        用于向Animation组件添加动画切片
        注意：这些切片一定要来自于统一的FBX文件，除非是你自己做的动画可以不统一
    Blend
        用于制定一个动画切片名字后设定这个动画切片的显示权重
        显示权重：权重值越高越会覆盖别的动画
    GetClip
        通过切片名返回动画切片对象
    GetClipCount
        返回Animation组件中有多少个动画切片
    RemoveClip
        制定动画切片名，删除对应的动画切片
    Play
        指定动画切片名播放动画，如果没有动画名他会自动播放clip属性中的动画
        举例：
             bool Play("动画片段的名称");
             bool Play("动画片段的名称", PlayMode.StopAll);
             // PlayMode 枚举(StopAll、StopSameLayer)

    PlayQueued
        设定播放序列，你可以在正在播放的时候调用指定当前动画播放完后再继续播放你指定的动画
        注意：他适用于warpMode是Once的动画，循环动画就废了
    Rewind
        将指定的动画反过来放
        注意：他和warpMode的PingPong模式类似，但是PingPong会一直播放下去，而Rwind他是来回走一次结束播放
    Stop
        终止指定的动画
    CrossFade
        混合播放，预留之前动画的一部分和将要播放的动画混合后再播放，有播放过渡的意思
        举例：
            void CrossFade( "动画片段的名称" , int fadeLength, PlayMode.StopSameLayer );
            // fadeLength （与之前的动画片段融合的百分比）

    CrossFadeQueued
        动画过渡播放，的序列设定
        举例：
             AnimationState CrossFadeQueued( "动画片段的名称"， int fadeLength, QueuedMode.CompleteOthers );
             // QueueMode(CompleteOthers、PlayNow)

    AnimationState this[string name];
```

####切片的装载
```
    动画切片不能独立装载（除非你装载自定义的动画切片可以独立装载）
    对于FBX文件你必须将他底下存在所有动画切片一次性装回来
    对于FBX文件我们使用Resoures.LoadAll（路径文件是FBX的文件名）
    Animation必须支持的是旧动画系统。你不要让他支持新动画系统
```

####动画事件
```
   简介
       我们可以为每个动画切片添加一些根据播放时间来确定的方法，让动画播放到我们指定的时间时激活这个方法
       为了给一些动画追加特效

    AnimationEvent
        这个事件类不是组件，所以我们需要自己实例化（他只能给AnimationClip类添加）
    functionName
        我们可以给这个事件添加一个需要执行的方法的名字，字符串型的
        注意：事件的函数体不是说非要在我们的注册脚本中，只要是在GameObject中存在这么一个函数就行了，而且不理会他的修辞
    messageOptions
        设定当前事件对于注册方法的理会问题
        RequireReceiver（表示必须理会这注册函数，如果没有对应的函数体在游戏个体上不存在就报错）
        DontRequireReceiver（不管注册的事件函数是否存在，不存在也不报错，存在就执行）
    time
        用于设定当前事件在对应的AnimtionClip中执行时间点
        注意：时间一定要在当前切片的时间范围内

事件执行函数的参数

    类型（int、float、string、UnityEngine.Object）
    定义
        对于这个参数你只能定义一个，意识就是上述类型中你自己选一个类型
        注意：Object类型指的的是Unity引擎的任意类型，不是C#语言的任意类型
    获取
        你定义函数实现时直接写上对应类型的行参就可以获取
    传递
        在创建事件对象时就需要传递好实参内容，Animation组件会自动返回你传递好的参数

注意
    事件不要重复，重复就会多次执行
    事件必须添加到AnimationClip中
    我们可以通过AnimationClip返回对应的事件
    事件在没有什么用的时候不要去添加
    你可以使用AnimationState监视动画的播放情况然后执行对应函数就好了，这样可以节省效率

```
