##UGUI


###游戏菜单

    开始游戏（Button）
    游戏设置（Button）
    结束游戏（Button）
    声音开关（Toggle）
    音量大小（Slider）
    故事简介（Image）
        Title（Image）
        TextList（Image、ScrollRect、Mask）
        Scrollbar（Image、Scrollbar）

###按钮的点击事件、滑动条的事件

    Slider 改变模型的旋转速度
    通过Slider的OnValueChange事件改变模型物体上的脚本中的值（Dynamic float，把改变后的值传递给事件回调函数）

###开始界面

    开始按钮（Image、Button）
    音量按钮
    设置按钮
    ...
    自适应设置
    物体的显示顺序

###战斗界面

    头像状态（头像、血条、等级）
    体力进度条（Slider、Filled Image）
    技能按钮（冷却）
    ...
    Image类型
    Label类型（Shadow、Outline）

###角色面板

    角色属性(头像、属性)
    
    分栏（ToggleGroup）
        装备栏（Toggle）
            设置OnValueChanged事件，控制装备栏面板显示(GameObject.SetActive)
        消耗品栏(Toggle)
            ...
        材料栏(Toggle)
            ...
        装备栏面板
            背景(Image)
            格子（GridLayoutGroup）
                子物体最好使用空物体套壳，避免被改变尺寸
        消耗品面板
            ...
        材料栏面板
            ...

###关卡选择界面

    背景
    滚动面板
        ScrollRect（Content设置为GridLayouGroup的物体）、添加Mask、脚本控制滚动的位置（scrollRect.horizontalNormalizedPosition）
            GridLayoutGroup 控制所有的关卡排列
    Toggle组（控制滚动面板当前显示的页）
        滑动列表自动设置Toggle的选项
        设置Toggle的选项设置滚动面板的内容


###任务列表

    背景
    SrollRect（控制列表的滑动）
        VerticalLayoutGroup （控制所有子项的排列，可以设置Height为所有子项的高度和）
            任务项


###设置界面

    声音（Slider）
    难度（ToggleGroup）
    音效（ToggleGroup，OnValueChange事件触发脚本控制）
    音乐（ToggleGroup，OnValueChange事件触发脚本控制）


###游戏登陆界面（输入框）

    用户名
        InputField 组件
            Text组件（默认文本显示）
            Text组件（输入以后，用于文本显示）
    密码
        ...
        
###背包系统的物体拖拽

```javascript
    using UnityEngine.EventSystem;
    public class Item : MonoBehaviour , IPointDownHandler, IPointUpHandler, IDragHandler
    {
        public void OnPointDown(PointEventData eventData)
        {
            // 保存原来格子的引用
            // 当前物体放大些
            // 改变父物体到最后的位置
        }
        
        public void OnPointUp(PointEventData eventData)
        {
            RaycastHit2D hit = Physics2D.Raycast(Input.mousePosition, -Vector2.up)
            {
                if(hit.collider!= null && hit.collider.tag == "item")
                {
                    hit.collider.transform.localScale = Vector3.one;
                    if(hit.transform.childCount != 0)
                    {
                        // 放到新的格子下面
                        // 改变父物体
                        // 位置归0
                    }
                    else
                    {
                        // 放回原处
                        // 位置归0
                    }
                }
            }
        }
    }
```

🔚


