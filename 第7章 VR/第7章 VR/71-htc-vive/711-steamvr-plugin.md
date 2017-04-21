####• Assets/Plugins/openvr_api.cs 

```
这直接封装了steamvr SDK的支持。
是必须的脚本。它提供的所有功能steamvr暴露。不建议你做
更改此文件。应与相关的openvr_api DLL同步。
```


####•Assets/SteamVR/Scripts/SteamVR.cs 

```
系统的初始化和关闭。使用SteamVR.instance 访问。
由于任何原因，如果初始化失败，则可能返回null。
```


####• Assets/ SteamVR / Scripts / SteamVR_Camera.cs

```
VR摄像机，主要功能是将Unity摄像机的画面进行变化，形成Vive中的成像画面
```

####•
####•
####•