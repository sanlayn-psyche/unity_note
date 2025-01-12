### 获取输入
旧版系统：
Edit-> Project Settings-> Input Manger
里面可以找到 Axes
在 script 里面通过 Input.GetAxis 访问相应的名称

### Button
```C#
 class AbilityButton: MonoBehaviour
 {

 }

 class ButtonPannel: MonoBehaviour
 {
     [SerializeField]
     private AbilityButton button_prefab;

     private void OnEnable()
     {
         foreach (string name in AbilityFactory.Instance.get_ability_names())
         {
             var buttom = Instantiate(button_prefab);
             buttom.gameObject.name = name;
             //buttom.setAbilityName();
             buttom.transform.SetParent(transform);
         }
     }


 }

```



GameObject 与 Transform 有什么不同

### 模块化

#### 目前策略
配置文件为 ScriptableObject

```csharp
public abstract class MoveGenerator : ScriptableObject
{
    public abstract void get_move(ref Vector3 move);
}

public abstract class PositionModifier : ScriptableObject
{
    public abstract void act_mdf(Transform trans);
}

public class MoveDelegate : MonoBehaviour
{
    [SerializeField]
    private MoveGenerator generator;

    [SerializeField]
    private PositionModifier[] modifiers;
    public delegate void __delegate(Transform trans);
 
    private __delegate act_mdf;
}
```
每个配置文件除了属于自己的配置参数外，是完全的 readonly method class，所有绑定了该配置文件的 delegate 实际上访问的是同一个实例。
这样做无法解决动态改变配置属性的需求。


### 设计模式

### 工厂模式

包括使用对象池：通过预分配和重用对象，避免频繁的对象创建和销毁，从而减少垃圾回收的开销。
```C#
public abstract class Ability
{
    public abstract string Name { get; }
    public abstract void Process();
}

public class FireAbility : Ability
{
    public override string Name => "Fire";

    public override void Process()
    {
        throw new NotImplementedException();
    }
}

public class AbilityFactory
{
    private Dictionary<string, Type> _abilities;

    public AbilityFactory() 
    {
        var ability_type = Assembly.GetAssembly(typeof(Ability)).GetTypes()
            .Where(T => T.IsClass && !T.IsAbstract && T.IsSubclassOf(typeof(Ability)));

        _abilities = new Dictionary<string, Type>();
        foreach (var type in ability_type)
        {
            var temp = Activator.CreateInstance(type) as Ability;
            _abilities.Add(temp.Name, type);
        }
    }

    public Ability get_ability(string type)
    {
        if (_abilities.ContainsKey(type))
        {
            Type atype = _abilities[type];
            return Activator.CreateInstance(atype) as Ability;
        }
        return null;
    }
}
```

#### 单例模式

#### Event-Trigger

#### 待处理列表

#### Component

#### Flyweight

#### 状态机
状态机
