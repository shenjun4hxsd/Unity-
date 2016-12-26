##AutoLayout剖析

RectTransform作为单个UI元素布局是十分灵活的，但是当一组UI元素需要规律的排布时，就需要“AutoLayout”这种组级别的布局方式。AutoLayout能决定子元素的排布方式，同时子元素也可以影响父元素的排布。相对于RectTransform，AutoLayout的功能更容易被理解和上手。


###一、概要

自动布局系统主要提供两种功能“layout controllers（布局控制器）”和“layout elements（布局元素）”。一般情况下布局控制器是父元素如何控制子元素的布局，而布局元素是子元素控制自己本身的布局大小并可以影响父元素的布局方式。


![](/assets/20151121104017.png)



###二、布局控制器

####（一）、Layout Group（布局组）

布局组提供Horizontal Layout Group（水平布局组）和Vertical Layout Group（垂直布局组），以及Grid Layout Group（网格布局组）三种功能。

这三个布局组参数基本一致，详细说明可以参考官网文档：官网文档。但也有一些差别。

#####1、水平和垂直布局组特有的属性：Child Force Expand

![](/assets/20151121141320.png)

勾选Child Force Expand，如果父元素有额外可用空间时，会促使子元素强制扩充。一般配合布局元素组件的minimum,preferred和flexible尺寸使用。

#####2、网格布局组

• Start Corner和Child Alignment的区别：Start Corner是决定子元素排放顺序的开始位置，Child Alignment是决定所有子元素作为一个整体在父元素中的排放位置。

• 网格布局组，子布局元素设定的尺寸信息无效。在网格布局组下，其子布局元素设置的minimum,preferred和flexible尺寸无法生效，只会生效网格设定的尺寸。在网格布局组下，子布局元素只能通过Ignore Layout跳出布局控制器，其它属性设置无效。

![](/assets/20151121151229.png)

####（二）、控制自身的布局控制器

Content Size Fitter（内容尺寸适配器）和Aspect Ratio Fitter（宽高比适配器）是控制自身尺寸的布局控制器。

#####Content Size Fitter（内容尺寸适配器）

![](/assets/20151121145440.png)

通过子元素设定的布局元素minimum,preferred尺寸或内容本身的显示尺寸来调整本身的尺寸信息。最常用于文本内容的父元素，父元素通过文本内容的长度动态设置自身的尺寸。

#####Aspect Ratio Fitter（宽高比适配器）

![](/assets/20151121150020.png)

通过调整自身的宽或高来对应调整另一边的尺寸，也可以选择填满父元素。和网格布局组一样，子布局元素设定的尺寸信息无效。

###三、布局元素

布局元素是一个含有RectTransform组件的GameObject，作为布局元素并不能直接更改RectTransform中的尺寸信息，只能通过挂载“layout elements component”这个组件来设置布局信息，以供布局控制器计算。
























