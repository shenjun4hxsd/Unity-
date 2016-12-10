##Animator.GetBehaviours 获得行为列表

public T[] GetBehaviours();

描述：

返回匹配类型T或派生自T的所有状态机行为，如果没有找到返回null。

```javascript
using UnityEngine;
using System.Collections;
 
// An example StateMachineBehaviour.
public class BreathBehaviour : StateMachineBehaviour {
 
	public bool  fastBreath;
 
	// OnStateUpdate is called at each Update frame between OnStateEnter and OnStateExit callback
	override public void OnStateUpdate(Animator animator, AnimatorStateInfo stateInfo, int layerIndex) {
		animator.SetBool("FastBreath", fastBreath);
	}
}
 
 
public class RunBehaviour : StateMachineBehaviour {
 
	// OnStateUpdate is called at each Update frame between OnStateEnter and OnStateExit callback
	override public void OnStateUpdate(Animator animator, AnimatorStateInfo stateInfo, int layerIndex) 
	{
		BreathBehaviour[] breathBehaviours = animator.GetBehaviours<BreathBehaviour>();
        for(int i=0;i<breathBehaviours.Length();i++)
             breathBehaviours[i].fastBreath = true;
	}
}
```