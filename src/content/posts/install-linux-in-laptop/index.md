---
title: 小萌新装大系统的奇遇记
published: 2025-10-12
description: ''
image: './game-life1.png'
tags: [Linux, 系统安装]
category: 'Linux'
draft: false 
lang: ''
---

# 前言

这是篇关于我为什么如今选择使用`Kubuntu`，以及期间一些经历和解决方案的文章

我不喜欢windows，当然我也不是那种在desktop上堆一堆folder，连`brew`包管理和shell都不会用的果粉。

Windows遮掩了很多东西，过度的gui，冗余的组件，难堪其用的权限分配和目录树，见鬼一样的sh `env`等等都是我不用它干活的理由。您会不得不需要大量的`虚拟化环境`，啥都整合的`IDE`，以及大量的兼容性，版本管理问题。

那破目录树简直是什么考古垃圾堆...`Program Files`、`Program Files (x86)`、`AppData`、`System32`，再加上点盘符和反直觉路径名...一个库装完是要在四五个路径里找残骸的。后台进程是要抢IO的。cli tool是东拼西凑的。包管理`winget`、`choco`、`scoop`是三国割据的。脚本好像还互不兼容🤔？权限分配(UAC)像是大陆的学校门卫——平时谁都随便过，一旦你想干点大的，立刻把门锁死装眼瞎耳聋的（

PATH更神秘。用户级、系统级、session级，左右脑互搏各管各的，解析展开和合并生效也模糊不清。python装几遍、node装几遍，语言包管理和被管理主体永远指向不同版本。最后一看生效path和link，后宫宫斗这一块。

最后还有最爱的**遮羞布**：`GUI`。点开N层窗口才能找到真正的配置选项。日志散在十几个角落，想看端口还得`netstat`。这不是在开发，是在做考古。这些“外壳”都掩盖了DEV需要的透明度。

> 它不希望你理解它，只希望你别碰它。

`Windows`像是个被强行封装的黑盒系统，它允许你''运行程序''，却不鼓励乃至阻碍你"理解系统"。一言以蔽之：**Windows很适合日常消费者民用，但不适合多数触及本质的开发**。

以及：

1. wsl2有着众所周知的跨文件系统极其拉垮的读写性能
2. vmware有着神秘非常的网络配置问题
3. hyper-v的网桥数次让我Windows主体的`DE`崩溃。

我是个高度的实用主义者，我不在乎是`MacOS`还是`Linux`的哪个distro，方便好用就行。当然，如果您是做基架/计算机科学向(理论算法除外)的，那需要额外的考量。

------

## 安装 - 1. Arch Linux

### 背景

我想您曾在数次在各个社区/info上看到这句 "~~btw, i use arch~~"

Hey,bro,我想其中还有不少使用来自东方的二次元纸片小人，其中又有不少背景是彩虹或🍥（）

不可否认的是，`Arch wiki`在如今社区处理问题的便携性(对比以前)上可谓功不可没，such as:你在`debian`/`ubuntu`/`open suse(Tumbleweed)`等其他distro上碰上nvidia或者其他毛病，搜出来往往google SEO榜首是`Arch wiki`的某篇post ( ) 它写下了从零构建整个现代gnu/linux您所需要的近乎一切，看不懂/不会看是您的问题，不是它的（笑

但就我个人平心而论，我真不觉得complete newcomer用Arch是啥好主意，您会以至少6t/h的速率去高频骚扰agnet/llm，然后又使用更多的时间去问询关于linux基础架构和底层实现的知识，您会淹没在庞杂的command里，延伸出十几个、几十个、近百个问题及其分支中。不是每个人都做计算机科学的系统层方向，也不是所有人都会在乎哪个犄角旮旯的包是崩溃的罪魁祸首。您修/问等折腾的时间将会远远超过您在特定环境下完成期望的工作/学习的时间，请牢记：**You are just a newcomer,not long-term open-source contributor or scientist.** <span id="self-awareness">很多知识/技能不是您当下该学的，您也学不会，就算学会也要花费很多不值得的时间精力。</span>

时间到了，您自然会需要，会根据自己的需求接触到。

### 过程

我使用`Ventory`来创建`bootable USB drive`，u盘是Samsung 64g的闪存盘，我喜欢稳定和适量的冗余余地，同时我对镜像损坏和盘读写锁死有阴影。同样，因为我实用主义~~懒~~。这个工具创建`USB driver`可以同时放入多个镜像文件，而且只需要把iso拖入进存储分区就行，不需要反复formant，而且除了`UEFI`外的存文件的分区可以当作正常u盘来放文件，我可不想买个u盘只能当安装介质。

![UEFI](screen_uefi.png)

我使用`archinstall`来安装，这是一个helper library，很多人抱怨它不能手动分区（这一点其实新版的镜像附带的`archinstall`已经解决了）以及失去了手动学习arch的机会。但我不在乎这个，我只想有个合适的工作环境。~~**工具做出来就是给人用的嘛。**~~ 不过当然了，以后有机会/空闲时我会"朝圣"一番拜读纯手工安装的arch wiki的，但现在不是时候。然后就是非常固定的：

1. 进入`bios`，关闭`Secure Boot`(安全启动)，调整efi顺序为ventory优先

2. 联网和配置临时代理:

   ```sh
   export http_proxy="http://proxy-host-address:proxy-lan-port"
   export https_proxy="$http_proxy"
   ```
   
3. 进入`archlive`， `pacman -Sy archinstall`更新

4. `archinstall`，然后把它要求您必填的字段全处理好，包括Partition，Time zone，镜像下载来源，nvidia driver，root/user账密等等

5. 进入kde后，再次配置临时代理，安装yay，字体，fx5 input source，`fish shell`，您喜欢的proxy cli/gui

