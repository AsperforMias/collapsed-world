---
title: 软路由配置记录
published: 2025-10-17
description: 'gl-mt3000 + openwrt分支iStore'
image: './image.png'
tags: [软路由, openwrt, iStore]
category: 'SE'
draft: false 
lang: ''
---

## 背景

- 学校校园网网关限制了每个带宽账号2个`mac`——这对我的超过7个设备是极大的阻碍。
- AP隔离使得内网设备间不可通信。**AP/Client Isolation 在 L2 层面拦截站对站流量**，这意味着它阻止了无线客户端之间直接交换以太网帧（ARP、IPv4/IPv6 unicast）。因为没有 L2 通道，基于直接 IP 的会话（比如 SSH/TCP 连接、UDP 单播、广播）通常无法建立，这是帧 Frame 级别的阻拦，不可绕过——这对我多设备通信极不友好。
- 我有些设备不便于在本地代理，或者其l3/4无法被捕获/转发。
- 我没玩过软路由，~~给自己找点罪受，~~，体验实践一遍软路由整套部署的大致工作流。

普及下 router-based proxy 和 device-based proxy 的区别

router-based：

| 类型                           | 例子                                   | OSI 层  | 原理                                                         |
| ------------------------------ | -------------------------------------- | ------- | ------------------------------------------------------------ |
| **NAT（网络地址转换）**        | OpenWrt、iptables MASQUERADE           | L3 / L4 | 直接改写 IP 头和 TCP/UDP 端口，不看应用层内容。纯粹的数据包转发。 |
| **TProxy / REDIRECT 透明代理** | Clash、Surge、OpenClash、Xray 透明代理 | L3 / L4 | 捕获所有目的端口流量，把 TCP/UDP 流导入本地监听的代理进程（例如 HTTP/SOCKS）。代理进程再在 L7 层解析。 |
| **防火墙规则/路由策略**        | iptables、nftables、ip rule、ip route  | L3 / L4 | 依据 IP 和端口做流量重定向或丢弃，不理解 L7 内容。           |
| **软路由中的代理核心**         | Clash、Xray、V2Ray、Sing-box           | L7      | 在路由器中运行的代理引擎本身理解 HTTP、TLS、QUIC、SOCKS 协议。属于应用层代理，但它接收的流量是从内核 L4 转发来的。 |

device-based：

| 类型                   | 示例                                       | OSI 层                            | 原理                                                         |
| ---------------------- | ------------------------------------------ | --------------------------------- | ------------------------------------------------------------ |
| **HTTP 代理**          | 系统设置中的 HTTP Proxy、Surge、Quantumult | L7                                | 应用发送 HTTP 请求给代理，代理解析并转发。处理的是应用层 HTTP 协议。 |
| **SOCKS5 代理**        | Shadowsocks、Clash 本地模式                | L7                                | 代理客户端封装 TCP/UDP 连接请求，通过 SOCKS 协议转交代理服务器。 |
| **VPN App / Tun 驱动** | Clash Tun、WireGuard、Outline、OpenVPN     | L3~L4（虚拟网卡）+ L7（代理引擎） | 虚拟网卡捕获 IP 包（L3），然后交给 L7 的代理协议（如 Shadowsocks、Vmess、Trojan）处理。 |

然后这个 [gl-mt3000](https://openwrt.org/toh/gl.inet/gl-mt3000) 并不算是性价比很好的选择，而是个妥协。既要天线2.4g+5g高频宽，又要 usb-device + 2.5g wan口 接入，又要openwrt supporting list "榜上有名"，还要有轻便小巧+售后兜底，那肯定是有代价的（

`MediaTek MT7981BA` 这个CPU作为arm平台，那转发处理肯定是性能远不如E5等便宜好用x86平台的，但没办法，我总不能为了个软路由在宿舍单独放个itx+ups堆箱 or 裸露开发板+线缆放在宿舍小的可怜的桌面上，那也太抽象了。

这里鸣谢下 [molinyue](https://moliyue.xyz/) 。没有这位在网工上很厉害的朋友，面对软路由这个完全陌生的领域，我可能要自己摸索个少说三天往上（）

未完待续...