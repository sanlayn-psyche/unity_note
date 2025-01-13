### 表达式体成员（expression-bodied member）

```c#
public int Add(int a, int b)
{
    return a + b;
}
public int Add(int a, int b) => a + b;


public class MyClass
{
    private int _value;

    // 普通属性
    public int Value
    {
        get { return _value; }
        set { _value = value; }
    }

    // 表达式体属性
    public int Value
    {
        get => _value;
        set => _value = value;
    }
}

```

### 委托 Delegate

#### Event
Delegate 的一种

Action，简单参数值
EventHandler，可以带复杂的自定义数据，处理复杂逻辑，还能带上触发者信息

### 反射 Reflection
反射指的是程序在运行时能够“反射”自身，查看和操作自身的结构和行为。更深入的理解是，通过类型本身能获取到的信息，能执行的操作是非常有限的，从过反射能获取编译器生成的元数据，能以非常 High Level的角度进行操作。
Examples：
```
using System;
using System.Reflection;

public class Example
{
    public string Name { get; set; }

    public void PrintName()
    {
        Console.WriteLine(Name);
    }
}

public class Program
{
    public static void Main()
    {
        // 获取类型信息
        Type type = typeof(Example);

        // 动态创建对象
        object instance = Activator.CreateInstance(type);

        // 动态设置属性值
        PropertyInfo property = type.GetProperty("Name");
        property.SetValue(instance, "Reflection Example");

        // 动态调用方法
        MethodInfo method = type.GetMethod("PrintName");
        method.Invoke(instance, null);
    }
}
```

不过反射操作的开销较大，实际运行中需尽量通过表达式树，delegate 等方式减少反射

