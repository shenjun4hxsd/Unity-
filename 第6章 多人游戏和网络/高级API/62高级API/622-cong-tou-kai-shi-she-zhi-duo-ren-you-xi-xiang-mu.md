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
    
    public class TankMove : MonoBehaviour
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
    
    public class TankMove : NetworkBehaviour
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

###10、射击子弹（不联网）

&emsp;&emsp;• 制作子弹预制，并添加Rigidbody组件。

&emsp;&emsp;• 修改玩家移动脚本，并为子弹预设赋值：


```csharp
    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;
    using UnityEngine.Networking;
    
    public class TankMove : NetworkBehaviour
    {
    
        public GameObject bulletPrefab;
    
        Camera m_LocalCamera;
        public Vector3 m_CameraPosOffset = new Vector3(0, 4.5f, -6f);
        public Vector3 m_CameraLookOffset = new Vector3(0, 2f, 0);
    
        public float m_RotSpeed = 15;
    
        Transform firePos;
    
        float mouseX = 0;
    
        void Awake()
        {
            firePos = transform.Find("FirePos");
        }
    
        void Update()
        {
    
            if (!isLocalPlayer)
                return;
            
            var x = Input.GetAxis("Horizontal1") * 0.1f;
            var z = Input.GetAxis("Vertical1") * 0.1f;
    
            transform.Translate(x, 0, z);
            if (Input.GetMouseButton(0))
            {
                mouseX += Input.GetAxis("Mouse X");
            }
    
            transform.rotation = Quaternion.Euler(0, mouseX * m_RotSpeed, 0);
    
            if (Input.GetKeyDown(KeyCode.Space))
            {
                Fire();
            }
        }
    
        void LateUpdate()
        {
            if (m_LocalCamera && isLocalPlayer)
            {
                m_LocalCamera.transform.position = transform.TransformPoint(m_CameraPosOffset);
                m_LocalCamera.transform.LookAt(transform.position + m_CameraLookOffset);
            }
        }
    
        /// <summary>
        /// 创建本地玩家时调用
        /// </summary>
        public override void OnStartLocalPlayer()
        {
            m_LocalCamera = Camera.main;

            MeshRenderer render = transform.Find("TankTurret").GetComponent<MeshRenderer>();
            Material[] materials = render.materials;
            if (materials.Length > 0)
                materials[0].SetColor("_Color", Color.red);
        }
    
        void Fire()
        {
            var bullet = Instantiate<GameObject>(bulletPrefab, firePos.position, firePos.rotation);
            bullet.GetComponent<Rigidbody>().AddForce(transform.forward * 1500);
            Destroy(bullet, 2f);
        }
    }
```

###11、子弹射击（联网）

&emsp;&emsp;• 给子弹预设添加NetworkIdentity组件。

&emsp;&emsp;• 给子弹预设添加NetworkTransform组件。

&emsp;&emsp;• 选择NetworkManager并打开“Spawn Info”折叠。

&emsp;&emsp;• 用加号按钮添加新的生成预制。

&emsp;&emsp;• 将子弹预设拖到新生成的预制插槽中。

![](/assets/UNetTut12.png)

&emsp;&emsp;• 更新TankMove脚本以连接子弹：

&emsp;&emsp;• 通过添加[Command]自定义属性和“Cmd”前缀，将Fire函数更改为联网命令。

&emsp;&emsp;• 在子弹对象上使用NetworkServer.Spawn()。


