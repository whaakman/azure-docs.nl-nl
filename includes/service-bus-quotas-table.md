---
title: bestand opnemen
description: bestand opnemen
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 12/13/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: bb5dc4e01a872178af1bc7d5d57fc189424e7e9d
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553660"
---
De volgende tabel bevat quotumgegevens die specifiek zijn voor Azure Service Bus-berichten. Zie voor informatie over prijzen en andere quota's voor Service Bus [prijzen van Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

| Quotumnaam | Bereik | Opmerkingen | Value |
| --- | --- | --- | --- | --- |
| Maximum aantal Basic of Standard-naamruimten per Azure-abonnement |Naamruimte |De volgende aanvragen voor extra Basic of Standard-naamruimten worden geweigerd door de Azure-portal. |100|
| Maximum aantal Premium-naamruimten per Azure-abonnement |Naamruimte |De volgende aanvragen voor extra Premium-naamruimten worden geweigerd door de portal. |25 |
| Grootte van de wachtrij of onderwerp |Entiteit |Gedefinieerd bij het maken van de wachtrij of onderwerp. <br/><br/> Binnenkomende berichten worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |1, 2, 3, 4 of 5 GB.<br /><br />In de Premium-SKU, en de standaard-SKU met [partitioneren](/azure/service-bus-messaging/service-bus-partitioning) ingeschakeld, is de maximale grootte van de wachtrij of onderwerp 80 GB. |
| Aantal gelijktijdige verbindingen voor een naamruimte |Naamruimte |De volgende aanvragen voor extra verbindingen worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. REST-bewerkingen tellen niet mee voor gelijktijdige TCP-verbindingen. |NetMessaging: 1,000.<br /><br />AMQP: 5,000. |
| Aantal gelijktijdige aanvragen voor een entiteit wachtrij, onderwerp of abonnement ontvangen |Entiteit |Volgende ontvangen aanvragen zijn afgewezen en een uitzondering is ontvangen door de aanroepende code. Dit quotum is van toepassing op het gecombineerde aantal gelijktijdige bewerkingen voor alle abonnementen op een onderwerp ontvangen. |5.000 |
| Aantal onderwerpen of wachtrijen per naamruimte |Naamruimte |De volgende aanvragen voor het maken van een nieuw onderwerp of een wachtrij op de naamruimte worden geweigerd. Als gevolg hiervan, indien geconfigureerd via de [Azure-portal][Azure portal], een foutbericht dat wordt gegenereerd. Als met de naam van de beheer-API, wordt een uitzondering ontvangen door de aanroepende code. |1000 voor de Basic- of Standard-laag. Het totale aantal onderwerpen en -wachtrijen in een naamruimte moet kleiner dan of gelijk zijn aan 1000. <br/><br/>Voor de Premium-laag, 1000 per messaging-eenheid (MU). Maximale limiet is 4000. |
| Aantal [gepartitioneerd onderwerpen of wachtrijen](/azure/service-bus-messaging/service-bus-partitioning) per naamruimte |Naamruimte |De volgende aanvragen voor het maken van een nieuwe gepartitioneerde onderwerp of een wachtrij op de naamruimte worden geweigerd. Als gevolg hiervan, indien geconfigureerd via de [Azure-portal][Azure portal], een foutbericht dat wordt gegenereerd. Als met de naam van de beheer-API, de uitzondering **QuotaExceededException** wordt ontvangen door de aanroepende code. |Basic en Standard-lagen: 100.<br/><br/>Gepartitioneerde entiteiten worden niet ondersteund in de [Premium](../articles/service-bus-messaging/service-bus-premium-messaging.md) laag.<br/><br />Elke gepartitioneerde wachtrij of onderwerp telt mee voor het quotum van 1000 entiteiten per naamruimte. |
| Maximale grootte van alle berichten pad van de entiteit: wachtrij of onderwerp |Entiteit |- |260 tekens. |
| Maximale grootte van alle berichten entiteitnaam: naamruimte, abonnement of het abonnement regel |Entiteit |- |50 tekens. |
| Maximale grootte van een [bericht-ID](/dotnet/api/microsoft.azure.servicebus.message.messageid) | Entiteit |- | 128 |
| Maximale grootte van een bericht [sessie-ID](/dotnet/api/microsoft.azure.servicebus.message.sessionid) | Entiteit |- | 128 |
| Grootte van het bericht voor een entiteit wachtrij, onderwerp of abonnement |Entiteit |Binnenkomende berichten die groter zijn dan deze quota worden geweigerd en een uitzondering is ontvangen door de aanroepende code. |Maximale berichtgrootte: 256 KB voor [Standard-laag](../articles/service-bus-messaging/service-bus-premium-messaging.md), 1 MB voor [Premium-laag](../articles/service-bus-messaging/service-bus-premium-messaging.md). <br /><br />Vanwege de systeembelasting is deze limiet kleiner dan deze waarden.<br /><br />Maximale header-grootte: 64 KB.<br /><br />Maximum aantal eigenschappen van de koptekst in eigenschappenverzameling: **byte/int. MaxValue**.<br /><br />Maximale grootte van de eigenschap in de eigenschappenverzameling: Er is geen expliciete limiet. Beperkt door de maximale header-grootte. |
| De eigenschap berichtgrootte voor een entiteit wachtrij, onderwerp of abonnement |Entiteit | De uitzondering **SerializationException** wordt gegenereerd. |Maximale berichtgrootte eigenschap voor elke eigenschap is 32.000. Cumulatieve grootte van alle eigenschappen niet langer zijn dan 64.000. Deze limiet geldt voor de hele koptekst van de [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage), die is voorzien zowel gebruikersgegevens en -eigenschappen, zoals [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber), [Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label), en [ MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid). |
| Aantal abonnementen per onderwerp |Entiteit |De volgende aanvragen voor het maken van aanvullende abonnementen voor het onderwerp worden geweigerd. Als gevolg hiervan als geconfigureerd via de portal, wordt een foutbericht weergegeven. Als met de naam van de beheer-API, wordt een uitzondering ontvangen door de aanroepende code. |Standard-laag: Elk abonnement telt mee voor het quotum van 1000 entiteiten, dat wil zeggen, wachtrijen, onderwerpen en abonnementen, per naamruimte. <br/> <br/> Premium-laag: 2,000. |
| Het aantal SQL-filters per onderwerp |Entiteit |De volgende aanvragen voor het maken van aanvullende filters op het onderwerp worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |2,000 |
| Aantal correlatiefilters per onderwerp |Entiteit |De volgende aanvragen voor het maken van aanvullende filters op het onderwerp worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |100.000 |
| Grootte van de SQL-filters of acties |Naamruimte |De volgende aanvragen voor het maken van extra filters worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |Maximale lengte van de filtertekenreeks voor de voorwaarde: 1024 (1 K).<br /><br />Maximale lengte van de regel actie tekenreeks: 1024 (1 K).<br /><br />Maximum aantal expressies per regelactie: 32. |
| Aantal [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) regels per naamruimte, wachtrij of onderwerp |Entiteit, naamruimte |De volgende aanvragen voor het maken van aanvullende regels worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |Maximum aantal regels: 12. <br /><br /> Regels die zijn geconfigureerd op een Service Bus-naamruimte van toepassing op alle wachtrijen en onderwerpen in die naamruimte. |
| Aantal berichten per transactie | Transactie | Extra binnenkomende berichten worden geweigerd en een uitzondering met de mededeling ' kan niet meer dan 100 berichten verzenden in één transactie"is ontvangen door de aanroepende code. | 100 <br /><br /> Voor beide **Send()** en **SendAsync()** bewerkingen. |

[Azure portal]: https://portal.azure.com