:::tip
这里有个小坑，一些游戏本会在新live环境自动启动飞行模式以禁用网卡，造成您在iwconfig时看不到网卡存在，您只需要用键盘的Fn key关闭飞行模式即可<br>
强烈建议您用rj45网线链接路由器lan口，然后用手机clash系代理app进行LAN Proxy，以最大化减少不必要的麻烦
:::

:::warning
请给`/`和`/root`划入不同的分区，便于您后续修`/root`时保留自己的用户数据<br>
请务必选择nvidia的闭源专有驱动，以及在这之后禁用自带的nouveau开源驱动（让你看个亮的）
:::

至此主体安装以及结束了，但剩下的细枝末节实在是难以数尽，包括但不限于: 

- 部分app window在`Wayland`下字体渲染模糊
- 内置网卡上下行速率奇慢无比(树外驱动)，~~这里特指联发科某些偏门/邪门玩意~~
- nvidia睡死，桌面环境渲染输出崩溃
- 明明支持`Wayland`但默认不启用，查arch wiki：u need add the flag `--ozone-platform-hint=auto --enable-wayland-ime `to the launch file for Chrome (often located in `/usr/share/applications/google-chrome.desktop` ) [**Native Wayland support**](https://wiki.archlinux.org/title/Chromium#2.9:~:text=Xwayland%2Drelated%20crashes.-,Native%20Wayland%20support,-Chromium%20140%20supports)
- 滚挂（设置下timeshort在每次-Syu之前备份快照，其实修起来还好）

于是乎，我放弃arch了。因为 <a href="#self-awareness">上文所提</a>。

------

## 安装 - 2. Kubuntu

### 背景

在被Arch拷打半个月后，感觉自己的的怒气值和跟llm间的亲密度，尤其是日用期间修问题时与日俱增😡(这也可能与我双硬盘双系统设备本身有关)，因而转头被`Kubuntu`捞走了。一开始其实期望是`open suse`，但我对ubuntu这边的包管理熟悉度好些(以前用`Kali`的)。契机：暑假时偶然认识到了一位叫做 [Glavo](https://github.com/Glavo) 的猫猫，得知其居然是大名鼎鼎的 [HMCL](https://github.com/HMCL-dev/HMCL) 的唯一maintainer以及 [PLCT](https://github.com/plctlab) 的疑似lv5前辈，感到十分震惊。看了几天他在bili上的日常issue、code、pr&merge以及我最头大的config env都是在`Kubuntu 24LTS`下进行。几日观察+和群u聊天提问后选择了这个distro，因为——它真的很懒。

### 过程

**<u>遭罪，非常的遭罪。</u>**

我一开始为了图更新的包而选择了*Latest*而不是***LTS***，相信我，这简直是赛博酷刑。

我的所有config都是接近最优最简解的**Best Practice**，和Arch以及多数distro类似的前置配置，config proxy，apt update&upgrade，然后最恶心的来了: `ubuntu-drivers` 

这个cli本身没有任何问题，它同时也是被社区共同认可/设计的、面对 *"Which proprietary graphics driver to install?"* 时的Best Practice，详情可见[Community](https://askubuntu.com/questions/543325/how-to-download-all-required-ubuntu-drivers)。可以说你压根不用动脑子，直接:

```sh
sudo ubuntu-drivers install   
sudo reboot   
```

即可解君愁。~~一开始我就是如此相信的（~~ 

但直到我花了整整八次去重装并输出内核报错结果以及显卡硬件信息反馈后我发现：

> **Best practice is NOT ALWAYS the BEST (**

我用了2次尝试在best practice下查明问题所在和发生了什么：情况一. 网卡部分内核级崩溃，别说无线，连rj45有线网卡，usb网卡都不识别。 情况二. Nvidia经典一睡不醒，这是安装过程中的随机事件，但我日志看了半天看不明白为什么发生。控制变量完全一致。但如果您使用了大陆高校/大厂镜像源的话可能性疑似更大

然后用了5次尝试了所有`sudo ubuntu-drivers devices`下的所有drivers，open kernel的，proprietary的，新的老的全试了一遍，最后得到结论：在我的设备环境下(CPU：amd R9-7945HX，GPU：RTX 4060 Laptop，Wireless LAN Card：**<u>MediaTek Wi-Fi 6E MT7922 (*RZ616*)</u>** ，Lenovo R900P 2024)，安装`Kubuntu latest`（KDE 6.x , Wayland）将会100%复现情况一，概率复现情况二。且reddit社区对联发科(MediaTek)的这位邪门网卡驱动表现可谓[骂声一片](https://www.reddit.com/r/MSI_Gaming/comments/1bbq600/amd_wifi_rz616_mediatek_crappy_wifi_drivers/), linux上[同样](https://www.reddit.com/r/linuxquestions/comments/1n17dhs/slow_and_unstable_wifi_on_mediatek_rz616_mt7922/), 感兴趣可自行goggle search下这位神秘网卡。

然后红温了，直接照搬Glavo~~猫猫大人~~的配置，成为究极懒狗。1. `Kubuntu 24LTS` 2. `sudo ubuntu-drivers install` 3. Install `fish shell`, 啥都不改，`WM`也是24LTS的`X11`，`DE`也是default `KDE 5.x`。

然后过上了幸福的日子，解决了我近乎一切烦恼，除了💩的 **<u>MediaTek Wi-Fi 6E MT7922 (*RZ616*)</u>** ，它在linux下的速率和信号接收仍然是依托答辩，不负它在社区的显赫骂名（ 

后续：我又重装了两三次确认，在24LTS下无论装那种devices optional下的Nvidia drivers，都没有出现任何问题。
