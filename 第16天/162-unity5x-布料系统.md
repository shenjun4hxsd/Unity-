##Unity5.x 布料系统

Unity4中, 需要对SkinnedMeshRenderer使用SkinnedCloth, 或者对Cloth Renderer使用Interactive Cloth, 很麻烦, 在Unity5中, Cloth合二为一了.

Cloth只能必须和Skinned Mesh Renderer搭配使用, 但是这不代表使用简单的物体时还必须在Max中导出一个带有蒙皮信息的FBX, 其实可以新建一个GameObject然后赋予Cloth组件, 这会自

动添加Skinned Mesh Renderer组件, 然后在Skinned Mesh Renderer组件中赋予基本体的Mesh上去并且设置正确的材质也完全可以.

布料可以接受外部影响,但完全不会将影响赋予外部刚体, 换句话说, 布料系统的物理模拟是单向的.

![](/assets/newCloth01.png)


出于性能的考虑, 可以对Cloth产生影响的Collider只有两种, SphereCollider以及CapsuleCollider, 但是SphereCollider可以赋予两个为一组(当然了还可以只赋予一个, 第二个保持null), 那么通过组合这两个SphereCollider可以出现第三种, Unity会将这组的两个SphereCollider进行相连, 那么就可以有圆锥形的Collider

![](/assets/newCloth02.png)


打开编辑Constraint模式

![](/assets/newCloth03.png)

Max Distance可以设置每个顶点最大可移动距离. 最常用的用法是将不能动的顶点的Max Distance设置为0.
Surface Penetration控制的是顶点最大可以嵌入到Mesh里面的程度. 在布料网格顶点比较稀疏的情况下可以明显对比出差别. 将顶点的Surface Penetration设置得很低会看起来很不正常, 默认, 也就是Unconstrained应该是一个较高的值, 没有任何问题. 


![](/assets/newCloth04.png)


Visualization这里能够选择当前在Scene视图中预览Max Distance还是Surface Penetration. (这两者二选一), 还能选择是否让操作影响视口背面的顶点.

Select编辑模式要先通过框选, 或者Shift+点击来多选, 来选中顶点, 然后勾选Max Distance或者Surface Penetration前面的复选框, 代表我现在要改变选中的顶点的值了! 然后再在后面的数值里面输入想要的数值就OK了, 要想将当前有数值的顶点设置为Unconstrained, 只需要选中那些带有数值的顶点然后将对应复选框取消勾选即可.



在Paint模式中, 这个按钮代表要绘制
![](/assets/newCloth05.png)
这里设置的值了!
