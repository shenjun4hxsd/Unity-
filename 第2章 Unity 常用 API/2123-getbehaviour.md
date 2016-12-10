##Animator.GetBehaviour 获得行为

public T GetBehaviour();

描述：

返回匹配类型T或派生自T的首个状态机行为，如果没有找到返回null。

```javascript
public class RunBehaviour : StateMachineBehaviour {
 
	// OnStateUpdate is called at each Update frame between OnStateEnter and OnStateExit callback
	override public void OnStateUpdate(Animator animator, AnimatorStateInfo stateInfo, int layerIndex) 
	{
		if (animator.GetComponent<Recorder>().enabled && !animator.GetComponent<Recorder>().isRecording) return;
 
		Transform transform = animator.GetComponent<Transform>();
 
		RaycastHit hitInfo;
		Vector3 dir = transform.TransformDirection(Vector3.forward);
		if (Physics.Raycast(transform.position + new Vector3(0, 1.5f, 0), dir, out hitInfo, 10))
		{
			if (hitInfo.collider.tag == "Obstacle")
			{
				animator.GetBehaviour<SlideBehaviour>().target = transform.position + 1.25f * hitInfo.distance * dir;
				if(hitInfo.distance < 6)
					animator.SetTrigger("Slide");
			}
		}
	}
}
 
public class SlideBehaviour : StateMachineBehaviour {
 
	public Vector3 target;
 
	public float slideMatchTargetStart = 0.11f;
	public float slideMatchTargetStop = 0.40f;
 
	// OnStateUpdate is called at each Update frame between OnStateEnter and OnStateExit callback
	override public void OnStateUpdate(Animator animator, AnimatorStateInfo stateInfo, int layerIndex) {
		animator.MatchTarget(target, new Quaternion(), AvatarTarget.Root, new MatchTargetWeightMask(new Vector3(1, 0, 1), 0), slideMatchTargetStart, slideMatchTargetStop);
	}
}
```