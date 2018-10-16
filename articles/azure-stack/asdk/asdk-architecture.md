---
title: Azure Stack Development Kit-architectuur | Microsoft Docs
description: Beschrijft de architectuur Azure Stack Development Kit (ASDK).
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 21c54e2e996bb987f7a27ac3e6333df6f74d6f4b
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49338621"
---
# <a name="microsoft-azure-stack-development-kit-architecture"></a>Architectuur van Microsoft Azure Stack Development Kit
De Azure Stack Development Kit (ASDK) is een implementatie van één knooppunt van Azure Stack. Alle onderdelen zijn geïnstalleerd op virtuele machines die worden uitgevoerd op één enkele hostcomputer. 

## <a name="logical-architecture-diagram"></a>Diagram van de logische architectuur
Het volgende diagram illustreert de logische architectuur van de ASDK en de bijbehorende onderdelen.

![ASDK-architectuur](media/asdk-architecture/image1.png)

## <a name="virtual-machine-roles"></a>Virtuele-machinefuncties
De ASDK biedt services met behulp van de volgende virtuele machines die worden gehost op de hostcomputer development kit:

| Naam | Beschrijving |
| ----- | ----- |
| **AzS-ACS01** | Azure Stack storage-services.|
| **AzS-ADFS01** | Active Directory Federatieservices (ADFS).  |
| **AzS-BGPNAT01** | Edge-router en NAT- en VPN biedt voor Azure Stack. |
| **AzS-CA01** | Certificaatservices van de instantie voor Azure Stack-functieservices.|
| **AzS-DC01** | Active Directory, DNS en DHCP-services voor Microsoft Azure Stack.|
| **AzS-ERCS01** | Noodherstel Console VM. |
| **AzS-GWY01** | Edge-gateway-services zoals VPN site-naar-site-verbindingen voor tenant-netwerken.|
| **AzS-NC01** | Netwerkcontroller, die Azure Stack-netwerkservices beheert.  |
| **AzS-SLB01** | De taakverdeling multiplexer services in Azure Stack voor zowel tenants en infrastructuurservices van Azure Stack.  |
| **AzS-SQL01** | Interne gegevens opslaan voor Azure Stack-infrastructuur-rollen.  |
| **AzS-WAS01** | Azure Stack-beheerdersportal en Azure Resource Manager-services.|
| **AzS-WASP01**| Azure Stack-portal voor gebruiker (tenant) en Azure Resource Manager-services.|
| **AzS-XRP01** | Infrastructuur-beheercontroller voor Microsoft Azure Stack, met inbegrip van de resourceproviders Compute, netwerk en opslag.|


## <a name="next-steps"></a>Volgende stappen
[Meer informatie over basistaken voor ASDK-beheer](asdk-admin-basics.md)
