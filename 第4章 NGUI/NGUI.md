##UIRoot

    注意：组件需要打开，否则看不到框，无法用鼠标菜单。

##Lable

##Sprite
    Slice

##Button

##Panel

##Widiget

##Anchor

##AtlasMaker

##DynamicFont

##Tween

##Slider

   需要使用sprite组件

##Play Sound脚本

##Typewriter Effect脚本 （打字机效果）

##Slider Colors脚本

    滑动条根据不同的位置，颜色会有变化。

##Poput List

    需要添加子物体Label，用Label显示当前的选项，把Lable拖入事件OnValueChange，然后选择事件类型为SetCurrentSelection。
    
##CheckBox

    添加BoxCollider、Toggle Script脚本
    
    指定UIToggle中State Transition中的Sprite，用于切换状态时显示的图片。

##设计选项（声音大小、难度选项）

##监听控件的事件

    声音大小  UIProgressBar.current.value 用来获取当前滑动条的值（最好对字符串Trim()处理下）
    游戏难度  UIPopupList.current.value 获取当前PopupList的值
    是否全屏  UIToggle.current.value 获取当前CheckBox的值

