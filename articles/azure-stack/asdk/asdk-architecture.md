---
title: Azure-Stack Development Kit architectuur | Microsoft Docs
description: Beschrijft de architectuur Azure Stack Development Kit (ASDK).
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 68da3ac0eb135f5956dfea76e186d9c57beea79c
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="microsoft-azure-stack-development-kit-architecture"></a>Architectuur van Microsoft Azure-Stack Development Kit
Azure Stack Development Kit (ASDK) is een implementatie met één knooppunt van de Azure-Stack. Alle onderdelen zijn geïnstalleerd op virtuele machines op een machine één host. 

## <a name="logical-architecture-diagram"></a>Logische architectuur-diagram
Het volgende diagram illustreert de logische architectuur van de ASDK en de bijbehorende onderdelen.

![ASDK-architectuur](media/asdk-architecture/image1.png)

## <a name="virtual-machine-roles"></a>Virtuele-machinefuncties
De ASDK biedt services met behulp van de volgende virtuele machines die worden gehost op de hostcomputer development kit:

| Naam | Beschrijving |
| ----- | ----- |
| **AzS-ACS01** | Azure Stack storage-services.|
| **AzS-ADFS01** | Active Directory Federation Services (ADFS).  |
| **AzS-BGPNAT01** | Edge-router en NAT- en VPN-mogelijkheden biedt voor Azure-Stack. |
| **AzS-CA01** | Certificate services-autoriteit voor Azure-Stack-functieservices.|
| **AzS-DC01** | Active Directory, DNS en DHCP-services voor Microsoft Azure-Stack.|
| **AzS-ERCS01** | Noodherstel Console VM. |
| **AzS-GWY01** | Gateway aan de rand services zoals VPN-site-naar-site-verbindingen voor tenantnetwerken.|
| **AzS-NC01** | Netwerkcontroller die Azure-Stack netwerkservices beheert.  |
| **AzS-SLB01** | Taakverdeling multiplexer services in Azure-Stack voor tenants en Stack Azure-infrastructuurservices.  |
| **AzS-SQL01** | Interne gegevens opslaan voor Azure-Stack infrastructuur functies.  |
| **AzS-WAS01** | Azure beheerdersrechten Stack-portal en Azure Resource Manager-services.|
| **AzS-WASP01**| Azure-Stack gebruikersportal (tenant) en Azure Resource Manager-services.|
| **AzS-XRP01** | Infrastructuur beheercontroller voor Microsoft Azure-Stack, met inbegrip van de resourceproviders voor Compute, netwerk en opslag.|


## <a name="next-steps"></a>Volgende stappen
[Meer informatie over basic ASDK beheertaken](asdk-admin-basics.md)
