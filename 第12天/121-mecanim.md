##Retarging 人形动画的重定向

人形动画的重定向是Mecanim系统中最强大的功能之一，这意味着用户可以通过简单的操作将一组动画应用到各种各样的人形角色模型上。特别地，重定向只能应用于人形模型，在此情况下，为了保证模型间骨骼结构的对应关系，必须正确匹配Avatar。以下介绍推荐的层次结构。

1、当使用Mecanim动画系统时，场景中应包含以下元素：

    导入的角色模型，其中包含一个Avatar。
    Animator组件，其中引用了一个Animator Controller资源。
    一组被Animator Controller 引用的动画片段。
    用于角色动画的脚本。
    角色相关组件，比如Character Controller等。

2、项目中还应该含有另一个具有有效Avatar的角色模型。推荐的建立方法：

    建立一个GameObject，可以包含Character Controller组件。
    具有Animator组件的角色模型作为子物体。

3、在另一个模型上重用角色动画，需要：

   关闭原始模型。
   将所需要的模型拖动到GameObject中，使其成为GameObject的另外一个子物体。
   确保新模型上的Animator Controller属性引用了与原始模型相同的Animator Controller资源。

##Sub-State Machine 子状态机

##BlendTree 1D