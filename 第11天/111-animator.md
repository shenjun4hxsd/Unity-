##Animator

新动画系统

    Rig标签：

        Generic
        Humanoid


支持 Avatar 

    Avatar : Unity提供的简化的基本骨骼信息，与模型的骨骼信息匹配，匹配成功后，会生成一个Avatar文件。

    可以实现动画的重定向
    可以借助AvatarMask文件的支持实现动画分层融合显示。
    可以实现Humanoid中的骨骼IK（反向运动学）
    可以通过参数传递切换动画状态


RuntimeAnimationController

    用来记录状态机。
    在记录状态机时他也记录了状态机切换使用的参数

    float
    int
    bool
    Trigger(会自动复位的bool)

    继承自UnityEngine.Object，所以他需要使用数据装载函数来实现创建
    注意：它其实就是一个文件的载体，什么功能都没有
