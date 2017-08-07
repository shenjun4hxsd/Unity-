##LuaFramework

Lua源码目录介绍：

---3rd：里面是第三方的一些插件lua、实例源码文件，比如：cjson、pbc、pblua、sproto等。

---Common：公用的lua文件目录，如define.lua文件，一些变量声明，全局配置等，functions.lua常用函数库，通讯的protocal.lua协议文件。

---Controller：控制器目录，它不依赖于某一个Lua面板，它是独立存活在Luavm中的一个操作类，操作数据、控制面板显示而已。

---Logic：目录里面存放的是一些管理器类，比如GameManager游戏管理器、NetworkManager网络管理器，如果你有新的管理器可以放到里面。

---System：这个目录是cstolua的系统目录，里面存放都是一些常用的lua类，为了优化lua调用速度，用lua重写的unity常用类。

---View：这是面板的视图层，里面都是一些被Unity调用的面板的变量，走的是Unity GameObject的生命周期的事件调用。

lua和C#的交互两个工具类：
```csharp
    LuaHelper.cs静态类    //lua调用C#
    Util.cs              //静态方法CallMethod C#调用lua
```


分析图：

![](/assets/20160506154319148)

配置文件：

![](/assets/20160506154351680)

启动流程：

![](/assets/20160506154414941)