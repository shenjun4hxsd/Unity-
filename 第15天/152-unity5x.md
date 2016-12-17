##Unity5.x

####几个概念

```
PBS
    基于物理的Shader
    
    Standard以及Standard(Specular setup)两者的区别是，Standard使用了金属模拟的外观，而Standard(Specular Setup)使用了镜面模拟的外观。
使用中， 对于任何不是金属的材质，都应该选择Standard(Specular Setup)。

PBR
    基于物理的渲染

光影GI
    直接光照
    间接光照
    环境光
    镜面反射光

Light Probe
    光照探头

Reflection Probe
    反射探头
    Reflection Resource

UV
    三个UV通道
        贴图UV通道
        光照贴图UV通道
        Procomputed Realtime GI通道

RenderPath
    Forward
        静态和动态相结合
    Deferred
        使用于PC与主机端
        完全的动态光影

```



