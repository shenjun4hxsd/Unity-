##输入控制（Input）

输入系统的接口，用来监视我们所有的硬件输入设备（麦克风除外）。

Input 这个类能够读取输入管理器设置的按键，以及访问移动设备的多点触控或加速感应数据。

Input 这个类给我们提供的属性和方法全是静态的。

---

|键盘控制|说明|
|:--|:--|
|GetKey|当用户按下由name名称确定的按键时，返回 true。|
|GetKeyDown|当用户按下指定名称的按键时的那一帧返回 true。|
|GetKeyUp|在用户释放给定名字的按键的那一帧返回 true。|

**示例1:**
```csharp
    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour
    {
        public void Update()
        {
            // 当按下 数字键1 的那一帧，在控制台输出鼠标在屏幕上的位置。
            if(Input.GetKeyDown(KeyCode.Alpha1))
            {
                Debug.Log(Input.mousePosition);
            }
        }
    }
```

**示例2:**
```csharp

    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour
    {
        public void Update()
        {
            // 当按下a键的那一帧，在控制台输出鼠标在屏幕上的位置。
            if(Input.GetKeyDown("a"))
            {
                Debug.Log(Input.mousePosition);
            }
        }
    }
```

---

|鼠标的控制|说明|
|:--|:--|
|GetMouseButton|当指定的鼠标按钮被按下时返回 true。|
|GetMouseButtonDown|在用户按下指定鼠标按键的那一帧返回 true。|
|GetMouseButtonUp|在用户释放指定鼠标按键的那一帧返回 true。|

>数字键 0 表示鼠标左键；数字键 1 表示鼠标右键；数字键 2 表示鼠标中键。

示例：
```csharp

    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour
    {
        public void Update()
        {
            // 当按下鼠标左键的那一帧，在控制台输出鼠标在屏幕上的位置。
            if(Input.GetMouseButtonDown(0))
            {
                Debug.Log(Input.mousePosition);
            }
        }
    }
```

---

|轴值的判断|说明|
|:--|:--|
|GetAxis| 根据 `axisName` 名称返回虚拟输入轴中的值。|
|GetAxisRaw|通过 `axisName` 名称返回一个不使用平滑滤波器的虚拟轴值。|

>默认轴：

>`Horizontal` 和 `Vertical` 映射于控制杆、A、W、S、D 和箭头键（方向键）。

>`Mouse X` 和 `Mouse Y` 映射于鼠标。

**示例1:**
```csharp

    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour {

        public float speed = 10.0F;
        public float rotationSpeed = 100.0F;

        void Update() {
            float translation = Input.GetAxis("Vertical") * speed;
            float rotation = Input.GetAxis("Horizontal") * rotationSpeed;
            translation *= Time.deltaTime;
            rotation *= Time.deltaTime;
            transform.Translate(0, 0, translation);
            transform.Rotate(0, rotation, 0);
        }
    }
```

**示例2:**
```csharp

    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour
    {
        public float horizontalSpeed = 2.0F;
        public float verticalSpeed = 2.0F;

        void Update() {

            float h = horizontalSpeed * Input.GetAxis("Mouse X");
            float v = verticalSpeed * Input.GetAxis("Mouse Y");

            transform.Rotate(v, h, 0);
        }
    }
```

---

|功能键|说明|
|:--|:--|
|GetButton|当由 `buttonname` 确定的虚拟按键被按下时，返回 true。|
|GetButtonDown|在用户按下由 `buttonName` 名称确定的虚拟按钮的那一帧返回true。|
|GetButtonUp|在用户释放根据 `buttonName` 名称的虚拟按钮时返回 true。|

**示例：**
```csharp

    using UnityEngine;
    using System.Collections;

    public class ExampleClass : MonoBehaviour
    {
        public GameObject projectile;

        void Update()
        {
            if (Input.GetButtonDown("Fire1"))
                Instantiate(projectile, transform.position, transform.rotation);
        }
    }
```

|其他方法|说明|
|:--|:--|
|ResetInputAxes|在一帧中重置所有的输入，重置输入指令之后所有的方向轴都被设置为 0 并且所有的按键都被设置为 0。|
|GetTouch|返回一个存放触摸信息的对象（不允许分配临时变量）。|
|GetAccelerationEvent|返回上一帧发生的指定的加速度测量（不允许分配临时变量）。|
|GetJoystickNames|返回一个用来描述已连接的摇杆的字符串数组。|
|IsJoystickPreconfigured|确定Unity是否一个特定的操纵杆模型已预先配置。（仅Linux）。|

|静态变量|说明|
|:--|:--|
|acceleration|上一次测量的设备在三维空间中的线性加速度（只读）。|
|accelerationEventCount|上一帧发生的所测量的的加速度次数。|
|accelerationEvents|返回上一帧测量的加速度数据列表（只读）（分配临时变量）|
|anyKey|当前是否有任意键或鼠标键被按下（只读）？|
|anyKeyDown|在用户按下任意按键或鼠标按键首帧返回true（只读）。|
|compass|访问指南针的属性（仅手持设备）。|
|compensateSensors|该属性控制输入传感器在屏幕方向的补偿。|
|compositionCursorPos|当前文本输入位置，使用于IME来打开窗口。|
|compositionString|当前用户正在输入的IME组合字符串。|
|deviceOrientation|由操作系统所报告的设备的物理方向（只读）。|
|gyro|返回默认的陀螺仪。|
|imeCompositionMode|控制IME输入组合的启用和禁用。|
|imeIsSelected|用户是否已选择IME键盘输入源?|
|inputString|返回在这一帧的键盘输入（只读）|
|location|用于访问设备位置信息（仅手持设备）（只读）。|
|**mousePosition**|在屏幕坐标空间当前鼠标的位置（只读）。|
|mouseScrollDelta|当前鼠标滚动增量（只读）。|
|multiTouchEnabled|指示系统是否启用多点触控。|
|simulateMouseWithTouches|启用/禁用鼠标模拟触摸。默认情况下启用此项。|
|touchCount|触摸的数量。每一帧之内都一定不会改变（只读）|
|touches|返回代表上一帧所有的触摸状态的对象列表（只读）（分配临时变量）|
|touchSupported|返回设备是否在当前运行的应用程序支持触摸输入。|


🔚
