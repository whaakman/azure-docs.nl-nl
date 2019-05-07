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
ms.date: 04/18/2019
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 302354484142619813ef58bf185c3556d4492de1
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65147788"
---
# <a name="what-are-availability-zones-in-azure"></a>Wat zijn Beschikbaarheidszones in Azure?
Beschikbaarheidszones is een aanbieding die uw toepassingen en gegevens beveiligt tegen storingen in datacenters hoge beschikbaarheid. Beschikbaarheidszones zijn unieke fysieke locaties binnen een Azure-regio. Elke zone bestaat uit een of meer datacenters die zijn uitgerust met onafhankelijke voeding, koeling en netwerken. Om voor tolerantie te zorgen, is er een minimum van drie afzonderlijke zones in alle ingeschakelde regio's. De fysieke scheiding tussen beschikbaarheidszones binnen een Azure-regio beschermt toepassingen en gegevens tegen storingen op zoneniveau. Zone-redundante services repliceren uw toepassingen en gegevens in meerdere Beschikbaarheidszones om te beschermen tegen enkele punten van de fout. Met beschikbaarheidszones biedt Azure de beste uptime SLA voor VM’s van de branche, van 99,99%. In de volledige [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) wordt de gegarandeerde beschikbaarheid van Azure als geheel uitgelegd.

Een beschikbaarheidszone in een Azure-regio is een combinatie van een foutdomein en een updatedomein. Als u bijvoorbeeld drie of meer virtuele machines in drie zones in een Azure-regio maakt, worden uw virtuele machines effectief over drie foutdomeinen en drie updatedomeinen verdeeld. Het Azure-platform herkent deze verdeling over updatedomeinen om ervoor te zorgen dat virtuele machines in verschillende zones niet op hetzelfde moment worden bijgewerkt.

Hoge beschikbaarheid bouwen in uw toepassingsarchitectuur door het plaatsen van uw resources compute, opslag, netwerken en gegevens binnen een zone en gerepliceerd in andere zones. Azure-services die ondersteuning voor Beschikbaarheidszones worden onderverdeeld in twee categorieën:

- **Zonegebonden services** – u de resource vastmaken aan een specifieke zone (bijvoorbeeld virtuele machines, beheerde schijven, IP-adressen), of
- **Zone-redundante services** – platform automatisch worden gerepliceerd in zones (bijvoorbeeld: zone-redundante opslag, SQL Database).

Bouw uw toepassingsarchitectuur met behulp van de combinatie van de Beschikbaarheidszones met Azure-regioparen voor het bereiken van uitgebreide bedrijfscontinuïteit in Azure. U kunt uw toepassingen en gegevens met behulp van Beschikbaarheidszones binnen een Azure-regio voor hoge beschikbaarheid synchroon te repliceren en asynchroon repliceert tussen Azure-regio's voor disaster recovery-beveiliging.
 
![Conceptueel overzicht van een zone in een regio uitvalt](./media/az-overview/az-graphic-two.png)

## <a name="services-support-by-region"></a>Ondersteuning voor services per regio

De combinaties van Azure-services en regio's die ondersteuning voor Beschikbaarheidszones zijn:


|                                 |Noord- en Zuid-Amerika |              |           |           | Europa |              |          |              | Azië en Stille Oceaan |                 |
|----------------------------|----------|----------|---------|---------|--------------|------------|--------|----------|----------|-------------|
|          |US - centraal|US - oost|US - oost 2|US - west 2|Frankrijk - centraal|Europa - noord|Verenigd Koninkrijk Zuid|Europa -west|Japan - oost|Azië - zuidoost|
| **Compute**                         |            |              |           |           |                |              |          |             |            |                |
| Linux Virtual Machines          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| Windows Virtual Machines        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| Virtual Machine Scale Sets      | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| **Storage**   |            |              |           |           |                |              |          |             |            |                |
| Beheerde schijven                   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| Zone-redundant Storage          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| **Netwerken**                     |            |              |           |           |                |              |          |             |            |                |
| Standaard IP-adres        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| Standaardversie van Load Balancer     | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |
| VPN Gateway                     | &#10003;   |              | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    |            | &#10003;       |
| ExpressRoute                    | &#10003;   |              | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    |            | &#10003;       |
| Application Gateway (Preview)   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    |            | &#10003;       |
| **Databases**                     |            |              |           |           |                |              |          |             |            |                |
| SQL Database                    | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    |            | &#10003;       |
| **Analytische gegevens**                       |            |              |           |           |                |              |          |             |            |                |
| Event Hubs                      | &#10003;   |              | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    |            | &#10003;       |
| **Integratie**                     |            |              |           |           |                |              |          |             |            |                |
| Servicebus (alleen Premiumlaag) | &#10003;   |              | &#10003;  | &#10003;  | &#10003;       | &#10003;     |          | &#10003;    |            | &#10003;       |



## <a name="services-resiliency"></a>Services tolerantie
Alle Azure management-services zijn ontworpen om tolerantie van niveau regio fouten. In het spectrum van fouten hebben een of meer fouten binnen een Beschikbaarheidszone binnen een regio een kleinere fout radius in vergelijking met een hele regio-fout. Azure kunt herstellen vanuit een storing zone-niveau van van beheerservices binnen de regio of vanuit een andere Azure-regio. Azure voert één zone essentieel onderhoud op een tijdstip binnen een regio, om te voorkomen dat eventuele fouten die invloed hebben op de resources van de klant is geïmplementeerd in meerdere Beschikbaarheidszones binnen een regio.

## <a name="pricing"></a>Prijzen
Er is geen extra kosten voor virtuele machines die in een Beschikbaarheidszone is geïmplementeerd. Virtuele machine een uptime van 99,99% SLA wordt aangeboden wanneer twee of meer virtuele machines in twee of meer Beschikbaarheidszones binnen een Azure-regio worden geïmplementeerd. Er zijn aanvullende tussen beschikbaarheid Zone VM naar VM kosten voor gegevensoverdracht. Raadpleeg voor meer informatie de [bandbreedte prijzen](https://azure.microsoft.com/pricing/details/bandwidth/) pagina.


## <a name="get-started-with-availability-zones"></a>Aan de slag met Beschikbaarheidszones
- [Maak een virtuele machine](../virtual-machines/windows/create-portal-availability-zone.md)
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
