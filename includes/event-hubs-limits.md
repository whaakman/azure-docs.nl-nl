---
title: bestand opnemen
description: bestand opnemen
services: event-hubs
author: sethmanheim
ms.service: event-hubs
ms.topic: include
ms.date: 05/22/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: b105fb14608d53c5c2ef469ab44e211ccdf4d3c8
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67812526"
---
De volgende tabel geeft een lijst van quota en limieten specifiek zijn voor [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Zie voor meer informatie over prijzen van Event Hubs [prijzen van Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

| Limiet | Scope | Opmerkingen | Waarde |
| --- | --- | --- | --- |
| Het aantal Event Hubs-naamruimten per abonnement |Subscription |- |100 |
| Het aantal eventhubs per naamruimte |Naamruimte |De volgende aanvragen voor het maken van een nieuwe event hub worden geweigerd. |10 |
| Het aantal partities per event hub |Entiteit |- |32 |
| Aantal consumentengroepen per event hub |Entiteit |- |20 |
| Aantal AMQP-verbindingen per naamruimte |Naamruimte |De volgende aanvragen voor extra verbindingen worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |5,000 |
| Maximale grootte van Event Hubs-gebeurtenis|Entiteit |- |1 MB |
| Maximale grootte van de naam van een event hub |Entiteit |- |50 tekens |
| Aantal niet-epoche ontvangers per consumentengroep |Entiteit |- |5 |
| Maximale bewaarperiode van gebeurtenisgegevens |Entiteit |- |1-7 dagen |
| Maximum aantal Throughput Units |Naamruimte |De doorvoer unit-limiet zorgt ervoor dat uw gegevens om te worden beperkt en genereert een [server bezet uitzondering](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception). Om aan te vragen een groter aantal doorvoereenheden voor Standard-laag, het bestand een [ondersteuningsaanvraag](/azure/azure-supportability/how-to-create-azure-support-request). [Aanvullende throughput units](../articles/event-hubs/event-hubs-auto-inflate.md) zijn beschikbaar in blokken van 20 op basis van toegezegde aankoop. |20 |
| Aantal regels per naamruimte |Naamruimte|De volgende aanvragen voor het maken van de regel autorisatie worden geweigerd.|12 |
| Aantal aanroepen naar de methode GetRuntimeInformation | Entiteit | - | 50 per seconde | 
| Aantal virtuele netwerk (VNet) en IP-Config-regels | Entiteit | - | 128 | 

### <a name="event-hubs-dedicated---quotas-and-limits"></a>Event Hubs Dedicated - quota en limieten
De aanbieding Event Hubs Dedicated wordt in rekening gebracht tegen een vaste maandelijkse prijs met een minimum van 4 uur van het gebruik van. De speciale laag biedt alle functies van de Standard-abonnement, maar met enterprise schaal capaciteiten en limieten voor klanten met veeleisende workloads. 

| Functie | Limieten |
| --- | ---|
| Bandbreedte |  20 Cu 's |
| Naamruimten | 50 per Capaciteitseenheid |
| Event Hubs |  1000 per naamruimte |
| Ingangsgebeurtenissen | Inbegrepen |
| Berichtgrootte | 1 Million Bytes |
| Partities | 2000 per Capaciteitseenheid |
| Consumergroepen | Geen limiet per Capaciteitseenheid, 1000 per event hub |
| Brokered Connections | 100 K inbegrepen |
| Bewaartermijn voor berichten | Omhoog tot 7 dagen (90 dagen bewaard binnenkort beschikbaar), inbegrepen 10 TB per Capaciteitseenheid |
| Capture | Inbegrepen |
