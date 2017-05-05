##VRDevice

包含与VR设备相关的所有功能。


|静态变量|说明|
|:--|:--|
|isPresent|成功检测到VR设备正常工作。|
|model|装载VR设备的具体型号。|
|refreshRate|以赫兹显示的刷新率。|

---

静态功能

GetNativePtr	此方法返回一个IntPtr，表示一个可用的VR设备的本地指针，否则该值将为IntPtr.Zero。
GetTrackingSpaceType	返回设备当前的TrackingSpaceType。该值确定相机相对于其起始位置的位置。有关更多信息，请参阅VROverview中的“了解相机”一节。
SetTrackingSpaceType	设置设备当前的TrackingSpaceType。成功返回true。如果不支持给定的TrackingSpaceType或设备无法切换，则返回false。