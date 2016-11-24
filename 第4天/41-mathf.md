##Mathf 数学函数
常见的数学函数集合。

|静态变量|说明|
|:--|:--|
|Deg2Rad|度到弧度的转化常量。（只读）|
|Epsilon|一个很小的浮点数值。（只读）|
|Infinity|表示正无穷，也就是无穷大，∞ （只读）|
|NegativeInfinity|表示负无穷，也就是无穷小，-∞ （只读）|
|PI|圆周率（π）的值3.14159265358979323846…（只读）|
|Rad2Deg|弧度到度的转化常量。（只读）|

|静态方法|说明|
|:--|:--|
|Abs|返回f的绝对值。|
|Acos|返回参数 f 的反余弦值 - 弧度角的余弦是f。|
|Approximately|比较两个浮点数值，看它们是否非常接近。|
|Asin|返回参数 f 的反正弦值 - 弧度角的正弦是f。|
|Atan|返回参数 f 的反正切值 - 弧度角的正切是f。|
|Atan2|返回弧度角的正切是y/x。|
|Ceil|返回大于或等于f的最小整数。|
|CeilToInt|返回大于或等于f的最小整数。|
|Clamp|限制value的值在min和max之间， 如果value小于min，返回min。|
|Clamp01|限制value在0,1之间并返回value。如果value小于0，返回0。|
|ClosestPowerOfTwo|返回距离value最近的2的次方数。|
|Cos|返回由参数 f 指定的角的余弦值（介于 -1.0 与 1.0 之间的值）。|
|DeltaAngle|计算给定的两个角之间最短的差异。|
|Exp|返回 e 的 power 次方的值。|
|Floor|返回小于或等于该数的最大整数。|
|FloorToInt|返回小于或等于该数的最大整数。|
|GammaToLinearSpace|转换给定值从gamma到线性颜色空间。|
|InverseLerp|计算两个值之间的Lerp参数。也就是value在from和to之间的比例值。|
|IsPowerOfTwo|如果该值是2的次方，返回true。|
|Lerp|基于浮点数t返回a到b之间的插值，t限制在0～1之间。|
|LerpAngle|和Lerp的原理一样，当他们环绕360度确保插值正确。|
|LinearToGammaSpace|转换给定值从线性到伽马颜色空间。|
|Log|返回指定基数的指定对数。|
|Log10|返回指定值的对数，基数为10。|
|Max|返回两个或更多值中最大的值。|
|Min|返回两个或更多值中最小的值。|
|MoveTowards|改变一个当前值向目标值靠近。|
|MoveTowardsAngle|像MoveTowards,但是当它们环绕360度时确保插值正确。|
|NextPowerOfTwo|返回最接近的二次方的值。|
|PerlinNoise|生成二维柏林噪波。|
|PingPong|让数值t在 0到length之间往返。t值永远不会大于length的值，也永远不会小于0。|
|Pow|计算并返回 f 的 p 次幂。|
|Repeat|循环t值，从来不会比length大，并且从不会小于0，取值在0~length之间。|
|Round|返回浮点数 f 进行四舍五入最接近的整数。|
|RoundToInt|返回浮点数 f 进行四舍五入最接近的整数。|
|Sign|返回 f 的数字符号。|
|Sin|返回弧度角 f 的正弦值。|
|SmoothDamp|随着时间的推移逐渐改变一个值到目标值。|
|SmoothDampAngle|随着时间的推移逐渐改变一个给定的角度到期望的角度。|
|SmoothStep|min和max之间平滑插值。|
|Sqrt|计算并返回 f 的平方根。|
|Tan|返回互动f角的正切值。|

🔚
