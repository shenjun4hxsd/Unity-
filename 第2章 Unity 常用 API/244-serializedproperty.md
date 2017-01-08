##SerializedProperty



|变量|解释|
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

&emsp;

|方法|解释|
|:--|:--|
|Next|	移动到下一个属性。|
|NextVisible|	移动到下一个可见属性。Reset	 移动到对象的第一个属性。|
|CountRemaining|	 计算余下的可见属性。
|Copy|	复制SerializedProperty。|
|DuplicateCommand|	复制命令。|
|DeleteCommand|	删除命令。|
|FindPropertyRelative|	检索当前序列化属性相关的属性|
|ClearArray|	删除所有元素的数组。|
|CountInProperty|	计算该属性的所有子属性的数量,包括这个属性本身。|
|GetArrayElementAtIndex	|返回指定索引数组中的元素。|
|GetEndProperty|	检索SerializedProperty,定义了该属性的范围|
|GetEnumerator|	检索一个迭代器,允许您在当前的迭代器的下一个序列化的属性。|
|InsertArrayElementAtIndex|	插入一个空元素指定数组中的索引。|
|MoveArrayElement|	将一个数组元素从srcIndex dstIndex。|
|DeleteArrayElementAtIndex|	删除数组中的指定索引元素。|

