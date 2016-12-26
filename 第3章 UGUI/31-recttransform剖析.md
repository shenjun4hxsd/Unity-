##RectTransform 剖析

###一、基本要点

RectTransform继承于Transform，在 Transform 基础上，RectTransform 增加了 轴心（pivot）、锚点（anchors）、和 尺寸变化量（sizeDelta）。


其中anchors和pivot都是坐标原点在左下角的0-1向量空间，0-1代表的是比例。anchors的向量空间是子UI相对父UI的比例位置，pivot的向量空间是相对UI本身的比例位置。


![](/assets/rect_anchors_pivot.png)


###二、Anchors（锚点或锚框）