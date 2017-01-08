## GUILayout

|函数|说明|返回类型|
|:--|:--|:--|
|Label|创建一个自动布局的标签。|void|
|Box|创建一个自动布局的box。|void| 
|Button|创建一个单次按钮。当用户点击按钮会立即发生一些事件。|bool|
|RepeatButton|创建一个重复按钮。当用户点击按钮会立即发生一些事件。只要用户按住鼠标，按钮返回true。|bool|
|TextField|创建一个单行文本字段，用户可以编辑其中的字符串。|string|
|PasswordField|创建一个单行密码文本字段，用户可以输入密码。|string| 
|TextArea|创建一个多行密码文本区域，用户可以编辑其中字符串。|string|
|Toggle|创建一个开关按钮。|bool|
|Toolbar|创建一个工具栏。|int|
|SelectionGrid|创建一个选择表格。|int|
|HorizontalSlider|创建一个水平滑动条，用户可以拖动改变在最小和最大值之间的值。|float|
|VerticalSlider|创建一个垂直滑动条，用户可以拖动改变在最小和最大值之间的值。|float|  
|HorizontalScrollbar|创建水平滚动条。滚动条是能通过滚动来浏览文档，大多数情况下，你可以使用scrollView代替。|float|
|VerticalScrollbar|创建垂直滚动条。滚动条是能通过滚动来浏览文档，大多数情况下，你可以使用scrollView代替。|float|
|Space|在当前层组插入空白。|void|
|FlexibleSpace|插入一个弹性空白元素。|void|
|BeginHorizontal|开始一个水平控件的组。|void|
|EndHorizontal|关闭由BeginHorizontal开始的一个组。|void|
|BeginVertical|开始一个垂直控件的组。|void|
|EndVertical|关闭由BeginVertical开始的一个组。|void| 
|BeginArea|在屏幕上开始一个固定大小的布局区域。|void|
|EndArea|关闭由BeginArea开始GUILayout布局块。|void|
|BeginScrollView|开始一个自动布局滚动视图。|Vector2|
|EndScrollView|结束一个由BeginScrollView开始的滚动视图。|void|
|**Window**|创建一个弹出窗口，它的内容是自动布局的。|GUILayoutOption|
|**Width**|传递给控件一个绝对的宽度。|GUILayoutOption|
|**MinWidth**|传递给控件指定一个最小宽度的选项。|GUILayoutOption|
|**MaxWidth**|传递给控件指定一个最大宽度的选项。|GUILayoutOption|
|**Height**|传递给控件一个绝对的高度。|GUILayoutOption|
|**MinHeight**|传递给控件指定一个最小高度的选项。|GUILayoutOption|
|**MaxHeight**|传递给控件指定一个最大高度的选项。|GUILayoutOption|
|**ExpandWidth**|传递给控件是否允许水平液态方向的选项。|GUILayoutOption|
|**ExpandHeight**|传递给控件是否允许垂直液态方向的选项。|GUILayoutOption|

