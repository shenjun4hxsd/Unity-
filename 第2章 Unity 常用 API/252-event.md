##Event

class in UnityEngine

Description 描述

A UnityGUI event.

UnityGUI的事件。

Events correspond to user input (key presses, mouse actions), or are UnityGUI layout or rendering events.

对应于用户输入事件（按键，鼠标行为），或是UnityGUI布局界面，或渲染事件。

For each event OnGUI is called in the scripts; so OnGUI is potentially called multiple times per frame. Event.current corresponds to “current” event inside OnGUI call.

对于每个事件在脚本OnGUI中被调用；OnGUI潜在每帧被多次调用。Event.current对应于在OnGUI里调用当前事件。

See Also: EventType.

Static Variables 静态变量

current	The current event that's being processed right now.
目前正在处理的事件。

```
Variables 变量

alt	Is Alt/Option key held down? (Read Only)
Alt或Option键按住了么（只读）？
button	Which mouse button was pressed.
哪个鼠标按键被按下。
capsLock	Is Caps Lock on? (Read Only)
Caps Lock键打开了么（只读）？
character	The character typed.
字符类型；就是键盘键入的对应的字符。
clickCount	How many consecutive mouse clicks have we received.
我们收到的鼠标连续点击多少次。
command	Is Command/Windows key held down? (Read Only)
Command/Windows键被按住了么（只读）？
commandName	The name of an ExecuteCommand or ValidateCommand Event.
ExecuteCommand或ValidateCommand事件的名字。
control	Is Control key held down? (Read Only)
Ctrl键被按住了么（只读）？
delta	The relative movement of the mouse compared to last event.
相对于上次事件，鼠标的相对移动。
displayIndex	Index of display that the event belongs to.
该事件所属的显示器索引。
functionKey	Is the current keypress a function key? (Read Only)
当前按下一个功能键？（只读）
isKey	Is this event a keyboard event? (Read Only)
这个事件是键盘事件么（只读）？
isMouse	Is this event a mouse event? (Read Only)
这个事件是鼠标事件么（只读）？
keyCode	The raw key code for keyboard events.
用于键盘事件的原始键码。
modifiers	Which modifier keys are held down.
哪一个修改键被按住。比如，Ctrl，Alt，Shift等
mousePosition	The mouse position.
鼠标的位置。
numeric	Is the current keypress on the numeric keyboard? (Read Only)
当前按键是数字键么（只读）？
shift	Is Shift held down? (Read Only)
Shift键按住了么（只读）？
type	The type of event.
事件的类型。
```


```
Public Functions 共有函数

GetTypeForControl	Get a filtered event type for a given control ID.
获取一个给定controlID的事件类型。
Use	Use this event.
使用这个事件。
```

```
Static Functions 静态函数

GetEventCount	Returns the current number of events that are stored in the event queue.
返回储存在当前事件队列中的事件数量。
KeyboardEvent	Create a keyboard event.
创建一个键盘事件。
PopEvent	Get the next queued [Event] from the event system.
从事件系统获取下个队列的事件。
```
🔚
