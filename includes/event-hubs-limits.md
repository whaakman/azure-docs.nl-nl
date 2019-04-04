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
ms.openlocfilehash: 9d6b54027adcf2b12c6ca4081a11208a31f620e8
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58919210"
---
De volgende tabel geeft een lijst van quota en limieten specifiek zijn voor [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Zie voor meer informatie over prijzen van Event Hubs [prijzen van Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

| Limiet | Bereik | Opmerkingen | Value |
| --- | --- | --- | --- |
| Het aantal Event Hubs-naamruimten per abonnement |Abonnement |- |100 |
| Het aantal eventhubs per naamruimte |Naamruimte |De volgende aanvragen voor het maken van een nieuwe event hub worden geweigerd. |10 |
| Het aantal partities per event hub |Entiteit |- |32 |
| Aantal consumentengroepen per event hub |Entiteit |- |20 |
| Aantal AMQP-verbindingen per naamruimte |Naamruimte |De volgende aanvragen voor extra verbindingen worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |5.000 |
| Maximale grootte van Event Hubs-gebeurtenis|Entiteit |- |1 MB |
| Maximale grootte van de naam van een event hub |Entiteit |- |50 tekens |
| Aantal niet-epoche ontvangers per consumentengroep |Entiteit |- |5 |
| Maximale bewaarperiode van gebeurtenisgegevens |Entiteit |- |1-7 dagen |
| Maximum aantal Throughput Units |Naamruimte |De doorvoer unit-limiet zorgt ervoor dat uw gegevens om te worden beperkt en genereert een [server bezet uitzondering](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception). Om aan te vragen een groter aantal doorvoereenheden voor Standard-laag, het bestand een [ondersteuningsaanvraag](/azure/azure-supportability/how-to-create-azure-support-request). [Aanvullende throughput units](../articles/event-hubs/event-hubs-auto-inflate.md) zijn beschikbaar in blokken van 20 op basis van toegezegde aankoop. |20 |
| Aantal regels per naamruimte |Naamruimte|De volgende aanvragen voor het maken van de regel autorisatie worden geweigerd.|12 |
