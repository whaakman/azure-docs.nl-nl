---
title: bestand opnemen
description: bestand opnemen
services: service-bus-messaging
author: sethmanheim
ms.service: service-bus-messaging
ms.topic: include
ms.date: 02/12/2018
ms.author: sethm
ms.custom: include file
ms.openlocfilehash: 74732008b336dc1b95ec96e8550d218105973ca4
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
De volgende tabel bevat de quota voor specifieke informatie voor Service Bus-berichtenservice. Zie voor informatie over prijzen en andere quota's voor Service Bus de [prijzen van Service Bus](https://azure.microsoft.com/pricing/details/service-bus/) overzicht.

| De naam van de quota | Bereik | Opmerkingen | Waarde |
| --- | --- | --- | --- | --- |
| Maximum aantal basic / standaard naamruimten per Azure-abonnement |Naamruimte |De volgende aanvragen voor aanvullende basic / standaard naamruimten worden geweigerd door de portal. |100|
| Maximum aantal premium-naamruimten per Azure-abonnement |Naamruimte |De volgende aanvragen voor aanvullende premium-naamruimten worden geweigerd door de portal. |10 |
| De grootte van de wachtrij/onderwerp |Entiteit |Na het maken van de wachtrij/onderwerp gedefinieerd. <br/><br/> Daaropvolgende binnenkomende berichten worden geweigerd en een uitzondering is ontvangen door de aanroepende code. |1, 2, 3, 4 of 5 GB.<br /><br />Als [partitioneren](../articles/service-bus-messaging/service-bus-partitioning.md) is ingeschakeld, de grootte van de maximale wachtrij/onderwerp is 80 GB. |
| Aantal gelijktijdige verbindingen voor een naamruimte |Naamruimte |De volgende aanvragen voor aanvullende verbindingen worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. REST-bewerkingen meetellen niet voor gelijktijdige TCP-verbindingen. |NetMessaging: 1000<br /><br />AMQP: 5.000 |
| Aantal gelijktijdige aanvragen voor een entiteit onderwerp-wachtrij-abonnement ontvangen |Entiteit |Geen verdere ontvangen aanvragen worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. Dit quotum is van toepassing op het gecombineerde aantal gelijktijdige bewerkingen ontvangen via alle abonnementen op een onderwerp. |5.000 |
| Aantal onderwerpen/wachtrijen per Servicenaamruimte |Naamruimte |De volgende aanvragen voor het maken van een nieuw onderwerp of een wachtrij op de naamruimte van de service worden geweigerd. Als gevolg hiervan, als geconfigureerd via de [Azure-portal][Azure portal], een foutbericht wordt gegenereerd. Als de beheer-API aangeroepen, wordt een uitzondering ontvangen door de aanroepende code. |10.000<br /><br />Het totale aantal onderwerpen plus wachtrijen in een Servicenaamruimte moet kleiner dan of gelijk aan 10.000.<br/>Dit is niet van toepassing op Premium omdat alle entiteiten worden gepartitioneerd. |
| Aantal gepartitioneerde onderwerpen/wachtrijen per Servicenaamruimte |Naamruimte |De volgende aanvragen voor het maken van een nieuw gepartitioneerde onderwerp of een wachtrij op de naamruimte van de service worden geweigerd. Als gevolg hiervan, als geconfigureerd via de [Azure-portal][Azure portal], een foutbericht wordt gegenereerd. Als aangeroepen vanuit de beheer-API, een **QuotaExceededException** uitzondering wordt ontvangen door de aanroepende code. |Basis en standaard lagen - 100<br />[Premium](../articles/service-bus-messaging/service-bus-premium-messaging.md) -1000 (per messaging-eenheid)<br/><br />Elke gepartitioneerde wachtrij of onderwerp telt naar het quotum van 10.000 entiteiten per naamruimte. |
| Maximale grootte van elk messaging entiteit-pad: wachtrij of onderwerp |Entiteit |- |260 tekens bevatten |
| Maximale grootte van alle messaging entiteitsnaam: naamruimte, abonnement of abonnement regel |Entiteit |- |50 tekens |
| Grootte van berichten voor een entiteit van het onderwerp-wachtrij-abonnement |Entiteit |Binnenkomende berichten die groter is dan deze quota worden geweigerd en een uitzondering is ontvangen door de aanroepende code. |Maximale berichtgrootte: 256 KB ([standaardcategorie](../articles/service-bus-messaging/service-bus-premium-messaging.md)) / 1 MB ([Premium-laag](../articles/service-bus-messaging/service-bus-premium-messaging.md)). <br /><br />**Opmerking** vanwege systeembelasting, deze limiet is meestal iets minder.<br /><br />Maximale header-grootte: 64 KB<br /><br />Maximum aantal eigenschappen in de eigenschappenverzameling header: **byte/int. MaxValue**<br /><br />Maximale grootte van de eigenschap in de eigenschappenverzameling: geen expliciete limiet. Beperkt door de maximale header-grootte. |
| Eigenschap berichtgrootte voor een entiteit van het onderwerp-wachtrij-abonnement |Entiteit |Een **SerializationException** uitzondering wordt gegenereerd. |Maximale berichtgrootte eigenschap voor elke eigenschap is 32 K. cumulatieve grootte van alle eigenschappen kan niet groter zijn dan 64 k Dit geldt voor de hele koptekst van de [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage), die beide heeft gebruikerseigenschappen evenals Systeemeigenschappen (zoals [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber), [Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label), [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid), enzovoort). |
| Het aantal abonnementen per onderwerp |Entiteit |De volgende aanvragen voor het maken van aanvullende abonnementen voor het onderwerp worden geweigerd. Als gevolg hiervan als geconfigureerd via de portal, wordt een foutbericht weergegeven. Als de naam van de API management wordt een uitzondering ontvangen door de aanroepende code. |2,000 |
| Aantal SQL-filters per onderwerp |Entiteit |De volgende aanvragen voor het maken van meer filters in het onderwerp worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |2,000 |
| Aantal correlatie filters per onderwerp |Entiteit |De volgende aanvragen voor het maken van meer filters in het onderwerp worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |100,000 |
| Grootte van de SQL-filters/acties |Naamruimte |De volgende aanvragen voor het maken van meer filters worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |Maximale lengte van de filtertekenreeks voor de voorwaarde: 1024 (1K).<br /><br />Maximale lengte van de regel actie tekenreeks: 1024 (1K).<br /><br />Maximum aantal expressies per regelactie: 32. |
| Aantal [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) regels per naamruimte, wachtrij of onderwerp |Entiteit, naamruimte |De volgende aanvragen voor het maken van aanvullende regels worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |Maximum aantal regels: 12. <br /><br /> Regels die zijn geconfigureerd op een Service Bus-naamruimte van toepassing op alle wachtrijen en onderwerpen in die naamruimte. |
| Aantal berichten per transactie | Transactie | Extra berichten worden geweigerd en een uitzondering met de mededeling ' kan niet meer dan 100 berichten verzenden in één transactie' wordt ontvangen door de aanroepende code. | 100 <br /><br /> Voor beide **Send()** en **SendAsync()** bewerkingen. |

[Azure portal]: https://portal.azure.com