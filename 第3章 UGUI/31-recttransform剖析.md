##RectTransform 剖析

###一、基本要点

RectTransform继承于Transform，在 Transform 基础上，RectTransform 增加了 轴心（pivot）、锚点（anchors）、和 尺寸变化量（sizeDelta）。


其中anchors和pivot都是坐标原点在左下角的0-1向量空间，0-1代表的是比例。anchors的向量空间是子UI相对父UI的比例位置，pivot的向量空间是相对UI本身的比例位置。


![](/assets/rect_anchors_pivot.png)


###二、Anchors（锚点或锚框）

锚点功能的引入，使UI相对布局和绝对布局，自适应，等比缩放等完美融合一起。在uGUI中Anchors并不一定是锚点，也可能是一个矩形。当Anchors汇聚成一个点时，我们通常称之为锚点；当Anchors是一个矩形状时，我们通常称之为锚框。Anchors是子UI在父UI中的映射位置，但并不代表子UI的实际大小，子UI的实际大小，还会通过这些属性(width,height,posX,posY,left,top,right,bottom)相对Anchors进行调整。


####基本概念：

• Pos (X, Y, Z) ，矩形轴心点（pivot）与锚点（anchors）之间的距离。

• Left, Top, Right, Bottom，矩形的四条边与锚框（anchors）之间的间距。


#####（一）Anchors是一个点时

![](/assets/20151031-0.png)

• 在数值上表示为：achorMin.x==achorMax.x && achorMin.y==achorMax.y

• Width/Height可以设置，不受Anchors影响。

• PosX/PosY可以设置。设置此值后，矩形轴心点与锚点之间的距离就恒定了，不管如何改变父UI的尺寸。

• 子UI不会随父UI进行拉伸，位置和大小是固定的。