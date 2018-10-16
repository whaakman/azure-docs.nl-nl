---
title: Inzicht in digitale dubbels-modellen en ruimtelijke intelligence graph | Microsoft Docs
description: Met behulp van Azure digitale dubbels grafiekverwerking om relaties tussen personen, locaties en apparaten
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: 1c2068af510cb3733ce99a6ae7b40487a8c1a015
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324085"
---
# <a name="understanding-digital-twins-object-models-and-spatial-intelligence-graph"></a>Inzicht krijgen in digitale dubbels objectmodellen en ruimtelijke intelligence graph

Azure van digitale dubbels is een Azure IoT-service die wordt gebruikt door de uitgebreide virtuele representaties van fysieke omgevingen en bijbehorende apparaten, sensoren en personen. Het verbetert ontwikkeling door organiseren domeinspecifieke concepten in nuttige modellen die zich bevindt op een grafiek ruimtelijke intelligence. Deze begrippen model zonder de relaties en interacties tussen mensen, spaties en apparaten.

_Digitale dubbels objectmodellen_ beschrijven domeinspecifieke concepten, categorieën en eigenschappen. Modellen zijn vooraf gedefinieerd door gebruikers die behoefte hebben aan om aan te passen van de oplossing voor hun specifieke behoeften. Samen vormen deze vooraf gedefinieerde modellen voor digitale dubbels Object gezamenlijk een _ontologie_. Een slimme gebouwen ontologie beschrijft regio's, evenementen, verdiepingen, kantoren, zones, conferentieruimten en focus ruimten. Een raster energie ontologie beschrijft verschillende power stations, substations, energiebronnen en klanten. Deze digitale dubbels objectmodellen en anthologieën kunnen Azure digitale dubbels op diverse scenario's en behoeften aanpassen.

Met _digitale dubbels objectmodellen_ en _ontologie_ aanwezig is, een kunt vullen een _ruimtelijke Graph_. Ruimtelijke grafieken zijn virtuele representaties van de vele relaties tussen spaties, apparaten en mensen die relevant zijn voor een IoT-oplossing. Het volgende diagram toont een voorbeeld van een ruimtelijke graaf met behulp van een slimme gebouwen ontologie.

![Digitale dubbels ruimtelijke grafiek samenstellen][1]

<a id="model" />

De ruimtelijke grafiek combineert spaties, apparaten, sensoren en gebruikers. Elk is gekoppeld aan een manier die de echte wereld modellen: 43 venue heeft vier verdiepingen, elk met meerdere verschillende gebieden. Gebruikers zijn gekoppeld aan hun werkstation en toegang krijgen tot gedeelten van de grafiek.  Bijvoorbeeld, hoeft een beheerder rechten wijzigingen aanbrengen aan de ruimtelijke grafiek terwijl de bezoeker mogelijk alleen tot bepaalde gegevens maken.

## <a name="digital-twins-object-models"></a>Digitale dubbels objectmodellen

Digitale dubbels objectmodellen ondersteunen deze hoofdcategorieën van objecten:

- **Opslagruimten** zijn virtuele of fysieke locaties, bijvoorbeeld `Tenant`, `Customer`, `Region`, `Venue`.
- **Apparaten** zijn virtuele of fysieke onderdelen van apparatuur, bijvoorbeeld `AwesomeCompany Device`, `Raspberry Pi 3`.
- **Sensoren** zijn objecten die detecteren van gebeurtenissen, bijvoorbeeld `AwesomeCompany Temperature Sensor`, `AwesomeCompany Presence Sensor`.
- **Gebruikers** personen en hun kenmerken te identificeren.

Er zijn andere categorieën van objecten:

