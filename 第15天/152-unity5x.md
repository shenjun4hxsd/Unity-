##Unity5.x

####几个概念

```
PBS
    基于物理的Shader，支持GI。
    
    Standard以及Standard(Specular setup)两者的区别是，Standard使用了金属模拟的外观，而Standard(Specular Setup)使用了镜面模拟的外观。
使用中， 对于任何不是金属的材质，都应该选择Standard(Specular Setup)。

PBR
    基于物理的渲染

光影GI
    直接光照
    间接光照
    环境光
    镜面反射光
    
    GI分为两种：
    一种是Precomputed Reatime GI，这种GI需要预先计算，计算场景中所有的Static物体的信息，并且允许在运行时任意修改光源的Bounce Intensity或者移动光源的位置。所有的变化都是实时的。
    第二种是Baked GI，这种GI不会预先计算但会进行预先烘焙，无法像Precomputed Realtime GI那样在运行时更改光源。

    GI的重点在于Bounce，可以将Static物体上的光反弹到其他Static物体上，无法将非Static物体上的光反弹到Static物体上。也无法将Static物体上的光反弹到非Static物体上，但是Unity5引入了新的概念，那就是Realtime的LightProbe，Unity4中的LightProbe只能静态烘焙，而Unity5的LightProbe会实时捕捉包括Precomputed Realtime GI的Bounce光在内的任何光线信息并且将其赋予非Static物体上。

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



