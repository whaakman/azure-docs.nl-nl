---
title: Architectuur van Microsoft Azure-Stack Development Kit | Microsoft Docs
description: De architectuur van Microsoft Azure-Stack Development Kit weergeven.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: a7e61ea4-be2f-4e55-9beb-7a079f348e05
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.openlocfilehash: b754ff5b5a82ac284eb59ff9b9d30a581f3d3af5
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2018
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
[Azure Stack implementeren](azure-stack-deploy.md)

[Eerste scenario's om te proberen](azure-stack-first-scenarios.md)

