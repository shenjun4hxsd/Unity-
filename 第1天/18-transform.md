## Transform 类

|变量|说明|
|:--|:--|
|childCount|该变换的子对象数量。|
|eulerAngles|此旋转作为欧拉角度。|
|forward|在世界空间坐标，变换的蓝色轴。也就是z轴。|






hasChanged

Has the transform changed since the last time the flag was set to 'false'?

此变换自从上次标识是否被设置为false了？

localEulerAngles

The rotation as Euler angles in degrees relative to the parent transform's rotation.

旋转作为欧拉角度，相对于父级的变换旋转。

localPosition

Position of the transform relative to the parent transform.

相对于父级的变换的位置。

localRotation

The rotation of the transform relative to the parent transform's rotation.

该变换的旋转角度相对于父级变换的旋转角度。

localScale

The scale of the transform relative to the parent.

相对于父级变换的缩放。

localToWorldMatrix

Matrix that transforms a point from local space into world space (Read Only).

变换点的矩阵从局部坐标到世界坐标（只读）。

lossyScale

The global scale of the object (Read Only).

该对象的整体缩放（只读）。

parent

The parent of the transform.

该变换的父对象。

position

The position of the transform in world space.

在世界空间坐标transform的位置。

right

The red axis of the transform in world space.

在世界坐标空间，变换的红色轴。也就是x轴。

root

Returns the topmost transform in the hierarchy.

返回最高层级的变换。

rotation

The rotation of the transform in world space stored as a Quaternion.

在世界坐标空间，储存为四元数变换的旋转角度。

up

The green axis of the transform in world space.

在世界坐标空间，变换的绿色轴。也就是Y轴。

worldToLocalMatrix

Matrix that transforms a point from world space into local space (Read Only).

变换点的矩阵从世界坐标到局部坐标（只读）。





