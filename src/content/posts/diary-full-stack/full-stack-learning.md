---
title: 一个普通人的全栈学习日记
published: 2025-09-26
description: '我为仰望炽阳之蚍蜉，憧憬那闪烁永夜的天光'
image: './universe.jpg'
tags: [学习记录, 前端, Web]
category: '日记-全栈'
draft: false 
lang: ''
---
## 建立缘由

我是个很普通的人，自然也会有偷懒，迷茫，恍惚的时光。写这份日记是为了记录自己，看着自己一天天，一点点向上攀爬的样子。希望多年后自己能成为很厉害的人，回首时一边看着自己的来时路，一边哑然失笑。

## 对看这份日记的人，一些想说的

如果您是个很厉害的，已经在半山腰或是临近山巅的成熟开发者，这只不过是个小萌新步履蹒跚的玩具日记罢了。但如果您也是个对计算机，对前端有些兴趣，但却患得患失，不知从何入手的普通人，或许它给您心中的困惑带来些答案。

老实说我是个很懒的人，也不喜欢写文档，写文章。我一直秉持着“不是同一个世界的人，再怎么接近，也终究无法重合”和观点。因此我其实对docs持悲观态度，这份日记也主要是为了我自己而已。

不过每个人都有自己的局限性，我也不例外。这份日记基本算我的备忘录和流水账日常，因此只是一份参考。同时屏幕对面的您要做好一个准备就是：**<u>您不一定能完全复刻我的道路</u>**。这不是对您的看轻，不屑或是歧视，而是字面意思上的陈述。我是个很平庸但比较幸运的孩子，在我的一路有很多对我影响深刻的人，或是前辈或是朋友也或是陌路，都对我帮助了很多很多。~~某种意义上，我是个百家理念的集成品（？~~ 可以说是拾人牙慧，也可以说是被“托举而上”的。同时我的专业技术虽然不怎么样，但我对计算机有较高的亲和度，从源头追溯可以到小学三四年级乃至幼儿园时期。

在当今社会环境下，我见过太多太多在大陆国行手机和低俗短视频平台下，失去对计算机基本理解和亲和的孩子了。傻瓜式的手机阻碍了基本的系统操作能力，无用的庞杂信息流妨碍了自主的探究思考，“民用”化的Windows遮掩了计算机的本质。

每个人都有各自的方向，烧菜的厨子不一定需要了解生火需要的热力学理论，也不一定需要学习如何生出更好用的火，但不能太离谱，至少TA得知道火是什么。**您可以在计算机科学上薄弱，但不能完全被蒙蔽双眼。**

![math](./math.png)

btw，我如今几乎所有工作环境都在`mac`和`gnu/linux`下完成，后者选个您顺眼的，(k)ubunttu就不错。我不喜欢Windows，原因有点罄竹难书，不过如果一路走下去的话，未来您自然会知道的。

:::warning
However，**DONT USE Arch IF U ARE A COMPLETE NEWCOMER**
:::

------

## 预期roadmap

大致预期路线：

### 语言/框架

`HTML` -> `CSS` -> `Tailwind css` -> `JS/TS` -> `Vue/React`

（TS在vue期间补充）

```
Branch: Vue   -> scss
        React -> css-in-js, css modules
```

### 工具链

Build tool: `Vite` `Webpack`, serving to bundle and optimize web applications.

Bundler: `ESBuild`

Deploy: `Vercel`, `Netlify`

Linter: `ESlint`, `SonarQuote`

------

## ~~流水账~~

### 2025-10-9  00:20

W3S tutorial, `HTML`

Now, Chapter : `Forms` Section : `HTML Input Attributes`

### 2025-10-9  09:00

W3S tutorial, `HTML`

Finish `Forms` chapter learning

Now, Chapter : `Media` 

### 2025-10-10  02:18

W3S tutorial, `CSS`

Finish `HTML` part learning

预期学习概况：

必学：

- 选择器：基础（`#id`, `.class`, `tag`），组合选择器，伪类（`:hover`, `:focus`），伪元素（`::before`, `::after`）
- 盒模型：`margin`, `padding`, `border`, `width/height`, `box-sizing`
- 布局：`display`（block, inline, inline-block, flex, grid），`position`（static, relative, absolute, fixed, sticky）
- 字体与文本：`font-size`, `line-height`, `font-family`, `text-align`
- 背景与颜色：`background`, `color`, `opacity`
- 响应式：媒体查询（`@media`）
- 动画/过渡：`transition`, `transform`, `animation`（了解即可，常用在按钮 hover 等场景）

初学阶段可以不学：

- CSS 自定义属性（`--var`）→ 初学不常用
- CSS 函数（`calc()`, `clamp()` 等）→ 可后面补
- CSS 高级选择器（`nth-child`, `nth-of-type`）→ 先不碰复杂情况
- CSS Grid 高级特性（自动布局区域命名）→ 学基础网格即可
- CSS 高级动画（关键帧复杂动画）→ React/Tailwind 常用库替代

