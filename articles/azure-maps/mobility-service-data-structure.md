---
title: Mobility Service-gegevensstructuren in Azure Maps | Microsoft Docs
description: Gegevensstructuren in Azure Maps Mobility-Service
author: walsehgal
ms.author: v-musehg
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 29e8a9d7555ca836b6266879f3b3c1e32ffd3980
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735554"
---
# <a name="data-structures-in-azure-maps-mobility-service"></a>Gegevensstructuren in Azure Maps Mobility-Service

In dit artikel introduceert het concept van Metro gebied in [Mobility-Service van Azure Maps](https://aka.ms/AzureMapsMobilityService) en enkele van de algemene velden geretourneerd via de services, wanneer deze wordt opgevraagd voor de openbare doorvoer stopt en regels. Het is raadzaam om in dit artikel voordat u begint met de Mobility Service-API's te doorlopen. Deze algemene velden hieronder besproken.

## <a name="metro-area"></a>Metro gebied

Mobility Service-gegevens gesplitst in ondersteunde metro gebieden. Metro gebieden Volg niet de grenzen van de plaats, een metro gebied kan bevatten meerdere steden, bijvoorbeeld dichtbevolkte plaats en de omringende steden; en een land/regio kan één metro gebied. 

De `metroID` is een gebied in metro-ID die kan worden gebruikt om aan te roepen de [ophalen in Metro gebied Info API](https://aka.ms/AzureMapsMobilityMetroAreaInfo) ondersteund doorvoer aanvraagtypen en aanvullende informatie voor de metro gebieden zoals doorvoer in de VS en actieve waarschuwingen. U kunt de Azure kaarten ophalen in Metro-API gebruiken om aan te vragen van de ondersteunde metro gebieden en metroIDs. Metro gebied-id's kunnen worden gewijzigd.

**metroID:** 522 **naam:** Seattle-Tacoma-Bellevue

![Seattle-metro-area](./media/mobility-service-data-structure/seattle-metro.png)

## <a name="stop-ids"></a>Id's stoppen

Doorvoer niet meer kunnen worden verwezen door twee typen-id's, de [algemene doorvoer Feed specificatie (GFTS)](https://gtfs.org/) -ID (aangeduid als stopKey) en de Azure-kaarten stoppen ID (aangeduid als stopId). Met betrekking tot stopt na verloop van tijd, is het nodig om te gebruiken de Azure-kaarten stop-ID, zoals deze ID meer stabiel is en als de fysieke stop bestaat waarschijnlijk niet zullen veranderen. De GTFS stop-ID is gewijzigd vaker worden gebruikt, bijvoorbeeld in het geval de GTFS-provider nodig heeft om deze te wijzigen of nieuwe GTFS versie is uitgebracht, hoewel de fysieke stoppen niet gewijzigd is.

Als u wilt starten, kunt u aanvragen in de buurt doorvoer stopt met behulp van [ophalen in de buurt doorvoer API](https://aka.ms/AzureMapsMobilityNearbyTransit).

## <a name="line-groups-and-lines"></a>Regelgroepen en regels

Mobility-Service maakt gebruik van een parallelle gegevensmodel voor regels en regelgroepen beter omgaan met wijzigingen die zijn overgenomen van [GTFS](https://gtfs.org/) routes en trips gegevensmodel.


### <a name="line-groups"></a>Regelgroepen

Een groep is een entiteit, die alle regels die logisch deel uitmaken van dezelfde groep worden gegroepeerd. Een groep bevatten gewoonlijk twee regels, één gaan van punt A naar B, en de andere retourneren vanaf punt B naar A, beide behoren naar de dezelfde openbaar vervoer-instantie en met hetzelfde nummer. Echter, kunnen er gevallen waarin een groep meer dan twee regels of slechts één regel in het heeft.


### <a name="lines"></a>regels

Zoals hierboven wordt beschreven, wordt de groep van elke regel bestaat uit een verzameling van regels. Elke regel worden vaak beschreven een richting en elke regel groep bestaat uit twee regels. Maar er zijn gevallen in welke regels bestaan uit een groep, bijvoorbeeld er is een regel die soms omzeilingen via een bepaalde groep en soms niet bestaat en wordt beheerd in beide gevallen onder de dezelfde regelnummer en er zijn andere gevallen waarin een regel g groeperen bestaat uit één regel, bijvoorbeeld een circulaire lijn met één richting.

Als u wilt beginnen, kunt u regelgroepen aanvragen met behulp van de [ophalen doorvoer regel API](https://aka.ms/AzureMapsMobilityTransitLine) en hoger Zoom in op regels.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over om aan te vragen van doorvoer gegevens met behulp van de Mobility-Service:

> [!div class="nextstepaction"]
> [Hoe u gegevens van aanvragen van doorvoer](how-to-request-transit-data.md)

Meer informatie over om aan te vragen van realtime gegevens met behulp van de Mobility-Service:

> [!div class="nextstepaction"]
> [Hoe u realtime gegevens van aanvragen](how-to-request-real-time-data.md)

De documentatie van Azure Maps Mobility Service-API verkennen

> [!div class="nextstepaction"]
> [Mobility Service-API-documentatie](https://aka.ms/AzureMapsMobilityService)
