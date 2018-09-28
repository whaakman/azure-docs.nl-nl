---
title: Azure Service Bus-onderwerpfilters | Microsoft Docs
description: Filteren van Azure Service Bus-onderwerpen
services: service-bus-messaging
documentationcenter: ''
author: clemensv
manager: timlt
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2018
ms.author: spelluru
ms.openlocfilehash: 08a90811356f508eebfed9f88500a694f2fbd83e
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47407124"
---
# <a name="topic-filters-and-actions"></a>Onderwerpfilters en acties

Abonnees kunnen definiëren welke berichten ze willen ontvangen van een onderwerp. Deze berichten zijn opgegeven in de vorm van een of meer benoemde abonnementsregels. Elke regel bestaat uit een voorwaarde waarmee wordt bepaald berichten en een actie die de geselecteerde bericht annotates geselecteerd. Voor elke regelvoorwaarde waaraan wordt voldaan, produceert het abonnement een kopie van het bericht, dat anders voor elke overeenkomende regel anders kan worden geannoteerd.

Elk onderwerpabonnement zojuist gemaakte heeft een regel voor initiële standaard-abonnement. Als u niet expliciet een filtervoorwaarde voor de regel opgeeft, wordt de toegepaste filter is de **waar** filter waarmee alle berichten worden geselecteerd in het abonnement. De standaardregel heeft geen bijbehorende aantekening-actie.

Service Bus ondersteunt drie filtervoorwaarden:

-   *Booleaanse filters* : de **TrueFilter** en **FalseFilter** ofwel ervoor zorgen dat alle binnenkomende berichten (**waar**) of geen van de binnenkomende berichten (**false**) worden geselecteerd voor het abonnement.

-   *SQL-Filters* : een **SqlFilter** bevat een SQL-achtige voorwaardelijke expressie die wordt geëvalueerd in de broker op basis van de gebruiker gedefinieerde eigenschappen en eigenschappen van de binnenkomende berichten. Alle Systeemeigenschappen moeten worden voorafgegaan door `sys.` in de voorwaardelijke expressie. De [subset van de SQL-taal voor de filtervoorwaarden](service-bus-messaging-sql-filter.md) tests sprake is van de eigenschappen (`EXISTS`), evenals als voor null-waarden (`IS NULL`), logische NOT/en/of, relationele operators, eenvoudige numerieke rekenkundige bewerking, en eenvoudige tekst patroon die overeenkomt met `LIKE`.

-   *Correlatiefilters* : een **CorrelationFilter** bevat een set voorwaarden die worden vergeleken met een of meer van de gebruikers- en eigenschappen van een binnenkomende bericht. Er wordt vaak gebruikt voor het vergelijken van de **CorrelationId** eigenschap, maar de toepassing kan er ook voor kiezen om te vergelijken met **ContentType**, **Label**,  **MessageId**, **ReplyTo**, **ReplyToSessionId**, **SessionId**, **naar**, en een door de gebruiker gedefinieerd de eigenschappen. Een overeenkomst wordt gevonden wanneer de waarde van een binnenkomende bericht voor een eigenschap gelijk aan de waarde die is opgegeven in het correlatiefilter is. Voor expressies voor verbindingsreeksen, zijn de vergelijking is hoofdlettergevoelig. Bij het opgeven van meerdere identieke eigenschappen, het filter combineert ze als een logische voorwaarde en, wat betekent dat voor het filter zodat deze overeenkomen met, moeten aan alle voorwaarden voldoet.

Alle filters evalueren eigenschappen van berichten. De berichttekst kunnen niet worden geëvalueerd door filters.

Filterregels voor complexe nodig verwerkingscapaciteit. Het gebruik van SQL-filterregels resulteert in het bijzonder in lagere totale doorvoer van berichten op het niveau van het abonnement, het onderwerp en naamruimte. Kies indien mogelijk toepassingen moeten correlatiefilters via SQL-achtige filters, omdat ze nog veel meer efficiënte verwerking zijn en daarom minder invloed op de doorvoer hebben.

## <a name="actions"></a>Acties

Met de voorwaarden van SQL-filter, kunt u een actie die aantekeningen toevoegen aan het bericht door toe te voegen, te verwijderen of vervangen van eigenschappen en hun waarden definiëren. De actie [maakt gebruik van een expressie voor een SQL-achtige](service-bus-messaging-sql-filter.md) die los leans op de syntaxis van de UPDATE van de SQL-instructie. De actie is uitgevoerd op het bericht nadat deze zijn afgestemd en voordat het bericht is geselecteerd in het onderwerp. De wijzigingen in de berichteigenschappen zijn exclusief zijn voor het bericht gekopieerd naar het abonnement.

## <a name="usage-patterns"></a>Gebruikspatronen

Het eenvoudigste gebruiksscenario voor een onderwerp is dat voor elk abonnement ontvangt een kopie van elk bericht dat wordt verzonden naar een onderwerp, waarmee een broadcast-patroon.

Filters en acties kunnen twee meer groepen van patronen: partitionering en routering.

Partitioneren maakt gebruik van filters om berichten te verdelen tussen meerdere onderwerpabonnementen voor bestaande op een manier voorspelbare en sluiten elkaar wederzijds uit. Het partitioneren patroon wordt gebruikt wanneer een systeem voor het afhandelen van veel verschillende contexten in identiek afdelingen die elk een subset van de algemene gegevens; Houd is geschaald bijvoorbeeld: klant-profielgegevens. Met partities, verzendt een uitgever van het bericht in een onderwerp zonder enige kennis van het partitioneren model. Het bericht wordt vervolgens verplaatst naar het juiste abonnement waarin het kan vervolgens worden opgehaald door de berichtenhandler van de partitie.

Routering worden filters gebruikt om berichten te verdelen tussen onderwerpabonnementen op een voorspelbare wijze, maar niet per se exclusieve. In combinatie met de [automatisch doorsturen](service-bus-auto-forwarding.md) functie onderwerp filters kunnen worden gebruikt om te maken met complexe routering grafieken binnen een Service Bus-naamruimte voor bericht distributie binnen een Azure-regio. U kunt complexe globale topologieën met directe integratie in line-of-business-toepassingen maken met Azure Functions of Azure Logic Apps die fungeert als een brug tussen Azure Service Bus-naamruimten.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichten, de volgende onderwerpen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [SQLFilter-syntaxis](service-bus-messaging-sql-filter.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)