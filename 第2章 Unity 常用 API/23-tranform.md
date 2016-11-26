## Transform 类

|变量|说明|
|:--|:--|
|**childCount**|该变换的子对象数量。|
|**eulerAngles**|此旋转作为欧拉角度。|
|**forward**|在世界空间坐标，变换的蓝色轴。也就是z轴。|
|hasChanged|此变换自从上次标识是否被设置为false了？|
|localEulerAngles|旋转作为欧拉角度，相对于父级的变换旋转。|
|**localPosition**|相对于父级的变换的位置。|
|**localRotation**|该变换的旋转角度相对于父级变换的旋转角度。|
|**localScale**|相对于父级变换的缩放。|
|localToWorldMatrix|变换点的矩阵从局部坐标到世界坐标（只读）。|
|lossyScale|该对象的整体缩放（只读）。|
|**parent**|该变换的父对象。|
|**position**|在世界空间坐标transform的位置。|
|**right**|在世界坐标空间，变换的红色轴。也就是x轴。|
|root|返回最高层级的变换。|
|**rotation**|在世界坐标空间，储存为四元数变换的旋转角度。|
|**up**|在世界坐标空间，变换的绿色轴。也就是Y轴。|
|worldToLocalMatrix|变换点的矩阵从世界坐标到局部坐标（只读）。|

|函数|说明|
|:--|:--|
|DetachChildren|所有子对象解除父子关系。|
|**Find**|通过名字查找子对象并返回它。|
|**GetChild**|通过索引返回一个变换的子对象。|
|GetSiblingIndex|获取该对象的同级索引。|
|InverseTransformDirection|变换的方向从世界坐标转换到局部坐标。和Transform.TransformDirection相反。|
|InverseTransformPoint|变换位置从世界坐标到局部坐标。和Transform.TransformPoint相反。|
|InverseTransformVector|变换一个向量从世界坐标空间到局部坐标空间。这个操作与Transform.TransformVector相反。|
|IsChildOf|这个变换是parent的子对象？|
|**LookAt**|旋转此变换，让向前向量指向target的当前位置。(照相机的视口对准目标)|
|**Rotate**|应用一个欧拉角的旋转角度，eulerAngles.z度围绕z轴，eulerAngles.x度围绕x轴，eulerAngles.y度围绕y轴（这样的顺序）。|
|**RotateAround**|围绕世界坐标的point点的axis旋转该变换angle度。|
|SetAsFirstSibling|移动该变换到此局部变换列表的开始。|
|SetAsLastSibling|移动该变换到此局部变换列表的末尾。|
|**SetParent**|设置该变换的父级。|
|SetSiblingIndex|设置同级对象的索引。|
|TransformDirection|变换方向从局部坐标转换到世界坐标。|
|TransformPoint|变换位置从局部坐标到世界坐标。|
|TransformVector|变换一个向量从局部坐标空间到世界坐标空间。|
|**Translate**|移动transform在translation的方向和距离。|



🔚