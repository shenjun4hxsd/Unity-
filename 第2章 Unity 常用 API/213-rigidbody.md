##RigidBody


|变量|说明|
|:--|:--|
|angularDrag|刚体的角阻力。|
|angularVelocity|刚体的角速度向量。|
|centerOfMass|相对于变换原点的质心。|
|collisionDetectionMode|刚体的碰撞检测模式。|
|constraints|控制该刚体模拟允许的自由度。|
|detectCollisions|碰撞检测应否启用？（默认总是启用的）|
|drag|刚体的阻力。|
|freezeRotation|控制物理是否改变物体的旋转。|
|inertiaTensor|相对于重心的质量的惯性张量对角线。|
|inertiaTensorRotation|惯性张量的旋转。|
|interpolation|插值允许你以固定的帧率平滑物理运行效果。|
|**isKinematic**|控制物理是否够影响这个刚体。|
|mass|刚体的质量。|
|maxAngularVelocity|刚体的最大角速度，（默认7）范围{0, 无穷大}|
|maxDepenetrationVelocity|当非穿透状态时，刚体的最大速度。|
|**position**|该刚体的位置。|
|**rotation**|刚体的旋转角度。|
|sleepThreshold|哪个对象开始进入休眠状态，质量归一化的能量阈值。|
|solverIterationCount|允许你覆盖每个刚体的求解迭代次数。|
|useConeFriction|用于该刚体的锥形摩擦力。|
|**useGravity**|控制重力是否影响整个刚体。|
|**velocity**|刚体的速度向量。|
|worldCenterOfMass|在世界坐标空间的刚体的质量中心。（只读）|

|公共方法|说明|
|:--|:--|
|AddExplosionForce|应用一个力到刚体来模拟爆炸效果。|
|**AddForce**|添加到刚体的力。|
|AddForceAtPosition|在position位置应用force力。作为结果这个将在这个物体上应用一个扭矩和力。|
|AddRelativeForce|添加力到刚体。相对于它的系统坐标。|
|AddRelativeTorque|相对于它的局部坐标系统添加扭矩到刚体。|
|AddTorque|添加扭矩到刚体。|
|ClosestPointOnBounds|指定位置到该刚体附加的碰撞器的最近点。|
|GetPointVelocity|刚体在世界坐标空间，worldPoint点的速度。|
|GetRelativePointVelocity|相对于刚体在relativePoint点的速度。|
|IsSleeping|刚体在休眠么？|
|**MovePosition**|移动刚体到新位置。|
|**MoveRotation**|旋转刚体到新角度。|
|ResetCenterOfMass|重置刚体的质心。|
|ResetInertiaTensor|重置惯性张量值和旋转。|
|SetDensity|设置基于附加的碰撞器假设一个固定的密度质量。|
|Sleep|强制刚体休眠。|
|SweepTest|如果一个刚体碰到任何东西触发测试。|
|SweepTestAll|就像Rigidbody.SweepTest，当返回的是所有碰撞信息。|
|WakeUp|强制一个刚体唤醒。|

🔚

