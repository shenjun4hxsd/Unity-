##Camera 摄像机

Camera是一个设备，玩家通过它看世界。

屏幕空间点用像素定义，屏幕的左下为(0,0);右上是（PixelWidth，pixelHeight）.Z的位置是以世界单位衡量的到相机的距离。

视口空间点是规范的并相对于相机的。相机的左下为（0,0）；右上是（1,1）；Z的位置是以世界为单位衡量的到相机的距离。


|静态变量|说明|
|:--|:--|
|allCameras|返回场景中所有相机。|
|allCamerasCount|当前场景中相机的数量。|
|**current**|当前用于渲染的相机，只用于低级的渲染控制（只读）。|
|**main**|第一个启用的被标记为“MainCamera”的相机（只读）。|


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
|farClipPlane|远裁剪面的距离。|
|fieldOfView|相机的视野，以度为单位。|
|hdr|高动态范围渲染。|
|layerCullDistances|每层的消隐距离。|
|layerCullSpherical|相机如何执行每层的消隐。|
|nearClipPlane|近裁剪面的距离。|
|orthographic|相机是正交的（true），是透视的（false）？|
|orthographicSize|在正交模式下相机的一半尺寸。|
|pixelHeight|相机有多高，以像素单位（只读）。|
|pixelRect|相机被渲染到屏幕像素中的位置。|
|pixelWidth|相机有多宽，以像素单位（只读）。|
|projectionMatrix|设置自定义的投影矩阵。|
|rect|相机被渲染到屏幕规范化坐标中的位置。|
|renderingPath|渲染路径。|
|stereoConvergence|虚拟眼相交点间距|
|stereoEnabled|立体渲染。|
|stereoSeparation|虚拟眼间距离。|
|targetTexture|描述渲染纹理（仅Unity专业版）。|
|transparencySortMode|透明物体的排序模式。|
|useOcclusionCulling|无论是否在渲染过程中相机都会使用遮挡剔除。|
|velocity|获取世界空间中相机的速度（只读）。|
|worldToCameraMatrix|从世界到相机空间的变换矩阵。|


|公有函数|说明|
|:--|:--|
|CalculateObliqueMatrix|计算并返回倾斜接近水平的投影矩阵。|
|CopyFrom|使这个相机的设置与其他相机相同。|
|Render|手动渲染相机。|
|RenderToCubemap|从这个相机渲染到一个立方贴图。|
|RenderWithShader|用shader替代渲染相机。|
|ResetAspect|恢复长宽比为屏幕的长宽比。|
|ResetProjectionMatrix|让投影反映正常的相机参数。|
|ResetReplacementShader|从相机上移除shader替换。|
|ResetWorldToCameraMatrix|在场景中让渲染位置反映相机的位置|
|**ScreenPointToRay**|返回一条射线从摄像机通过一个屏幕点。|
|ScreenToViewportPoint|从屏幕空间到视窗空间的变换位置。|
|ScreenToWorldPoint|从屏幕空间到世界空间的变化位置。|
|SetReplacementShader|使相机渲染用shader替换。|
|SetTargetBuffers|设置相机渲染到一个或多个RenderTextures所选择的缓冲区。|
|ViewportPointToRay|返回从相机出发穿过视点的一个射线。|
|ViewportToScreenPoint|从视口空间到屏幕空间的变换位置。|
|ViewportToWorldPoint|从视窗空间到世界空间的变换位置。|
|WorldToScreenPoint|从世界空间到屏幕空间变换位置。|
|WorldToViewportPoint|从世界空间到视窗空间的变换位置。|


|静态方法|说明|
|:--|:--|
|GetAllCameras|用当前场景中的摄像机填补相机的数组，而不需要分配一个新的数组。|


|Message 消息|说明|
|:--|:--|
|OnPostRender|OnPostRender在相机渲染场景之后调用。|
|OnPreCull|OnPreCull在相机开始裁剪场景之前调用。|
|OnRenderImage|OnRenderImage在所有渲染完成后被调用，来渲染图片的后期处理效果（仅限UnityPro）。|
|OnRenderObject|OnRenderObject 在相机渲染完场景以后被调用。|
|OnWillRenderObject|如果物体可见，每个相机都会调用OnWillRenderObject。|























