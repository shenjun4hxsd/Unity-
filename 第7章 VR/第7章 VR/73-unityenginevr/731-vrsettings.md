##VRSettings

全局VR相关设置

|静态变量|说明|
|:--|:--|
|enabled|全局启用或禁用VR应用程序。|
|eyeTextureHeight|加载设备的眼睛纹理的当前高度。|
|eyeTextureWidth|加载设备的眼睛纹理的当前宽度。|
|isDeviceActive|只读值可用于确定VR设备是否处于活动状态。|
|loadedDeviceName|当前加载的VR设备的类型。|
|renderScale|控制镜头校正前的纹理像素比例，锐度的交易性能。|
|||


renderViewportScale	控制镜头校正前的纹理像素比例，锐度的交易性能。
showDeviceView	如果可能，将设备上显示的内容与主显示器进行镜像。
supportedDevices	返回构建时包含的支持的VR设备列表。
静态功能

LoadDeviceByName	在下一帧开始时加载所请求的设备。