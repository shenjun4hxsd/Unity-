##Unity5.x 动画系统


![](/assets/newAnimator04.jpg)


首先，我们发现的是3个默认的状态，这些状态是Unity自动帮我们创建的同时也无法删除：

```
Entry：表示当进入当前状态机时的入口，该状态连接的状态会成为进入状态机后的第一个状态；
Any State：表示任意的状态，其作用是其指向的状态是在任意时刻都可以切换过去的状态；
Exit：表示退出当前的状态机，如果有任意状态指向该出口，表示可以从指定状态退出当前的状态机；
```

**● Has Exit Time**

![](/assets/newAnimator05.jpg)

如果我们勾选了该项，在动画转换时会等待当前动画播放完毕才会转换到下一个动画，如果当前动画是循环动画会等待本次播放完毕时转换，所以对于需要立即转换动画的情况时记得要取消勾选。

还有一种情况时，当我当前的动画播放完毕后就自动转换到箭头所指的下一个状态（没有其他跳转条件），此时必须勾选该选项，否则动画播放完毕后就会卡在最后一帧，如果是循环动画就会一直循环播放。


```
新的BlendTree
    Direct
        多个动作可以同时播放
            适用于做面部表情
        可以通过参数控制不同动作片段的权重
```       

**● 常用API(需要继承StateMachineBehaviour)：**

![](/assets/newAnimator02.png)

![](/assets/newAnimator03.png)
        
在新的动画系统中，连线变得简单明了，每增加一个动画，就像下面一样连起来就是了。至于脚本的编写，只需点击图中的任意一个动画状态，然后在Inspector面板中点击Add Behaviour，即可为该动画状态编写脚本。这里我建了一个int变量控制整个状态机的状态变换，在右边的5个退出条件中，全部取消掉Has Exit Time(退出时间改为用脚本控制)。

![](/assets/newAnimator01.png)

```javascript
using UnityEngine;  
using System.Collections;  
  
public class Idle : StateMachineBehaviour {  
  
    override public void OnStateUpdate(Animator animator, AnimatorStateInfo stateInfo, int layerIndex)  
    {  
        if (Input.GetKeyDown(KeyCode.H))  
            animator.SetInteger("state", (int)CharacterState.Attack1);  
        if ((Input.GetAxisRaw("Horizontal") != 0) || (Input.GetAxisRaw("Vertical") != 0))  
            animator.SetInteger("state", (int)CharacterState.Run);  
    }  
  
}  
```

```javascript
using UnityEngine;  
using System.Collections;  
  
public class Attack1 : StateMachineBehaviour {  
  
    override public void OnStateUpdate(Animator animator, AnimatorStateInfo stateInfo, int layerIndex)  
    {  
        if (Input.GetKeyDown(KeyCode.H) && (stateInfo.normalizedTime > 0.5f))  
            animator.SetInteger("state", (int)CharacterState.Attack2);  
        if(stateInfo.normalizedTime > 0.9f)  
            animator.SetInteger("state", (int)CharacterState.Idle);  
    }  
  
}  
```

```javascript
using UnityEngine;  
using System.Collections;  
  
public class Attack2 : StateMachineBehaviour {  
  
    override public void OnStateUpdate(Animator animator, AnimatorStateInfo stateInfo, int layerIndex)  
    {  
        if (Input.GetKeyDown(KeyCode.H) && (stateInfo.normalizedTime > 0.5f))  
            animator.SetInteger("state", (int)CharacterState.Attack3);  
        if (stateInfo.normalizedTime > 0.9f)  
            animator.SetInteger("state", (int)CharacterState.Idle);  
    }  
  
}  
```

```javascript
using UnityEngine;  
using System.Collections;  
  
public class Attack3 : StateMachineBehaviour {  
  
    override public void OnStateUpdate(Animator animator, AnimatorStateInfo stateInfo, int layerIndex)  
    {  
        if (stateInfo.normalizedTime > 0.7f)  
            animator.SetInteger("state", (int)CharacterState.Idle);  
    }  
  
}  
```

```javascript
using UnityEngine;  
using System.Collections;  
  
public class Run : StateMachineBehaviour {  
  
    override public void OnStateUpdate(Animator animator, AnimatorStateInfo stateInfo, int layerIndex)  
    {  
        if ((Input.GetAxisRaw("Horizontal") == 0) && (Input.GetAxisRaw("Vertical") == 0))  
            animator.SetInteger("state", (int)CharacterState.Idle);  
    }  
  
}  
```


###● 自定义编辑器
另外，可以用代码来创建AnimatorController。

```javascript
using UnityEngine;  
using UnityEditor;  
using UnityEditor.Animations;  
  
public class CreateController : EditorWindow {  
  
    [MenuItem("Window/CreateController")]  
    static void Create()  
    {  
        // Creates the controller  
        var controller = AnimatorController.CreateAnimatorControllerAtPath ("Assets/StateMachineTransitions.controller");  
   
        // Add parameters  
        controller.AddParameter("TransitionNow", AnimatorControllerParameterType.Trigger);  
        controller.AddParameter("Reset", AnimatorControllerParameterType.Trigger);  
        controller.AddParameter("GotoB1", AnimatorControllerParameterType.Trigger);  
        controller.AddParameter("GotoC", AnimatorControllerParameterType.Trigger);  
   
        // Add StateMachines  
        var rootStateMachine = controller.layers[0].stateMachine;  
        var stateMachineA = rootStateMachine.AddStateMachine("smA");  
        var stateMachineB = rootStateMachine.AddStateMachine("smB");  
        var stateMachineC = stateMachineB.AddStateMachine("smC");  
   
        // Add States  
        var stateA1 = stateMachineA.AddState("stateA1");  
        var stateB1 = stateMachineB.AddState("stateB1");  
        var stateB2 = stateMachineB.AddState("stateB2");  
        stateMachineC.AddState("stateC1");  
        var stateC2 = stateMachineC.AddState("stateC2"); // don’t add an entry transition, should entry to state by default  
   
        // Add Transitions  
        var exitTransition = stateA1.AddExitTransition();  
        exitTransition.AddCondition(AnimatorConditionMode.If, 0, "TransitionNow");  
        exitTransition.duration = 0;  
   
        var resetTransition = stateMachineA.AddAnyStateTransition(stateA1);  
        resetTransition.AddCondition(AnimatorConditionMode.If, 0, "Reset");  
        resetTransition.duration = 0;  
   
        var transitionB1 = stateMachineB.AddEntryTransition(stateB1);  
        transitionB1.AddCondition(AnimatorConditionMode.If, 0, "GotoB1");  
        stateMachineB.AddEntryTransition(stateB2);  
        stateMachineC.defaultState = stateC2;  
        var exitTransitionC2 = stateC2.AddExitTransition();  
        exitTransitionC2.AddCondition(AnimatorConditionMode.If, 0, "TransitionNow");  
        exitTransitionC2.duration = 0;  
   
        var stateMachineTransition = rootStateMachine.AddStateMachineTransition(stateMachineA, stateMachineC);  
        stateMachineTransition.AddCondition(AnimatorConditionMode.If, 0, "GotoC");  
        rootStateMachine.AddStateMachineTransition(stateMachineA, stateMachineB);  
    }    
}  
```



🔚




