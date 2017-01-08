##EditorGUIUtility


类 UnityEditor / 继承自: GUIUtility

EditorGUI的其他辅助的东西。



|静态变量|描述|
|:--|:--|
|currentViewWidth|当前EditorWindow GUI区域的宽度或其他视图。|
|editingTextField|目前是一个文本字段编辑文本吗?|
|fieldWidth|像素保留的最小宽度的字段编辑器GUI控件。|
|hierarchyMode|是GUI编辑器层级模式?|
|isProSkin|用户目前使用pro皮肤吗? (只读)|
|labelWidth|宽度的像素用于标签编辑器GUI控件。|
|singleLineHeight|得到高度用于单个编辑一行EditorGUI等控制。 文本框或EditorGUI.Popup。|
|standardVerticalSpacing|得到垂直间距的高度使用默认控件。|
|systemCopyBuffer|系统复制缓冲区。|
|whiteTexture|得到一个白色的纹理。|
|wideMode|是当前广泛的GUI编辑器模式?|



|静态函数|描述|
|AddCursorRect|添加一个自定义鼠标指针来控制。|
|CommandEvent|创建一个事件。|
|DrawColorSwatch|画一个色卡。|
|DrawCurveSwatch|画一条曲线的样布。|
|DrawRegionSwatch|画样本两个SerializedProperty曲线之间的填充区域。|
|FindTexture|从源头得到纹理文件名。|
|GetBuiltinSkin|得到一个内置的GUI的皮肤,可以游戏视图,检查员或场景视图皮肤作为选择的参数。|
|GetFlowLayoutedRects|布局的字符串列表条目从左到右,从上到下的特定区域。|
|GetIconSize|使用SetIconSize获取的大小设置。|
|GetObjectPickerControlID|目前显示的controlID对象选择器。|
|GetObjectPickerObject|	当前在对象选择器中选择的对象。|
|HasObjectThumbnail|一个给定的类有逐对象式缩略图吗?|
|IconContent|从Unity安装在内部的资源获取GUIContent名字。|
|IsDisplayReferencedByCameras|检查是否启用相机可以呈现特定的显示。|
|Load|加载一个内置的资源。|
|LoadRequired|负载必须有一个内置的资源。|
|ObjectContent|返回一个GUIContent对象与对象的名称和图标。|
|PingObject|平安保险对象在一个窗口点击检查员。|
|QueueGameViewInputEvent|发送一个输入事件进入游戏。|
|SetIconSize|设置图标的一部分呈现GUIContent呈现在一个特定的大小。|
|ShowObjectPicker|显示的对象选择器代码。|