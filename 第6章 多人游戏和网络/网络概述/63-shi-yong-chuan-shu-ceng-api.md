##使用传输层API

网络传输不稳定，因此需要对数据进行多次编码和校验来确保数据的有效传输。

在实现网络传输时，人们把通信问题划分成了多个小问题，然后为每个小问题设计了一个单独的协议，使得每个协议都比较简单。

国际化标准组织（ISO）和国际电报电话咨询委员会（CCITT）共同制定了开放系统互联的七层参考模型，即把网络传输划分为7个独立的层次。

ISO七层网络模型

|层级|说明|
|:--|:--|
|应用层|应用程序提供的服务|
|表示层|格式化数据，以便为应用程序提供通用接口|
|会话层|在两个节点之间建立端连接|
|传输层|面向连接或无连接的常规数据递送，TCP和UDP协议属于传输层协议|
|网络层|通过寻址来建立两个节点之间的连接，IP协议属于网络层协议|
|数据链路层|将数据分帧，添加校验机制，并处理流控制|
|物理层|原始二进制流的传输|

>其中应用层、表示层和会话层统称为应用层，没有明确的界定，一般由程序开发者实现。

>传输层、网络层、数据链路层和物理层为底层，由操作系统提供。


####应用层（应用层、表示层、会话层）

把用户的数据转化成二进制流传递给传输层。

####传输层

以TCP协议为例：

TCP提供了IP环境下的数据可靠传输，它实现了数据流传送、可靠性校验、流量控制等功能。

传输层的数据转换：数据前添加一个TCP首部。  

    TCP首部|01010110


####网络层

以IP协议为例：

IP协议用于将多个包的交换网络连接起来，它在源地址和目的地址之间传送数据包，它还提供了对数据进行重新组装的功能，以适应不同网络对包大小的不同要求。

网络层的数据转换：IP协议会给数据加上目的地地址（IP地址和端口）等信息，必要时还会拆分数据。

    IP首部|TCP首部|01010110


####数据链路层

传输中若发生差错，为了达成只将有错的有限数据进行重发，数据链路层将二进制流组合成帧，然后以帧为单位进行传送。每个帧除了要传送的数据外，还包括校验码，以使接收方能发现传输中的差错。

>比如奇偶校验码是在原信息的最后添加一位用于奇校验或偶校验的代码。如果原始信息为“10010”，因为1的个数是2，所以会在后面添加一个0，形成“100100”。如果原始信息为“11010”，因为1的个数是3，所以要在后面添加一个1，形成“110101”。对端程序只要检验最后一位，便能够初步判定数据是否正确发送，如果不正确则要求重发。可见数据链路层会拆分数据并添加一些额外数据。

    帧首部|数据  帧首部|数据  帧首部|数据


####物理层

物理层传输就是数据通过物理介质进行传输的过程，物理介质包括电缆、光纤等物质。这些数据通过物理介质传输到目的地，目的地再依照与上述相反的过程进行解析，最后得到用户的数据。

---

####IP与端口

网络上的计算机都是通过IP地址进行识别的，应用程序通过通信端口彼此通信。

#####&emsp;&emsp;IP地址

    例如：
        IP：192.168.1.100

在Windows命令提示符中输入ipconfig，便能够查看本机的IP地址。


#####&emsp;&emsp;端口

“端口“是英文port的意译，是设备与外界通信交流的出口，每台计算机可以分配0到65535共65536个端口。其中0到1023号端口称为众所周知的端口号，它们被分配给一些固定的服务，比如80端口分配给WWW服务，21端口分配给FTP服务。

>我们在浏览器中输入一个网址（网址通过域名系统转换为IP地址）时不必指定端口号，因为在默认情况下WWW服务的端口是80。


#####&emsp;&emsp;C#中的相关类型

C#的System.Net命名空间提供了两个IP和端口相关的类IPAddress和IPEndPoint。

IPAddress ： 指示IP地址，如“127.0.0.1”。
IPEndPoint ： 指示IP地址和端口对的组合，如“127.0.0.1:80”。

|IPAddress的常用属性|说明|
|:--|:--|
|IPAddress.Any|使用机器上一个可用的IP来初始化这个IP地址对象|
|IPAddress.Parse|根据IP地址创建IPAddress对象，如IPAddress.Parse("192.168.0.1")|

|IPEndPoint的常用构造函数|说明|
|:--|:--|
|IPEndPoint(Int64, Int32)|用指定的地址和端口号初始化|
|IPEndPoint(IPAddress, Int32)|用IPAddress指定的地址和端口号初始化|

