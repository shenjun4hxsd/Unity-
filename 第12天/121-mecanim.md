##Retarging 人形动画的重定向

人形动画的重定向是Mecanim系统中最强大的功能之一，这意味着用户可以通过简单的操作将一组动画应用到各种各样的人形角色模型上。特别地，重定向只能应用于人形模型，在此情况下，为了保证模型间骨骼结构的对应关系，必须正确匹配Avatar。以下介绍推荐的层次结构。

1、当使用Mecanim动画系统时，场景中应包含以下元素：

    导入的角色模型，其中包含一个Avatar。
    Animator组件，其中引用了一个Animator Controller资源。
    一组被Animator Controller 引用的动画片段。
    用于角色动画的脚本。
    角色相关组件，比如Character Controller等。

2、项目中还应该含有另一个具有有效Avatar的角色模型。

##Sub-State Machine 子状态机

##BlendTree 1D