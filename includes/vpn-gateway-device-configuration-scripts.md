---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 01/09/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 78dfd57fba6365f9c8937b30b5cf96b840749c68
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54211831"
---
| **Leverancier** | **Apparaatfamilie** | **Firmwareversie** |
| --- | --- | --- |
|Cisco | ISR| IOS 15.1 (Preview)|
|Cisco | ASA | ASA (*) op route gebaseerd (IKEv2-geen BGP) voor ASA hieronder 9,8 |
|Cisco | ASA | ASA RouteBased (IKEv2 - geen BGP) voor ASA 9,8 + |
|Juniper | SRX_GA | 12.x|
|Juniper | SSG_GA | ScreenOS 6.2.x|
|Juniper | JSeries_GA | JunOS 12.x|
|Juniper | SRX | JunOS 12.x RouteBased BGP |
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased VTI|
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased BGP|

> [!NOTE]
> ( * ) Vereist: NarrowAzureTrafficSelectors (optie UsePolicyBasedTrafficSelectors inschakelen) en CustomAzurePolicies (IKE/IPsec)
>
