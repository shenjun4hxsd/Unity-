##UGUI中的Canvas、Camera和Canvas Scaler

###Canvas.RenderMode：ScreenSpace - Overlay

当是ScreenOverlay的情况，因为根本没有经过投影空间，而是直接在屏幕空间绘制，所以可以更加简单地讨论问题。

因为在屏幕空间的坐标直接绘制像素，所以Canvas的RectTransform的width*scaleX必须等于屏幕的宽、RectTransform的height*scaleY必须等于屏幕的高。
其中，scaleXY事实上是可以通过Canvas.scaleFactor给脚本设置的，也就是说：

Canvas的核心功能是，自动根据屏幕设备的分辨率、当前的scaleXY，算出正确的width和height，确保和屏幕匹配。

注，Overlay情况下，Scene View是世界空间的、Canvas是屏幕空间的，两者处于不同空间，它们的大小比较也就没有可比性了。但也总得把Canvas在SceneView显示出来对吧，所以Unity这里的Editor实现是，屏幕空间1像素等于世界空间1米地把Canvas在SceneView显示出来。

CanvasScaler是ScaleWithScreenSize的作用就是：
根据参考分辨率和当前设备分辨率，得出一个“合适”的scaleFactor。至于设备分辨率和参考分辨率比例不同，这个问题交由Canvas自动设置width/height来解决。


###Canvas.RenderMode：ScreenSpace - Camera

由于使用了Camera，所以此时的Canvas和Scene View里的任意GameObject一样，都是处于世界空间了。
所以对于ScreenSpace - Camera的Canvas来说，关键是如何把处于世界空间中的Canvas能够占满投影空间的投影平面。

然而，由于在世界坐标系中任意GameObject的默认scaleXYZ都是1，Canvas也不例外。所以，需要根据Camera的设置、Canvas的plane distance设置，算出scaleFactorCamera出来。

正交（Orthographic）Camera
正交Camera的可调参数是Size。Size的定义为视体的投影平面高的1/2。 比如Camera的Size是300，即投影平面高为600。投影平面的宽根据平面的高和屏幕比例得出。
所以Canvas必须和这个投影平面一样大小（且位置重合）。此时，有两种方法和投影平面大小一样：

调整Canvas的width/height
调整Canvas的ScaleXY
由于第1种方法会导致切换Overlay和Camera的方式时，UI的表现不一致，所以Unity的实现是采取了第2种方法。即：

当Canvas是Screen Space - Overlay时，
Canvas.RectTransform.scaleXYZ = Canvas.scaleFactor
当Canvas是Screen Space - Camera时，
Canvas.RectTransform.scaleXYZ = Canvas.scaleFactor * scaleFactorCamera
所以，在这种情况下，

Canvas.height==Screen.height
Canvas.height * scaleFactor * scaleFactorCamera = Camera.size * 2，
所以scaleFactorCamera = Camera.size * 2 / Canvas.height / scaleFactor = Camera.size * 2 / Screen.height / scaleFactor
透视（Perspective）Camera
透视Camera的可调参数为fov。同时Canvas的plane distance也认为是参数之一。

Canvas.height == Screen.height
Canvas.height * scaleFactor * scaleFactorCamera = 2 * Canvas.planeDistance * tan(Camera.fov/2)
所以scaleFactorCamera = 2 * Canvas.planeDistance * tan(Camera.fov/2) / Screen.height / scaleFactor
CanvasScaler
至此，已求出不同Camera模式的scaleFactorCamera。

在ScreenSpace - Camera的情况下，不管CanvasScaler的模式再怎么变，都是在上面Overlay的情况下的CanvasScaler的讨论基础上，再乘多一个scaleFactorCamera即可。


###Canvas.RenderMode：WorldSpace

Canvas的width和height、scaleXYZ都可以任意设置。Canvas不再自动根据屏幕设备的分辨率、当前的scaleXY，算出正确的width和height。
此时的CanvasScaler在此模式下几乎形同虚设。只是拿来设置Canvas.referencePixelsPerUnit



