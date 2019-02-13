---
title: Functies en platformen worden ondersteund door Azure Security Center | Microsoft Docs
description: Dit document bevat een lijst met functies en platformen worden ondersteund door Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/9/2019
ms.author: rkarlin
ms.openlocfilehash: 5422ba1b29ac48a7255f25ac0beb43e3481d256a
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56105889"
---
# <a name="platforms-and-features-supported-by-azure-security-center"></a>Platforms en functies die worden ondersteund door Azure Security Center

Status van beveiligingsbewaking en aanbevelingen zijn beschikbaar voor virtuele machines (VM's) gemaakt met behulp van zowel de klassieke en Resource Manager-implementatiemodel, en computers.

> [!NOTE]
> Meer informatie over de [klassieke en Resource Manager-implementatiemodel](../azure-classic-rm.md) voor Azure-resources.
>
>

## <a name="platforms-that-support-the-data-collection-agent"></a>Platforms die ondersteuning bieden voor de agent voor gegevensverzameling 

Deze sectie vindt u de platforms waarop de Azure Security Center-agent kan worden uitgevoerd en het verzamelen van gegevens.

### <a name="supported-platforms-for-windows-computers-and-vms"></a>Ondersteunde platforms voor Windows-computers en virtuele machines
De volgende Windows-besturingssystemen worden ondersteund:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

> [!NOTE]
> Integratie met Windows Defender ATP ondersteunt alleen Windows Server 2012 R2 en Windows Server 2016.
>
>

### <a name="supported-platforms-for-linux-computers-and-vms"></a>Ondersteunde platforms voor Linux-computers en virtuele machines
De volgende Linux-besturingssystemen worden ondersteund:

* Ubuntu-versie 12.04 LTS, 14.04 LTS en 16.04 LTS.
* Debian-versie 6, 7, 8 en 9.
* CentOS versie 5, 6 en 7.
* Red Hat Enterprise Linux (RHEL) versie 5, 6 en 7.
* SUSE Linux Enterprise Server (SLES) versie 11 en 12.
* Oracle Linux-versies 5, 6 en 7.
* Amazon Linux 2012.09 tot 2017.
* OpenSSL 1.1.0 wordt alleen ondersteund op 64-bits x86_64 platforms.

## <a name="vms-and-cloud-services"></a>Virtuele machines en Cloudservices
Virtuele machines die worden uitgevoerd in een cloudservice worden ook ondersteund. Alleen cloud services web- en werkrollen-rollen die worden uitgevoerd in productiesites worden bewaakt. Zie voor meer informatie over cloudservices, [overzicht van Azure Cloud Services](../cloud-services/cloud-services-choose-me.md).


## <a name="supported-iaas-features"></a>Ondersteunde IaaS-functies

> [!div class="mx-tableFixed"]
> 

|Server|Windows||Linux||
|----|----|----|----|----|
|Omgeving|Azure|Niet-Azure|Azure|Niet-Azure|
|VMBA dagelijks geconstateerde waarschuwingen|✔|✔|✔ (op ondersteunde versies)|✔|
|Waarschuwingen voor detectie van bedreigingen op basis van netwerk|✔|X|✔|X|
|Windows Defender ATP-integratie|✔ (op ondersteunde versies)|✔|X|X|
|Ontbrekende patches.|✔|✔|✔|✔|
|Beveiligingsconfiguraties|✔|✔|✔|✔|
|Eindpuntbeveiliging|✔|✔|X|X|
|JIT-VM-toegang|✔|X|✔|X|
|Adaptieve toepassingsbesturingselementen|✔|X|X|X|
|FIM|✔|✔|✔|✔|
|Schijfversleuteling|✔|X|✔|X|
|Implementatie van derden|✔|X|✔|X|
|NSG's|✔|X|✔|X|
|Detectie van bedreigingen fileless|✔|✔|X|X|
|Netwerktoewijzingen|✔|X|✔|X|
|Besturingselementen voor adaptieve netwerk|✔|X|✔|X|


### <a name="supported-endpoint-protection-solutions"></a>Oplossingen voor ondersteunde eindpuntbeveiliging

De volgende tabel bevat een matrix met:
 - U kunt of Azure Security Center gebruiken voor het installeren van elke oplossing voor u.
 - Welke endpoint protection solutions Security Center kan detecteren. Als een van deze oplossingen voor eindpuntbeveiliging wordt gedetecteerd, wordt in Security Center niet aanbevolen een installeren.

| Eindpuntbeveiliging| Platformen | Security Center-installatie | Security Center Discovery |
|------|------|-----|-----|
| Windows Defender (Microsoft Antimalware)| Windows Server 2016| Nee, ingebouwd in besturingssysteem| Ja |
| System Center Endpoint Protection (Microsoft Antimalware) | Windows Server 2012 R2, 2012, 2008 R2 (Zie opmerking hieronder) | Via extensie | Ja |
| Trend Micro: alle versies | Windows Server-familie  | Nee | Ja |
| Symantec v12.1.1100+| Windows Server-familie  | Nee | Ja |
| McAfee v10+ | Windows Server-familie  | Nee | Ja |
| Kaspersky| Windows Server-familie  | Nee | Nee  |
| Sophos| Windows Server-familie  | Nee | Nee  |

> [!NOTE]
> - Detectie van System Center Endpoint Protection (SCEP) op een virtuele machine van Windows Server 2008 R2 moet SCEP moet worden geïnstalleerd na PowerShell 3.0 (of een hogere versie).
>
>

## <a name="supported-paas-features"></a>Ondersteunde PaaS-functies 


|Service|Aanbevelingen|Detectie van bedreigingen|
|----|----|----|
|SQL|✔| ✔|
|PostGreSQL*|✔| ✔|
|MySQL*|✔| ✔|
|Azure Blob storage-accounts *|✔| ✔|
|App-services|✔| ✔|
|Cloud Services|✔| X|
|VNets|✔| N.v.t.|
|Subnetten|✔| N.v.t.|
|NIC’s|✔| ✔|
|NSG's|✔| N.v.t.|
|Abonnement|✔| ✔|

\* Deze functies worden momenteel ondersteund in openbare preview. 



## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [plannen en de ontwerpoverwegingen die zich conformeerde aan Azure Security Center kennen](security-center-planning-and-operations-guide.md).
- Meer informatie over [gedragsanalyse voor virtuele machine en crash dump analyse van geheugengebruik in Security Center](security-center-alerts-type.md#virtual-machine-behavioral-analysis).
- Zoek [Veelgestelde vragen over het gebruik van Azure Security Center](security-center-faq.md).
- Zoek [blogberichten over de Azure-beveiliging en naleving](https://blogs.msdn.com/b/azuresecurity/).