```csharp

    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;
    using UnityEngine.Networking;
    
    public class TankMove : NetworkBehaviour
    {
    
        public GameObject bulletPrefab;
    
        Camera m_LocalCamera;
        public Vector3 m_CameraPosOffset = new Vector3(0, 4.5f, -6f);
        public Vector3 m_CameraLookOffset = new Vector3(0, 2f, 0);
    
        public float m_RotSpeed = 15;
    
        public Transform firePos;
    
        float mouseX = 0;
    
        void Awake()
        {
            firePos = transform.Find("FirePos");
        }
    
        void Update()
        {
    
            if (!isLocalPlayer)
                return;
            
            var x = Input.GetAxis("Horizontal1") * 0.1f;
            var z = Input.GetAxis("Vertical1") * 0.1f;
    
            transform.Translate(x, 0, z);
            if (Input.GetMouseButton(0))
            {
                mouseX += Input.GetAxis("Mouse X");
            }
    
            transform.rotation = Quaternion.Euler(0, mouseX * m_RotSpeed, 0);
    
            if (Input.GetKeyDown(KeyCode.Space))
            {
                // 本地调用 服务器执行
                CmdFire();
            }
        }
    
        void LateUpdate()
        {
            if (m_LocalCamera && isLocalPlayer)
            {
                m_LocalCamera.transform.position = transform.TransformPoint(m_CameraPosOffset);
                m_LocalCamera.transform.LookAt(transform.position + m_CameraLookOffset);
            }
        }
    
        /// <summary>
        /// 创建本地玩家时调用
        /// </summary>
        public override void OnStartLocalPlayer()
        {
            m_LocalCamera = Camera.main;
    
            MeshRenderer render = transform.Find("TankTurret").GetComponent<MeshRenderer>();
            Material[] materials = render.materials;
            if (materials.Length > 0)
                materials[0].SetColor("_Color", Color.red);
        }
    
        // 设置为本地调用 服务器执行的命令
        [Command]
        void CmdFire()
        {
            var bullet = Instantiate<GameObject>(bulletPrefab, firePos.position, firePos.rotation);
            bullet.GetComponent<Rigidbody>().AddForce(transform.forward * 1500);
    
            NetworkServer.Spawn(bullet);
            Destroy(bullet, 2f);
        }
    }
```


###12、子弹碰撞

&emsp;&emsp;• 为子弹预设及玩家预设添加Collider组件。

&emsp;&emsp;• 为子弹预设添加脚本“Bullet”。

```csharp
    using UnityEngine;
    
    public class Bullet : MonoBehaviour
    {
        void OnCollisionEnter(Collision col)
        {
            var hit = col.gameObject;
            var hitPlayer = hit.GetComponent<TankMove>();
            if (hitPlayer != null)
            {
                Destroy(gameObject);
            }
        }
    }
```

&emsp;&emsp;• 当子弹击中玩家时，它会被销毁。当服务器上的子弹被销毁时，由于它是由NetworkServer产生的对象，所以在客户端也将被销毁。


###13、玩家状态（非联网血量）

&emsp;&emsp;• 玩家被子弹攻击后，血量减少。

&emsp;&emsp;• 给玩家添加脚本，添加血量属性及受伤害函数。

```csharp
    using UnityEngine;
    
    public class Combat : MonoBehaviour {
    
        public const int k_MaxHealth = 100;
        public int m_Health = k_MaxHealth;
    
        public void TakeDamage(int amount)
        {
            m_Health -= amount;
            if(m_Health < 0)
            {
                m_Health = 0;
                Debug.Log("Dead!");
            }
        }
    }
```

&emsp;&emsp;• 更新Bullet脚本，玩家被子弹击中后调用TakeDamage函数。

```csharp
    using UnityEngine;
    
    public class Bullet : MonoBehaviour
    {
        void OnCollisionEnter(Collision col)
        {
            var hit = col.gameObject;
            var hitPlayer = hit.GetComponent<TankMove>();
            if (hitPlayer != null)
            {
                hit.GetComponent<Combat>().TakeDamage(10);
                Destroy(gameObject);
            }
        }
    }
```


&emsp;&emsp;• 为了使玩家被子弹击中后能观察到血量的变化，给玩家添加血条（使用OnGUI）。

