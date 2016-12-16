##Unity5.x 布料系统

Unity4中, 需要对SkinnedMeshRenderer使用SkinnedCloth, 或者对Cloth Renderer使用Interactive Cloth, 很麻烦, 在Unity5中, Cloth合二为一了.

Cloth只能必须和Skinned Mesh Renderer搭配使用, 但是这不代表使用简单的物体时还必须在Max中导出一个带有蒙皮信息的FBX, 其实可以新建一个GameObject然后赋予Cloth组件, 这会自

动添加Skinned Mesh Renderer组件, 然后在Skinned Mesh Renderer组件中赋予基本体的Mesh上去并且设置正确的材质也完全可以.

布料可以接受外部影响,但完全不会将影响赋予外部刚体, 换句话说, 布料系统的物理模拟是单向的.