|IPEndPoint的常用属性|说明|
|:--|:--|
|Address|获取或设置终结点的IP地址|
|Port|获取或设置终结点的端口号|


---

####TCP协议

TCP是一种面向连接的、可靠的、基于字节流的传输层通信协议，而与TCP相对应的UDP协议则是无连接的、不可靠的协议（但传输效率比TCP高）。

#####&emsp;&emsp;TCP连接的建立

TCP是面向连接的，无论哪一方在向另一方发送数据之前，都必须先在双方之间建立一条连接。在TCP／IP协议中，TCP协议提供可靠的连接服务，连接是通过三次握手进行初始化的。三次握手的目的是同步连接双方的序列号和确认号，并交换TCP窗口大小的信息。

#####&emsp;&emsp;TCP的数据传输

发送一个数据后，发送方并不能确保数据一定会被接收方接收。于是发送方会等待接收方的回应，如果太长时间没有收到回应，发送方会重新发送数据。

#####&emsp;&emsp;TCP连接的终止

客户端和服务器通过三次握手建立了TCP连接以后，待数据传送完毕，便要断开连接。与三次握手相似，TCP通过“四次挥手”来确保双端都断开了连接。

第一次挥手：主机1（可以是客户端也可以是服务器）向主机2发送一个终止信号（FIN），此时，主机1进入FIN_WAIT_1状态，它没有需要发送的数据，只需要等待主机2的回应。

第二次挥手：主机2收到了主机1发送的终止信号（FIN），向主机1回应一个ACK。收到ACK的主机1进入FIN_WAIT_2状态。

第三次挥手：在主机2把所有数据发送完毕后，主机2向主机1发送终止信号（FIN），请求关闭连接。

第四次挥手：主机1收到主机2发送的终止信号（FIN），向主机2回应ACK。然后主机1进入TIME_WAIT状态（等待一段时间，以便处理主机2的重发数据）。主机2收到主机1的回应后，关闭连接。至此，TCP的四次挥手便完成了，主机1和主机2都关闭了连接。

---

####Socket套接字

#####&emsp;&emsp;Socket连接的流程

套接字是支持TCP／IP协议网络通信的基本操作单元，可以将套接字看作不同主机间的进程双向通信的端点，它构成了单个主机内及整个网络间的编程界面。套接字存在于通信域中，通信域是为了处理一般的线程通过套接字通信而引进的一种抽象概念。套接字通常会和同一个域中的套接字交换数据（数据交换也可能会穿越域的界限，但这时一定要执行某种解释程序）。各种进程使用这个相同的域用Internet协议来进行互相之间的通信。

![](/assets/Socket.png)


Socket通信的基本流程具体步骤如下所示：

1）开启一个连接之前，需要先完成Socket和Bind两个步骤。Socket是新建一个套接字，Bind指定套接字的IP和端口（客户端在调用Connect时会由系统分配端口，因此可以省去Bind）。

2）服务端通过Listen开启监听，等待客户端接入。

3）客户端通过Connect连接服务器，服务端通过Accept接收客户端连接。在connect-accept过程中，操作系统将会进行三次握手。

4）客户端和服务端通过Send和Receive发送和接收数据，操作系统将会完成TCP数据的确认、重发等步骤。

5）通过Close关闭连接，操作系统会进行四次挥手。

#####&emsp;&emsp;Socket类

&emsp;&emsp;System.Net.Sockets命名空间的Socket类为网络通信提供了一套丰富的方法和属性。

|Socket类的一些常用方法|说明|
|:--|:--|
|Bind|使Socket与一个本地终结点相关联|
|Listen|将Socket置于侦听状态|
|Accept|为新建连接创建新的Socket|
|Connect|建立与远程主机的连接|
|Send|将数据发送到连接的Socket|
|Receive|接收来自绑定的Socket的数据|
|Close|关闭Socket连接并释放所有关联的资源|
|Shutdown|禁用某Socket上的发送和接收功能|
|Disconnect|关闭套接字连接并允许重用套接字|
|BeginAccept|开始一个异步操作来接受一个传入的连接尝试|
|EndAccept|结束异步接受传入的连接尝试|
|BeginConnect|开始一个对远程主机连接的异步请求|
|EndConnect|结束挂起的异步连接请求|
|BeginDisconnect|开始异步请求从远程终结点断开连接|
|EndDisconnect|结束挂起的异步断来连接请求|
|BeginReceive|开始从连接的Socket中异步接收数据|
|EndReceive|将数据异步发送到连接的Socket|
|BeginSend|开始异步发送数据|
|EndSend|结束挂起的异步发送|
|GetSocketOption|返回Socket选项的值|
|SetSocketOption|设置Socket选项|
|Poll|确定Socket的状态|
|Select|确定一个或多个套接字的状态|


