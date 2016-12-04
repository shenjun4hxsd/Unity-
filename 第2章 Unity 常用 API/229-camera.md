##Camera 摄像机

Camera是一个设备，玩家通过它看世界。

屏幕空间点用像素定义，屏幕的左下为(0,0);右上是（PixelWidth，pixelHeight）.Z的位置是以世界单位衡量的到相机的距离。

视口空间点是规范的并相对于相机的。相机的左下为（0,0）；右上是（1,1）；Z的位置是以世界为单位衡量的到相机的距离。


|静态变量|说明|
|:--|:--|
|allCameras|返回场景中所有相机。|
|allCamerasCount|当前场景中相机的数量。|
|current|当前用于渲染的相机，只用于低级的渲染控制（只读）。|
|main|第一个启用的被标记为“MainCamera”的相机（只读）。|


|变量|说明|
|:--|:--|
|actualRenderingPath|实际使用的渲染路径（只读）。|
|aspect|长宽比（宽度除以高度）。|
|backgroundColor|屏幕将被清理为这个颜色。|
|cameraToWorldMatrix|从相机空间到世界空间的变换矩阵（只读）。|
|clearFlags|相机如何清除背景。|
|cullingMask|这个用来选择性的渲染部分场景。|
|depth|相机在渲染顺序上的深度。|
|depthTextureMode|相机生成怎样的一个深度纹理。|
|eventMask|遮挡相机的触发事件图层。|













