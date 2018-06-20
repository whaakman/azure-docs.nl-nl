---
title: Wat zijn Azure beschikbaarheid Zones? | Microsoft Docs
description: Zones beschikbaarheid bieden voor het maken van maximaal beschikbare en robuuste toepassingen in Azure, fysiek gescheiden locaties die u gebruiken kunt om uit te voeren van uw resources.
services: ''
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/08/2018
ms.author: iainfou
ms.custom: mvc I am an ITPro and application developer, and I want to protect (use Availability Zones) my applications and data against data center failure (to build Highly Available applications).
ms.openlocfilehash: 7cfd3fba459f409079cb0c621edab3197070285e
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232315"
---
# <a name="what-are-availability-zones-in-azure"></a>Wat zijn de beschikbaarheid van Zones in Azure?
Beschikbaarheid van Zones is een aanbieding die uw toepassingen en gegevens beveiligt tegen storingen van datacenter hoge beschikbaarheid. Beschikbaarheid Zones zijn unieke fysieke locaties binnen een Azure-regio. Elke zone bestaat uit een of meer datacenters uitgerust met onafhankelijke stroom, koeling en netwerken. Tolerantie, zodat is er een minimum van drie afzonderlijke zones in alle ingeschakelde regio's. De fysieke scheiding van beschikbaarheid Zones binnen een regio beschermt toepassingen en gegevens tegen storingen datacenter. Zone-redundante services repliceren uw toepassingen en gegevens over beschikbaarheid Zones beschermen tegen één punten van storingen. Met Zones, beschikbaarheid biedt Azure bedrijfstak best 99,99% VM bedrijfstijd SLA. In de volledige [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) wordt de gegarandeerde beschikbaarheid van Azure als geheel uitgelegd.

Een Zone beschikbaarheid in een Azure-regio is een combinatie van een domein met fouten en een updatedomein. Bijvoorbeeld, als u drie of meer virtuele machines over drie zones in een Azure-regio maakt, wordt uw virtuele machines worden effectief in drie domeinen met fouten en drie update domeinen gedistribueerd. De Azure-platform herkent deze verdeling tussen domeinen update om ervoor te zorgen dat virtuele machines in verschillende zones op hetzelfde moment niet worden bijgewerkt.

Hoge beschikbaarheid in de toepassingsarchitectuur van uw bouwen door CO-locatie op uw resources berekening, opslag, netwerken en gegevens binnen een zone en gerepliceerd in andere zones. Azure-services die ondersteuning bieden voor beschikbaarheid Zones onderverdeeld in twee categorieën:

- **Zonal services** – vast van de resource toe aan een specifieke zone (bijvoorbeeld virtuele machines, beheerde schijven, IP-adressen), of
- **Zone-redundante services** – platform repliceert automatisch diverse zones (bijvoorbeeld, zone-redundante opslag, SQL Database).

Bouw de toepassingsarchitectuur van uw met behulp van de combinatie van beschikbaarheid Zones met Azure-regio paren zodat uitgebreide zakelijke continuïteit in Azure. U kunt uw toepassingen en gegevens met behulp van beschikbaarheid Zones binnen een Azure-regio voor hoge beschikbaarheid synchroon worden gerepliceerd en asynchroon repliceren naar Azure-regio's voor disaster recovery-beveiliging.
 
![Conceptueel overzicht van één zone tijdelijk niet beschikbaar in een regio](./media/az-overview/az-graphic-two.png)

## <a name="regions-that-support-availability-zones"></a>Regio's die ondersteuning bieden voor beschikbaarheid Zones

- VS - midden
- Frankrijk - centraal
- VS-Oost 2 (Preview)
- West-Europa (Preview)
- Zuidoost-Azië (Preview)


## <a name="services-that-support-availability-zones"></a>Services die ondersteuning bieden voor beschikbaarheid Zones
De Azure-services die ondersteuning bieden voor beschikbaarheid Zones zijn:

- Linux Virtual Machines
- Windows Virtual Machines
- Virtual Machine Scale Sets
- Beheerde schijven
- Load Balancer
- Openbaar IP-adres
- Zoneredundante opslag
- SQL Database


## <a name="pricing"></a>Prijzen
Er is geen extra kosten voor virtuele machines die zijn geïmplementeerd in een Zone beschikbaarheid. Actieve tijdsduur van 99,99% VM SLA wordt aangeboden als twee of meer virtuele machines zijn geïmplementeerd op twee of meer beschikbaarheid Zones binnen een Azure-regio. Er zijn extra tussen beschikbaarheid Zone VM-VM gegevensoverdracht kosten. Raadpleeg voor meer informatie de [bandbreedte prijzen](https://azure.microsoft.com/pricing/details/bandwidth/) pagina.


## <a name="get-started-with-availability-zones"></a>Aan de slag met beschikbaarheid Zones
- [Een virtuele machine maken](../virtual-machines/windows/create-portal-availability-zone.md)
- [Voeg een schijf worden beheerd met behulp van PowerShell toe](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
- [Maak een zone redundant virtuele-machineschaalset](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
- [Taakverdeling van virtuele machines in zones met een standaard Load Balancer met een zone-redundante frontend.](../load-balancer/load-balancer-standard-public-zone-redundant-cli.md)
- [Taakverdeling van virtuele machines in een zone met een standaard Load Balancer met een zonal frontend.](../load-balancer/load-balancer-standard-public-zonal-cli.md)
- [Zone-redundante opslag](../storage/common/storage-redundancy-zrs.md)
- [SQL Database](../sql-database/sql-database-high-availability.md#zone-redundant-configuration-preview)


## <a name="next-steps"></a>Volgende stappen
- [Snelstartsjablonen](http://aka.ms/azqs)
