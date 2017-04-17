##从头开始设置多人游戏项目

本文档描述了使用新的网络系统设置新的多人项目的步骤。这个分步过程是通用的，但是可以为许多类型的多人游戏开始定制。

要开始，创建一个新的空的Unity项目。

###1、NetworkManager设置

第一步是在项目中创建一个`NetworkManager`对象：

&emsp;&emsp;• 创建一个空物体并添加`NetworkManager`组件。该组件管理游戏的网络状态。

&emsp;&emsp;• 继续添加`NetworkManagerHUD`组件。该组件在您的游戏中提供了一个简单的用户界面来控制网络状态。


###2、设置玩家预制体

下一步是设置代表游戏中玩家的预制体。默认情况下，`NetworkManager`通过克隆玩家预制体来为每个玩家实例化一个对象。在这个例子中，玩家对象将是一个坦克。

&emsp;&emsp;• 给玩家的预制体添加`NetworkIdentity`组件。此组件用于标识服务器和客户端之间的对象。

&emsp;&emsp;• 将`NetworkIdentity`上的“`Local Player Authority`”复选框设置为`true`。这将允许客户端控制玩家的移动。

###3、注册玩家预制体

一旦玩家预制体已创建，它必须向网络系统注册。

&emsp;&emsp;• 将`NetworkManager`组件上的`Spawn Info`中的`Player Prefab`设置为玩家预制体。


&emsp;&emsp;• 可以将当前场景保存为“offlineScene”。


###4、玩家移动（单人版）


第一个游戏功能是移动玩家对象。这将首先在没有任何网络的情况下完成，因此它只能在单人模式下工作。

编写玩家移动脚本：

```csharp
    using UnityEngine;
    
    public class PlayerMove : MonoBehaviour
    {
        void Update()
        {
            var x = Input.GetAxis("Horizontal")*0.1f;
            var z = Input.GetAxis("Vertical")*0.1f;
    
            transform.Translate(x, 0, z);
        }
    }
```

###5、测试托管游戏

运行游戏，此时应该显示NetworkMangerHUD默认用户界面：

![](/assets/LocalConnectionCallBack.png)

&emsp;&emsp;• 选择“LAN Host(H)”。这会创建玩家对象，并且HUD将更改以显示服务器处于活动状态。这个游戏作为一个“主机”运行。（这是同一个进程中一个服务器和一个客户端。）


###6、测试客户端玩家的移动

&emsp;&emsp;• 将当前场景发布成一个可执行程序并运行。

&emsp;&emsp;• 移动玩家。

&emsp;&emsp;• 在编译器中运行游戏。HUD界面选择“LAN Client(C)”作为客户端连接到主机。

&emsp;&emsp;• 此时场景中应该有两个玩家的角色，但是角色移动无法同步，这是因为移动脚本不是网络感知的。


###7、使玩家移动联网

&emsp;&emsp;• 给玩家预设添加`NetworkTransform`组件。该组件使对象同步整个网络中的位置。

&emsp;&emsp;• 修改玩家移动脚本：

```csharp
    using UnityEngine;
    using UnityEngine.Networking;
    
    public class PlayerMove : NetworkBehaviour
    {
        void Update()
        {
            if (!isLocalPlayer)
                return;
    
            var x = Input.GetAxis("Horizontal")*0.1f;
            var z = Input.GetAxis("Vertical")*0.1f;
    
            transform.Translate(x, 0, z);
        }
    }
```

###8、测试多人玩家运动

&emsp;&emsp;• 再次构建并运行独立播放器，并作为主机启动。

&emsp;&emsp;• 在编辑器中进入播放模式并作为客户端连接。

&emsp;&emsp;• 玩家对象现在应该彼此独立地移动，并由他们的客户端上的本地玩家控制。


###9、识别你的玩家

游戏中的多个玩家的颜色一样，因此用户无法辨认其中的哪一个是自己。要识别玩家，我们将使本地玩家的颜色变红。

&emsp;&emsp;• 添加`OnStartLocalPlayer`函数的实现来更改玩家对象的颜色。

```csharp
    public override void OnStartLocalPlayer()
    {
        MeshRenderer render = transform.Find("TankTurret").GetComponent<MeshRenderer>();
        Material[] materials = render.materials;
        if (materials.Length > 0)
            materials[0].SetColor("_Color", Color.red);
    }
```

###10、射击子弹


