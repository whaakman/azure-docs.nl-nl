---
title: Overzicht van de beschikbaarheid van Zones | Microsoft Docs
description: Dit artikel bevat een overzicht van beschikbaarheid Zones in Azure.
services: 
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
tags: 
ms.assetid: 
ms.service: 
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2017
ms.author: markgal
ms.custom: mvc I am an ITPro and application developer, and I want to protect (use Availability Zones) my applications and data against data center failure (to build Highly Available applications).
ms.openlocfilehash: ac89562a5156282bb0fe8a6aa6761d6acb97f785
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="overview-of-availability-zones-in-azure-preview"></a>Overzicht van beschikbaarheid Zones in Azure (Preview)

Beschikbaarheid Zones helpen u te beschermen tegen storingen datacenter-niveau. Ze bevinden zich in een Azure-regio en elk een eigen onafhankelijke heeft energie-bron-, netwerk- en koeling. Tolerantie, zodat is er een minimum van drie afzonderlijke zones in alle ingeschakelde regio's. De fysieke en logische scheiding van beschikbaarheid Zones binnen een regio beschermt toepassingen en gegevens tegen zoneniveau fouten. 

![Conceptueel overzicht van één zone tijdelijk niet beschikbaar in een regio](./media/az-overview/az-graphic-two.png)

## <a name="regions-that-support-availability-zones"></a>Regio's die ondersteuning bieden voor beschikbaarheid Zones

- VS - oost 2
- West-Europa
- Frankrijk - centraal

## <a name="services-that-support-availability-zones"></a>Services die ondersteuning bieden voor beschikbaarheid Zones

De Azure-services die ondersteuning bieden voor beschikbaarheid Zones zijn:

- Virtuele Linux-machines
- Virtuele Windows-machines
- Zonal virtuele-Machineschaalsets
- Beheerde schijven
- Load Balancer
- Openbaar IP-adres

## <a name="supported-virtual-machine-size-families"></a>Ondersteunde virtuele machines grootte families

- Av2
- Dv2
- DSv2


## <a name="get-started-with-the-availability-zones-preview"></a>Aan de slag met de preview beschikbaarheid Zones

De evaluatieversie van de beschikbaarheid van Zones is beschikbaar in de VS-Oost 2, West-Europa en Frankrijk centrale regio's voor de specifieke Azure-services. 

1. [Aanmelden voor een voorbeeld bekijken van de beschikbaarheid van Zones](http://aka.ms/azenroll). 
2. Aanmelden bij uw Azure-abonnement.
3. Kies een regio die ondersteuning biedt voor beschikbaarheid Zones.
4. Gebruik een van de volgende koppelingen om te starten met behulp van de beschikbaarheid van Zones met uw service. 
    - [Een virtuele machine maken](../virtual-machines/windows/create-portal-availability-zone.md)
    - [Maken van een zonal virtuele-machineschaalset](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md)
    - [Voeg een schijf worden beheerd met behulp van PowerShell toe](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
    - [Load balancer](../load-balancer/load-balancer-standard-overview.md)

## <a name="next-steps"></a>Volgende stappen
- [Snelstartsjablonen](http://aka.ms/azqs)
