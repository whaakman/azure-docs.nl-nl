---
title: Azure Event raster-concepten
description: Beschrijving van Azure Event Grid en de concepten ervan. Hiermee definieert u enkele belangrijke onderdelen van gebeurtenis raster.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/23/2018
ms.author: tomfitz
ms.openlocfilehash: abc1302f0317c8d5ecdc7ddaf8ca6d3a9e82b582
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626032"
---
# <a name="concepts-in-azure-event-grid"></a>Concepten in Azure Event raster

In dit artikel beschrijft de belangrijkste concepten in Azure gebeurtenis raster.

## <a name="events"></a>Gebeurtenissen

Een gebeurtenis is de kleinste hoeveelheid informatie op die volledig beschrijft iets die hebben plaatsgevonden in het systeem. Elke gebeurtenis heeft een algemene informatie, zoals: bron van de gebeurtenis wanneer de gebeurtenis plaatsgevonden en de unieke id heeft. Elke gebeurtenis heeft ook specifieke informatie die is alleen relevant zijn voor het specifieke type van de gebeurtenis. Een gebeurtenis over een nieuw bestand wordt gemaakt in Azure Storage heeft bijvoorbeeld meer informatie over het bestand, zoals de `lastTimeModified` waarde. Of een Event Hubs-gebeurtenis heeft de URL van de Capture-bestand. 

Elke gebeurtenis is beperkt tot 64 KB aan gegevens.

Zie voor de eigenschappen die worden verzonden in een gebeurtenis, [Azure gebeurtenis raster gebeurtenis schema](event-schema.md).

## <a name="publishers"></a>Uitgevers

Een uitgever is de gebruiker of organisatie die wil gebeurtenissen verzenden naar Event raster. Microsoft publiceert gebeurtenissen voor verschillende Azure-services. U kunt gebeurtenissen publiceren van uw eigen toepassing. Organisaties die als host van services buiten Azure kan gebeurtenissen tot en met gebeurtenis raster kunnen publiceren.

## <a name="event-sources"></a>Gebeurtenisbronnen

Een gebeurtenisbron is waar de gebeurtenis plaatsvindt. Elke gebeurtenisbron betrekking heeft op een of meer gebeurtenistypen. Azure Storage is bijvoorbeeld de gebeurtenisbron voor blob gemaakt van gebeurtenissen. IoT-Hub is de gebeurtenisbron voor apparaat gebeurtenissen gemaakt. De toepassing is de gegevensbron voor aangepaste gebeurtenissen die u definieert. Bronnen van gebeurtenissen zijn verantwoordelijk voor het verzenden van gebeurtenissen naar de gebeurtenis raster.

Zie voor meer informatie over het implementeren van een van de ondersteunde gebeurtenis raster bronnen [bronnen van gebeurtenissen in Azure gebeurtenis raster](event-sources.md).

## <a name="topics"></a>Onderwerpen

De gebeurtenis raster-onderwerp vindt u een eindpunt waar de bron gebeurtenissen verzendt. De uitgever van het raster onderwerp maakt en bepaalt of de bron van een gebeurtenis een of meer dan één onderwerp moet. Een onderwerp wordt gebruikt voor een verzameling van gerelateerde gebeurtenissen. Om te reageren op bepaalde typen gebeurtenissen, besluit abonnees welke onderwerpen om u te abonneren op.

Systeemonderwerpen zijn ingebouwde onderwerpen die worden geleverd door de Azure-services. Systeemonderwerpen niet wordt weergegeven in uw Azure-abonnement omdat de uitgever van de eigenaar is van de onderwerpen, maar u zich op deze abonneren kunt. Abonneren, geeft u informatie over de resource die u wilt ontvangen van gebeurtenissen uit. Als u hebt toegang tot de bron, kunt u zich abonneert op de gebeurtenissen.

Aangepaste onderwerpen zijn toepassing en onderwerpen van derden. Als u maakt of toegang zijn toegewezen aan een aangepaste onderwerp, ziet u dat aangepaste onderwerp in uw abonnement.