| Socket类的一些常用属性    | 说明                                      |
| ----------------- | --------------------------------------- |
| AddressFamily     | 获取Socket的地址族                            |
| Available         | 获取已经从网络接收且可供读取的数据量                      |
| Blocking          | 获取或设置一个值，该值指示Socket是否处于阻止模式             |
| Connected         | 获取一个值，该值指示Socket是否连接                    |
| IsBound           | 指示Socket是否绑定到特定的本地端口                    |
| OSSupportsIPv6    | 指示操作系统和网络适配器是否支持Internet协议第6版（IPv6）     |
| ProtocolType      | 获取Socket的协议类型                           |
| SendBufferSize    | 指定Socket发送缓冲区的大小                        |
| SendTimeout       | 发送数据（Send）的超时时间                         |
| ReceiveBufferSize | 指定Socket接收缓冲区的大小                        |
| ReceiveTimeout    | 接收数据（Receive）的超时时间                      |
| Ttl               | 指定Socket发送的Internet协议（IP）数据包的生存时间（TTL）值 |



---

#### 同步Socket程序

&emsp;&emsp;现在，编写一套简单的网络程序，这套网络程序分为客户端和服务端两个部分。客户单发送一行文本给服务端，服务端收到后将文本稍加改动后发回客户端。



##### 编写服务端程序



&emsp;&emsp;服务器遵照Socket通信的基本流程，先创建Socket，再调用Bind绑定IP地址和端口号，之后调用 Listen等待客户端连接。最后在while循环中调用Accept接收客户端的连接，并回应消息。


```csharp
    using System;
    using System.Net;
    using System.Net.Sockets;
    using System.Text;
    
    namespace SocketServer
    {
        class MainClass
        {
            public static void Main(string[] args)
            {
    			// 1. 创建服务器端Socket实例   (AddressFamily.InterNetwork : 使用IPv4 / AddressFamily.InterNetworkV6 : 使用IPv6)
    			Socket server_socket = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
    
                // 2. 绑定IP地址和端口号
                IPAddress ipAddr = IPAddress.Parse("127.0.0.1");
                IPEndPoint ipEndPoint = new IPEndPoint(ipAddr, 1234);
    
                //IPAddress[] ips = Dns.GetHostAddresses(Dns.GetHostName());
                //Console.WriteLine("ipaddress length : " + ips.Length + ", ip :" + ips[0]);
                //IPEndPoint ipEndPoint = new IPEndPoint(ips[0], 22222);
                server_socket.Bind(ipEndPoint);
    
                // 3. 开始监听
                server_socket.Listen(0);
                Console.WriteLine("服务器启动成功");
    
                while (true)
                {
                    // 4. 接收客户端的请求
                    Console.WriteLine("正在接收客户端的请求。。。");
                    Socket client = server_socket.Accept();  // 阻塞方法 会返回连接的客户端的套接字
    
                    Console.WriteLine("[服务器]Accept");
                    IPEndPoint clientIP = (IPEndPoint)client.RemoteEndPoint;  // 获取客户端的端点信息
                    Console.WriteLine("connect with client:" + clientIP.Address + ", at port:" + clientIP.Port);
    
                    // 5. 给客户端发送信息
                    //string welcome = "welcome connect";
                    //byte[] data = Encoding.ASCII.GetBytes(welcome);
                    //client.Send(data, data.Length, SocketFlags.None);
    
                    // 6. 接收客户端的消息
                    byte[] data = new byte[1024];
                    while (true)
                    {
                        int recv = client.Receive(data);
                        string recvStr = Encoding.UTF8.GetString(data, 0, recv);
                        Console.WriteLine("接收到消息：" + recvStr);
                        if(recvStr.ToLower() == "exit")
                        {
                            break;
                        }
                        data = Encoding.UTF8.GetBytes(clientIP.ToString() + " : " + recvStr);
                        client.Send(data);
                    }
    				client.Close();
                    Console.WriteLine("客户端：" + clientIP + " 断来连接");
    
                }
            }
        }
    }
```


```csharp
    Socket server_socket = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
```



