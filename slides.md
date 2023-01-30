---
theme: seriph
background: https://s1.ax1x.com/2022/07/15/j4ugET.png
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: fade
preload: false
---

<h1 class="text-green-200">Python botpy</h1>

<div class="text-green-200">基于 机器人开放平台API 实现的机器人框架</div>

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/tencent-connect/botpy" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

---

# SDK提供平台
[QQ开放平台](https://q.qq.com/#/)
<img src="https://qqminiapp.cdn-go.cn/open-platform/1e1f57af/img/name.9193f0c3.png">

<img src="https://qqminiapp.cdn-go.cn/open-platform/1e1f57af/img/robot.b167c62c.png">


<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
img{
  display: block;
  margin: 0 auto;
}
</style>

---
layout: image-left
image: https://s1.ax1x.com/2022/07/15/j4u2UU.png
---

# 机器人实现


<div class="text-xl mt-12rem text-green-300">TypeScriptActions</div>


<style>
  h1 {
    color: white;
  }
  div{

    font-size: 3rem;
  }
</style>


---
layout: two-cols
---

# 目录

<br>

<Toc columns="2" />






---
layout: full
---
# 目前的功能
<br>

`基础回复`

- ping -----------------------------------------`会返回pong 🐹` `(证明存活呢没坏)`
- @TypeScriptActions /ping   ----------------- `和上一条一样的功能`
- 赢/win ---------------------------------------`返回Win!` `(没什么 就是赢！)`
- 盒-------------------------------------------- `返回 哪里有盒？开盒！😋 ` `(开盒咯)`
- 🥰-------------------------------------------`返回 🥰`
- 👿/😈---------------------------------------`返回 😈/👿` `(没什么 就是恶魔头)`
- 🤡-------------------------------------------`返回一张图 《小丑竟在我身边》`
- @TypeScriptActions 叫----------------------`让机器人叫你些什么（一般是叫爹）`
- 骰子------------------------------------------`返回一个随机数`
- xxx我好喜欢你suki---------------------------`从17句共4225字的发病文学中随机选取一句`


---
layout: full
---

# 目前的功能
<br>

`调用api的回复`
- @TypeScriptActions 一言 ---------------------`返回一句话`
- @TypeScriptActions 毒鸡汤 -------------------`返回一句话（毒鸡汤）`
- 历史上的今天 ----------------------------------`返回历史上的今天`
- 历史上的xx-xx---------------------------------`返回xx月xx日历史上的今天`
- 天气 -------------------------------------------`返回钱塘24小时天气预报`
- @TypeScriptActions 我谁 --------------------`返回一个ARK模板 内容包括我的头像、名称、加入频道的日期`
- @xxx 你谁 -------------------------------------`返回一个ARK模板 内容包括xxx的头像、名称、加入频道的日期`


---
layout: full
---

# 目前的功能
<br>

`调用数据库的回复`
- @TypeScriptActions /签到 ----------------------`返回签到信息`
- @TypeScriptActions 积分 -----------------------`返回积分信息`
- @TypeScriptActions 排名 -----------------------`返回排名信息`



---
layout: full
---

# 目前的功能
<br>

`测试中的好玩的功能`
- 情绪分析



[![jo6L6g.png](https://s1.ax1x.com/2022/07/18/jo6L6g.png)](https://imgtu.com/i/jo6L6g)
<br>

[百度智能云对话情绪识别](https://ai.baidu.com/ai-doc/NLP/rk6z52hlz)

---
layout: full
---

# 目前的功能
<br>

`测试中的好玩的功能`
- 情绪分析



[![jo6L6g.png](https://s1.ax1x.com/2022/07/15/j4ua4g.png)](https://s1.ax1x.com/2022/07/15/j4ua4g.png)
<br>

[百度智能云对话情绪识别](https://ai.baidu.com/ai-doc/NLP/rk6z52hlz)

---

# Code

文件目录结构

```ts {all|1|2|3|4|5|6|7|8|9|10|11|12|13|14|all}
│  active_speak.py              # 主动说话（上号夺舍咯）
│  apis.py                      # api调用
│  botpy.log                    # 日志文件
│  config.yaml                  # 配置文件
│  douapi.py                    # api调用
│  emoji.py                     # emoji表情
│  forbid.py                    # 禁止广告
│  history.py                   # 历史上的今天
│  main.py                      # 主程序
│  mongo.py                     # mongo数据库
│  reply.py                     # 回复
│  sign.py                      # 签到
│  weather.py                   # 天气
│  who.py                       # 查询用户信息
```

<style>
.footnotes-sep {
  @apply mt-20 opacity-10;
}
.footnotes {
  @apply text-sm opacity-75;
}
.footnote-backref {
  display: none;
}
</style>


---


# Code

```python {monaco}
import datetime
from botpy.message import Message
from playwright.async_api import async_playwright

async def get_history(self, message: Message):
    day = str(datetime.datetime.now())[5:10].replace('-', '')
    url = 'http://www.people.com.cn/GB/historic/{}/'.format(day)
    async with async_playwright() as p:
        browser = await p.chromium.launch()
        page = await browser.new_page()
        await page.goto(url)
        a = await page.locator("td.t14l14").inner_text()
        await browser.close()
        await self.api.post_message(channel_id=message.channel_id, content=a[:-1], msg_id=message.id)

async def get_someday_history(self, message: Message, day):
    url = 'http://www.people.com.cn/GB/historic/{}/'.format(str(day))
    async with async_playwright() as p:
        browser = await p.chromium.launch()
        page = await browser.new_page()
        await page.goto(url)
        a = await page.locator("td.t14l14").inner_text()
        await browser.close()
        await self.api.post_message(channel_id=message.channel_id, content=a[:-1], msg_id=message.id)

```


---
layout: fact
---

# 数据分析

<Chart />

---
layout: center
preload: false
---
# Powered By Slidev

<div class="w-60 relative mt-6">
  <div class="relative w-40 h-40">
    <img
      v-motion
      :initial="{ x: 800, y: -100, scale: 1.5, rotate: -50 }"
      :enter="final"
      class="absolute top-0 left-0 right-0 bottom-0"
      src="https://sli.dev/logo-square.png"
    />
    <img
      v-motion
      :initial="{ y: 500, x: -100, scale: 2 }"
      :enter="final"
      class="absolute top-0 left-0 right-0 bottom-0"
      src="https://sli.dev/logo-circle.png"
    />
    <img
      v-motion
      :initial="{ x: 600, y: 400, scale: 2, rotate: 100 }"
      :enter="final"
      class="absolute top-0 left-0 right-0 bottom-0"
      src="https://sli.dev/logo-triangle.png"
    />
  </div>

  <div
    class="text-5xl absolute top-14 left-40 text-[#2B90B6] -z-1"
    v-motion
    :initial="{ x: -80, opacity: 0}"
    :enter="{ x: 0, opacity: 1, transition: { delay: 2000, duration: 1000 } }">
    Slidev
  </div>
</div>

<!-- vue script setup scripts can be directly used in markdown, and will only affects current page -->
<script setup lang="ts">
const final = {
  x: 0,
  y: 0,
  rotate: 0,
  scale: 1,
  transition: {
    type: 'spring',
    damping: 10,
    stiffness: 20,
    mass: 2
  }
}
</script>

<div
  v-motion
  :initial="{ x:35, y: 40, opacity: 0}"
  :enter="{ y: 0, opacity: 1, transition: { delay: 3500 } }">

[了解一下](https://sli.dev/guide/animations.html#motion)

</div>
