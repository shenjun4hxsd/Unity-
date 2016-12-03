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


