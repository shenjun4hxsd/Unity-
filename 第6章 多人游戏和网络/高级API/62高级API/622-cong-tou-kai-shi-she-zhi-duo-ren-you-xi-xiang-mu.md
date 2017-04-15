##从头开始设置多人游戏项目

本文档描述了使用新的网络系统设置新的多人项目的步骤。这个分步过程是通用的，但是可以为许多类型的多人游戏开始定制。

要开始，创建一个新的空的Unity项目。

###1、NetworkManager设置

第一步是在项目中创建一个`NetworkManager`对象：

&emsp;&emsp;• 创建一个空物体并添加NetworkManager组件。该组件管理游戏的网络状态。

&emsp;&emsp;• 继续添加NetworkManagerHUD组件。该组件在您的游戏中提供了一个简单的用户界面来控制网络状态。


###2、设置玩家预制体

下一步是设置代表游戏中玩家的预制体。默认情况下，`NetworkManager`通过克隆玩家预制体来为每个玩家实例化一个对象。在这个例子中，玩家对象将是一个简单的立方体。

&emsp;&emsp;• 给玩家的预制体添加NetworkIdentity组件。此组件用于标识服务器和客户端之间的对象。

&emsp;&emsp;• 将`NetworkIdentity`上的“`Local Player Authority`”复选框设置为`true`。这将允许客户端控制玩家的移动。

###3、注册玩家预制体

一旦玩家预制体已创建，它必须向网络系统注册。

&emsp;&emsp;• 将`NetworkManager`组件上的`Spawn Info`中的`Player Prefab`设置为玩家预制体。