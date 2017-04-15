##从头开始设置多人游戏项目

本文档描述了使用新的网络系统设置新的多人项目的步骤。这个分步过程是通用的，但是可以为许多类型的多人游戏开始定制。

要开始，创建一个新的空的Unity项目。

###1、NetworkManager设置

第一步是在项目中创建一个`NetworkManager`对象：

&emsp;&emsp;• 创建一个空物体并添加NetworkManager组件。该组件管理游戏的网络状态。

&emsp;&emsp;• 继续添加NetworkManagerHUD组件。该组件在您的游戏中提供了一个简单的用户界面来控制网络状态。