- **Resources** zijn gekoppeld aan een spatie en vertegenwoordigen meestal Azure-resources moet worden gebruikt door objecten in de ruimtelijke grafiek, bijvoorbeeld `IoTHub`.
- **BLOBs** zijn gekoppeld aan objecten (zoals spaties, apparaten, sensoren en gebruikers) en gebruikt als bestanden met mime-type en metagegevens, bijvoorbeeld `maps`, `pictures`, `manuals`.
- **Uitgebreide typen** zijn extensible inventarisaties die de ervaring van entiteiten met specifieke kenmerken, bijvoorbeeld `SpaceType`, `SpaceSubtype`.
- **Anthologieën** vertegenwoordigt een set uitgebreide typen, bijvoorbeeld `Default`, `Building`, `BACnet`, `EnergyGrid`.
- **Sleutels en waarden** zijn aangepaste kenmerken van opslagruimten, apparaten, sensoren en gebruikers. Ze kunnen worden gebruikt naast de ingebouwde kenmerken, bijvoorbeeld `DeltaProcessingRefreshTime` als sleutel en `10` als waarde.
- **Rollen** sets met machtigingen worden toegewezen aan gebruikers en apparaten in de ruimtelijke grafiek, bijvoorbeeld `Space Administrator`, `User Administrator`, `Device Administrator`.
- **Roltoewijzingen** zijn de koppeling tussen een rol en een object in de ruimtelijke grafiek, bijvoorbeeld verlenen van een gebruiker of een service-principal-machtiging voor het beheren van een spatie in de ruimtelijke grafiek.
- **Beveiliging sleutel winkels** bieden de sleutels voor alle apparaten in de hiërarchie onder een bepaalde ruimte-object waarmee het apparaat veilig communiceren met de digitale Twins-service.
- **Gebruiker-gedefinieerde functies** of **UDF's** aanpasbare sensor telemetrie verwerken binnen de ruimtelijke grafiek toestaan. Bijvoorbeeld, kunt een UDF stelt u de waarde van een sensor, aangepaste logica op basis van sensorwaarden, serverlogs, uitvoeren en stel de uitvoer van een spatie metagegevens koppelen aan een spatie en meldingen te verzenden wanneer vooraf gedefinieerde voorwaarden wordt voldaan. Op dit moment kunnen UDF's worden geschreven in JavaScript.
- **Vergelijkingsprogramma voor** objecten om te welke UDF's bepalen wordt uitgevoerd voor een bepaalde telemetrieberichten zijn.
- **Eindpunten** zijn de locaties waar berichten over Telemetrie en gebeurtenissen van digitale dubbels kunnen worden gerouteerd, bijvoorbeeld `Event Hub`, `Service Bus`, `Event Grid`.

<a id="graph" />

## <a name="spatial-intelligence-graph"></a>Ruimtelijke intelligence graph

**Ruimtelijke graph** is de hiërarchische grafiek van opslagruimten, apparaten en mensen die zijn gedefinieerd in de **digitale dubbels objectmodel**. Biedt ondersteuning voor de ruimtelijke grafiek _overname_, _filteren_, _doorlopen_, _schaalbaarheid_, en _uitbreidbaarheid_ . Gebruikers kunnen beheren en communiceren met hun ruimtelijke graph met een verzameling van REST-API's (Zie hieronder).

