---
title: Azure Event raster-concepten
description: Beschrijving van Azure Event raster en de concepten. Hiermee definieert u enkele belangrijke onderdelen van gebeurtenis raster.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 04/24/2018
ms.author: babanisa
ms.openlocfilehash: 8ddde98b448f4d6d6f24a2ee47acf9240593622c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="concepts-in-azure-event-grid"></a>Concepten in Azure Event raster

In dit artikel beschrijft de belangrijkste concepten in Azure gebeurtenis raster.

## <a name="events"></a>Gebeurtenissen

Een gebeurtenis is de kleinste hoeveelheid informatie op die volledig beschrijft iets die hebben plaatsgevonden in het systeem. Elke gebeurtenis heeft een algemene informatie, zoals: bron van de gebeurtenis wanneer de gebeurtenis plaatsgevonden en de unieke id heeft. Elke gebeurtenis heeft ook specifieke informatie die is alleen relevant zijn voor het specifieke type van de gebeurtenis. Een gebeurtenis over een nieuw bestand wordt gemaakt in Azure Storage heeft bijvoorbeeld meer informatie over het bestand, zoals de `lastTimeModified` waarde. Of een Event Hubs-gebeurtenis heeft de URL van de Capture-bestand. Elke gebeurtenis is beperkt tot 64 KB aan gegevens.

## <a name="event-sourcespublishers"></a>Gebeurtenisuitgevers bronnen

Een gebeurtenisbron is waar de gebeurtenis plaatsvindt. Azure Storage is bijvoorbeeld de gebeurtenisbron voor blob gemaakt van gebeurtenissen. Azure VM-infrastructuur is de gegevensbron voor gebeurtenissen van de virtuele machine. Bronnen van gebeurtenissen zijn verantwoordelijk voor het publiceren van gebeurtenissen naar de gebeurtenis raster.

Zie voor meer informatie over het implementeren van een van de ondersteunde gebeurtenis raster bronnen [bronnen van gebeurtenissen in Azure gebeurtenis raster](event-sources.md).

## <a name="topics"></a>Onderwerpen

Uitgevers categoriseren gebeurtenissen in onderwerpen. De gebeurtenis raster-onderwerp bevat een eindpunt waar de uitgever gebeurtenissen verzendt. Om te reageren op bepaalde typen gebeurtenissen, besluit abonnees welke onderwerpen om u te abonneren op. Onderwerpen bevatten ook een gebeurtenis schema zodat abonnees leren kunnen gebruiken voor de gebeurtenissen op de juiste wijze.

Systeemonderwerpen zijn ingebouwde onderwerpen die worden geleverd door de Azure-services. Aangepaste onderwerpen zijn toepassing en onderwerpen van derden.

Bij het ontwerpen van uw toepassing, hebt u flexibiliteit wanneer u beslist hoeveel onderwerpen maken. Maak een eigen onderwerp voor elke categorie van gerelateerde gebeurtenissen voor grote oplossingen. Neem bijvoorbeeld een toepassing die gebeurtenissen met betrekking tot het wijzigen van gebruikersaccounts en verwerken van bestellingen verzendt. Het lijkt onwaarschijnlijk dat een gebeurtenishandler wil beide soorten gebeurtenissen. Maak twee aangepaste onderwerpen en gebeurtenis-handlers abonneren op die het interesseert laten. Voor kleine oplossingen, moet u liever alle gebeurtenissen verzenden naar een enkel onderwerp. Gebeurtenisabonnees kunnen filteren op de typen gebeurtenissen die ze willen.

## <a name="event-subscriptions"></a>Gebeurtenisabonnementen

Een abonnement geïnstrueerd gebeurtenis raster op welke gebeurtenissen op een onderwerp een abonnee geïnteresseerd in ontvangst is. Een abonnement bevat ook informatie over hoe gebeurtenissen moeten worden bezorgd bij de abonnee.

## <a name="event-handlers"></a>Gebeurtenis-handlers

Een gebeurtenis-handler is vanuit het perspectief van een raster gebeurtenis, de plaats waar de gebeurtenis wordt verzonden. De handler duurt enkele verdere actie voor het verwerken van de gebeurtenis. Raster gebeurtenis biedt ondersteuning voor meerdere Abonneetypen. Afhankelijk van het type van abonnee volgt gebeurtenis raster verschillende mechanismen voor het garanderen van de levering van de gebeurtenis. Voor HTTP-webhook gebeurtenis-handlers de gebeurtenis is vaak herhaald totdat de handler statuscode retourneert `200 – OK`. Azure Storage-wachtrij, worden de gebeurtenissen vaak herhaald totdat de Queue-service kan worden verwerkt op de push-bewerking van het bericht in de wachtrij.

Zie voor meer informatie over het implementeren van een van de ondersteunde raster gebeurtenis-handlers [gebeurtenis-handlers in Azure gebeurtenis raster](event-handlers.md).

## <a name="filters"></a>Filters

Wanneer u zich abonneert op een gebeurtenis raster onderwerp, kunt u de gebeurtenissen die worden verzonden naar het eindpunt filteren. U kunt filteren op gebeurtenistype of onderwerp patroon. Zie voor meer informatie [gebeurtenis raster abonnement schema](subscription-creation-schema.md).

## <a name="security"></a>Beveiliging

Gebeurtenis raster biedt beveiliging voor uw abonnement op onderwerpen en publiceren van onderwerpen. Als u zich abonneert, moet u voldoende machtigingen hebben op het raster resource of gebeurtenis onderwerp. Wanneer u publiceert, moet u een SAS-token of verificatie van de sleutel voor het onderwerp hebben. Zie voor meer informatie [gebeurtenis raster beveiligings- en verificatie](security-authentication.md).

## <a name="failed-delivery"></a>Mislukte bezorging

Als gebeurtenis raster kan niet bevestigen dat een gebeurtenis is ontvangen door de abonnee eindpunt, redelivers wordt de gebeurtenis. Zie voor meer informatie [gebeurtenis raster berichtbezorging en probeer het opnieuw](delivery-and-retry.md).

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot gebeurtenis raster, [over gebeurtenis raster](overview.md).
* Zie om snel aan de slag met Event raster [maken en route aangepaste gebeurtenissen met Azure Event raster](custom-event-quickstart.md).
