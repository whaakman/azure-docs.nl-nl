---
title: Inzicht in digitale dubbels objectmodellen en ruimtelijke intelligence graph | Microsoft Docs
description: Gebruik Azure Digital Twins om relaties tussen mensen, plaatsen en apparaten te modelleren
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: alinast
ms.openlocfilehash: 912a3ed558f8fabfcad517aeb7b7e864cf8f359e
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53436922"
---
# <a name="understand-digital-twins-object-models-and-spatial-intelligence-graph"></a>Inzicht in digitale dubbels objectmodellen en ruimtelijke intelligence graph

Azure van digitale dubbels is een Azure IoT-service die wordt gebruikt door de uitgebreide virtuele representaties van fysieke omgevingen en bijbehorende apparaten, sensoren en personen. Dit verbetert de ontwikkeling door het organiseren van domeinspecifieke concepten in nuttige modellen. De modellen worden vervolgens in een grafiek ruimtelijke intelligence gelegen. Deze begrippen model zonder de relaties en interacties tussen mensen, spaties en apparaten.

Digitale dubbels objectmodellen beschrijven domeinspecifieke concepten, categorieën en eigenschappen. Modellen zijn vooraf gedefinieerd door gebruikers die willen aanpassen van de oplossing voor hun specifieke behoeften. Samen vormen deze vooraf gedefinieerde digitale dubbels objectmodellen gezamenlijk een _ontologie_. Een slimme gebouwen ontologie beschrijving van regio's, evenementen, verdiepingen, kantoren, zones, conferentieruimten en focus ruimten. Een raster energie ontologie beschrijving van verschillende power stations, substations, energiebronnen en klanten. Met digitale dubbels objectmodellen en anthologieën, kunnen diverse scenario's en behoeften worden aangepast.

U kunt vullen met digitale dubbels objectmodellen en een ontologie op locatie, een _ruimtelijke graph_. Ruimtelijke grafieken zijn virtuele representaties van de vele relaties tussen spaties, apparaten en mensen die relevant voor een IoT-oplossing zijn. In dit diagram toont een voorbeeld van een ruimtelijke grafiek die gebruikmaakt van een slimme gebouwen ontologie.

![Digitale dubbels ruimtelijke grafiek samenstellen][1]

<a id="model"></a>

De ruimtelijke grafiek combineert spaties, apparaten, sensoren en gebruikers. Elk gekoppeld is op een manier die de echte wereld-modellen. In dit voorbeeld heeft venue 43 vier verdiepingen, elk met veel verschillende gebieden. Gebruikers zijn die zijn gekoppeld aan hun werkstation en toegang krijgen tot gedeelten van de grafiek. Een beheerder heeft de rechten wijzigingen aanbrengen aan de ruimtelijke grafiek, terwijl een bezoeker heeft rechten om alleen bepaalde gebouw-gegevens weer te geven.

## <a name="digital-twins-object-models"></a>Digitale dubbels objectmodellen

Digitale dubbels objectmodellen ondersteunen deze hoofdcategorieën van objecten:

- **Opslagruimten** zijn virtuele of fysieke locaties, bijvoorbeeld `Tenant`, `Customer`, `Region`, en `Venue`.
- **Apparaten** zijn virtuele of fysieke stukjes apparatuur, bijvoorbeeld `AwesomeCompany Device` en `Raspberry Pi 3`.
- **Sensoren** zijn objecten die detecteren van gebeurtenissen, bijvoorbeeld `AwesomeCompany Temperature Sensor` en `AwesomeCompany Presence Sensor`.
- **Gebruikers** personen en hun kenmerken te identificeren.

Er zijn andere categorieën van objecten:

