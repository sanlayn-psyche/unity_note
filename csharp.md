#### 表达式体成员（expression-bodied member）

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

#### 委托 Delegate

#### Event

Action，简单参数值
EventHandler，可以带复杂的自定义数据，处理复杂逻辑，还能带上触发者信息

