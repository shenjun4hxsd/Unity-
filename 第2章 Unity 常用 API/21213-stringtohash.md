##Animator.StringToHash 字符串到哈希

**static int StringToHash(string name);**

|参数|说明|
|:--|:--|
|name|该字符串转换到ID|


描述：

从字符串生成一个参数ID。

ID是用于参数的存取器优化（setters 和 getters）。




>Hash，一般翻译做“散列”，也有直接音译为"哈希"的，就是把任意长度的输入（又叫做预映射， pre-maping），通过散列算法，变换成固定长度的输出，该输出就是散列值。这种转换是一种压缩映射，也就是，散列值的空间通常远小于输入的空间，不同的输入可能会散列成相同的输出，而不可能从散列值来唯一的确定输入值。

>1) 对象相等则hashCode一定相等；
2) hashCode相等对象未必相等。