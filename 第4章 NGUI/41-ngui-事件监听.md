##NGUI事件监听

NGUI事件的种类很多，比如点击、双击、拖动、滑动等等，他们处理事件的原理几乎万全一样，本文只用按钮来举例。

1.直接监听事件

把下面脚本直接绑定在按钮上，当按钮点击时就可以监听到，这种方法很不灵活。

```javascript
	void OnClick()
	{
		Debug.Log("Button is Click!!!");
	}
```


2.使用SendMessage

选择按钮后，在Unity导航菜单栏中选择Component->Interaction->Button Message 组件。

Target：接收按钮消息的游戏对象。

Function Name：接收按钮消息的方法，拥有这个方法的脚本必须绑定在上面Target对象身上。

Trigger：触发的事件，OnClick显然是一次点击。

Include Children ：是否让该对象的所有子对象也发送这个点击事件。

![](/assets/屏幕快照-2013-06-24-下午5.28.53.png)

到UIButtonMessage.cs这个脚本中看看，其实很简单就是调用Unity自身的SendMessage而已。

```javascript
	void Send ()
	{
		if (string.IsNullOrEmpty(functionName)) return;
		if (target == null) target = gameObject;
 
		if (includeChildren)
		{
			Transform[] transforms = target.GetComponentsInChildren<Transform>();
 
			for (int i = 0, imax = transforms.Length; i < imax; ++i)
			{
				Transform t = transforms[i];
				t.gameObject.SendMessage(functionName, gameObject, SendMessageOptions.DontRequireReceiver);
			}
		}
		else
		{
			target.SendMessage(functionName, gameObject, SendMessageOptions.DontRequireReceiver);
		}
	}
```

3.使用UIListener

这个也是推荐大家使用的一种方法，选择按钮后在Unity导航菜单栏中选择Component->NGUI->Internal ->Event Listener 。 挂在按钮上就可以，它没有任何参数。。

![](/assets/屏幕快照-2013-06-24-下午5.38.30.png)

在任何一个脚本或者类中即可得到按钮的点击事件、把如下代码放在任意类中或者脚本中。

```javascript
	void Awake () 
	{	
                //获取需要监听的按钮对象
		GameObject button = GameObject.Find("UI Root (2D)/Camera/Anchor/Panel/LoadUI/MainCommon/Button");
                //设置这个按钮的监听，指向本类的ButtonClick方法中。
		UIEventListener.Get(button).onClick = ButtonClick;
	}
 
        //计算按钮的点击事件
	void ButtonClick(GameObject button)
	{
		Debug.Log("GameObject " + button.name);
 
	}
```
