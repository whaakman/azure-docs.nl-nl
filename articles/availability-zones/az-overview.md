---
title: Wat zijn Azure-Beschikbaarheidszones? | Microsoft Docs
description: Beschikbaarheidszones bieden hoge beschikbaarheid en robuuste om toepassingen te maken in Azure, fysiek gescheiden locaties die u gebruiken kunt om uit te voeren van uw resources.
services: ''
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2018
ms.author: cynthn
ms.custom: mvc I am an ITPro and application developer, and I want to protect (use Availability Zones) my applications and data against data center failure (to build Highly Available applications).
ms.openlocfilehash: 11b29a1639be5bf6a7820b872cbc2ce78f002b4f
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55565175"
---
# <a name="what-are-availability-zones-in-azure"></a>Wat zijn Beschikbaarheidszones in Azure?
Beschikbaarheidszones is een aanbieding die uw toepassingen en gegevens beveiligt tegen storingen in datacenters hoge beschikbaarheid. Beschikbaarheidszones zijn unieke fysieke locaties binnen een Azure-regio. Elke zone bestaat uit een of meer datacenters die zijn uitgerust met onafhankelijke voeding, koeling en netwerken. Om ervoor te zorgen tolerantie, is er een minimum van drie afzonderlijke zones in alle ingeschakelde regio's. De fysieke scheiding tussen Beschikbaarheidszones binnen een regio beschermt toepassingen en gegevens tegen storingen in datacenters. Zone-redundante services repliceren uw toepassingen en gegevens in meerdere Beschikbaarheidszones om te beschermen tegen enkele punten van de fout. Met Beschikbaarheidszones biedt Azure branche aanbevolen VM uptime SLA van 99,99%. In de volledige [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) wordt de gegarandeerde beschikbaarheid van Azure als geheel uitgelegd.

Een Beschikbaarheidszone in een Azure-regio is een combinatie van een domein met fouten en een updatedomein. Bijvoorbeeld, als u drie of meer virtuele machines in drie zones in een Azure-regio maken, wordt uw virtuele machines worden effectief in drie foutdomeinen en updatedomeinen drie gedistribueerd. Het Azure-platform herkent deze distributie in meerdere domeinen bijwerken om ervoor te zorgen dat virtuele machines in verschillende zones niet op hetzelfde moment worden bijgewerkt.

Hoge beschikbaarheid bouwen in uw toepassingsarchitectuur door het plaatsen van uw resources compute, opslag, netwerken en gegevens binnen een zone en gerepliceerd in andere zones. Azure-services die ondersteuning voor Beschikbaarheidszones worden onderverdeeld in twee categorieën:

- **Zonegebonden services** – u de resource vastmaken aan een specifieke zone (bijvoorbeeld virtuele machines, beheerde schijven, IP-adressen), of
- **Zone-redundante services** – platform automatisch worden gerepliceerd in zones (bijvoorbeeld: zone-redundante opslag, SQL Database).

Bouw uw toepassingsarchitectuur met behulp van de combinatie van de Beschikbaarheidszones met Azure-regioparen voor het bereiken van uitgebreide bedrijfscontinuïteit in Azure. U kunt uw toepassingen en gegevens met behulp van Beschikbaarheidszones binnen een Azure-regio voor hoge beschikbaarheid synchroon te repliceren en asynchroon repliceert tussen Azure-regio's voor disaster recovery-beveiliging.
 
![Conceptueel overzicht van een zone in een regio uitvalt](./media/az-overview/az-graphic-two.png)

## <a name="regions-that-support-availability-zones"></a>Regio's die ondersteuning voor Beschikbaarheidszones

- US - centraal
- US - oost 2
- Frankrijk - centraal
- Europa - noord
- Azië - zuidoost 
- Europa -west
- US - west 2



## <a name="services-that-support-availability-zones"></a>Services die ondersteuning voor Beschikbaarheidszones bieden
De Azure-services die ondersteuning voor Beschikbaarheidszones zijn:

- Linux Virtual Machines
- Windows Virtual Machines
- Virtual Machine Scale Sets
- Beheerde schijven
- Load Balancer
- Openbaar IP-adres
- Zone-redundante opslag
- SQL Database
- Event Hubs
- Servicebus (alleen Premiumlaag)
- VPN Gateway
- ExpressRoute
- Application Gateway (preview)


## <a name="pricing"></a>Prijzen
Er is geen extra kosten voor virtuele machines die in een Beschikbaarheidszone is geïmplementeerd. Virtuele machine een uptime van 99,99% SLA wordt aangeboden wanneer twee of meer virtuele machines in twee of meer Beschikbaarheidszones binnen een Azure-regio worden geïmplementeerd. Er zijn aanvullende tussen beschikbaarheid Zone VM naar VM kosten voor gegevensoverdracht. Raadpleeg voor meer informatie de [bandbreedte prijzen](https://azure.microsoft.com/pricing/details/bandwidth/) pagina.


## <a name="get-started-with-availability-zones"></a>Aan de slag met Beschikbaarheidszones
- [Een virtuele machine maken](../virtual-machines/windows/create-portal-availability-zone.md)
- [Toevoegen van een beheerde schijf maken met behulp van PowerShell](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
- [Een schaalset voor zone-redundante virtuele-machine maken](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
- [Taakverdeling tussen VM's in zones met behulp van een Standard Load Balancer met een zone-redundante front](../load-balancer/load-balancer-standard-public-zone-redundant-cli.md)
- [Taakverdeling tussen VM's binnen een zone met behulp van een Standard Load Balancer met een zonegebonden frontend.](../load-balancer/load-balancer-standard-public-zonal-cli.md)
- [Zone-redundante opslag](../storage/common/storage-redundancy-zrs.md)
- [SQL Database](../sql-database/sql-database-high-availability.md#zone-redundant-configuration)
- [Geo-noodherstel Event Hubs](../event-hubs/event-hubs-geo-dr.md#availability-zones)
- [Geo-noodherstel Service Bus](../service-bus-messaging/service-bus-geo-dr.md#availability-zones)
- [Een zone-redundante virtuele netwerkgateway maken](../vpn-gateway/create-zone-redundant-vnet-gateway.md)


## <a name="next-steps"></a>Volgende stappen
- [Snelstartsjablonen](https://aka.ms/azqs)
