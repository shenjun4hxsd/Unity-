##使用传输层API
除了我们提供的高级网络API之外，它还提供了易于使用的系统来管理您的玩家，联网的GameObjects以及其他常见需求 - 我们还可以访问称为“传输层”的较低级别的API。这为您提供了在较低级别构建自己的网络系统的能力，如果您对游戏网络有更具体或更高级的要求，这将非常有用。

传输层是操作系统基于套接字的网络之上的一个层。它能够发送和接收表示为字节数组的消息，并提供许多不同的“服务质量”选项，以适应不同的场景。它专注于灵活性和性能，并在`UnityEngine.Networking.NetworkTransport`类中公开一个API。

传输层支持网络通信的基础服务。这些基础服务包括：

```
    1、建立连接
    2、沟通使用各种“服务质量”
    3、流量控制
    4、基本统计
    5、附加服务，如通过中继服务器进行通信或本地发现
```

传输层可以使用两种协议：用于通用通信的`UDP`和`WebGL`的`WebSockets`。
要直接使用传输层，典型的工作流程如下：

```
    1、初始化网络传输层
    2、配置网络拓扑
    3、创建主机
    4、开始通信（处理连接和发送/接收消息）
    5、使用后关闭库。
    6、初始化网络传输层
```

初始化网络传输层时，您可以选择默认的初始化，没有参数，也可以提供控制网络层整体行为的参数，例如最大数据包大小和线程超时限制。

```csharp
    // Initializing the Transport Layer with no arguments (default settings)
    NetworkTransport.Init();
```
```csharp
    // An example of initializing the Transport Layer with custom settings
    GlobalConfig gConfig = new GlobalConfig();
    gConfig.MaxPacketSize = 500;
    NetworkTransport.Init(gConfig);
```

在上面的第二个示例中，传输层使用指定为500的自定义`“MaxPacketSize”`值进行初始化。只有在具有异常网络环境并熟悉所需的特定设置时，才应使用自定义初始值。作为一个经验法则，如果您正在开发一个典型的多人游戏，旨在通过互联网播放，默认的`Init（）`设置，无参数应该是适当的。

###组态

下一步是配置对等体之间的连接。您可能需要定义几个通信通道，每个通道具有指定的不同服务质量级别，以适应您要发送的特定消息类型，以及它们在游戏中的相对重要性。

```csharp
    ConnectionConfig config = new ConnectionConfig();
    int myReiliableChannelId  = config.AddChannel(QosType.Reliable);
    int myUnreliableChannelId = config.AddChannel(QosType.Unreliable);
```

在上面的例子中，我们定义了两个具有不同服务质量值的通信通道。`“QosType.Reliable”`将传递消息并确保消息被传递，而`“QosType.Unreliable”`将发送消息没有任何保证，但会更快地做到这一点。

还可以通过调整`ConnectionConfig`对象上的属性来为每个连接指定特定的配置设置。但是，当从一个客户端到另一个客户端进行连接时，两个连接的对等体的设置应该相同，否则连接将失败并出现`CRCMismatch`错误。

###拓扑

网络配置的最后一步是拓扑定义。网络拓扑定义允许多少连接以及使用什么连接配置：

```csharp
    HostTopology topology = new HostTopology(config, 10);
```

这里我们创建了最多可以连接10个的拓扑，每个连接将通过上一步中定义的参数进行配置。

###主机创建

正如所有初步步骤所做，我们可以创建主机（开放套接字）：

```csharp
    int hostId = NetworkTransport.AddHost(topology, 8888);
```

这里我们在端口8888和任何IP地址上添加新主机。该主机最多支持10个连接，每个连接将具有我们在config对象中定义的参数。

###通讯

作为主持人，我们可以开始我们的沟通。为了做到这一点，我们发送不同的命令给主机并检查其状态。有3个主要命令可以发送：

