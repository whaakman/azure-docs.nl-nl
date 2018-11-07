---
title: Functies en platformen worden ondersteund door Azure Security Center | Microsoft Docs
description: Dit document bevat een lijst met functies en platformen worden ondersteund door Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: f4bc90b2d1a80125ae88b4b5c4c11e42a34a985a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240423"
---
# <a name="platforms-and-features-supported-by-azure-security-center"></a>Platforms en functies die worden ondersteund door Azure Security Center

Status van beveiligingsbewaking en aanbevelingen zijn beschikbaar voor virtuele machines (VM's), die zijn gemaakt met zowel de klassieke en Resource Manager-implementatiemodel, en computers.

> [!NOTE]
> Meer informatie over de [klassieke en Resource Manager-implementatiemodel](../azure-classic-rm.md) voor Azure-resources.
>
>

## <a name="supported-platforms"></a>Ondersteunde platforms 

Deze sectie vindt u de platforms waarop de Azure Security Center-agent kan worden uitgevoerd en het verzamelen van gegevens.

### <a name="supported-platforms-for-windows-computers-and-vms"></a>Ondersteunde platforms voor Windows-computers en virtuele machines
Ondersteunde Windows-besturingssystemen:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016


### <a name="supported-platforms-for-linux-computers-and-vms"></a>Ondersteunde platforms voor Linux-computers en virtuele machines
Ondersteunde Linux-besturingssystemen:

* Versies van Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS
* Debian-versie 6, 7, 8, 9
* CentOS-versie 5, 6, 7
* Red Hat Enterprise Linux (RHEL) versie 5, 6, 7
* SUSE Linux Enterprise Server (SLES) versie 11, 12
* Oracle Linux-versies 5, 6, 7
* Amazon Linux 2012.09 tot 2017
* Openssl 1.1.0 wordt alleen ondersteund op x86_64 platforms (64-bits)

> [!NOTE]
> Gedragsanalyse voor virtuele machine zijn nog niet beschikbaar voor Linux-besturingssystemen.
>
>

## <a name="vms-and-cloud-services"></a>Virtuele machines en Cloudservices
Virtuele machines die worden uitgevoerd in een cloudservice worden ook ondersteund. Alleen cloud services-web- en werkrollen rollen in productie sleuven worden bewaakt. Zie voor meer informatie over de service in de cloud, [overzicht van Cloud Services](../cloud-services/cloud-services-choose-me.md).


## <a name="supported-iaas-features"></a>Ondersteunde IaaS-functies

> [!div class="mx-tableFixed"]
> 

|Server|Windows||Linux||
|----|----|----|----|----|
|Omgeving|Azure|Niet-Azure|Azure|Niet-Azure|
|VMBA dagelijks geconstateerde waarschuwingen|✔|✔|✔ (op ondersteunde versies)|✔|
|Meldingen voor geconstateerde bedreigingen op basis van netwerk|✔|X|✔|X|
|Windows Defender ATP-integratie *|✔ (op ondersteunde versies)|✔|X|X|
|Ontbrekende patches.|✔|✔|✔|✔|
|Beveiligingsconfiguraties|✔|✔|✔|✔|
|Anti-malware|✔|✔|X|X|
|JIT-VM-toegang|✔|X|✔|X|
|Adaptieve toepassingsbesturingselementen|✔|X|X|X|
|FIM|✔|✔|✔|✔|
|Schijfversleuteling|✔|X|✔|X|
|Implementatie van derden|✔|X|✔|X|
|NSG's|✔|X|✔|X|
|Detectie van bedreigingen Filess|✔|✔|X|X|
|Netwerktoewijzingen|✔|X|✔|X|
|Besturingselementen voor adaptieve netwerk|✔|X|✔|X|

\* Deze functies worden momenteel ondersteund in openbare preview.


## <a name="supported-paas-features"></a>Ondersteunde PaaS-functies


|Service|Aanbevelingen|Detectie van bedreigingen|
|----|----|----|
|SQL|✔| ✔|
|PostGreSQL *|✔| ✔|
|MySQL *|✔| ✔|
|BLOB storage-accounts *|✔| ✔|
|App-services|✔| ✔|
|Cloud services|✔| X|
|Vnet 's|✔| N.v.t.|
|Subnetten|✔| N.v.t.|
|NIC’s|✔| ✔|
|NSG's|✔| N.v.t.|
|Abonnement|✔| ✔|

\* Deze functies worden momenteel ondersteund in openbare preview.

## <a name="next-steps"></a>Volgende stappen

- [Azure Security Center Planning and Operations Guide](security-center-planning-and-operations-guide.md) : meer informatie over het plannen en te begrijpen van de ontwerpoverwegingen die zich conformeerde aan Azure Security Center
- [Beveiligingswaarschuwingen per type in Azure Security Center](security-center-alerts-type.md#virtual-machine-behavioral-analysis) : meer informatie over de virtuele machine gedragsanalyse en crash dump geheugenanalyse in Security Center
- [Veelgestelde vragen over Azure Security Center](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service
- [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/) : Lees blogberichten over de Azure-beveiliging en naleving