De gebruiker die een service digitale Twins in hun abonnement implementeert, wordt de globale beheerder van het hoofdknooppunt automatisch verlenen volledige toegang tot de volledige structuur. Deze gebruiker kan vervolgens inrichten spaties in de grafiek met de `Space` API. Apparaten kunnen worden ingericht met behulp van de `Device` API, sensoren, kunnen worden ingericht met behulp van `Sensor` API, enzovoort. We bieden ook [open source-hulpprogramma's](https://github.com/Azure-Samples/digital-twins-samples-csharp) voor het inrichten van de grafiek in één bulkbewerking.

Grafiek _overname_ is van toepassing op de machtigingen en de eigenschappen die het resultaat van een bovenliggend knooppunt alle knooppunten eronder. Bijvoorbeeld, wanneer een rol is toegewezen aan een gebruiker op een bepaald knooppunt, heeft de gebruiker van die rol machtigingen voor het knooppunt en elk knooppunt eronder. Bovendien wordt elke sleutel en het uitgebreide type gedefinieerd voor een bepaald knooppunt worden overgenomen door alle knooppunten onder dat knooppunt.

Grafiek _filteren_ kunnen gebruikers resultaten van de aanvraag-id's, naam, typen, subtypen, bovenliggende ruimte, bijbehorende opslagruimten, sensor-gegevenstypen, sleutels en waarden, bladeren, minLevel, maxLevel en andere OData-filter verfijnen de parameters.

Grafiek _doorlopen_ kunnen gebruikers de ruimtelijke graph via de diepgang en uitgebreidheid navigeren. Voor diepte, kan de grafiek worden doorlopen boven naar beneden of beneden met parameters voor de navigatie `traverse`, `minLevel`, `maxLevel`. Voor brede, kan de grafiek om op te halen van knooppunten op hetzelfde niveau die rechtstreeks zijn gekoppeld aan een bovenliggende ruimte of een van de afgeleiden ervan wordt genavigeerd. Bij het opvragen van een object, kan u alle gerelateerde objecten die relaties hebben met dat object door de `includes` parameter van de ophalen-API's.

Azure van digitale dubbels garandeert graph _schaalbaarheid_, zodat uw echte workloads kunnen worden verwerkt. Digitale dubbels kunnen worden gebruikt voor grote portfolio van onroerend goed, infrastructuur, apparaten, sensoren, Telemetrie en meer.

Grafiek _uitbreidbaarheid_ kunnen gebruikers de onderliggende digitale dubbels objectmodellen met nieuwe typen en anthologieën aanpassen. Uw digitale dubbele gegevens kunnen ook worden uitgebreid met extensible eigenschappen en waarden.

### <a name="spatial-intelligence-graph-management-apis"></a>Ruimtelijke intelligence graph Management-API 's

Wanneer u implementeert Azure van digitale dubbels uit de [Azure-portal](https://portal.azure.com), wordt de [Swagger](https://swagger.io/tools/swagger-ui/) URL van de beheer-API's wordt automatisch gegenereerd en wordt weergegeven in de Azure-portal **overzicht** sectie met de volgende indeling:

```plaintext
https://yourInstanceName.yourLocation.azuresmartspaces.net/management/swagger
```

| Naam van aangepast kenmerk | Vervangen |
| --- | --- |
| `yourInstanceName` | De naam van uw Azure digitale Twins-exemplaar |
| `yourLocation` | Welke regio u uw exemplaar wordt gehost op server |

 De volledige URL-indeling kan worden weergegeven in onderstaande afbeelding wordt gebruikt:

![Digitale dubbels Portal Management-API][2]

Voor meer informatie over het gebruik van ruimtelijke Intelligence grafieken, gaat u naar de Azure-API voor het beheer van digitale dubbels eerst te zien.

> [!TIP]
> Een voorbeeld van Swagger eerst wordt geleverd ter illustratie van de API-functie instellen.
> Deze wordt gehost op [docs.westcentralus.azuresmartspaces.net/management/swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

Meer informatie over [over het gebruik van Swagger](how-to-use-swagger.md).

Alle API-aanroepen moeten worden geverifieerd met behulp van [OAuth](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code). Volgt u de API's [Microsoft REST API-richtlijnen conventies](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md). Ondersteuning voor de meeste van de API's die resulteren in verzamelingen [OData](http://www.odata.org/getting-started/basic-tutorial/#queryData) system-opties voor query's.

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over connectiviteit van apparaten en hoe u berichten over telemetrie verzenden naar digitale dubbels Azure-service, [Azure digitale dubbele apparaat connectiviteit en telemetrie inkomend](concepts-device-ingress.md).

Lees voor meer informatie over API Management-beperkingen en vertragingen, [Azure digitale dubbels API management en beperkingen](concepts-service-limits.md).

<!-- Images -->
[1]: media/concepts/digital-twins-spatial-graph-building.png
[2]: media/concepts/digital-twins-spatial-graph-management-api-url.png
