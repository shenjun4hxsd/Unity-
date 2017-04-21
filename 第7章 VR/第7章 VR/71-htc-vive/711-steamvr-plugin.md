##SteamVR Plugin脚本分析


|脚本|说明|
|:--|:--|
|openvr_api.cs|1、这直接封装了steamvr SDK的支持。<br>2、是必须的脚本。它提供的所有steamvr暴露功能。<br>3、不建议你更改此文件。应与相关的openvr_api DLL同步。|
|SteamVR.cs|1、系统的初始化和关闭。使用SteamVR.instance 访问。<br>2、由于任何原因，如果初始化失败，则可能返回null。|
|SteamVR_Camera.cs|1、VR摄像机，主要功能是将Unity摄像机的画面进行变化，形成Vive中的成像画面。<br>2、为了对抗畸变校正引起的拉伸，以更高的分辨率渲染场景。 <br>3、由于Unity中的所有相机都是顺序渲染的，所以在每个眼睛相机之间共享一个静态渲染纹理。<br>4、SteamVR提供了一个特定的渲染目标大小，以减轻失真，然而，渲染到更高的分辨率会在相关费用下提供额外的多采样优势。<br>5、这可以通过SteamVR_Camera.sceneResolutionScale控制。<br>6、注意：GUILayer和FlareLayer与SteamVR_Camera不兼容，因为它们在屏幕空间，而不是世界空间中渲染。<br>7、这些将自动移动SteamVR_GameView对象，该对象本身将自动添加到SteamVR_Camera的父'head'对象。AudioListener也被转移到头部，以便音频被适当的空间化。|
|SteamVR_Overlay.cs|1、此组件用于协助在VR中呈现2D内容。<br>2、使用特殊的渲染路径将指定的texture合成到虚拟曲面上的场景中，以提高保真度。|
|SteamVR_TrackedObject.cs|1、将其添加到要使用跟踪的任何对象。该 HMD有一个设置为自动。<br>2、对于控制器，选择要映射到的对象的索引。<br>3、一般来说，您应该将这些对象加载到相机的“origin”对象，以便它们在相同的空间中跟踪。<br>4、但是，如果不方便，您可以在TrackedObject本身中指定“origin”。|
|SteamVR_RenderModel.cs|1、该组件用于渲染手柄的模型，并且跟踪手柄的位置。|
|SteamVR_Utils.cs|1、用于在Unity中使用SteamVR API的各种位，包括一个简单的事件系统<br>2、用于处理向量/四元数对的RigidTransform类，矩阵转换和其他有用的功能。|
|SteamVR_PlayArea|1、用于显示玩家可游玩区域。使用方法，在Origin物体上添加该脚本即可|



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

---

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

关于控制器的相关信息都包含在SteamVR_Controller这个脚本之中。
注意：SteamVR_Controller是非Monobehavior的脚本，并没有挂在场景下，其运行是通过SteamVR_Render脚本对于其中Update（）函数的调用从而一直循环获取手柄相关信息。

而SteamVR_Render脚本是在程序运行时自动加载至场景。

ButtonMask类：手柄各按键的名称。

Device类：最重要的类，封装了跟踪设备的全部信息。

Update()函数：被SteamVR_Render脚本调用，更新各跟踪设备的信息。

Input()函数：根据Index初始化Device[]数组，并实例化所有16个Device。

![](/assets/977418-20170410101623344-1408084167.png)

DeviceRelation和GetDeviceIndex()结合获取特定的设备Index，如leftmost、rightmost。在SteamVR_TestController脚本中有使用到该函数获取用户的左右手手柄的Index。用该方法应该可以识别出用户所拿的手柄是哪一个。

####下面着重分析Device类：
针对按键的三个函数，每种函数有两个重载，参数为ButtonMask或EVRButtonId类，第二种形式会将EVRButtonId转换为ButtonMask后调用第一种得到结果。


|方法|说明|
|:--|:--|
|GetPress|按下按键一直触发|
|GetPressDown|按下按键只触发一次|
|GetPressUp|松开按键只触发一次|


**Touchpad**
针对Touchpad的三个函数，一样的两种形式。

|方法|说明|
|:--|:--|
|GetTouch|按下按键一直触发|
|GetTouchDown|按下按键只触发一次|
|GetTouchUp|松开按键只触发一次|


GetAxis()

获取Touchpad圆盘坐标或Trigger的行程值（0-1），函数默认参数是手柄上的Touchpad。共有5个AxisId参数可选，0是TouchPad，1是Trigger，2,3,4应该是没有用的，且此函数只接受EVRButtonId类参数而不接受ButtonMask。

**手柄震动**

手柄震动控制函数，参数名称解释的是时间，默认500,但实际上控制的是震动的强度。默认AxisId是EVRButtonId_touchpad，选择其他EVRButtonId没用（等价参数axis0可以）,其会调用OpenVR中的同名函数。参数超过4000会无效，导致震动不触发。可以通过协程的while（true）+waitforsecond控制震动间隔，通过stop协程控制震动结束。


GetHairTrigger
GetHairTriggerDown定义为有false转至true的过程，判断先前状态和当前状态。
GetHairTriggerUp 定义为有true转至false的过程，判断先前状态和当前状态。

这三个函数获取HairTrigger的状态，其值对应rAxis1.x的值。But什么是HairTrigger？形象点说就是像头发般轻轻的触发。HairTrigger是检测当你握住扳手超过一个固定值（0.1，可调）时即触发的状态。相当于利用Trigger的变化量来做一个功能的触发，很棒不是吗。轻轻碰一下Trigger使得行程大于0.1即可触发一个功能。


**整个脚本的执行顺序如下：**

Update（）；
Input()初始化16个设备的信息存储空间，并返回某个设备；
Device.Update():做两件事

1. GetControllerStateWithPose（）调用OpenVR中的函数获取手柄状态和姿态。结果存在这两个变量State（VRControllerState_t）和Pose（TrackedDevicePose_t）中，由GetState()和GetPose()获取并由此返回一系列的可查询参数及各种Get函数。

各可查询参数说明如下（相当于对TrackedDevicePose_t中的参数翻译了下）

| 可查询参数       | 说明                                    |
| ----------- | ------------------------------------- |
| valid       | GetControllerStateWithPose()函数调用是否成功； |
| connected   | 判断设备是否连接；                             |
| hasTracking | 判断设备是否跟踪正常；                           |



 根据ETrackingResult的结果得到下面三个参数：

| 参数            | 说明          |
| ------------- | ----------- |
| outOfRange    | 判断设备是否超出范围； |
| calibrating   | 判断设备是否正在校正； |
| uninitialized | 判断设备是否未初始化； |

transform：获取的结果是包含12个元素的一维数组，通过SteamVR_Utils.RigidTransform函数将12个元素重组为3X4矩阵并针对Unity的坐标系进行修正，同时添加了对position和rotation方便的引用。



velocity和angularVelocity：这两个速度也针对Unity的坐标系进行修正，lighthouse跟踪的空间轴方向与Unity存在偏差。



2. UpdateHairTrigger（）。检测自定义的一种操作：轻按Trigger。 


关于DeviceRelation和GetDeviceIndex()没有太多解释，可以深入挖一挖如何使用。

ButtonMusk及其他地方有用到很多按位进行的操作，可以补充一下相关知识。

Unity环境下空间坐标轴的修正原理没有研究：Unity使用的是左手坐标系，lighthouse的结果应该是右手坐标系，所以才会有速度的z分量相反和角速度x、y分量相反的结果。





















