##Unity事件

####概念

    提供了委托、事件的命名空间。
    利用动作和事件两个方向实现总的监听
        Unity的事件执行不是单纯去执行某个事件集合而是可以多个集合放在它的事件函数中执行。
        简而言之：Unity的事件可以监听多个动作，每个动作又可以关联多个方法。
    Unity事件系统
        事件动作，就是一个C#语言的事件委托
        收集动作然后由统一的事件执行体来执行

####动作

    不带參动作 
        UnityAction
        在定义订阅函数时不能写行參和返回值。
    带參动作
        UnityAciton<T0>
        UnityAction<T0, T1>
        UnityAction<T0, T1, T2>
        最多只能带三个参数，最少必须有一个，不支持返回值。
    注意：对于动作，必须实例化，传订阅函数时可以不传递目标对象，直接传递订阅函数的函数地址。

####事件

    总体特性：
        AddListener
            用于追加订阅的动作，如果已经存在相同的动作了，会覆盖掉以前的动作。
        RemoveListener
            用于删除已经存在的动作，必须传递存在的UnityAction系列。
        Invoke
            在该调用的地方需要调用这个函数，让已经注册了的监听动作全部执行，可以在这里传入参数。
        FindMethod_Impl
            通过订阅的实例对象可以返回其中一个方法的方法信息列表。

    执行不带參动作：
        不带参的事件执行只能对应UnityAction动作对象。

    执行带參动作：
        必须对应UnitAction<>系列的动作，动作有多少个参，你的UnityEvent就必须有多少个参
        带参的UnityEvent你不能直接实例化，因为他加了一个 “虚类”的标签，所以你必须继承一下，在继承的时候你可以直接给予他需要行参类型，或者你也是用范型，将你的范型传递给UnityEvent<>系列函数

####总结

    对于带参和不带参的动作，一定要对应带参与不带参的事件。
    对于带参动作，我们使用委托的时候需要定义成范型函数。
        对于带参动作累加订阅的时候需要注意，你必须在委托实参外侧构建一个UnityAction<T>将其包裹，然后重新AddListener到事件中去。
    lambda
        对于UnityEvent系列类，我在AddListener时可以使用lambda写法，当然是针对我们的函数内容比较少的时候，你可以这么做。
        
---

UnityAction本质上是delegate，且有数个泛型版本（参数最多是4个）,一个UnityAction可以添加多个函数（多播委托）

UnityEvent本质上是继承自UnityEventBase的类，它的AddListener()方法能够注册UnityAction，RemoveListener能够取消注册UnityAction，还有Invoke()方法能够一次性调用所有注册了的UnityAction。UnityEvent也有数个泛型版本（参数最多也是4个），但要注意的一点是，UnityAction的所有带参数的泛型版本都是抽象类（abstract），所以如果要使用的话，需要自己声明一个类继承之，然后再实例化该类才可以使用。



```javascript
//使用UnityEvent 和 UnityAction：
using UnityEngine;
using System.Collections;
using UnityEngine.Events;

public class UnityActionAndEvent : MonoBehaviour {

    public UnityAction action;
    public UnityEvent myEvent = new UnityEvent();

    void Start()
    {       
        action = new UnityAction(MyFunction);
        action+=MyFunction2;
        myEvent.AddListener(action);
    }

    void Update()
    {
        if(Input.GetKeyDown(KeyCode.P))
        {
            myEvent.Invoke();
        }
    }

    public void MyFunction()
    {
        print ("Hello: ");
    }

    public void MyFunction2()
    {
        print ("Hello2: ");
    }

}
```


```javascript
// 使用带参数的UnityEvent
using UnityEngine;
using System.Collections;
using UnityEngine.Events;

//因为UnityEvent<T0>是抽象类，所以需要声明一个类来继承它
public class MyEvent:UnityEvent<int>{}

public class UnityActionWithParameter : MonoBehaviour {

    public MyEvent myEvent = new MyEvent();
    public UnityAction<int> action;

    void Start () {
        action= new UnityAction<int>(MyFunction);
        action += MyFunction2;
        myEvent.AddListener(action);
    }   

    void Update () {
        if(Input.GetKeyDown(KeyCode.A))
        {
            myEvent.Invoke(10);
        }
    }

    public void MyFunction(int i)
    {
        print (i);
    }
    public void MyFunction2(int i)
    {
        print(i*2);
    }
}

```

>Unity中通过面板中添加的Listener和通过脚本添加的Listener实际上是两种不同类型的Listener：

>1、在脚本中通过AddListener()添加的是一个0个参数的delegate(UnityAction)回调。是不可序列化的，在Inspector中是无法看到的。这种Listener是常规Listener。

>2、在Inspector中添加的则是永久性的Listener（persistent listener)。他们需要指定GameObject、方法以及方法需要的参数。他们是序列化的，用脚本是无法访问到的。