```csharp
    using UnityEngine;
    
    public class HealthBar : MonoBehaviour {
    
        GUIStyle healthStyle;
        GUIStyle backStyle;
        Combat combat;
    
        void Awake()
        {
            combat = GetComponent<Combat>();
        }
       
        void OnGUI()
        {
            InitStyle();
    
            var pos = Camera.main.WorldToScreenPoint(transform.position);
    
            // 绘制血条背景
            GUI.color = Color.grey;
            GUI.backgroundColor = Color.grey;
            GUI.Box(new Rect(pos.x - 26, Screen.height - pos.y + 20, Combat.k_MaxHealth/2, 7), ".", backStyle);
    
            // 绘制血条
            if (combat.m_Health != 0)
            {
                GUI.color = Color.green;
                GUI.backgroundColor = Color.green;
                GUI.Box(new Rect(pos.x - 25, Screen.height - pos.y + 21, combat.m_Health / 2, 5), ".", healthStyle);
            }
        }
    
        void InitStyle()
        {
            if (healthStyle == null)
            {
                healthStyle = new GUIStyle(GUI.skin.box);
                healthStyle.normal.background = MakeTex(2, 2, new Color(0, 1, 0, 1));
            }
    
            if (backStyle == null)
            {
                backStyle = new GUIStyle(GUI.skin.box);
                backStyle.normal.background = MakeTex(2, 2, new Color(0, 0, 0, 1));
            }
        }
    
        Texture2D MakeTex(int width, int height, Color col)
        {
            Color[] pix = new Color[width * height];
            for (int i = 0; i < pix.Length; i++)
            {
                pix[i] = col;
            }
    
            Texture2D tex = new Texture2D(width, height);
            tex.SetPixels(pix);
            tex.Apply();
            return tex;
        }
    }
```


###14、玩家状态（联网血量）

&emsp;&emsp;• 修改Combat脚本。

```csharp
    using UnityEngine;
    using UnityEngine.Networking;
    
    public class Combat : NetworkBehaviour {
    
        public const int k_MaxHealth = 100;
    
        [SyncVar]    // 同步变量
        public int m_Health = k_MaxHealth;
    
        /// <summary>
        /// 只在服务器上应用
        /// </summary>
        /// <param name="amount">Amount.</param>
        public void TakeDamage(int amount)
        {
            if (!isServer)
                return;
    
            m_Health -= amount;
            if(m_Health < 0)
            {
                m_Health = 0;
                Debug.Log("Dead!");
            }
        }
    }
```

###15、死亡和重生

&emsp;&emsp;• 当玩家血量为零时死亡，死亡以后重生。

&emsp;&emsp;• 添加一个[ClientRpc]函数来重生玩家对象。

&emsp;&emsp;• 当血量为零时，调用服务器上的重生功能。

&emsp;&emsp;• 修改Combat脚本：

```csharp
    using UnityEngine;
    using UnityEngine.Networking;
    
    public class Combat : NetworkBehaviour {
    
        public const int k_MaxHealth = 100;
    
        [SyncVar]    // 同步变量
        public int m_Health = k_MaxHealth;
    
        /// <summary>
        /// 只在服务器上应用
        /// </summary>
        /// <param name="amount">Amount.</param>
        public void TakeDamage(int amount)
        {
            if (!isServer)
                return;
    
            m_Health -= amount;
            if(m_Health <= 0)
            {
                m_Health = k_MaxHealth;
                RpcReSpawn();
            }
        }
    
        [ClientRpc]
        void RpcReSpawn()
        {
            if(isLocalPlayer)   // hasAuthority
            {
                transform.position = Vector3.zero;
            }
        }
    }
```

&emsp;&emsp;• 在这个游戏中，客户端控制玩家对象的位置 - 玩家对象在客户端上具有“本地权限”。如果服务器只是将玩家的位置设置为起始位置，那么客户端将被覆盖，因为客户端有权限。为了避免这种情况，服务器告诉拥有的客户端将玩家对象移到起始位置。




###16、非玩家对象

