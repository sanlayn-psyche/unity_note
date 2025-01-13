### 社区
Reddit的r/gamedev：这个社区非常活跃，有很多关于游戏开发的讨论和资源分享。

IndieDB：一个专门为独立游戏开发者设计的平台，可以发布游戏、寻找合作伙伴和获得反馈。

TIGSource：一个关注独立游戏开发的论坛，有很多关于游戏设计、编程和艺术的讨论。

Game Jolt：一个游戏开发者社区，提供游戏发布、合作和获得玩家反馈的平台。

Itch.io：一个非常流行的独立游戏平台，开发者可以发布游戏并与玩家互动。

### 获取输入
旧版系统：
Edit-> Project Settings-> Input Manger
里面可以找到 Axes
在 script 里面通过 Input.GetAxis 访问相应的名称
新版系统：

在 Package Manager 中安装 Input System。
在 Project Serring -> Player -> OtherSetting -> Active Input Handling 中选择新版系统

新版的输入系统是以一个 Asset 的形式加入项目的，需要创建 Input Actions，双击 Asset 进入设置。

新版input system 有不同的使用方式
1. Componen：可以为game object添加 player input 的 Component 
2. C# code
    * 点击 Input Actions Asset，选择生成 C# class
    * 创建相应的 player input action 的实例，在需要用到的地方获取handle，并且激活它 （Enable）
    * 获取输入时，调用 player_input_action.Player.Move.ReadValue<Vector2>();

### RayCast

求交时可以通过 layerMask 来规定只于哪一类单位求交，在 Inspector 的 Tag 旁边设置 Layer

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

#### 工厂模式

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

一个比较简单的做法，将 Instance 设置为 {get; private set;}
初始化时 Instance = this;


#### Event-Trigger

#### 待处理列表

#### Component

#### Flyweight

#### 状态机
状态机
