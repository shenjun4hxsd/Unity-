##Unity5.x 布料系统

Unity4中，需要对SkinnedMeshRenderer使用SkinnedCloth，或者对Cloth Renderer使用Interactive Cloth，很麻烦，在Unity5中，Cloth合二为一了。

Cloth只能必须和Skinned Mesh Renderer搭配使用，但是这不代表使用简单的物体时还必须在Max中导出一个带有蒙皮信息的FBX，其实可以新建一个GameObject然后赋予Cloth组件，这会自动添加Skinned Mesh Renderer组件，然后在Skinned Mesh Renderer组件中赋予基本体的Mesh上去并且设置正确的材质也完全可以。

布料可以接受外部影响，但完全不会将影响赋予外部刚体，换句话说, 布料系统的物理模拟是单向的。

![](/assets/newCloth01.png)


出于性能的考虑，可以对Cloth产生影响的Collider只有两种，SphereCollider以及CapsuleCollider，但是SphereCollider可以赋予两个为一组(当然了还可以只赋予一个，第二个保持null)，那么通过组合这两个SphereCollider可以出现第三种，Unity会将这组的两个SphereCollider进行相连，那么就可以有圆锥形的Collider

![](/assets/newCloth02.png)


打开编辑Constraint模式

![](/assets/newCloth03.png)

Max Distance可以设置每个顶点最大可移动距离。最常用的用法是将不能动的顶点的Max Distance设置为0。
Surface Penetration控制的是顶点最大可以嵌入到Mesh里面的程度。在布料网格顶点比较稀疏的情况下可以明显对比出差别。将顶点的Surface Penetration设置得很低会看起来很不正常，默认，也就是Unconstrained应该是一个较高的值，没有任何问题。


![](/assets/newCloth04.png)


Visualization这里能够选择当前在Scene视图中预览Max Distance还是Surface Penetration。(这两者二选一)，还能选择是否让操作影响视口背面的顶点。

Select编辑模式要先通过框选，或者Shift+点击来多选，来选中顶点，然后勾选Max Distance或者Surface Penetration前面的复选框，代表我现在要改变选中的顶点的值了！然后再在后面的数值里面输入想要的数值就OK了，要想将当前有数值的顶点设置为Unconstrained，只需要选中那些带有数值的顶点然后将对应复选框取消勾选即可。



在Paint模式中，这个按钮代表要绘制
![](/assets/newCloth05.png)

这里设置的值了！
![](/assets/newCloth06.png)

而这个复选框代表的意思是是否想为现在已经有值的顶点Paint成为Unconstrained的顶点。

![](/assets/newCloth07.png)


![](/assets/newCloth08.png)

Stretching Stiffness---拉扯硬度。

Bending Stiffness---弯曲硬度。

Use Tethers---用绳或链拴住，默认开启，用于方式过度拉伸

Use Gravity---是否使用世界重力。

Damping---阻尼会应用于每个布料顶点。要想打造看上去抖动更小的布料，可以试试这个。

External Acceleration---常量外力。

Random Acceleration---随机外力。

World Velocity Scale---与World Acceleration Scale共同组成布料的GameObject.transfrom的运动会对物理模拟造成的影响比例。

World Acceleration Scale---与World Velocity Scale共同组成布料的GameObject.transfrom的运动会对物理模拟造成的影响比例。

![](/assets/newCloth09.png)

Friction---当布料碰到在这个列表中存在的Collider时所产生的摩擦力，这只会影响布料的模拟。上面说过了布料的物理模拟是单向的。

Collision Mass Scale---增加碰撞粒子的质量。。

Use Continuous Collision---使用Continuous Collision, 增加消耗，减少直接穿透碰撞的几率。

Use Virtual Particles---每三角形增加一个虚拟粒子以提高碰撞稳定性。

Solver Frequency---每秒钟迭代的频率。显然是一个优化参数，默认120很高了，可以试着调低一些。

Sleep Threshold---静止阈值。

Capsule Colliders---要对布料产生交互的胶囊碰撞体。

Sphere Colliders---要对布料产生交互的ClothSphereColliderPairs. 可以理解为他是按照一组来的，一组中可以只有一个SphereCollider，也可以有两个，当有两个的时候，那么这两个SphereCollider会在布料的碰撞系统中被“焊接”起来。这样就允许通过两个大小不同的SphereCollider来组合成一个圆锥形状的碰撞体了。


🔚

