# FAQ 常见问题解答

## 覆写RecieveTick无效？

只有当蓝图中的Tick事件启用时，Lua中的Tick才会执行。这个是UE自己的设计，开启Tick有两种方法，一种是在C++中把 `bCanEverTick` 设为true，另一种就是在蓝图中创建 `Event Tick` 节点，可见 `FKismetCompilerContext::SetCanEverTick()` 函数。

## Lua代码打包后加载不到？

在 "Additional Non-Asset Directories to Package" 一栏中加入Script目录。

## 蓝图函数带了空格，在Lua里如何调用？

可以使用Lua原生的语言特性，通过 `[]` 来访问得到 `function` 对象后调用。

```lua
self["function name with space"]() -- “静态函数”
self["function name with space"](self) -- “实例函数”
```

## 为什么覆写蓝图方法时，都需要带上 `Receive` 这样的前缀？

我们在蓝图中看到的方法名，很多都是经过对阅读友好化处理过的，以 `Actor` 上最常见的 `BeginPlay` 为例，在C++中是这样的：

```cpp
    /** Event when play begins for this actor. */
    UFUNCTION(BlueprintImplementableEvent, meta=(DisplayName = "BeginPlay"))
	void ReceiveBeginPlay();
```

实际上蓝图中对应的就是 `ReceiveBeginPlay` 这个方法，只是通过 `DisplayName` 让我们在蓝图里看起来更友好一些。

## 多继承层次里不能覆写接口的方法吗？

只能够覆写标记了 `BlueprintNativeEvent` 和 `BlueprintImplementationEvent` 的 `UFUNCTION`。

## 有没有办法只重写蓝图中的部分逻辑？

可以将部分逻辑合并成函数（Collapse To Function），然后在Lua中覆写这个函数。

## 如何使用调试？

参考文档：[调试](Debugging.md)

## 如何使用IntelliSense（智能提示）？

参考文档：[智能提示](IntelliSense.md)

## 有哪些产品正在使用UnLua？

腾讯内部已知的有四十款左右项目在使用UnLua，外部项目暂时无法统计。
