##XPath详解

1)"/bookstore/book/price”
XPath的开头是一个斜线（/）代表这是绝对路径，这可以选出所有路径符合这个模式的元素。这代表选出所有bookstore根节点下的book元素下的price元素。

2)"//title”
开头是两个斜线（//）表示文件中所有符合模式的元素都会被选出来，即使是处于树中不同的层级也会被选出来。这代表选出所有title元素。

3)"/bookstore/book/author/\*”
使用星号（*）可以选择未知的元素。这代表选出bookstore根节点下的book节点下的anthor节点下的所有可能的节点。

4)"/bookstore/book/*/age”
上面的语法会选出所有bookstore根节点下的book节点的隔一个层次的age节点。

5)"/\*/*/age”
选取有两个层级的age元素

6)"bookstore/book[1]/title”
使用中括号可以选择分支。这代表选择bookstore根节点下第一个book节点的title节点。

7)"bookstore/book[last()]/title”
同样，这里表示选择最后一个节点

8)"bookstore/book/author[sex]”
这里表示选取所有带有sex元素并满足一定层次结构的author节点

9)"/bookstore/book[price=66.5]/title”
选取price元素值是66.5的book节点的title子节点

10)"/bookstore/book/title | /bookstore/book/price”
使用Or操作数(|)就可以选择一个以上的路径。这里表示选取符合一定层次结构的title元素与price元素

11)"//@name"
在XPath中，除了选择元素以外，也可以选择属性，属性都是以@开头。这里表示选取所有的name属性。

12)"//author[@name]/age”
这里表示选取所有具有name属性的author节点的age节点。

13)"//\*[@*]”
选取所有有属性的节点。

14)"//author[@name='Wang']/sex”
选取所有属性name值为Wang的author节点的sex子节点。