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
ms.date: 01/21/2019
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: c69b124f84e87e8f0b937dfa275378c376894f9b
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57447197"
---
# <a name="microsoft-azure-stack-development-kit-architecture"></a>Architectuur van Microsoft Azure Stack Development Kit
De Azure Stack Development Kit (ASDK) is een implementatie van één knooppunt van Azure Stack uitgevoerd op een afzonderlijke host-computer. Edge-routeringsonderdelen zijn geïnstalleerd op de hostcomputer NAT en VPN-mogelijkheden bieden voor Azure Stack. Azure Stack-infrastructuur rollen uitvoeren in de Hyper-V-laag van de fysieke hostcomputer.


## <a name="virtual-machine-roles"></a>Virtuele-machinefuncties
De ASDK biedt services met behulp van de volgende virtuele machines die worden gehost op de hostcomputer development kit:

| Name | Description |
| ----- | ----- |
| **AzS-ACS01** | Azure Stack storage-services.|
| **AzS-ADFS01** | Active Directory Federation Services (ADFS).  |
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
