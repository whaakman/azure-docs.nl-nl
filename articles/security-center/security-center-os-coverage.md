---
title: Functies en platformen worden ondersteund door Azure Security Center | Microsoft Docs
description: Dit document bevat een lijst met functies en platformen worden ondersteund door Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/03/2019
ms.author: monhaber
ms.openlocfilehash: e2d66db2ae3c379db86d40603d919489fdf15eed
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67144262"
---
# <a name="platforms-and-features-supported-by-azure-security-center"></a>Platforms en functies die worden ondersteund door Azure Security Center

Status van beveiligingsbewaking en aanbevelingen zijn beschikbaar voor virtuele machines (VM's) gemaakt met behulp van zowel de klassieke en Resource Manager-implementatiemodel, en computers.

> [!NOTE]
> Meer informatie over de [klassieke en Resource Manager-implementatiemodel](../azure-classic-rm.md) voor Azure-resources.
>

## <a name="platforms-that-support-the-data-collection-agent"></a>Platforms die ondersteuning bieden voor de agent voor gegevensverzameling 

Deze sectie vindt u de platforms waarop de Azure Security Center-agent kan worden uitgevoerd en het verzamelen van gegevens.

### <a name="supported-platforms-for-windows-computers-and-vms"></a>Ondersteunde platforms voor Windows-computers en virtuele machines
De volgende Windows-besturingssystemen worden ondersteund:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

> [!NOTE]
> Integratie met Windows Defender ATP ondersteunt alleen Windows Server 2012 R2 en Windows Server 2016.

### <a name="supported-platforms-for-linux-computers-and-vms"></a>Ondersteunde platforms voor Linux-computers en virtuele machines

De volgende Linux-besturingssystemen worden ondersteund:

> [!NOTE]
> Aangezien de lijst met ondersteunde Linux-besturingssystemen wordt voortdurend gewijzigd als u liever, klikt u op [hier](https://github.com/microsoft/OMS-Agent-for-Linux#supported-linux-operating-systems) om weer te geven van de meest recente lijst met ondersteunde versies, als er wijzigingen zijn aangebracht sinds de laatste publicatie van dit onderwerp.

64-bit
* CentOS 6 en 7
* Amazon Linux 2017.09
* Oracle Linux 6 en 7
* Red Hat Enterprise Linux Server 6 and 7
* Debian GNU/Linux 8 en 9
* Ubuntu Linux 14.04 LTS, 16.04 LTS en 18.04 LTS
* SUSE Linux Enterprise Server 12

32-bit
* CentOS 6
* Oracle Linux 6
* Red Hat Enterprise Linux Server 6
* Debian GNU/Linux 8 en 9
* Ubuntu Linux 14.04 LTS en 16.04 LTS

## <a name="vms-and-cloud-services"></a>Virtuele machines en Cloudservices
Virtuele machines die worden uitgevoerd in een cloudservice worden ook ondersteund. Alleen cloud services web- en werkrollen-rollen die worden uitgevoerd in productiesites worden bewaakt. Zie voor meer informatie over cloudservices, [overzicht van Azure Cloud Services](../cloud-services/cloud-services-choose-me.md).


## <a name="supported-iaas-features"></a>Ondersteunde IaaS-functies

> [!div class="mx-tableFixed"]
> 

|Server|Windows|||Linux|||Prijzen|
|----|----|----|----|----|----|----|----|
|**Omgeving**|**Azure**||**Non-Azure**|**Azure**||**Non-Azure**||
||**Virtuele machine**|**Virtuele-Machineschaalset**||**Virtuele machine**|**Virtuele-Machineschaalset**|
|VMBA dagelijks geconstateerde waarschuwingen|✔|✔|✔|✔ (op ondersteunde versies)|✔ (op ondersteunde versies)|✔|(Gratis) Bedreigingsdetectie aanbevelingen (standaard)|
|Waarschuwingen voor detectie van bedreigingen op basis van netwerk|✔|✔|X|✔|✔|X|Standard|
|Windows Defender ATP-integratie|✔ (op ondersteunde versies)|✔ (op ondersteunde versies)|✔|X|X|X|Standard|
|Ontbrekende patches.|✔|✔|✔|✔|✔|✔|Gratis|
|Beveiligingsconfiguraties|✔|✔|✔|✔|✔|✔|Gratis|
|Endpoint protection-evaluatie|✔|✔|✔|X|X|X|Gratis|
|JIT-VM-toegang|✔|X|X|✔|X|X|Standard|
|Adaptieve toepassingsbesturingselementen|✔|X|✔|✔|X|✔|Standard|
|FIM|✔|✔|✔|✔|✔|✔|Standard|
|Evaluatie van de schijf-versleuteling|✔|✔|X|✔|✔|X|Gratis|
|Implementatie van derden|✔|X|X|✔|X|X|Gratis|
|NSG-evaluatie|✔|✔|X|✔|✔|X|Gratis|
|Detectie van bedreigingen fileless|✔|✔|✔|X|X|X|Standard|
|Netwerktoewijzingen|✔|✔|X|✔|✔|X|Standard|
|Besturingselementen voor adaptieve netwerk|✔|✔|X|✔|✔|X|Standard|
|Wettelijke naleving dashboard en rapport|✔|✔|✔|✔|✔|✔|Standard|
|Aanbevelingen en detectie van bedreigingen op IaaS gehoste Docker-containers|X|X|X|✔|✔|✔|Standard|

### <a name="supported-endpoint-protection-solutions"></a>Oplossingen voor ondersteunde eindpuntbeveiliging

De volgende tabel bevat een matrix met:
 - U kunt of Azure Security Center gebruiken voor het installeren van elke oplossing voor u.
 - Welke endpoint protection solutions Security Center kan detecteren. Als een van deze oplossingen voor eindpuntbeveiliging wordt gedetecteerd, wordt in Security Center niet aanbevolen een installeren.

Zie voor meer informatie over wanneer aanbevelingen worden gegenereerd voor elk van deze beveiligingen [Endpoint Protection evaluatie en aanbevelingen](security-center-endpoint-protection.md).

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

## <a name="supported-paas-features"></a>Ondersteunde PaaS-functies


|Service|Aanbevelingen (gratis)|Detectie van bedreigingen (standaard)|
|----|----|----|
|SQL|✔| ✔|
|PostGreSQL*|✔| ✔|
|MySQL*|✔| ✔|
|Azure Blob storage-accounts|✔| ✔|
|App-services|✔| ✔|
|Cloud Services|✔| X|
|Vnet 's|✔| N.V.T.|
|Subnetten|✔| N.V.T.|
|NIC’s|✔| N.V.T.|
|NSG's|✔| N.V.T.|
|Abonnement|✔ **| ✔|
|Batch|✔| N.V.T.|
|Service Fabric|✔| N.V.T.|
|Automation-account|✔| N.V.T.|
|Load balancer|✔| N.V.T.|
|Search|✔| N.V.T.|
|Service Bus|✔| N.V.T.|
|Stream Analytics|✔| N.V.T.|
|Event Hub|✔| N.V.T.|
|Logische apps|✔| N.V.T.|
|Storage-account|✔| N.V.T.|
|Redis|✔| N.V.T.|
|Data lake analytics|✔| N.V.T.|
|Key Vault|✔| N.V.T.|




\* Deze functies worden momenteel ondersteund in openbare preview.

\*\* AAD-aanbevelingen zijn alleen beschikbaar voor Standard-abonnementen



## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [plannen en de ontwerpoverwegingen die zich conformeerde aan Azure Security Center kennen](security-center-planning-and-operations-guide.md).
- Meer informatie over [gedragsanalyse voor virtuele machine en crash dump analyse van geheugengebruik in Security Center](security-center-alerts-type.md#virtual-machine-behavioral-analysis).
- Zoek [Veelgestelde vragen over het gebruik van Azure Security Center](security-center-faq.md).
- Zoek [blogberichten over de Azure-beveiliging en naleving](https://blogs.msdn.com/b/azuresecurity/).
