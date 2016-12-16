##Unity5.x 动作系统

Entry 状态
Exit 状态
Behaviour 脚本
新的BlendTree
    Direct
        多个动作可以同时播放
            适用于做面部表情
        可以通过参数控制不同动作片段的权重
        

常用API(需要继承StateMachineBehaviour)：
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










