##AnimationEvent 动画事件

描述：

AnimationEvent类似于SendMessage让你调用一个脚本函数，这个脚本是动画播放的一部分

动画事件函数支持0参数或一个参数，参数可以是浮点型，字符串，object或AnimationEvent。


|变量|说明|
|:--|:--|
|animationState|该事件引发的动画状态（只读）。当该方法在动画事件回调之外被调用时，返回null。注意，该成员从动画组件（旧版）调用时，仅能设置。|
|animatorClipInfo|与此事件有关的动画器剪辑信息（只读）。|
|animatorStateInfo|有关该事件的动画器状态信息（只读）。|
|floatParameter|浮点型参数，储存在该事件中并发送给函数。|
|functionName|被调用的函数的名称。这与调用gameObject.SendMessage(animationEvent.functionName, animationEvent)相同|
|intParameter|整数型参数，储存在事件中并发送给函数。|
|isFiredByAnimator|如果该动画事件已经由Animator组件触发，返回true。|
|isFiredByLegacy|如果该动画事件已经由Animation组件触发，返回true。|
|messageOptions|函数调用选项。如果选项设置为 SendMessageOptions.RequireReceiver (默认),当消息没有被任何组件接收时打印一个错误信息.|
|objectReferenceParameter|储存在事件中的引用对象参数，并发送给函数。|
|stringParameter|储存在该事件中的字符串参数，并发送给函数。|
|time|引发该事件的时间点。|


示例：

```javascript
// Add an Animation Event to a GameObject that has an Animator
using UnityEngine;
using System.Collections;

public class Example : MonoBehaviour {

	public void Start()
	{
		// existing components on the GameObject
		AnimationClip clip;
		Animator anim;
		
		// new event created
		AnimationEvent evt;
		evt = new AnimationEvent();

		// put some parameters on the AnimationEvent
		//  - call the function called PrintEvent()
		//  - the animation on this object lasts 2 seconds
		//    and the new animation created here is
		//    set up to happens 1.3s into the animation		
		evt.intParameter = 12345;
		evt.time = 1.3f;
		evt.functionName = "PrintEvent";

		// get the animation clip and add the AnimationEvent
		anim = GetComponent<Animator>();
		clip = anim.runtimeAnimatorController.animationClips[0];
		clip.AddEvent(evt);

	}

	// the function to be called as an event
	public void PrintEvent(int i) {
		print("PrintEvent: " + i + " called at: " + Time.time);
	}
}
```


🔚
