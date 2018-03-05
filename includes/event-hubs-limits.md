---
title: bestand opnemen
description: bestand opnemen
services: event-hubs
author: sethmanheim
ms.service: event-hubs
ms.topic: include
ms.date: 02/26/2018
ms.author: sethm
ms.custom: include file
ms.openlocfilehash: ab4c5b98ed9f6fcc8c271797db2d81dcc7ec4449
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
De volgende tabel geeft een lijst van quota en limieten specifiek voor [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Zie voor meer informatie over prijzen van Event Hubs [prijzen van Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

| Limiet | Bereik | Opmerkingen | Waarde |
| --- | --- | --- | --- | --- |
| Nummer van event hubs per naamruimte |Naamruimte |De volgende aanvragen voor het maken van nieuwe event hub worden geweigerd. |10 |
| Aantal partities per event hub |Entiteit |- |32 |
| Aantal consumergroepen per event hub |Entiteit |- |20 |
| Aantal AMQP-verbindingen per naamruimte |Naamruimte |De volgende aanvragen voor aanvullende verbindingen wordt geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |5.000 |
| Maximale grootte van Event Hubs-gebeurtenis|Entiteit |- |256 kB |
| Maximale grootte van de naam van een event hub |Entiteit |- |50 tekens |
| Het aantal niet-epoche ontvangers per klantengroep |Entiteit |- |5 |
| Maximale bewaarperiode van gebeurtenisgegevens |Entiteit |- |1-7 dagen |
| Maximum aantal Throughput Units |Naamruimte |De limiet is overschreden doorvoer eenheid zorgt ervoor dat uw gegevens worden beperkt en genereert een  **[ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)**. U kunt een groter aantal doorvoereenheden voor een standaard aanvragen laag met indiening een [ondersteuningsaanvraag](/azure/azure-supportability/how-to-create-azure-support-request). [Aanvullende doorvoereenheden](../articles/event-hubs/event-hubs-auto-inflate.md) zijn beschikbaar in blokken van 20 op basis van de vastgelegde aankoop. |20 |
| Aantal autorisatieregels per naamruimte |Naamruimte|De volgende aanvragen voor het maken van de regel autorisatie worden geweigerd.|12 |
