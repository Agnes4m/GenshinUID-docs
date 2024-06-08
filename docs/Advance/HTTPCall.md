# HTTP调用<Badge type="tip" text="普通" />

:::info

在某些情况下，你的**业务需求**可能并不允许你进行WS连接，

这时候就需要使用HTTP调用方式。

HTTP调用方式**依旧需要提供**完整的MessageReceive结构体。

:::

1. 打开GsCore的HTTP模式（和WS**并不冲突**，但仍然建议**不使用时关闭**）
   - 打开`gsuid_core/gsuid_core/config.json`
   - 找到`ENABLE_HTTP`配置项，将其设置为`true`
2. 依旧是默认8765端口（如有自行修改，以修改的为准），终结点为`/api/send_msg`
3. POST方法，数据内容为`MessageReceive`，[数据结构](../CodeAdapter/Pack)，以下为调用示例：

```python
async def http_test():
    msg = to_builtins(
        MessageReceive(
            content=[
                MessageSegment.text('强制刷新'),
            ]
        )
    )

    async with httpx.AsyncClient(timeout=20) as client:
        response = await client.post(
            'http://127.0.0.1:8765/api/send_msg',
            json=msg,
        )
        print(response.text)
        print(response.status_code)
```

4. 返回结构为一个字典，并带有两个参数：

   | Key列表     | Value列表   | 可能的值                                |
   | ----------- | ----------- | --------------------------------------- |
   | status_code | int         | 200：成功<br />-100：失败               |
   | data        | MessageSend | 成功为：`MessageSend`<br />失败为：None |

   