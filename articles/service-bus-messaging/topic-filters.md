---
title: Azure Service Bus-onderwerp filters | Microsoft Docs
description: Filteren van Azure Service Bus-onderwerpen
services: service-bus-messaging
documentationcenter: 
author: clemensv
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2017
ms.author: sethm
ms.openlocfilehash: b3fe467b7d6ae9b207956ece4980bf558a69761f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="topic-filters-and-actions"></a>Onderwerp-filters en acties

Abonnees kunnen definiëren welke berichten die ze willen ontvangen van een onderwerp. Deze berichten worden opgegeven in de vorm van een of meerdere benoemde abonnementsregels. Elke regel bestaat uit een voorwaarde waarmee wordt bepaald berichten en een actie die het geselecteerde bericht annotates geselecteerd. Voor elke overeenkomende regelvoorwaarde produceert het abonnement een kopie van het bericht dat anders voor elke overeenkomende beleidsregel kan worden voorzien.

Elk onderwerpabonnement gemaakte heeft een initiële standaardregel abonnement. Als u niet expliciet een filtervoorwaarde voor de regel opgeeft, wordt het filter is toegepast is de **true** filter waarmee alle berichten in het abonnement worden geselecteerd. De standaardregel heeft geen bijbehorende aantekening-actie.

Service Bus ondersteunt drie filtervoorwaarden:

-   *Booleaanse filters* : de **TrueFilter** en **FalseFilter** ofwel ertoe leiden dat alle inkomende berichten (**true**) of geen van de inkomende berichten (**false**) voor het abonnement worden geselecteerd.

-   *SQL-Filters* - A **SqlFilter** bevat een SQL-achtige voorwaardelijke expressie die wordt geëvalueerd in de broker op basis van de gebruiker gedefinieerde eigenschappen en de eigenschappen van de binnenkomende berichten. Alle Systeemeigenschappen moeten worden voorafgegaan door `sys.` in de conditionele expressie. De [subset van de SQL-taal voor de filtervoorwaarden](service-bus-messaging-sql-filter.md) tests op de aanwezigheid van de eigenschappen (EXISTS), ook als voor null-waarden (IS NULL), logische AND-niet/OR, relationele operators, eenvoudige numerieke rekenkundige bewerking en eenvoudige tekstpatroon die overeenkomt met dergelijke.

-   *Correlatie Filters* - A **CorrelationFilter** bevat een set voorwaarden die worden vergeleken met een of meer van de gebruikers- en eigenschappen van een binnenkomende bericht. Er wordt vaak gebruikt voor het vergelijken van de **CorrelationId** eigenschap, maar de toepassing kunt ook kiezen voor het vergelijken van **ContentType**, **Label**,  **MessageId**, **ReplyTo**, **ReplyToSessionId**, **SessionId**, **naar**, en alle door de gebruiker gedefinieerd Eigenschappen. Een overeenkomst wordt gevonden wanneer de waarde voor een eigenschap van een binnenkomende bericht is gelijk aan de opgegeven waarde in het filter correlatie. Voor expressies voor verbindingsreeksen is de vergelijking hoofdlettergevoelig. Wanneer u meerdere eigenschappen van de overeenkomst opgeeft, wordt het filter combineert ze als een logische en voorwaarde, wat betekent dat voor het filter overeen met, moeten overeenkomen met alle voorwaarden.

Alle filters evalueren berichteigenschappen. Filters kunnen de berichttekst niet evalueren.

Filterregels voor complexe vereisen verwerkingscapaciteit. Het gebruik van SQL-filterregels resulteert in het bijzonder in lagere totale bericht doorvoer op het niveau van het abonnement, het onderwerp en naamruimte. Kies indien mogelijk toepassingen moeten correlatie filters via SQL-achtige filters, omdat ze veel efficiënter bij de verwerking zijn en daarom minder invloed op de doorvoer hebben.

## <a name="actions"></a>Acties

Met de SQL-filtervoorwaarden en met die, kunt u een actie die aantekeningen toevoegen aan het bericht toe te voegen, verwijderen of vervangen van eigenschappen en hun waarden te definiëren. De actie [maakt gebruik van een SQL-achtige expressie](service-bus-messaging-sql-filter.md) die leans los van de syntaxis van de UPDATE van SQL-instructie. De actie wordt uitgevoerd op het bericht nadat is gematcht en voordat het bericht is geselecteerd in het onderwerp. De wijzigingen in de berichteigenschappen zijn persoonlijke aan het bericht gekopieerd naar het abonnement.

## <a name="usage-patterns"></a>Gebruikspatronen

Het eenvoudigste gebruiksscenario voor een onderwerp is dat elk abonnement ontvangt een kopie van elk bericht verzonden naar een onderwerp, waardoor een broadcast-patroon.

Filters en acties kunnen twee verdere groepen patronen: partitionerings- en routering.

Partitioneren maakt gebruik van filters voor de distributie van berichten over enkele bestaande onderwerpabonnementen op een manier voorspelbare en sluiten elkaar wederzijds uit. De partitionering patroon wordt gebruikt wanneer een systeem wordt uitgebreid voor het afhandelen van veel verschillende contexten in identiek compartimenten dat elk een subset van de algemene gegevens bevatten; bijvoorbeeld: klant-profielgegevens. Met partitioneren, verzendt een uitgever van het bericht in een onderwerp zonder dat daarvoor kennis van de partitionering model. Het bericht wordt vervolgens verplaatst naar het juiste abonnement waarvan het kan vervolgens worden opgehaald door de handler voor de partitie.

Routering maakt gebruik van filters voor de distributie van berichten over onderwerpabonnementen op voorspelbare manier,, maar niet noodzakelijkerwijs exclusieve. In combinatie met de [automatisch doorsturen](service-bus-auto-forwarding.md) functie, onderwerp filters kunnen worden gebruikt voor het maken van complexe routering grafieken in een Service Bus-naamruimte voor bericht-distributie binnen een Azure-regio. U kunt complexe topologieën door globale directe integratie met line-of-business-toepassingen maken met Azure Functions of Azure Logic Apps die fungeert als een brug tussen Azure Service Bus-naamruimten.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Service Bus-berichtenservice, de volgende onderwerpen:

* [Grondbeginselen van Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-wachtrijen, -onderwerpen en -abonnementen](service-bus-queues-topics-subscriptions.md)
* [SQLFilter-syntaxis](service-bus-messaging-sql-filter.md)
* [Service Bus-onderwerpen en -abonnementen gebruiken](service-bus-dotnet-how-to-use-topics-subscriptions.md)