##EditorWindow

|静态变量|类型|解释|
|:--|:--|
|focusedWindow|EditorWindow|焦点窗口|
|mouseOverWindow|EditorWindow|鼠标悬停的窗口|

|变量|说明|
|:--|:--|
|autoRepaintOnSceneChange|Does the window automatically repaint whenever the scene has changed?|
|maximized|Is this window maximized.|
|maxSize|The maximum size of this window.|
|minSize|The minimum size of this window.|
|position|The position of the window in screen space.|
|titleContent|The GUIContent used for drawing the title of EditorWindows.|
|wantsMouseMove|Does the GUI in this editor window want MouseMove events?|




Public Functions

BeginWindows	Mark the beginning area of all popup windows.
Close	Close the editor window.
EndWindows	Close a window group started with EditorWindow.BeginWindows.
Focus	Moves keyboard focus to this EditorWindow.
RemoveNotification	Stop showing notification message.
Repaint	Make the window repaint.
SendEvent	Sends an Event to a window.
Show	Show the EditorWindow.
ShowAsDropDown	Shows a window with dropdown behaviour and styling.
ShowAuxWindow	Show the editor window in the auxiliary window.
ShowNotification	Show a notification message.
ShowPopup	Shows an Editor window using popup-style framing.
ShowUtility	Show the EditorWindow as a floating utility window.
Static Functions

FocusWindowIfItsOpen	Focuses the first found EditorWindow of specified type if it is open.
GetWindow	Returns the first EditorWindow of type t which is currently on the screen.
GetWindowWithRect	Returns the first EditorWindow of type t which is currently on the screen.
Messages

Awake	Called as the new window is opened.
OnDestroy	OnDestroy is called when the EditorWindow is closed.
OnFocus	Called when the window gets keyboard focus.
OnGUI	Implement your own editor GUI here.
OnHierarchyChange	Called whenever the scene hierarchy has changed.
OnInspectorUpdate	OnInspectorUpdate is called at 10 frames per second to give the inspector a chance to update.
OnLostFocus	Called when the window loses keyboard focus.
OnProjectChange	Called whenever the project has changed.
OnSelectionChange	Called whenever the selection has changed.
Update	Called multiple times per second on all visible windows.