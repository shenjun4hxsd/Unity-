##UIRoot

    注意：组件需要打开，否则看不到框，无法用鼠标菜单。

##Lable

##Sprite
    Slice   九宫格 修改Border属性
    Filled  技能冷却动画

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
    
##界面之间的切换

    对界面添加TweenPosition脚本（脚本可以通过PlayForward、playReverse方法播放）
    
##实现技能的CD效果

##输入框（单行文本、多行文本）

    InputField脚本组件（指定Label属性）
    Widget/Pivot会影响文字的开始位置

##注册界面

    UIInput（使用value的值改变显示的文本）
    
##可拖拽功能和可调节大小