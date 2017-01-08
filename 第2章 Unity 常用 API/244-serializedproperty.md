##SerializedProperty



|变量|	解释|
|:--|:--|
|serializedObject|这个属性属于SerializedObject（只读）。|
|name|属性的名称（只读）。|
|type|属性的类型名称（只读）。|
|tooltip|属性的工具提示（只读）。|
|depth|属性的嵌套深度（只读）。|
|propertyPath|属性的完整路径（只读）。|
|editable|属性可编辑么（只读）？|
|isExpanded|在检视面板属性是否扩展？|
|hasChildren|是否有子属性？（只读）|
|hasVisibleChildren|是否有可见的子属性？（只读）|
|isInstantiatedPrefab|属性是一个预设实例的一部分？（只读）|
|prefabOverride|预设属性的值是否不同？|
|propertyType|该属性的类型（只读）。|
|intValue|整数属性值。|
|boolValue|布尔属性值。|
|floatValue|浮点数属性值。|
|stringValue|字符串属性值。|
|colorValue|颜色属性值。|
|animationCurveValue|动画曲线属性值。|
|objectReferenceValue|物体引用属性值。|
|enumValueIndex|一个枚举属性的枚举索引。|
|enumNames|一个枚举属性的枚举名称数组。|
|vector2Value|一个2D向量属性的值。|
|vector3Value|一个3D向量属性的值。|
|rectValue|一个矩形属性值。|
|boundsValue|边界盒属性的值。|
|arraySize|数组中元素的数量。如果SerializedObject包含多个对象,它将返回元素的最小数量。所以它总是可以遍历SerializedObject和只有属性在所有对象。|