- **Resources** zijn gekoppeld aan een spatie en vertegenwoordigen meestal Azure-resources moet worden gebruikt door objecten in de ruimtelijke grafiek, bijvoorbeeld `IoTHub`.
- **BLOBs** zijn gekoppeld aan objecten (zoals spaties, apparaten, sensoren en gebruikers). Ze worden gebruikt als bestanden met mime-type en metagegevens, bijvoorbeeld `maps`, `pictures`, en `manuals`.
- **Uitgebreide typen** zijn extensible inventarisaties die de ervaring van entiteiten met specifieke kenmerken, bijvoorbeeld `SpaceType` en `SpaceSubtype`.
- **Anthologieën** vertegenwoordigt een set uitgebreide typen, bijvoorbeeld `Default`, `Building`, `BACnet`, en `EnergyGrid`.
- **Sleutels en waarden** zijn aangepaste kenmerken van opslagruimten, apparaten, sensoren en gebruikers. Ze kunnen worden gebruikt samen met ingebouwde kenmerken, bijvoorbeeld `DeltaProcessingRefreshTime` als sleutel en `10` als waarde.
- **Rollen** sets met machtigingen worden toegewezen aan gebruikers en apparaten in de ruimtelijke grafiek, bijvoorbeeld `Space Administrator`, `User Administrator`, en `Device Administrator`.
- **Roltoewijzingen** zijn de koppeling tussen een rol en een object in de ruimtelijke grafiek. Bijvoorbeeld kan een gebruiker of een service-principal worden gemachtigd voor het beheren van een spatie in de grafiek voor ruimtelijke.
- **Security key winkels** de sleutels voor alle apparaten in de hiërarchie onder een bepaalde ruimte-object waarmee het apparaat veilig communiceren met digitale dubbels bieden.
- **De gebruiker gedefinieerde functies** (UDF's) toestaan aanpasbare sensor telemetrie verwerken binnen de ruimtelijke grafiek. Bijvoorbeeld, een UDF kunt doen:
  - Stel de waarde van een sensor.
  - Aangepaste logica op basis van sensorwaarden, serverlogs, uitvoeren en stel de uitvoer van een spatie.
  - Metagegevens koppelen aan een spatie.
  - Meldingen verzenden wanneer een vooraf gedefinieerde voorwaarden wordt voldaan. Op dit moment kunnen UDF's worden geschreven in JavaScript.
- **Vergelijkingsprogramma voor** objecten om te welke UDF's bepalen worden uitgevoerd voor een bepaalde telemetrieberichten zijn.
- **Eindpunten** zijn de locaties waar berichten over Telemetrie en gebeurtenissen van digitale dubbels kunnen worden gerouteerd, bijvoorbeeld `Event Hub`, `Service Bus`, en `Event Grid`.

<a id="graph"></a>

## <a name="spatial-intelligence-graph"></a>Grafiek voor ruimtelijke intelligentie

Ruimtelijke graph is de hiërarchische grafiek van opslagruimten, apparaten en mensen die zijn gedefinieerd in het digitale Twins-objectmodel. De ruimtelijke grafiek biedt ondersteuning voor overname, filteren, doorlopen, schaalbaarheid en uitbreidbaarheid. U kunt beheren en manipuleren van uw ruimtelijke graph met een verzameling van REST-API's.

Als u een digitale Twins-service in uw abonnement implementeert, worden u globale beheerder van het hoofdknooppunt. U hebt volledige toegang tot de volledige structuur vervolgens automatisch verleend. Spaties in de grafiek inrichten met behulp van de ruimte-API. Services inrichten met behulp van de apparaat-API en sensoren met behulp van de Sensor-API. [Open source-hulpprogramma's](https://github.com/Azure-Samples/digital-twins-samples-csharp) zijn ook beschikbaar voor het inrichten van de grafiek in één bulkbewerking.

**Graph-overname**. Overname is van toepassing op de machtigingen en de eigenschappen die het resultaat van een bovenliggend knooppunt alle knooppunten eronder. Wanneer een rol is toegewezen aan een gebruiker op een bepaald knooppunt, heeft de gebruiker van die rol machtigingen voor het knooppunt en elk knooppunt eronder. Elke sleutel en het uitgebreide type gedefinieerd voor een bepaald knooppunt wordt overgenomen door alle knooppunten onder dat knooppunt.

**Grafiek filteren**. Filteren wordt gebruikt voor het verfijnen van resultaten van de aanvraag. U kunt filteren op id, naam, typen, subtypen, bovenliggende ruimte en bijbehorende opslagruimten. U ook kunt filteren op de sensor-gegevenstypen, sleutels en waarden, *passeren*, *minLevel*, *maxLevel*, en andere parameters OData-filter.

**Grafiek doorlopen**. U kunt de ruimtelijke graph via de diepgang en uitgebreidheid passeren. Voor diepte, door u de graaf boven naar beneden of beneden gaan met behulp van de parameters *passeren*, *minLevel*, en *maxLevel*. Door de graaf om op te halen van knooppunten op hetzelfde niveau die rechtstreeks zijn gekoppeld aan een bovenliggende ruimte of een van de afgeleiden ervan voor breedte gaan. Wanneer u een object opvraagt, krijgt u alle gerelateerde objecten waarvoor relaties voor het object met behulp van de *bevat* parameter van de ophalen-API's.

**Graph-schaalbaarheid**. Digitale dubbels garandeert graph schaalbaarheid, zodat uw echte workloads kunnen worden verwerkt. Digitale dubbels kunnen worden gebruikt voor grote portfolio van onroerend goed, infrastructuur, apparaten, sensoren, Telemetrie en meer.

**Graph-uitbreidbaarheid**. Uitbreidbaarheid gebruiken voor het aanpassen van de onderliggende digitale dubbels objectmodellen met nieuwe typen en anthologieën. De gegevens van uw digitale dubbels kunnen ook worden uitgebreid met extensible eigenschappen en waarden.

### <a name="spatial-intelligence-graph-management-apis"></a>Ruimtelijke intelligence graph Management-API 's

Nadat u hebt geïmplementeerd digitale dubbels uit de [Azure-portal](https://portal.azure.com), wordt de [Swagger](https://swagger.io/tools/swagger-ui/) URL van de beheer-API's wordt automatisch gegenereerd. Deze wordt weergegeven in de Azure portal in de **overzicht** sectie met de volgende indeling.

```plaintext
https://YOUR_INSTANCE_NAME.YOUR_LOCATION.azuresmartspaces.net/management/swagger
```

| Name | Vervangen door |
| --- | --- |
| YOUR_INSTANCE_NAME | De naam van uw Digital Twins-exemplaar |
| YOUR_LOCATION | De serverregio waarin het exemplaar wordt gehost |

 De volledige URL-indeling wordt weergegeven in deze afbeelding.

![Digitale dubbels portal Management-API][2]

Voor meer informatie over het gebruik van ruimtelijke intelligence grafieken, gaat u naar de Azure-API voor het beheer van digitale dubbels eerst te zien.

> [!TIP]
> Een voorbeeld van Swagger eerst wordt geleverd ter illustratie van de API-functie instellen.
> Deze wordt gehost op [docs.westcentralus.azuresmartspaces.net/management/swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

Meer informatie over [over het gebruik van Swagger](how-to-use-swagger.md).

Alle API-aanroepen moeten worden geverifieerd met behulp van [OAuth](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code). Volgt u de API's [Microsoft REST API-richtlijnen conventies](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md). Ondersteuning voor de meeste van de API's die resulteren in verzamelingen [OData](http://www.odata.org/getting-started/basic-tutorial/#queryData) system-opties voor query's.

## <a name="next-steps"></a>Volgende stappen

- Lees meer over de connectiviteit van apparaten en hoe u berichten over telemetrie verzendt naar digitale dubbels [Azure digitale dubbele apparaat connectiviteit en telemetrie inkomend](concepts-device-ingress.md).

- Lees voor meer informatie over API Management-beperkingen en vertragingen, [Azure digitale dubbels API management en beperkingen](concepts-service-limits.md).

<!-- Images -->
[1]: media/concepts/digital-twins-spatial-graph-building.png
[2]: media/concepts/digital-twins-spatial-graph-management-api-url.png
