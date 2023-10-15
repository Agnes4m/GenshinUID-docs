# 简单参考 <Badge type="tip" text="普通" />

::: tip

该文件位于`gsuid_core/plugins/gs_test.py`

:::

```python
import asyncio

from gsuid_core.bot import Bot
from gsuid_core.sv import SL, SV
from gsuid_core.models import Event

sv_switch = SV('测试开关')


@sv_switch.on_prefix(('关闭', '开启'))
async def get_switch_msg(bot: Bot, ev: Event):
    name = ev.text
    if not name:
        return

    await bot.send('正在进行[关闭/开启开关]')

    if name in SL.lst:
        if ev.command == '关闭':
            SL.lst[name].disable()
            await bot.send('关闭成功！')
        else:
            SL.lst[name].enable()
            await bot.send('开启成功！')
    else:
        await bot.send('未找到该服务...')


@sv_switch.on_fullmatch('全匹配测试')
async def get_fullmatch_msg(bot: Bot, ev: Event):
    await bot.send('正在进行[全匹配测试]')
    await asyncio.sleep(2)
    await bot.send('[全匹配测试]校验成功！')


@sv_switch.on_fullmatch('开始游戏')
async def get_resp_msg(bot: Bot, ev: Event):
    await bot.send('正在进行[开始游戏测试]')
    await asyncio.sleep(2)
    await bot.send('[开始游戏测试]校验成功！')
    resp = await bot.receive_resp(
        '请选择一个选项!',
        ['🎨可爱的丛林', '🚀遥远的星空', '📝不如在家写作业', '✨或者看星星', '🚧这里是维护选项'],
    )
    if resp is not None:
        await bot.send(f'你输入的是{resp.text}')


@sv_switch.on_prefix('前缀测试')
async def get_prefix_msg(bot: Bot, ev: Event):
    await bot.send('正在进行[前缀测试]')
    await asyncio.sleep(2)
    await bot.send('[前缀测试]校验成功！')


@sv_switch.on_suffix('后缀测试')
async def get_suffix_msg(bot: Bot, ev: Event):
    await bot.send('正在进行[后缀测试]')
    await asyncio.sleep(2)
    await bot.send('[后缀测试]校验成功！')


@sv_switch.on_keyword('关键词测试')
async def get_keyword_msg(bot: Bot, ev: Event):
    await bot.send('正在进行[关键词测试]')
    await asyncio.sleep(2)
    await bot.send('[关键词测试]校验成功！')


@sv_switch.on_regex(r'\d+')
async def get_regex_msg(bot: Bot, ev: Event):
    await bot.send('正在进行[正则测试]')
    await asyncio.sleep(2)
    await bot.send('[正则测试]校验成功！')
```

