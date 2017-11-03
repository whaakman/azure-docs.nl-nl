---
title: Architectuur van Microsoft Azure-Stack Development Kit | Microsoft Docs
description: De architectuur van Microsoft Azure-Stack Development Kit weergeven.
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: a7e61ea4-be2f-4e55-9beb-7a079f348e05
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: e8ff17e069bd9237bf06cd79b4c222c50a999eda
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="microsoft-azure-stack-development-kit-architecture"></a>Architectuur van Microsoft Azure-Stack Development Kit

*Van toepassing op: Azure stapelen Development Kit*

De Azure-Stack Development Kit is een implementatie met één knooppunt van de Azure-Stack. Alle onderdelen zijn geïnstalleerd op virtuele machines op een machine één host. 

## <a name="logical-architecture-diagram"></a>Logische architectuur-diagram
Het volgende diagram illustreert de logische architectuur van de Azure-Stack development kit en de bijbehorende onderdelen.

![](media/azure-stack-architecture/image1.png)

## <a name="virtual-machine-roles"></a>Virtuele-machinefuncties
De Azure-Stack development kit biedt services met behulp van de volgende virtuele machines op de host:

| Naam | Beschrijving |
| ----- | ----- |
| **AzS ACS01** | Azure Stack storage-services.|
| **AzS ADFS01** | Active Directory Federatieservices (AD FS).  |
| **AzS BGPNAT01** | Edge-router en NAT- en VPN-mogelijkheden biedt voor Azure-Stack. |
| **AzS CA01** | Certificate services-autoriteit voor Azure-Stack-functieservices.|
| **AzS DC01** | Active Directory, DNS en DHCP-services voor Microsoft Azure-Stack.|
| **AzS ERCS01** | Noodherstel Console VM. |
| **AzS GWY01** | Gateway aan de rand services zoals VPN-site-naar-site-verbindingen voor tenantnetwerken.|
| **AzS NC01** | Netwerkcontroller die Azure-Stack netwerkservices beheert.  |
| **AzS SLB01** | Taakverdeling multiplexer services in Azure-Stack voor tenants en Stack Azure-infrastructuurservices.  |
| **AzS SQL01** | Interne gegevens opslaan voor Azure-Stack infrastructuur functies.  |
| **AzS WAS01** | Azure beheerdersrechten Stack-portal en Azure Resource Manager-services.|
| **AzS WASP01**| Azure-Stack gebruikersportal (tenant) en Azure Resource Manager-services.|
| **AzS XRP01** | Infrastructuur beheercontroller voor Microsoft Azure-Stack, met inbegrip van de resourceproviders voor Compute, netwerk en opslag.|


## <a name="next-steps"></a>Volgende stappen
[Azure Stack implementeren](azure-stack-deploy.md)

[Eerste scenario's om te proberen](azure-stack-first-scenarios.md)