| SocketType的值 | 含义                                       |
| ------------ | ---------------------------------------- |
| Dgram        | 支持数据报，即最大长度固定（通常很小）的无连接、不可靠消息。消息可能会丢失或重复并可能在到达时不按顺序排列。Dgram类型的Socket在发送和接收数据之前不需要任何连接，并且可以与多个对方主机进行通信。Dgram使用数据报协议（UDP）和InterNetworkAddressFamily |
| Raw          | 支持对基础传输协议的访问。通过使用SocketTypeRaw，可以使用Internet控制消息协议（ICMP）和Internet组管理协议（IGMP）这样的协议来进行通信。在发送时，应用程序必须提供完整的IP标头。所接收的数据报在返回时会保持其IP标头和选项不变 |
| Rdm          | 支持无连接、面向消息、以可靠方式发送的消息，并保留数据中的消息边界。RDM（以可靠方式发送的消息）消息会依次到达，不会重复。此外，如果消息丢失，则会通知发送方。如果使用Rdm初始化Socket，则在发送和接收数据之前无须建立远程主机连接。利用Rdm，可以与多个对方主机进行通信 |
| Seqpacket    | 在网络上提供排序字节流的面向连接且可靠的双向传输。Seqpacket不重复数据，它在数据流中保留边界。Seqpacket类型的Socket只与对方的单个主机进行通信，并且在通信开始之前需要建立远程主机连接 |
| Steam        | 支持可靠、双向、基于连接的字节流，既不重复数据，也不保留边界。此类型的Socket与对方的单个主机进行通信，并且在通信开始之前需要建立远程主机连接。Steam使用传输控制协议（TCP）和InterNetworkAddressFamily |
| UnKnown      | 指定未知的Socket类型                            |



ProtocolType用于指明协议



| 常用的协议       | 含义                    |
| ----------- | --------------------- |
| Ggp         | 网关到网关协议               |
| Icmp        | 网际消息控制协议              |
| IcmpV6      | 用于IPv6的Internet控制消息协议 |
| Idp         | Internet数据报协议         |
| Igmp        | Internet组管理协议         |
| IP          | Internet协议            |
| Internet    | 数据包交换协议               |
| PARC        | 通用数据包协议               |
| Raw         | 原始IP数据包协议             |
| Tcp         | 传输控制协议                |
| Udp         | 用户数据报协议               |
| UnKnown     | 未知协议                  |
| Unspecified | 未指定的协议                |



#####客户端程序

```csharp
    /*
     * Author: shenjun
     */
    
    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;
    using UnityEngine.UI;
    
    using System.Net;
    using System.Net.Sockets;
    
    public class ChatRoomClient : MonoBehaviour {
    
        Socket client;
    
        public InputField hostInput;
        public InputField portInput;
    
        public Button connectBtn;
        public Button clearBtn;
    
        // 聊天文本
        public Text recvText;
        // 客户端IP地址
        public Text clientText;
        // 客户端输入
        public InputField input;
    
        const int BUFFER_SIZE = 1024;
        byte[] buffer = new byte[BUFFER_SIZE];
    
        bool isLink = false;
    
    	void Start () {
            connectBtn.onClick.AddListener(Connect);
            input.onEndEdit.AddListener(OnChat);
            clearBtn.onClick.AddListener(OnClear);
    	}
    	
        void Connect()
        {
            // 创建套接字
            client = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
            string host = hostInput.text;
            int port = int.Parse(portInput.text);
    
            // 连接服务器（ 阻塞方法 ）
            client.Connect(host, port);
    
            // 发送消息给服务器
            clientText.text = "客户端地址 ：" + client.LocalEndPoint.ToString();
            byte[] bytes = System.Text.Encoding.UTF8.GetBytes(clientText.text);
            client.Send(bytes);
    
            // 接收服务器消息
            int count = client.Receive(buffer);
            string result = System.Text.Encoding.UTF8.GetString(buffer, 0, count);
            recvText.text += "\r\n" + result;
    
            isLink = true;
        }
    
        void OnChat(string msg)
        {
            if(!isLink) return;
    
            byte[] bytes = System.Text.Encoding.UTF8.GetBytes(msg);
            client.Send(bytes);
    
            int count = client.Receive(buffer);
            string result = System.Text.Encoding.UTF8.GetString(buffer, 0, count);
            recvText.text += "\r\n" + result;
    
            input.text = "";
        }
    
        void OnClear()
        {
            recvText.text = "";
        }
    }
    
```











