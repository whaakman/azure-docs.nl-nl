---
title: Ondersteunde platforms in Azure Security Center | Microsoft Docs
description: Dit document bevat een lijst van Windows en Linux operatings-systemen worden ondersteund in Azure Security Center.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2018
ms.author: terrylan
ms.openlocfilehash: 54d173caa0e3eb4bbd8bda7c924e56d546a99662
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297499"
---
# <a name="supported-platforms-in-azure-security-center"></a>Ondersteunde platforms in Azure Security Center
Status van beveiligingsbewaking en aanbevelingen zijn beschikbaar voor virtuele machines (VM's), die zijn gemaakt met zowel de klassieke en Resource Manager-implementatiemodel, en computers.

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

## <a name="next-steps"></a>Volgende stappen

- [Azure Security Center Planning and Operations Guide](security-center-planning-and-operations-guide.md) : meer informatie over het plannen en te begrijpen van de ontwerpoverwegingen die zich conformeerde aan Azure Security Center
- [Beveiligingswaarschuwingen per type in Azure Security Center](security-center-alerts-type.md#virtual-machine-behavioral-analysis) : meer informatie over de virtuele machine gedragsanalyse en crash dump geheugenanalyse in Security Center
- [Veelgestelde vragen over Azure Security Center](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service
- [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) : Lees blogberichten over de Azure-beveiliging en naleving
