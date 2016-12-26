##AutoLayout剖析

RectTransform作为单个UI元素布局是十分灵活的，但是当一组UI元素需要规律的排布时，就需要“AutoLayout”这种组级别的布局方式。AutoLayout能决定子元素的排布方式，同时子元素也可以影响父元素的排布。相对于RectTransform，AutoLayout的功能更容易被理解和上手。


###一、概要

自动布局系统主要提供两种功能“layout controllers（布局控制器）”和“layout elements（布局元素）”。一般情况下布局控制器是父元素如何控制子元素的布局，而布局元素是子素控制自己本身的布局大小并可以影响父元素的布局方式。



























