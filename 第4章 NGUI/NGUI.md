###1、UIRoot

        注意：需要打开Panel组件，否则看不到框，无法用鼠标菜单。

###2、Lable

        设置字体：Unity字体（动态字体）、NGUI字体
        
        Typewriter Effect脚本 （打字机效果，可配合Scroll View使用）


###3、Sprite
    
        功能类似UGUI中的Image
        Slice   九宫格 修改Border属性
        Filled  技能冷却动画 实现技能的CD效果

###4、Button
    
    Sprite、BoxCollider、Button
    Play Sound脚本
    
    举例：1）、使用Label创建按钮
                添加BoxCollider : 用于射线检测
                添加Button组件
    
         2）、使用Sprite创建按钮、添加子物体Label
                可以添加2个UIButton组件来分别控制Sprite和Label的颜色变化
                
    按键事件绑定
        NGUIEventListener.Get(obj).onClick = xxx;
    
        using UnityEngine;
        using System.Collections;
        
        public class EventUITest : MonoBehaviour {
        
            public UIButton _btn; 
            
            private void Awake()
            {
                //必须为 UIButton  类型
                EventDelegate.Add (this._btn.onClick, this.BtnClicked);
            }
                 
            private void BtnClicked()
            {
                Debug.Log("你通过EventDelegate点击了我！");
            }
        }

    按键点击穿透
        UICamera.hoveredObject
        
        通过射线来检测
            private RaycastHit hit = new RaycastHit();
            if (UICamera.Raycast (Input.mousePosition, out hit) == false) 
            {
                // 判断在UI界面层操作
            } 

###5、Panel

        用于分组管理子元素的容器、可以在Scene窗口中使用右键菜单功能
        可以通过Clipping 设置剪裁区域

###6、Widiget

        Pivot 中心点
        Depth 深度，用来控制绘制循序，数值小的先绘制
        Size 图片尺寸
        Aspect 比例 Free/BasedOnWidth/BasedOnHeight
        Anchors 锚点设置**

###7、AtlasMaker（图集制作）

    把碎图整合到一张大图中
    
>DynamicFont（动态字体制作）

>&emsp;&emsp;UIFont组件

###8、Tween （补间动画）

        Tween Alpha
            From/To
            PlayStyle : Once/Loop/PingPang
            Duration : 持续时间
            StartDelay : 多少事件后开始
            TweenGroup
            Ignore TimeScale
            
        Tween Color
            
        Tween Width
        
        Tween Height
        
        Tween Position
        
        Tween Rotation
        
        Tween Scale
        
        Tween Transform

###9、Slider

       需要使用UISlider组件
       
       可以在OnValueChanged事件中添加UILabel对象，使用SetCurrentPrecent或SetCurrentProgress事件对UILabel中的Text属性进行赋值。
       
       UISlider Colors脚本
            滑动条根据不同的位置，颜色会有变化。
            
       也可以添加UIButton组件，使鼠标悬停或点按时有颜色变化的效果。

###10、Poput List

        需要添加子物体Label，用Label显示当前的选项，把Lable拖入事件OnValueChange，然后选择事件类型为SetCurrentSelection。
    
###11、CheckBox

        添加BoxCollider、Toggle Script脚本
        
        指定UIToggle中State Transition中的Sprite，用于切换状态时显示的图片。
        
---

####练习一：设计选项（声音大小、难度选项）

###12、监听控件的事件

    声音大小  UIProgressBar.current.value 用来获取当前滑动条的值（最好对字符串Trim()处理下）
    游戏难度  UIPopupList.current.value 获取当前PopupList的值
    是否全屏  UIToggle.current.value 获取当前CheckBox的值
    
####练习二：界面之间的切换

    对界面添加TweenPosition脚本（脚本可以通过PlayForward、playReverse方法播放）
    

###13、输入框（单行文本、多行文本）

    InputField脚本组件（指定Label属性）
    Widget/Pivot会影响文字的开始位置

####练习三、注册界面

    UIInput（使用value的值改变显示的文本）
    
####练习四、可拖拽功能和可调节大小

    添加UIDrag Object组件、添加右下角拖拽手柄（BoxCollider、UIDrag Resize脚本、设置锚点）
    
###14、ScrollBar 滚动条

    UIScrollBar组件（Size属性设置拖动条的大小）
    

###15、TextList文本列表

    使用Label，然后添加UITextList脚本组件（运行后会清空文本，需要通过代码Add方法添加文本，可拖拽）
    
    Paragraph Histroy 设置保留最大的纪录数量
    
    Style : Chat 从下往上添加
    
###练习五：聊天系统的背景、滚动条（ScrollBar）、输入列表框(TextList)、输入框（UIInput、去掉输入后失去焦点的事件）

####练习六：背包

   拖拽物体添加 UIDRagDropItem 脚本组件，即可以进行拖拽，也可以继承 UIDragDropItem 类，重写 OnDragDropStart()、OnDragDropRelease(GameObject surface)、OnDragDropMove(Vector3 delta) 这三个方法。注意Start和Update方法不要覆盖父类方法。
   
```javascript
    public class MyDragDropItem : UIDragDropItem
    {
        protected override void OnDragDropRelease(GameObject surface)
        {
            base.OnDragDropRelease(surface);
            Debug.Log(surface);
        }
    }
```

###16、ScrollView

        设置可滚动的区域
        可以使用Slider作为Scroll Bar
        可滚动的子物体添加UIDrag Scroll View组件（例如：Label）
        
        
###17、Grid

###18、血条跟随
        可以使用 UIFollowTarget 脚本
        添加到UI物体上
        
        