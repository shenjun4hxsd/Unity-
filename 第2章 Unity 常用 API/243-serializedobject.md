## SerializedObject

| 变量 | 类型 | 解释 | 描述 |
| :--- | :--- | :--- | :--- |
| targetObject | Object | 目标物体 | 被检查的物体（只读）。
|targetObjects|Object[]| 检查对象(只读)。| | 
|isEditingMultipleObjects|bool|多个对象的序列化对象表示由于多目标编辑吗?(只读)||

 &emsp;

|函数|返回类型|解释|描述|  
|:--|:--|:--|:--|  
|Update|void|更新|更新序列化物体。|
|GetIterator|SerializedProperty|获取迭代器\|得到第一个被序列化属性。|你可以用这个来遍历所有属性的目标对象。|  
|FindProperty|SerializedProperty|查找属性|通过名字查找被序列化属性。|  
|ApplyModifiedProperties|bool|应用修改的属性|| 
|ApplyModifiedPropertiesMultiSelect|bool|应用修改的属性到选择|应用修改属性用在当前的选择上。|
|CopyFromSerializedProperty|void|Copies a value from a SerializedProperty to the same serialized property on this serialized object.||
|SetIsDifferentCacheDirty|void|Update hasMultipleDifferentValues cache on the next Update() call.||

🔚