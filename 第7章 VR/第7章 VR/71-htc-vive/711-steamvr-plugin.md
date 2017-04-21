##SteamVR Plugin脚本分析

####• openvr_api.cs 

```
这直接封装了steamvr SDK的支持。
是必须的脚本。它提供的所有功能steamvr暴露。不建议你做
更改此文件。应与相关的openvr_api DLL同步。
```


####• SteamVR.cs 

```
系统的初始化和关闭。使用SteamVR.instance 访问。
由于任何原因，如果初始化失败，则可能返回null。
```


####• SteamVR_Camera.cs

VR摄像机，主要功能是将Unity摄像机的画面进行变化，形成Vive中的成像画面


```
为了对抗畸变校正引起的拉伸，以更高的分辨率渲染场景。 
由于Unity中的所有相机都是顺序渲染的，所以在每个眼睛相机之间共享一个静态渲染纹理。

SteamVR提供了一个特定的渲染目标大小，以减轻失真，然而，渲染到更高的分辨率会在相关费用下提供额外的多采样优势。
这可以通过SteamVR_Camera.sceneResolutionScale控制。

注意：GUILayer和FlareLayer与SteamVR_Camera不兼容，因为它们在屏幕空间，而不是世界空间中渲染。

这些将自动移动SteamVR_GameView对象，该对象本身将自动添加到SteamVR_Camera的父'head'对象。AudioListener也被转移到头部，以便音频被适当的空间化。

```

####• SteamVR_Overlay.cs

此组件用于协助在VR中呈现2D内容。

```
使用特殊的渲染路径将指定的texture合成到虚拟曲面上的场景中，以提高保真度。
```


####• SteamVR_TrackedObject.cs

将其添加到要使用跟踪的任何对象。

```
该 HMD有一个设置为自动。
对于控制器，选择要映射到的对象的索引。
一般来说， 您应该将这些对象加载到相机的“origin”对象，以便它们在相同的空间中跟踪。
但是，如果不方便，您可以在TrackedObject本身中指定“origin”。
```

####• SteamVR_RenderModel.cs

该组件用于渲染手柄的模型，并且跟踪手柄的位置。

####• SteamVR_Utils.cs

用于在Unity中使用SteamVR API的各种位，包括一个简单的事件系统，用于处理向量/四元数对的RigidTransform类，矩阵转换和其他有用的功能。

####• SteamVR_PlayArea

用于显示玩家可游玩区域。使用方法，在Origin物体上添加该脚本即可

##SteamVR_ControllerManager和SteamVR_TrackedObject

控制器，主要用于设置和检测Vive控制器。

Vive控制器由菜单键(ApplicationMenu)，触摸板(Touchpad)，系统键/电源键(System)，扳机键(Trigger)，侧柄键(Grip)，组成

使用方法：

在Origin物体上添加2个子物体代表Vive的2个手柄，增加SteamVR_TrackedObject，Index设置为None

在Origin物体上添加SteamVR_ControllerManager，设置左右手柄

![](/assets/977418-20170410133419376-1614339157.png)

![](/assets/977418-20170410133443626-1429910370.png)


在origin上再加一个子对象主相机作为head如图，加上steam vr_tracked object

![](/assets/977418-20170410133812751-943762245.png)


####• SteamVR_Controller  控制器的相关信息


####•


####•





















