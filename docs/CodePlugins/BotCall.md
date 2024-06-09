# CallBot<Badge type="tip" text="简单" />

::: tip

进行发送消息时需要获取**当前准确**的**Bot实例**，用于调用`Bot.send()`

但在某些情况下，可能函数嵌套的层数过多，外层的Bot实例一层一层传入较为麻烦

我们可以使用**自带**的**Bot模块**中的`call_bot()`方法

找到当前的、准确的Bot实例

:::

代码参考如下：

```python
from gsuid_core.bot import call_bot

...

async def func(a: str, b: str):
    # 假设这是一个嵌套很多层的函数，传参不太好修改
    print(a)
    print(b)
    
    # 但是仍然想让bot在运行到该行时发送消息
    bot = call_bot()
    # 获取当前Bot实例，以及发送消息
    await bot.send(f'{a} + {b}')
    
    # 也可以写成一行
    await call_bot().send(f'{a} + {b}')
    
```

这么做的**好处**：

- 可以避免变量`bot`在一层一层函数间传递，结构化代码并且避免过度耦合
- 某些代码历史缘故，不太好增加或删除传参，但又想发送消息

这么做的**坏处**：

- Debug时难以定位具体发送位置
- 会有一定性能损耗（存在查找当前frame存在的实例过程）

该方法**不支持的**！！：

- 你的当前函数**运行于**一个**不存在于任何一个有效Bot的帧**中
  - 例如定时任务：无法直接使用`await call_bot().send()`，但是可以使用`await call_bot().target_send()`
  - 例如某些工具函数，不由Bot响应