```csharp
    connectionId = NetworkTransport.Connect(hostId, "192.16.7.21", 8888, 0, out error);
    NetworkTransport.Disconnect(hostId, connectionId, out error);
    NetworkTransport.Send(hostId, connectionId, myReiliableChannelId, buffer, bufferLength,  out error);
```
1、第一个命令将使用ip“192.16.7.21”和端口8888向对等体发送连接请求。它将返回分配给此连接的ID。
2、第二个将发送断开连接请求，
3、第三个将发送消息，与id相等的连接connectionId，使用id相等的可靠信道myReiliableChannelId，消息应存储在buffer[]该消息的长度上，由消息的长度定义bufferLength。
要检查主机状态，您可以使用两个功能：

NetworkTransport.Receive(out recHostId, out connectionId, out channelId, recBuffer, bufferSize, out dataSize, out error);
NetworkTransport.ReceiveFromHost(recHostId, out connectionId, out channelId, recBuffer, bufferSize, out dataSize, out error);
他们都返回事件，第一个函数将从任何主机返回事件（并返回主机ID recHostId）第二个表单检查主机与id recHostId。你可以使用任何这些功能里面的Update()方法：

void Update()
{
    int recHostId; 
    int connectionId; 
    int channelId; 
    byte[] recBuffer = new byte[1024]; 
    int bufferSize = 1024;
    int dataSize;
    byte error;
    NetworkEventType recData = NetworkTransport.Receive(out recHostId, out connectionId, out channelId, recBuffer, bufferSize, out dataSize, out error);
    switch (recData)
    {
        case NetworkEventType.Nothing:         //1
            break;
        case NetworkEventType.ConnectEvent:    //2
            break;
        case NetworkEventType.DataEvent:       //3
            break;
        case NetworkEventType.DisconnectEvent: //4
            break;
    }
}
要点1：没有返回任何有趣的事件。
点2：连接事件进来，它可以是新的连接，也可以是以前的连接命令响应：
myConnectionId = NetworkTransport.Connect(hostId, "192.16.7.21", 8888, 0, out error);
NetworkEventType recData = NetworkTransport.Receive(out recHostId, out connectionId, out channelId, recBuffer, bufferSize, out dataSize, out error);
switch (recData)
{
    case NetworkEventType.ConnectEvent: 
        if(myConnectionId == connectionId)
            //my active connect request approved
        else
            //somebody else connect to me
        break;
    //...   
}
要点3：收到数据。在这种情况下recHostId将定义主机，connectionId将定义连接，channelId将定义通道; dataSize将定义接收到的数据的大小。如果recBuffer足够大以容纳数据，数据将被复制到缓冲区中。如果没有，error将包含MessageToLong错误，您将需要重新分配缓冲区并再次调用此函数。
点4：断线信号进入，可能是建立连接断开或连接请求失败的信号。
myConnectionId = NetworkTransport.Connect(hostId, "192.16.7.21", 8888, 0, out error);
NetworkEventType recData = NetworkTransport.Receive(out recHostId, out connectionId, out channelId, recBuffer, bufferSize, out dataSize, out error);
switch (recData)
{
    case NetworkEventType. DisconnectEvent: 
        if(myConnectionId == connectionId)
            //cannot connect by some reason see error
        else
            //one of the established connection has been disconnected
        break;
    \\...   
}
WebGL支持

客户端上的WebSocket已被支持。对于客户端，上述所有步骤（包括拓扑和配置）应该相同。Web客户端只能连接到服务器，其中服务器是独立播放器（仅限Win，Mac或Linux）。在服务器上，你应该打电话

NetworkTransport.AddWebsocketHost(topology, port, ip);
在ip地址是侦听地址的地方，你可以通过null作为ip地址，在这种情况下主机将监听所有的网络接口。服务器只能支持一个Websocket主机，同时也可以处理通用主机：

NetworkTransport.AddWebsocketHost(topology, 8887, null);
NetworkTransport.AddHost(topology, 8888);
将打开tcp套接字处理端口8887上的Web套接字协议和udp套接字处理端口8888上的基因协议。