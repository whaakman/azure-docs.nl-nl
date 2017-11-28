---
title: Azure Event raster-concepten
description: Beschrijving van Azure Event raster en de concepten. Hiermee definieert u enkele belangrijke onderdelen van gebeurtenis raster.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 09/18/2017
ms.author: babanisa
ms.openlocfilehash: ccbd861c985e54a3808c0d4e8ea6169b6a61f134
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="concepts-in-azure-event-grid"></a>Concepten in Azure Event raster

De belangrijkste concepten in Azure gebeurtenis raster zijn:

## <a name="events"></a>Gebeurtenissen

Een gebeurtenis is de kleinste hoeveelheid informatie op die volledig beschrijft iets die hebben plaatsgevonden in het systeem.  Elke gebeurtenis heeft een algemene informatie, zoals: bron van de gebeurtenis wanneer de gebeurtenis plaatsgevonden en de unieke id heeft.  Elke gebeurtenis heeft ook specifieke informatie die is alleen relevant zijn voor het specifieke type van de gebeurtenis. Bijvoorbeeld een gebeurtenis over een nieuw bestand wordt gemaakt in Azure Storage bevat details over het bestand, zoals de `lastTimeModified` waarde. Of een gebeurtenis over een virtuele machine opnieuw opstarten bevat de naam van de virtuele machine en de reden voor het opnieuw opstarten. Elke gebeurtenis is beperkt tot 64 KB aan gegevens.

## <a name="event-sourcespublishers"></a>Gebeurtenisuitgevers bronnen

Een gebeurtenisbron is waar de gebeurtenis plaatsvindt. Azure Storage is bijvoorbeeld de gebeurtenisbron voor blob gemaakt van gebeurtenissen. Azure VM-infrastructuur is de gegevensbron voor gebeurtenissen van de virtuele machine. Bronnen van gebeurtenissen zijn verantwoordelijk voor het publiceren van gebeurtenissen naar de gebeurtenis raster.

## <a name="topics"></a>Onderwerpen

Uitgevers categoriseren gebeurtenissen in onderwerpen. Het onderwerp bevat een eindpunt waar de uitgever gebeurtenissen verzendt. Om te reageren op bepaalde typen gebeurtenissen, besluit abonnees welke onderwerpen om u te abonneren op. Onderwerpen bevatten ook een gebeurtenis schema zodat abonnees gebruiken de gebeurtenissen op de juiste wijze kunnen detecteren.

Systeemonderwerpen zijn ingebouwde onderwerpen die worden geleverd door de Azure-services. Aangepaste onderwerpen zijn toepassing en onderwerpen van derden.

## <a name="event-subscriptions"></a>Gebeurtenisabonnementen

Een abonnement geïnstrueerd gebeurtenis raster op welke gebeurtenissen op een onderwerp een abonnee geïnteresseerd in ontvangst is.  Een abonnement bevat ook informatie over hoe gebeurtenissen moeten worden bezorgd bij de abonnee.

## <a name="event-handlers"></a>Gebeurtenis-handlers

Een gebeurtenis-handler is vanuit het perspectief van een raster gebeurtenis, de plaats waar de gebeurtenis wordt verzonden. De handler duurt enkele verdere actie voor het verwerken van de gebeurtenis.  Raster gebeurtenis biedt ondersteuning voor meerdere Abonneetypen. Afhankelijk van het type van abonnee volgt gebeurtenis raster verschillende mechanismen voor het garanderen van de levering van de gebeurtenis.  Voor HTTP-webhook gebeurtenis-handlers de gebeurtenis is vaak herhaald totdat de handler statuscode retourneert `200 – OK`. Azure Storage-wachtrij, worden de gebeurtenissen vaak herhaald totdat de Queue-service kan worden verwerkt op de push-bewerking van het bericht in de wachtrij.

## <a name="filters"></a>Filters

Wanneer u zich abonneert op een onderwerp, kunt u de gebeurtenissen die worden verzonden naar het eindpunt filteren. U kunt filteren op gebeurtenistype of onderwerp patroon. Zie voor meer informatie [gebeurtenis raster abonnement schema](subscription-creation-schema.md).

## <a name="security"></a>Beveiliging

Gebeurtenis raster biedt beveiliging voor uw abonnement op onderwerpen en publiceren van onderwerpen. Als u zich abonneert, moet u voldoende machtigingen op het bron- of onderwerp hebt. Wanneer u publiceert, moet u een SAS-token of verificatie van de sleutel voor het onderwerp hebben. Zie voor meer informatie [gebeurtenis raster beveiligings- en verificatie](security-authentication.md).

## <a name="failed-delivery"></a>Mislukte bezorging

Wanneer gebeurtenis raster kan niet bevestigen dat een gebeurtenis is ontvangen door de abonnee eindpunt, redelivers wordt de gebeurtenis. Zie voor meer informatie [gebeurtenis raster berichtbezorging en probeer het opnieuw](delivery-and-retry.md).

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot gebeurtenis raster, [over gebeurtenis raster](overview.md).
* Zie om snel aan de slag met Event raster [maken en route aangepaste gebeurtenissen met Azure Event raster](custom-event-quickstart.md).
