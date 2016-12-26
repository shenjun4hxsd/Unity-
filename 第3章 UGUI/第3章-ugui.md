##AutoLayout剖析

RectTransform作为单个UI元素布局是十分灵活的，但是当一组UI元素需要规律的排布时，就需要“AutoLayout”这种组级别的布局方式。AutoLayout能决定子元素的排布方式，同时子元素也可以影响父元素的排布。相对于RectTransform，AutoLayout的功能更容易被理解和上手。


###一、概要

自动布局系统主要提供两种功能“layout controllers（布局控制器）”和“layout elements（布局元素）”。一般情况下布局控制器是父元素如何控制子元素的布局，而布局元素是子元素控制自己本身的布局大小并可以影响父元素的布局方式。


![](/assets/20151121104017.png)



###二、布局控制器

#####（一）、Layout Group（布局组）

布局组提供Horizontal Layout Group（水平布局组）和Vertical Layout Group（垂直布局组），以及Grid Layout Group（网格布局组）三种功能。

这三个布局组参数基本一致，详细说明可以参考官网文档：官网文档。但也有一些差别。

#####1、水平和垂直布局组特有的属性：Child Force Expand

![](/assets/20151121141320.png)

勾选Child Force Expand，如果父元素有额外可用空间时，会促使子元素强制扩充。一般配合布局元素组件的minimum,preferred和flexible尺寸使用。














