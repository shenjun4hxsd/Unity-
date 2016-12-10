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


>hash code是一种编码方式，在Java中，每个对象都会有一个hashcode，Java可以通过这个hashcode来识别一个对象。至于hashcode的具体编码方式，比较复杂（事实上这个编码是可以由程序员通过继承和接口的实现重写的），可以参考数据结构书籍。而hashtable等结构，就是通过这个哈希实现快速查找键对象。这是他们的内部联系，但一般编程时无需了解这些，只要知道hashtable实现了一种无顺序的元素排列就可以了。.
两个对象值相同(x.equals(y) == true)，则一定有相同的hash code。