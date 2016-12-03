##Gizmos 可视化辅助工具

Gizmos是用于在场景视图可视化调试或辅助设置。

所有gizmo绘制需要在脚本的OnDrawGizmos或OnDrawGizmosSelected里函数完成。

OnDrawGizmos在每帧调用。所有在OnDrawGizmos中渲染的gizmos都是可见的。OnDrawGizmosSelected仅在脚本附加的物体被选择时被调用。

|静态变量|说明|
|:--|:--|
|color|为随后绘制的gizmos设置颜色。|
|matrix|设置gizmo的矩阵用于绘制所有gizmos。|


|静态方法|说明|
|:--|:--|
|DrawCube|使用center和size参数，绘制一个实心立方体。|
|DrawFrustum|绘制相机可视区域，用当前的Gizmos.matrix设置它的位置和旋转。|
|DrawGUITexture|在屏幕上绘制纹理。|
|DrawIcon|在场景视图世界位置绘制一个图标。|
|DrawLine|绘制一条从from起点到to位置的线段。|
|DrawMesh|绘制一个网格。|
|DrawRay|绘制从起点沿正方向延伸的射线。|
|DrawSphere|使用center和radius参数，绘制一个实心球体。|
|DrawWireCube|使用center和size参数，绘制一个线框立方体。|
|DrawWireMesh|绘制一个线框网格。|
|DrawWireSphere|根据center和radius参数设置线框球体。|



```javascript
    using UnityEngine;
    using System.Collections;
 
    public class ExampleClass : MonoBehaviour {
        void OnDrawGizmosSelected() {
            Gizmos.color = new Color(1, 0, 0, 0.5F);
            Gizmos.DrawCube(transform.position, new Vector3(1, 1, 1));
        }
    }
```


