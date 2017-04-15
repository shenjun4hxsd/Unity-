##网络系统概念

###1、服务器和主机

在Unity网络系统中，游戏有一个服务器和多个客户端。当没有专用服务器时，其中一个客户端扮演服务器的角色 - 我们把这个客户称为“主机”。

![](/assets/NetworkHost.png)

主机是同一进程中的服务器和客户端。主机使用一种称为`LocalClient`的特殊客户端，而其他客户端则为`RemoteClient`。`LocalClient`通过直接函数调用和消息队列与（本地）服务器进行通信，因为它是在相同的过程中。它实际上与服务器共享场景。`RemoteClients`通过常规网络连接与服务器进行通信。


网络系统的目标之一是将`LocalClients`和`RemoteClients`的代码相同，以便开发人员大多数时候只需要考虑一种类型的客户端。



###2、实例化和生成

在Unity中，`GameObject.Instantiate`创建新的Unity游戏对象。但是，通过网络系统，对象也必须“生成”才能在网络上处于活动状态。这只能在服务器上完成，并导致在连接的客户端上创建对象。产生物体后，生成系统使用分布式对象生命周期管理和状态同步原则。

###3、玩家、本地玩家和权限

在网络系统中，玩家对象是特殊的。有一个玩家对象与每个玩游戏的人相关联，命令被路由到该对象。一个人不能对另一个人的玩家对象调用命令 - 只能是自己的。所以有一个“我的”玩家对象的概念。当添加玩家并且通过连接进行关联时，该玩家对象成为该玩家的客户端上的“本地玩家”对象。有一个属性`isLocalPlayer`设置为true，并且在客户端上的对象上调用的回调`OnStartLocalPlayer()`。下图显示了两个客户及其当地玩家。

![](/assets/NetworkLocalPlayers.png)

只有“你的”玩家对象将设置`isLocalPlayer`标志。这可以用于过滤输入处理，处理相机附件，或做任何其他客户端的事情，只能为您的玩家完成。

除了`isLocalPlayer`之外，玩家对象可以拥有“本地权限”。这意味着其所有者的客户端上的玩家对象对该对象负责 - 它具有权限。这最常用于控制运动，但也可以用于其他的东西。`NetworkTransform`组件了解这一点，如果设置了这一点，将从客户端发送移动。`NetworkIdentity`有一个用于设置`LocalPlayerAuthority`的复选框。

对于非玩家对象（如敌人），没有关联的客户端，所以权限驻留在服务器上。

![](/assets/NetworkAuthority.png)


`NetworkBehaviour`上有一个属性“`hasAuthority`”，可以用来判断对象是否具有权限。因此，非玩家对象在服务器上具有权限，并且具有`localPlayerAuthority`集合的玩家对象对其所有者的客户端具有权限。


###非玩家对象的客户端权限

从Unity版本5.2开始，可以对非玩家对象有客户端权限。有两种方法可以做到这一点。一个是使用`NetworkServer.SpawnWithClientAuthority`产生对象，并传递客户端的网络连接以获取所有权。另一个是使用`NetworkIdentity.AssignClientAuthority`与客户端的网络连接来取得所有权。

向客户端分配权限会导致在对象上的NetworkBehaviours上调用OnStartAuthority（），并且hasAuthority属性将为true。在其他客户端上，hasAuthority属性仍将为false。具有客户授权的非玩家对象可以发送命令，就像玩家可以一样。这些命令在对象的服务器实例上运行，而不是在与连接相关联的播放器上运行。

具有客户端权限的非玩家对象必须在其NetworkIdentity中检查LocalPlayerAuthority。

下面的示例产生一个对象，并将权限分配给生成它的播放器的客户端。















