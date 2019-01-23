---
title: Azure Event Grid-concepten
description: Beschrijving van Azure Event Grid en de concepten ervan. Hiermee definieert u enkele belangrijke onderdelen van Event Grid.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: spelluru
ms.openlocfilehash: 40c749adee08e66319bed88f26c3c898428a41ad
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54474350"
---
# <a name="concepts-in-azure-event-grid"></a>Concepten in Azure Event Grid

In dit artikel beschrijft de belangrijkste concepten in Azure Event Grid.

## <a name="events"></a>Gebeurtenissen

Een gebeurtenis is de kleinste hoeveelheid informatie die wordt iets volledig beschreven die hebben plaatsgevonden in het systeem. Elke gebeurtenis bevat algemene informatie, zoals: bron van de gebeurtenis wanneer de gebeurtenis plaatsgevonden en de unieke id heeft. Elke gebeurtenis heeft ook specifieke informatie die is alleen relevant zijn voor het specifieke type gebeurtenis. Bijvoorbeeld, een gebeurtenis over een nieuw bestand wordt gemaakt in Azure Storage vindt u informatie over het bestand, zoals de `lastTimeModified` waarde. Of een gebeurtenis van Event Hubs is de URL van de Capture-bestand. 

Elke gebeurtenis is beperkt tot 64 KB aan gegevens.

Zie voor de eigenschappen die worden verzonden in een gebeurtenis, [Azure Event Grid-gebeurtenisschema](event-schema.md).

## <a name="publishers"></a>Uitgevers

Een uitgever is de gebruiker of organisatie die wil verzenden van gebeurtenissen naar Event Grid. Microsoft publiceert gebeurtenissen voor meerdere Azure-services. U kunt gebeurtenissen publiceren vanaf uw eigen toepassing. Organisaties die als host van services buiten Azure kan gebeurtenissen tot en met Event Grid kunnen publiceren.

## <a name="event-sources"></a>Gebeurtenisbronnen

Een gebeurtenisbron is waar de gebeurtenis plaatsvindt. De bron van elke gebeurtenis is gerelateerd aan een of meer gebeurtenistypen. Azure Storage is bijvoorbeeld de bron van de gebeurtenis voor de blob gemaakt van gebeurtenissen. IoT Hub is de bron van de gebeurtenis voor het apparaat is gemaakt van gebeurtenissen. Uw toepassing is de bron van de gebeurtenis voor aangepaste gebeurtenissen die u definieert. Bronnen van gebeurtenissen zijn verantwoordelijk voor het verzenden van gebeurtenissen naar Event Grid.

Zie voor meer informatie over het implementeren van een van de ondersteunde Event Grid-bronnen [bronnen van gebeurtenissen in Azure Event Grid](event-sources.md).

## <a name="topics"></a>Onderwerpen

De event grid-onderwerp biedt een eindpunt waar de gebeurtenissen voor het verzenden van de bron. De uitgever maakt van de event grid-onderwerp en bepaalt of een gebeurtenisbron een of meer dan één onderwerp moet. Een onderwerp wordt gebruikt voor een verzameling van gerelateerde gebeurtenissen. Abonnees besluiten om te reageren op bepaalde typen gebeurtenissen, welke onderwerpen om u te abonneren op.

Systeemonderwerpen zijn ingebouwde onderwerpen geleverd door Azure-services. Systeemonderwerpen niet wordt weergegeven in uw Azure-abonnement omdat de uitgever van de eigenaar is van de onderwerpen, maar u kunt zich hierop abonneert. Om u te abonneren, geeft u informatie over de resource die u wilt ontvangen van gebeurtenissen uit. Als u hebt toegang tot de bron, kunt u zich abonneert op gebeurtenissen.

Aangepaste onderwerpen zijn toepassing en van derden onderwerpen. Wanneer u maakt of toegang tot een aangepast onderwerp zijn toegewezen, ziet u dat aangepaste onderwerp in uw abonnement.

Bij het ontwerpen van uw toepassing, hebt u flexibiliteit wanneer u beslist hoeveel onderwerpen om u te maken. Maak een aangepast onderwerp voor elke categorie van gerelateerde gebeurtenissen voor grote oplossingen. Neem bijvoorbeeld een toepassing die gebeurtenissen met betrekking tot het wijzigen van gebruikersaccounts en verwerken van orders verzendt. Is het onwaarschijnlijk dat een gebeurtenis-handler wil beide soorten gebeurtenissen. Twee aangepaste onderwerpen maakt en kunt u zich abonneert op het account dat u interesseert gebeurtenis-handlers. Voor kleine oplossingen wilt u mogelijk alle gebeurtenissen verzenden naar een enkel onderwerp. Gebeurtenisabonnees kunnen filteren op de typen gebeurtenissen die ze willen.

