﻿---
title: Lync Server 2013：为控制器配置证书
TOCTitle: 为控制器配置证书
ms:assetid: 22988186-15ae-43b1-92f4-0adb3b75a7fd
ms:mtpsurl: https://technet.microsoft.com/zh-cn/library/Gg398296(v=OCS.15)
ms:contentKeyID: 49312242
ms.date: 05/19/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 在 Lync Server 2013 中为控制器配置证书

 

_**上一次修改主题：** 2012-09-08_

<table>
<thead>
<tr class="header">
<th><img src="images/Gg398794.important(OCS.15).gif" title="important" alt="important" />重要提示：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>在运行证书向导时，请确保您使用作为组（已为其分配您将使用的证书模板类型的适当权限）的成员的帐户进行登录。默认情况下， Lync Server 2013 证书请求将使用 Web 服务器证书模板。如果您使用作为 RTCUniversalServerAdmins 组的成员的帐户来通过此模板请求证书，请确保已为该组分配使用此模板所需的注册权限。</td>
</tr>
</tbody>
</table>


每个控制器都需要默认证书、Web 内部证书和 Web 外部证书。有关控制器的证书要求的详细信息，请参阅规划文档中的 [Lync Server 2013 中内部服务器的证书要求](lync-server-2013-certificate-requirements-for-internal-servers.md)。

使用以下过程配置控制器证书。对每台控制器重复此过程。此过程的步骤介绍如何在组织部署的内部企业根证书颁发机构 (CA) 中通过脱机请求处理来配置证书。有关从外部 CA 获取证书的详细信息，请与支持团队联系。

## 为控制器或控制器池配置证书

1.  在 Lync Server 部署向导中，单击“步骤 3: 请求、安装或分配证书”旁边的“运行”。

2.  在“证书向导”页上，单击“请求”。

3.  在“证书请求”页上，单击“下一步”。

4.  在“延迟的请求或即时请求”页上，接受默认的“立即将请求发送至联机证书颁发机构”选项，然后单击“下一步”。

5.  在“选择证书颁发机构(CA)”页上，单击要使用的内部 Windows 证书颁发机构，然后单击“下一步”。

6.  如果登录时使用的帐户没有足够的权限请求证书，则在“证书颁发机构帐户”页上，指定要使用的备用凭据，然后单击“下一步”。

7.  在“指定替代证书模板”页上，单击“下一步”。

8.  在“名称和安全设置”页上，可以指定一个“友好名称”，并接受 2048 位密钥长度，然后单击“下一步”。

9.  在“组织信息”页上，可选择指定组织信息，然后单击“下一步”。

10. 在“地理信息”页上，可选择指定地理信息，然后单击“下一步”。

11. 在“使用者名称/使用者替代名称”页上，单击“下一步”。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>使用者替代名称列表应包含要安装控制器的计算机的名称（如果是单个控制器）或控制器池名称，以及为组织配置的简单 URL 名称。</td>
    </tr>
    </tbody>
    </table>


12. 在“使用者替代名称(SAN)的 SIP 域设置”页上，为希望控制器处理的所有域选择“已配置的 SIP 域”，然后单击“下一步”。

13. 在“配置其他使用者替代名称”页上，添加所需的任何其他使用者替代名称，然后单击“下一步”。

14. 在“证书请求摘要”页上，单击“下一步”。

15. 命令运行完以后，在“正在执行命令”页上单击“下一步”。

16. 在“联机证书请求状态”页上，单击“完成”。

17. 在“证书分配”页上，单击“下一步”。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Dn783119.note(OCS.15).gif" title="note" alt="note" />注意：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>如果要查看证书，请在列表中双击相应的证书。</td>
    </tr>
    </tbody>
    </table>


18. 在“证书分配摘要”页上，单击“下一步”。

19. 命令运行完以后，在“正在执行命令”页上单击“完成”。

20. 在“证书向导”页上，单击“关闭”。