当玩家对象在客户端连接到主机时产生，大多数游戏具有游戏世界中存在的非玩家对象，例如敌人。在本节中，添加了一个spawner，创建可以被拍摄和杀死的非玩家对象。

&emsp;&emsp;• 创建一个空物体，重命名为“EnemeySpawner”。

&emsp;&emsp;• 添加组件NetworkIdentity。

&emsp;&emsp;• 在NetworkIdentity中选择“Server Only”复选框。这使得spawner不会被发送到客户端。

&emsp;&emsp;• 添加“EnemySpawner”脚本。

&emsp;&emsp;• 实现虚函数OnStartServer来创建敌人。

```csharp
    using UnityEngine;
    using UnityEngine.Networking;
    
    public class EnemeySpawner : NetworkBehaviour
    {
        public GameObject enemyPrefab;
        public int numEnemies;
    
        public override void OnStartServer()
        {
            for (int i = 0; i < numEnemies; i++)
            {
                var pos = new Vector3(Random.Range(-8.0f, 8.0f), 
                              0.2f, 
                              Random.Range(-8.0f, 8.0f));
    
                var rotation = Quaternion.Euler(0, Random.Range(0, 180), 0);
    
                var enemy = Instantiate<GameObject>(enemyPrefab, pos, rotation);
                NetworkServer.Spawn(enemy);
            }
        }
    }
```

现在创建一个敌人预制：

&emsp;&emsp;• 为敌人预制添加NetworkIdentity组件。

&emsp;&emsp;• 为敌人预制添加NetworkTransform组件。

&emsp;&emsp;• 在NetworkManager的Spawn Info中添加一个新的可生成的预制。

&emsp;&emsp;• 为敌人预制添加Combat脚本。

&emsp;&emsp;• 为敌人预制添加HealthBar脚本。

&emsp;&emsp;• 修改Bullet脚本的碰撞检测。

```csharp
    using UnityEngine;
    
    public class Bullet : MonoBehaviour
    {
        void OnCollisionEnter(Collision col)
        {
            var hit = col.gameObject;
            var hitCombat = hit.GetComponent<Combat>();
            if (hitCombat != null)
            {
                hitCombat.TakeDamage(10);
                Destroy(gameObject);
            }
        }
    }
```



###17、摧毁敌人

当敌人血量为零时被销毁。

&emsp;&emsp;• 修改Combat脚本。

&emsp;&emsp;• 添加“destroyOnDeath”变量。

```csharp
    using UnityEngine;
    using UnityEngine.Networking;
    
    public class Combat : NetworkBehaviour {
    
        public const int k_MaxHealth = 100;
    
        public bool m_DestroyOnDeath;
    
        [SyncVar]    // 同步变量
        public int m_Health = k_MaxHealth;
    
        /// <summary>
        /// 只在服务器上应用
        /// </summary>
        /// <param name="amount">Amount.</param>
        public void TakeDamage(int amount)
        {
            if (!isServer)
                return;
    
            m_Health -= amount;
            if(m_Health <= 0)
            {
                if (m_DestroyOnDeath)
                {
                    Destroy(gameObject);
                }
                else
                {
                    m_Health = k_MaxHealth;
                    RpcReSpawn();
                }
            }
        }
    
        [ClientRpc]
        void RpcReSpawn()
        {
            
            if(isLocalPlayer)  // hasAuthority
            {
                transform.position = Vector3.zero;
            }
        }
    }
```


###18、玩家生成的位置

目前创建玩家的位置全部在零点。这可能会造成玩家之间相互重叠。为了使玩家在不同的出生地点，可以使用NetworkStartPosition组件来实现。

&emsp;&emsp;• 创建一个空物体，并添加NetworkStartPosition组件。调整其位置。

&emsp;&emsp;• 用以上方法创建多个。

&emsp;&emsp;• 在NetworkManager组件上打开“Spawn Info”折叠。

&emsp;&emsp;• 将“Player Spawn Method”改为“Round Robin”（依次循环）。




🔚











