##UGUI 射线检测

1.Graphic Raycaster
主要用于UI上的射线检测，挂有这个组件的物体，必须要挂上Canvas这个组件(当挂上Graphic Raycaster时Canvas也会自动挂上)。

Ignore Reversed Graphics：是否忽略反方向的图形，如果为true，则表示图形正面展示时，会接收到射线检测；反面展示时，不会接收到射线检测；否则，正反面展示都会接收到射线检测

Blocking Objects：屏蔽指定类型的(物理)对象，使它们不参与射线检测。渲染模式不为ScreenSpaceOverlay时起作用。可选值为：
None：不屏蔽任何物理对象
Two D：屏蔽2D物理对象(即具有2D碰撞体的对象)
Three D：屏蔽3D物理对象(即具有3D碰撞体的对象)
All：屏蔽所有物体对象

Blocking Mask：使屏蔽对象中的指定层不参与射线检测。渲染模式不为ScreenSpaceOverlay时，且Blocking Objects不为None时起作用。
例如，Blocking Objects为2D，Blocking Mask为UI，指2D物理对象中是UI层的东西都不会接收射线检测，而2D物理对象中的其他层还是能接收射线检测


2.Physics Raycaster & Physics2D Raycaster
主要用于物理对象上的射线检测，挂有这个组件的物体，必须要挂上Camera这个组件(当挂上组件时Camera也会自动挂上)。当物体(2D或者3D物理对象)受到射线检测时，物体上的实现了事件接口的方法会被调用

Physics Raycaster ：只会对3D物体对象产生影响，Camera的Projection，尽量选为Perspective(透视)模式
Physics2D Raycaster：只会对2D物体对象产生影响，Camera的Projection，尽量选为Orthography(正交)模式
EventMask：射线作用层，可以用来做事件屏蔽


3.Selectable
Selectable的意思是可选择的，它是UGUI中很多交互控件(Button/ Slider / InputField / Toggle / Scrollbar /)的基类，其中的Interactable，当值为false时，UI控件会进入disable状态(颜色会变为Disabled Color的那个颜色)，但是UI控件仍然能接收到射线检测！

![](/assets/20160321204010355.png)

4.CanvasGroup
用来批量地管理该控件以及该控件下的UI控件。Interactable的意思上面已经说到，而Blocks Raycasts，若为false，则UI控件不能接收射线检测

![](/assets/20160321205753394.png)

射线穿透

![](/assets/20160321211139604.png)

如上图，button和image都实现了点击接口，如果想点击两者的重叠部分，button的事件没有触发，而image的事件触发了，方法就是：
给button挂上Canvas Group组件，然后不勾选Blocks Raycasts

5.判断鼠标或者手指是否点击在UI上
对于EventSystem.current.IsPointerOverGameObject这个函数，因为EventSystem是UI的事件系统，所以IsPointerOverGameObject中的GameObject是针对UI的，而不是普遍意义上的GameObject

```javascript
    using UnityEngine;  
    using UnityEngine.UI;  
    using UnityEngine.EventSystems;  
      
    public class TestTouch : MonoBehaviour {  
      
        public Text text;  
      
        void Update()   
        {  
            if (Input.GetMouseButtonDown(0) || (Input.touchCount > 0 && Input.GetTouch(0).phase == TouchPhase.Began))  
            {  
    #if UNITY_ANDROID || UNITY_IPHONE  
                if (EventSystem.current.IsPointerOverGameObject(Input.GetTouch(0).fingerId))  
    #else  
                if (EventSystem.current.IsPointerOverGameObject())  
    #endif  
                      
                    text.text = "当前触摸在UI上";  
                else  
                    text.text = "当前没有触摸在UI上";  
            }  
        }  
      
    }
```