## <a name="event-subscriptions"></a>Gebeurtenisabonnementen

Een abonnement wordt Event Grid waarmee de gebeurtenissen op een onderwerp u geïnteresseerd bent in ontvangst. Bij het maken van het abonnement, geeft u een eindpunt voor het verwerken van de gebeurtenis. U kunt de gebeurtenissen die worden verzonden naar het eindpunt filteren. U kunt filteren op gebeurtenistype of onderwerp patroon. Zie voor meer informatie, [Event Grid-abonnementsschema](subscription-creation-schema.md).

Zie voor voorbeelden van het maken van abonnementen:

* [Azure CLI-voorbeelden voor Event Grid](cli-samples.md)
* [Azure PowerShell-voorbeelden voor Event Grid](powershell-samples.md)
* [Azure Resource Manager-sjablonen voor Event Grid](template-samples.md)

Zie voor meer informatie over het ophalen van uw huidige event grid-abonnementen [Query Event Grid-abonnementen](query-event-subscriptions.md).

## <a name="event-subscription-expiration"></a>Het abonnement is verlopen gebeurtenis

De [Event Grid-extensie](/cli/azure/azure-cli-extensions-list) voor Azure CLI kunt u instellen dat een verlopen bij het maken van een gebeurtenisabonnement datum. Als u de REST-API gebruiken `api-version=2018-09-15-preview`

Het gebeurtenisabonnement is na die datum automatisch verlopen. Instellen dat een verlopen voor gebeurtenisabonnementen die alleen nodig zijn voor een beperkte periode en u niet wilt zorgen maken over het opruimen van deze abonnementen. Bij het maken van een gebeurtenisabonnement voor het testen van een scenario, wilt u mogelijk een verloopdatum instellen. 

Zie voor een voorbeeld van het instellen van een verlopen [abonneren met geavanceerde filters](how-to-filter-events.md#subscribe-with-advanced-filters).

## <a name="event-handlers"></a>Event Handlers

Een gebeurtenis-handler is vanuit het perspectief van een Event Grid, de plaats waar de gebeurtenis wordt verzonden. De handler duurt enkele verdere actie voor het verwerken van de gebeurtenis. Event Grid ondersteunt verschillende handler typen. U kunt een ondersteunde Azure-service of uw eigen webhook gebruiken als de handler. Afhankelijk van het type van de handler volgt Event Grid verschillende mechanismen voor het garanderen van de levering van de gebeurtenis. Voor HTTP-webhook gebeurtenis-handlers, de gebeurtenis is vaak herhaald totdat de handler statuscode retourneert `200 – OK`. Voor Azure Storage-wachtrij, worden de gebeurtenissen opnieuw uitgevoerd totdat de Queue-service is de bericht-push in de wachtrij verwerkt.

Zie voor meer informatie over het implementeren van een van de ondersteunde Event Grid-handlers [gebeurtenis-handlers in Azure Event Grid](event-handlers.md).

## <a name="security"></a>Beveiliging

Event Grid biedt beveiliging voor het abonneren op onderwerpen en onderwerpen publiceren. Wanneer u zich abonneert, moet u voldoende machtigingen hebben op de resource of event grid-onderwerp. Wanneer u publiceert, moet u een SAS-token of sleutel-verificatie voor het onderwerp hebben. Zie voor meer informatie, [Event Grid-beveiliging en verificatie](security-authentication.md).

## <a name="event-delivery"></a>Levering van gebeurtenissen

Als Event Grid niet kan bevestigen dat een gebeurtenis is ontvangen door de abonnee-eindpunt, redelivers wordt de gebeurtenis. Zie voor meer informatie, [bezorging van berichten van Event Grid en probeer het opnieuw](delivery-and-retry.md).

## <a name="batching"></a>Batchverwerking

Wanneer u een aangepast onderwerp gebruikt, moeten altijd gebeurtenissen worden gepubliceerd in een matrix. Dit kan een batch van een voor scenario's met lage doorvoer, maar voor groot zijn, het wordt aanbevolen dat u batch-aantal gebeurtenissen gegroepeerd per publiceren voor het bereiken van hogere efficiëntie. Batches kunnen maximaal 1 MB zijn. Elke gebeurtenis nog steeds moet niet groter zijn dan 64 KB.

## <a name="next-steps"></a>Volgende stappen

* Zie [Een inleiding tot Event Grid](overview.md) voor een inleiding tot Event Grid.
* Als u wilt snel aan de slag met Event Grid, Zie [aangepaste gebeurtenissen maken en routeren met Azure Event Grid](custom-event-quickstart.md).
