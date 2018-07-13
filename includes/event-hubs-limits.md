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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38755611"
---
De volgende tabel geeft een lijst van quota en limieten specifiek zijn voor [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Zie voor meer informatie over prijzen van Event Hubs [prijzen van Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

| Limiet | Bereik | Opmerkingen | Waarde |
| --- | --- | --- | --- | --- |
| Het aantal eventhubs per naamruimte |Naamruimte |De volgende aanvragen voor het maken van een nieuwe event hub worden geweigerd. |10 |
| Het aantal partities per event hub |Entiteit |- |32 |
| Aantal consumentengroepen per event hub |Entiteit |- |20 |
| Aantal AMQP-verbindingen per naamruimte |Naamruimte |De volgende aanvragen voor extra verbindingen geweigerd en wordt een uitzondering wordt ontvangen door de aanroepende code. |5.000 |
| Maximale grootte van Event Hubs-gebeurtenis|Entiteit |- |256 kB |
| Maximale grootte van de naam van een event hub |Entiteit |- |50 tekens |
| Aantal niet-epoche ontvangers per consumentengroep |Entiteit |- |5 |
| Maximale bewaarperiode van gebeurtenisgegevens |Entiteit |- |1-7 dagen |
| Maximum aantal Throughput Units |Naamruimte |De doorvoer unit-limiet zorgt ervoor dat uw gegevens om te worden beperkt en genereert een  **[ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)**. U kunt een groter aantal doorvoereenheden voor een standaard aanvragen laag door indiening een [ondersteuningsaanvraag](/azure/azure-supportability/how-to-create-azure-support-request). [Aanvullende throughput units](../articles/event-hubs/event-hubs-auto-inflate.md) zijn beschikbaar in blokken van 20 op basis van toegezegde aankoop. |20 |
| Aantal regels per naamruimte |Naamruimte|De volgende aanvragen voor het maken van de regel autorisatie worden geweigerd.|12 |
