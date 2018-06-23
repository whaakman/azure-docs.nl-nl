---
title: Ondersteunde platforms in Azure Security Center | Microsoft Docs
description: Dit document bevat een lijst met Windows en Linux operatings-systemen worden ondersteund in Azure Security Center.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/22/2018
ms.author: terrylan
ms.openlocfilehash: 1bb0b61fe8a74edd35bb096fd295aedc67f6efb3
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36335887"
---
# <a name="supported-platforms-in-azure-security-center"></a>Ondersteunde platforms in Azure Security Center
Status beveiligingsbewaking en aanbevelingen zijn beschikbaar voor virtuele machines (VM's) gemaakt met behulp van zowel het klassieke en Resource Manager-implementatiemodel, en computers.

> [!NOTE]
> Meer informatie over de [klassieke en Resource Manager-implementatiemodel](../azure-classic-rm.md) voor Azure-resources.
>
>

## <a name="supported-platforms-for-windows-computers-and-vms"></a>Ondersteunde platforms voor Windows-computers en virtuele machines
Ondersteunde Windows-besturingssystemen:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016


## <a name="supported-platforms-for-linux-computers-and-vms"></a>Ondersteunde platforms voor Linux-computers en virtuele machines
Ondersteunde Linux-besturingssystemen:

* Ubuntu versies 12.04 TNS, 14.04 TNS, 16.04 TNS
* Debian versies 6, 7, 8, 9
* CentOS versies 5, 6, 7
* Red Hat Enterprise Linux (RHEL) versie 5, 6, 7
* SUSE Linux Enterprise Server (SLES) versies 11, 12
* Oracle Linux versies 5, 6, 7
* Amazon Linux 2012.09 via 2017
* Openssl 1.1.0 wordt alleen ondersteund op x86_64 platforms (64-bits)

> [!NOTE]
> Virtuele machine gedragsanalyse zijn nog niet beschikbaar voor Linux-besturingssystemen.
>
>

## <a name="vms-and-cloud-services"></a>Virtuele machines en Cloud-Services
Virtuele machines die worden uitgevoerd in een cloudservice worden ook ondersteund. Alleen cloud services-web- en werkrollen rollen die worden uitgevoerd in de productieomgeving sleuven worden bewaakt. Zie voor meer informatie over de cloudservice, [Cloud Services-overzicht](../cloud-services/cloud-services-choose-me.md).

## <a name="next-steps"></a>Volgende stappen

- [Azure Security Center Planning- en Bedieningsgids](security-center-planning-and-operations-guide.md) : informatie over het plannen en de ontwerpoverwegingen vast te stellen van Azure Security Center begrijpen
- [Beveiligingswaarschuwingen op typen in Azure Security Center](security-center-alerts-type.md#virtual-machine-behavioral-analysis) : meer informatie over de virtuele machine gedragsanalyse en crashes dump geheugenanalyse in Security Center
- [Veelgestelde vragen over Azure Security Center](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service
- [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) : Lees blogberichten over Azure-beveiliging en naleving
