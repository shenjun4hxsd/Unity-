##CharacterController 角色控制器

角色控制器让你在受制于碰撞的情况下很容易的进行运动，而不用处理刚体。

角色控制器不受力的影响，仅当你调用Move函数时才运动。它执行运动，但是受制于碰撞。


|变量|说明|
|:--|:--|
|center|相对于变换位置的角色胶囊体的中心。|
|collisionFlags|在最后的CharacterController.Move调用期间，胶囊体的哪个部分与周围环境相碰撞。|
|detectCollisions|检测其他的刚体和角色控制器是否与本角色控制器相碰撞（默认值始终启用）。|
|height|角色胶囊体的高度。|
|isGrounded|在最后一次移动角色控制器是否触碰地面？|
|radius|角色胶囊体的半径。|
|slopeLimit|角色控制器的坡度度数限制。|
|stepOffset|以米为单位的角色控制器的台阶偏移量。|
|velocity|角色当前的相对速度。|


|公有方法|说明|
|:--|:--|
|Move|一个更加复杂的运动函数，采取绝对的运动增量。|
|SimpleMove|根据速度speed移动角色。|


|Message 消息|说明|
|:--|:--|
|OnControllerColliderHit|当角色控制器碰到一个可执行移动的碰撞器时，OnControllerColliderHit被调用。|

###示例：

####CharacterController.Move 移动

public **CollisionFlags** **Move**(**Vector3** motion);

一个更加复杂的运动函数，采取绝对的运动增量。

尝试着通过motion移动控制器，motion只受限制于碰撞。它将沿着碰撞器滑动。CollisionFlags 是发生于Move的碰撞的概要。这个函数不应用任何重力。

```javascript
    using UnityEngine;
    using System.Collections;
 
    public class ExampleClass : MonoBehaviour {
        public float speed = 6.0F;
        public float jumpSpeed = 8.0F;
        public float gravity = 20.0F;
        private Vector3 moveDirection = Vector3.zero;

        void Update() {
            CharacterController controller = GetComponent<CharacterController>();
            if (controller.isGrounded) {
                moveDirection = new Vector3(Input.GetAxis("Horizontal"), 0, Input.GetAxis("Vertical"));
                moveDirection = transform.TransformDirection(moveDirection);
                moveDirection *= speed;
                if (Input.GetButton("Jump"))
                    moveDirection.y = jumpSpeed;
 
            }
            moveDirection.y -= gravity * Time.deltaTime;
            controller.Move(moveDirection * Time.deltaTime);
        }
    }
```

---

####CharacterController.SimpleMove 简单移动

public bool SimpleMove(Vector3 speed);

根据速度speed移动角色。

Y 轴上速度被忽略。速度以米/秒为单位。重力被自动应用。返回如果角色着地。建议你每帧只调用一次Move或者SimpleMove。

```javascript
    using UnityEngine;
    using System.Collections;
 
    [RequireComponent(typeof(CharacterController))]
    public class ExampleClass : MonoBehaviour {
        public float speed = 3.0F;
        public float rotateSpeed = 3.0F;
        void Update() {
            CharacterController controller = GetComponent<CharacterController>();
            transform.Rotate(0, Input.GetAxis("Horizontal") * rotateSpeed, 0);
            Vector3 forward = transform.TransformDirection(Vector3.forward);
            float curSpeed = speed * Input.GetAxis("Vertical");
            controller.SimpleMove(forward * curSpeed);
        }
    }
```



