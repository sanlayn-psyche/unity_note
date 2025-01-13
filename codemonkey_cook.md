
### Player: KitchenObjectParent
* 处理移动指令
    * 相关参数设置
    * 碰撞判断Physics.Cast，并进行复杂的转向修正
* 处理交互指令
    * 交互时可以通过求交指令获取于当前 game object 相交物体的 handle，通过 TryGetComponent or GetComponent 知道它是什么类型，进而触发交互对象的 Interact
* alt 交互 
* 记录当前交互的 ClearCounter


### GameInput
处理玩家的输入。在 Player 中保存一个到 Input Script 的引用，用于获取移动指令；

其它非持续性的按键操作实际上可以通过 Event 机制处理。此处具体的处理方式是，GameInput监听按键操作，触发事件，事件再由别的类监听。而不是由别的类直接监听按键操作。

### BaseCounter
Counter 的虚函数声明，包括 Ineract，altInteract

### ClearCounter: BaseCounter,KitchenObjectParent

### ContainerCounter: BaseCounter,KitchenObjectParent
绑定一个记录物品的 ScriptObject，当触发交互事件时生成相应 prefab 实例
记录当前桌上是否有东西

### CuttingCounter: BaseCounter
切菜，保存 CuttingRecipeSO，通过alt交互删除 input 创建 output
cutting progress

### ContainerCounterVisual
绑定 Counter 的视觉组件，触发开关门动作

### SelectedCounterVisual
监听 Player 发布的 ClearCounter 选择事件， 同时链接到相应的 ClearCounter ClearCounter 时隐藏自身
绑定 Counter 的组件

### KitchenObject
绑定记录该物品的 ScriptObject，绑定到 prefab
记录当前所在 KitchenObjectParent
设置自己所在位置
生成static或摧毁 KitchenObject

### KitchenObjectSO
模型，图标等信息

### CuttingRecipeSO
模型，图标等信息，记录：KitchenObjectSO 的 input->output
cutting progress max
发布 progress change event 信息

### PlayerAnimation
？？

### Interface KitchenObjectParent
追踪位置、获取Handle
放置 KitchenObject

### ProgressBarUI
跟 Image 绑定在一起
通过 progress change event 改变 image 的填充量