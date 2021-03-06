﻿---
title: Lync Server 2013：如何实施 SIP 中继？
TOCTitle: 如何实施 SIP 中继？
ms:assetid: 273a22b1-8a4c-4187-acf8-c57d5c6598ce
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/Gg425743(v=OCS.15)
ms:contentKeyID: 49312301
ms.date: 12/10/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 如何在 Lync Server 2013 中实施 SIP 中继？

 

_**上一次修改主题：** 2016-12-08_

要实现 SIP 中继，必须通过充当 Lync Server 2013 客户端与服务提供商之间的通信会话代理并在必要时转换媒体代码的 中介服务器来路由连接。

每台 中介服务器都有一个内部网络接口和一个外部网络接口。内部接口连接到 前端服务器。外部接口通常称为网关接口，因为它通常用于将 中介服务器连接到公用电话交换网 (PSTN) 网关或 IP-PBX。要实现 SIP 中继，需要将 中介服务器的外部接口连接到 ITSP 的外部边缘组件。

<table>
<thead>
<tr class="header">
<th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>ITSP 的外部边缘组件可以是会话边界控制器 (SBC)、路由器或网关。</td>
</tr>
</tbody>
</table>


有关中介服务器的详细信息，请参阅[Lync Server 2013 中的中介服务器组件](lync-server-2013-mediation-server-component.md)。

## 集中 SIP 中继和分布式 SIP 中继

*集中* SIP 中继通过 中央站点路由所有 IP 语音 (VoIP) 流量（包括 分支站点流量）。集中部署模型简单、经济实惠，通常是通过 Lync Server 2013 实现 SIP 中继的推荐方法。

*分布式* SIP 中继是一种在一个或多个分支站点实现本地 SIP 中继的部署模型。然后将 VoIP 流量直接从 分支站点路由到服务提供商，而无需通过 中央站点。

仅在出现以下情况时才需要使用分布式 SIP 中继：

  - 分支站点需要可存续电话连接（例如，如果 WAN 不可用）。应分别针对每个 分支站点分析此要求；某些分支可能需要冗余和故障转移，而其他分支可能不需要。

  - 两个 中央站点之间要求具有复原能力。需确保 SIP 中继终止于每个 中央站点。例如，如果有 Dublin 和 Tukwila 两个 中央站点，且两个站点都只使用一个站点的 SIP 中继，则当中继不可用时，另一个站点的用户将无法发出 PSTN 呼叫。

  - 分支站点和 中央站点位于不同的国家/地区。出于兼容性和合法性考虑，每个国家/地区至少需要一个 SIP 中继。例如，在欧盟，如果某个国家/地区的通信没有本地终止于中央点，则无法离开该国家/地区。

根据站点的地理位置和企业中的预期流量，您可能不希望通过中央 SIP 中继路由所有用户，或可能选择通过部分用户的 分支站点上的 SIP 中继路由这些用户。为分析您的需求，请回答以下问题：

  - 每个站点有多大（即为多少用户启用了 企业语音）？

  - 每个站点中哪个外线直拨分机 (DID) 号码收到的电话呼叫最多？

确定部署集中 SIP 中继还是分布式 SIP 中继之前需要进行成本效益分析。某些情况下，虽然并不是必需的，但选择分布式部署模型可能会比较有益。在完全集中的部署中，所有 分支站点流量均通过 WAN 链路路由。如果您不想支付 WAN 链路所需的带宽费用，则可能需要使用分布式 SIP 中继。例如，您可能希望在与 中央站点具有联盟关系的 分支站点部署 Standard Edition Server，或者希望部署具有小型网关的 Survivable Branch Appliance 或 Survivable Branch Server。

<table>
<thead>
<tr class="header">
<th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>有关分布式 SIP 中继的详细信息，请参阅<a href="lync-server-2013-branch-site-sip-trunking.md">Lync Server 2013 中的分支站点 SIP 中继</a>。</td>
</tr>
</tbody>
</table>


## 支持的 SIP 中继连接类型

Lync Server 支持以下连接类型的 SIP 中继：

  - 多协议标签交换 (MPLS) 是一种专用网络，用于定向数据并将其从一个网络节点传送到下一个网络节点。MPLS 网络中的带宽可与其他订阅者共享，并为每个数据包分配一个标签以便区别每个订阅者的数据。这种连接类型无需使用虚拟专用网 (VPN)。潜在的缺点是过多的 IP 流量会干扰 VoIP 操作，除非为 VoIP 流量指定优先级。

  - 没有其他流量的专用连接（例如，租用的光纤连接或 T1 线路）通常是最安全可靠的连接类型。此连接类型提供最大的呼叫传送容量，但通常也是最昂贵的。无需使用 VPN。专用连接适用于具有大量呼叫或严格的安全和可用性要求的组织。

  - Internet 是成本最低的连接类型，但也是最不可靠的。Internet 连接是唯一一种需要使用 VPN 的 Lync Server SIP 中继连接类型。

## 选择连接类型

最适用于企业的 SIP 中继连接类型取决于企业的需求和预算。

  - 对于大中型企业，MPLS 网络通常是最佳选择。它能提供必需的带宽，且费率比专用网络更低。

  - 大型企业可能需要专用的光纤、T1、T3 或更高连接（在欧盟为 E1、E3 或更高）。

  - 对于小型企业或呼叫量较低的 分支站点，通过 Internet 的 SIP 中继可能是最佳选择。建议大中型站点不要使用此连接类型。

## 带宽要求

实现所要求的带宽量取决于呼叫容量（必须能够支持的并发呼叫数）。需考虑到带宽可用性，以便能够充分利用购买的峰值容量。使用以下公式计算 SIP 中继峰值带宽的要求：

SIP 中继峰值带宽 = 最大并发呼叫数 x（64 kbps + 标头大小）

<table>
<thead>
<tr class="header">
<th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>标头最大大小为 20 字节。</td>
</tr>
</tbody>
</table>


## 编解码器支持

Lync Server 2013 仅支持以下编解码器：

  - G.711 a-law（主要在北美以外的国家/地区使用）

  - G.711 μ-law（在北美使用）

## Internet 电话服务提供商

SIP 中继连接的服务提供商端的实现方式因 ITSP 而异。有关部署信息，请与服务提供商联系。有关认证的 SIP 中继服务提供商的列表，请参阅 [Microsoft 统一通信开放互操作性计划网站](http://go.microsoft.com/fwlink/?linkid=287029)。

有关 Microsoft 认证的 SIP 中继提供商的详细信息，请与 Microsoft 代表联系。

<table>
<thead>
<tr class="header">
<th><img src="images/Gg398794.important(OCS.15).gif" title="important" alt="important" />重要提示：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>您必须使用 Microsoft 认证的服务提供商，才能确保 ITSP 支持遍历 SIP 中继的所有功能（例如，设置和管理会话以及支持所有扩展 VoIP 服务）。Microsoft 技术支持没有扩展到使用非认证提供商的配置。如果当前使用的是未认证 SIP 中继的 Internet 服务提供商，您可以选择继续使用该提供商作为您的 ISP，同时使用 Microsoft 认证的 SIP 中继提供商。</td>
</tr>
</tbody>
</table>

