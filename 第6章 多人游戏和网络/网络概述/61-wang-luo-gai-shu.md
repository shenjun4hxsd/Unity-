##网络概述

网络功能有两种用户：

&emsp;&emsp;• 用Unity制作多人游戏的用户。这些用户应该从`NetworkManager`或`High Level API`开始。
&emsp;&emsp;• 用户构建网络基础设施或高级多人游戏。这些用户应该从`NetworkTransport API`开始。



####高级脚本API


Unity的网络有一个“高级”脚本API（我们将其称为HLAPI）。使用这意味着您可以访问涵盖多用户游戏的大多数常见要求的命令，而无需担心“较低级别”实施细节。HLAPI允许您：