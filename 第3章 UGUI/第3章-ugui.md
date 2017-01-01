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

##关卡选择界面

    背景
    滚动面板
        ScrollRect（Content设置为GridLayouGroup的物体）、添加Mask、脚本控制滚动的位置（scrollRect.horizontalNormalizedPosition）
            GridLayoutGroup 控制所有的关卡排列
    Toggle组（控制滚动面板当前显示的页）

##设计关卡按钮和锁定关卡的按钮

##添加所有关卡、并利用Grid Layout进行排序

##关卡的滚动列表

##滑动列表按照页数滑动的单选按钮

##利用单选按钮控制列表的滑动

##任务列表（背景、人物项、）

##任务列表（使用VerticalLayout和ScrollRect控制任务列表的滑动）

##设置界面（背景、声音滑动条）

##游戏难度的单选按钮

##扩展Toggle控件（音效和音乐的开关）

##游戏登陆界面（输入框）