Bij het ontwerpen van uw toepassing, hebt u flexibiliteit wanneer u beslist hoeveel onderwerpen maken. Maak een eigen onderwerp voor elke categorie van gerelateerde gebeurtenissen voor grote oplossingen. Neem bijvoorbeeld een toepassing die gebeurtenissen met betrekking tot het wijzigen van gebruikersaccounts en verwerken van bestellingen verzendt. Het lijkt onwaarschijnlijk dat een gebeurtenishandler wil beide soorten gebeurtenissen. Maak twee aangepaste onderwerpen en gebeurtenis-handlers abonneren op die het interesseert laten. Voor kleine oplossingen, moet u liever alle gebeurtenissen verzenden naar een enkel onderwerp. Gebeurtenisabonnees kunnen filteren op de typen gebeurtenissen die ze willen.

## <a name="event-subscriptions"></a>Gebeurtenisabonnementen

Een abonnement vertelt gebeurtenis raster welke gebeurtenissen op een onderwerp u geïnteresseerd bent in ontvangst. Wanneer u het abonnement, kunt u een eindpunt opgeven voor het verwerken van de gebeurtenis. U kunt de gebeurtenissen die worden verzonden naar het eindpunt filteren. U kunt filteren op gebeurtenistype of onderwerp patroon. Zie voor meer informatie [gebeurtenis raster abonnement schema](subscription-creation-schema.md).

Zie voor voorbeelden van het maken van abonnementen:

* [Azure CLI-voorbeelden voor gebeurtenis raster](cli-samples.md)
* [Azure PowerShell-voorbeelden voor gebeurtenis raster](powershell-samples.md)
* [Azure Resource Manager-sjablonen voor gebeurtenis raster](template-samples.md)

Zie voor meer informatie over het ophalen van de huidige gebeurtenis raster abonnementen [Query gebeurtenis raster abonnementen](query-event-subscriptions.md).

## <a name="event-handlers"></a>Event Handlers

Een gebeurtenis-handler is vanuit het perspectief van een raster gebeurtenis, de plaats waar de gebeurtenis wordt verzonden. De handler duurt enkele verdere actie voor het verwerken van de gebeurtenis. Gebeurtenis raster ondersteunt meerdere handler typen. U kunt een ondersteunde Azure-service of uw eigen webhook gebruiken als de handler. Afhankelijk van het type van de handler volgt gebeurtenis raster verschillende mechanismen voor het garanderen van de levering van de gebeurtenis. Voor HTTP-webhook gebeurtenis-handlers de gebeurtenis is vaak herhaald totdat de handler statuscode retourneert `200 – OK`. Azure Storage-wachtrij, worden de gebeurtenissen vaak herhaald totdat de Queue-service kan worden verwerkt op de push-bewerking van het bericht in de wachtrij.

Zie voor meer informatie over het implementeren van een van de ondersteunde raster gebeurtenis-handlers [gebeurtenis-handlers in Azure gebeurtenis raster](event-handlers.md).

## <a name="security"></a>Beveiliging

Gebeurtenis raster biedt beveiliging voor uw abonnement op onderwerpen en publiceren van onderwerpen. Als u zich abonneert, moet u voldoende machtigingen hebben op het raster resource of gebeurtenis onderwerp. Wanneer u publiceert, moet u een SAS-token of verificatie van de sleutel voor het onderwerp hebben. Zie voor meer informatie [gebeurtenis raster beveiligings- en verificatie](security-authentication.md).

## <a name="event-delivery"></a>Levering van gebeurtenis

Als gebeurtenis raster kan niet bevestigen dat een gebeurtenis is ontvangen door de abonnee eindpunt, redelivers wordt de gebeurtenis. Zie voor meer informatie [gebeurtenis raster berichtbezorging en probeer het opnieuw](delivery-and-retry.md).

## <a name="next-steps"></a>Volgende stappen

* Zie [Een inleiding tot Event Grid](overview.md) voor een inleiding tot Event Grid.
* Zie om snel aan de slag met Event raster [maken en route aangepaste gebeurtenissen met Azure Event raster](custom-event-quickstart.md).
