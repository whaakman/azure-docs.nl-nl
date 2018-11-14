---
title: Gegevensverwerking en de gebruiker gedefinieerde functies met Azure digitale dubbels | Microsoft Docs
description: Overzicht van gegevensverwerking, vergelijkingsprogramma voor en door de gebruiker gedefinieerde functies met digitale dubbels van Azure
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: 2703778cd2eab582a9e7311aaf2024f100261889
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624519"
---
# <a name="data-processing-and-user-defined-functions"></a>Gegevensverwerking en door gebruikers gedefinieerde functies

Azure van digitale dubbels biedt geavanceerde Reken-mogelijkheden. Ontwikkelaars kunnen definiëren en aangepaste functies uitvoeren in binnenkomende berichten over telemetrie te verzenden van gebeurtenissen naar vooraf gedefinieerde eindpunten.

## <a name="data-processing-flow"></a>Gegevensverwerking stroom

Nadat apparaten telemetrische gegevens naar Azure digitale Twins verzenden, ontwikkelaars gegevens kunnen verwerken in vier fasen: valideren, overeenkomen met compute en verzending.

![Azure digitale dubbels gegevensverwerking stroom][1]

1. De fase valideren transformeert het binnenkomende telemetrie-bericht naar een algemeen begrepen [data transfer object](https://en.wikipedia.org/wiki/Data_transfer_object) indeling. Deze fase voert ook validatie van het apparaat en de sensor.
1. De fase van de overeenkomst wordt gezocht naar de juiste door de gebruiker gedefinieerde functies (UDF's) om uit te voeren. Vooraf gedefinieerde vergelijkingsprogramma voor vinden de UDF's op basis van het apparaat, sensor en ruimte gegevens uit de binnenkomende telemetrie-bericht.
1. De compute-fase wordt de UDF's die overeenkomen met in de vorige fase uitgevoerd. Deze functies kunnen lezen en bijwerken van de berekende waarden op ruimtelijke knooppunten in een grafiek en aangepaste meldingen kan verzenden.
1. De fase van de verzending van de routes aangepaste meldingen van de compute-fase naar eindpunten zijn gedefinieerd in de grafiek.

## <a name="data-processing-objects"></a>Gegevensverwerking objecten

Gegevens worden verwerkt in digitale dubbels Azure bestaat uit drie objecten te definiëren: vergelijkingsprogramma voor de gebruiker gedefinieerde functies en roltoewijzingen.

![Azure digitale dubbels gegevensverwerking objecten][2]

### <a name="matchers"></a>Vergelijkingsprogramma voor

Vergelijkingsprogramma voor definiëren een set voorwaarden evalueren welke acties worden uitgevoerd op basis van binnenkomende telemetriegegevens van de sensor. Voorwaarden voor het bepalen van de overeenkomst kunnen eigenschappen van de sensor, van de sensor bovenliggende apparaat en van de sensor bovenliggende ruimte bevatten. De voorwaarden worden uitgedrukt als vergelijkingen op basis van een [JSON-pad](http://jsonpath.com/) zoals wordt beschreven in dit voorbeeld:

- Alle sensoren van gegevenstype **temperatuur**
- Met `01` in hun poort
- Die deel uitmaken van apparaten met de sleutel van de uitgebreide eigenschap **fabrikant** ingesteld op de waarde `"GoodCorp"`
- Die deel uitmaken van opslagruimten van het type `"Venue"`
- Welke descendanten van bovenliggende zijn **SpaceId** `DE8F06CA-1138-4AD7-89F4-F782CC6F69FD`

```JSON
{
  "SpaceId": "DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "Name": "My custom matcher",
  "Description": "All sensors of datatype Temperature with 01 in their port that belong to devices with the extended property key Manufacturer set to the value GoodCorp and that belong to spaces of type Venue that are somewhere below space Id DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "Conditions": [
    {
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    },
    {
      "target": "Sensor",
      "path": "$.port",
      "value": "01",
      "comparison": "Contains"
    },
    {
      "target": "SensorDevice",
      "path": "$.properties[?(@.name == 'Manufacturer')].value",
      "value": "\"GoodCorp\"",
      "comparison": "Equals"
    },
    {
      "target": "SensorSpace",
      "path": "$.type",
      "value": "\"Venue\"",
      "comparison": "Equals"
    }
  ]
}
```

> [!IMPORTANT]
> - JSON-paden zijn hoofdlettergevoelig.
> - De JSON-nettolading is hetzelfde als de nettolading die wordt geretourneerd door:
>   - `/sensors/{id}?includes=properties,types` voor de sensor.
>   - `/devices/{id}?includes=properties,types,sensors,sensorsproperties,sensorstypes` voor de sensor van bovenliggende apparaat.
>   - `/spaces/{id}?includes=properties,types,location,timezone` voor de sensor van bovenliggende ruimte.
> - De vergelijkingen zijn niet hoofdlettergevoelig.

### <a name="user-defined-functions"></a>De gebruiker gedefinieerde functies

Een door de gebruiker gedefinieerde functie is een aangepaste functie die wordt uitgevoerd in een geïsoleerde omgeving in digitale dubbels van Azure. UDF's hebben toegang tot het onbewerkte sensor telemetrie-bericht als deze is ontvangen. UDF's hebben ook toegang tot de ruimtelijke graph- en verzender-service. Nadat de UDF is geregistreerd in de grafiek, moet een matcher (Zie boven) om op te geven bij het uitvoeren van de UDF worden gemaakt. Wanneer Azure digitale dubbels nieuwe telemetrie van een bepaalde sensor ontvangt, kunt de overeenkomende UDF een zwevend gemiddelde van de laatste paar sensorwaarden, serverlogs, bijvoorbeeld berekenen.

UDF's kunnen worden geschreven in JavaScript. Ontwikkelaars kunnen aangepaste fragmenten van code op basis van de sensor telemetrieberichten uitvoeren. Help-methoden werken met de grafiek in de uitvoeringsomgeving die door de gebruiker gedefinieerd. Ontwikkelaars kunnen met een UDF:

- Stel de sensor lezen rechtstreeks op de sensor-object in de grafiek.
- Een actie op basis van verschillende sensorwaarden, serverlogs, binnen een ruimte in de grafiek.
- Maak een melding wanneer bepaalde voorwaarden wordt voldaan voor een binnenkomende sensor lezen.
- De metagegevens van de grafiek koppelen aan de sensor lezen voordat u verstuurt een melding.

Zie voor meer informatie, [over het gebruik van de gebruiker gedefinieerde functies](how-to-user-defined-functions.md).

### <a name="role-assignment"></a>Roltoewijzing

Acties van een UDF zijn afhankelijk van het Azure digitale dubbels op rollen gebaseerd toegangsbeheer voor het beveiligen van gegevens in de service. Roltoewijzingen Zorg ervoor dat een bepaalde UDF de juiste machtigingen om te communiceren met de ruimtelijke grafiek heeft. Bijvoorbeeld, kan een UDF proberen te maken, lezen, bijwerken of verwijderen van de grafiekgegevens zijn onder de beschikbare ruimte. Het toegangsniveau van een UDF wordt gecontroleerd wanneer de UDF vraagt de grafiek om de gegevens of een actie probeert. Zie voor meer informatie, [Role-based access control van](security-create-manage-role-assignments.md).

Het is mogelijk dat een matcher voor het activeren van een UDF waarvoor geen roltoewijzingen. In dit geval mislukt de UDF gegevens lezen uit de grafiek.

## <a name="next-steps"></a>Volgende stappen

* Lees voor meer informatie over het doorsturen van gebeurtenissen en berichten over telemetrie naar andere Azure-services, [gebeurtenissen en -berichten routeren](concepts-events-routing.md).

* Lees voor meer informatie over het maken van vergelijkingsprogramma voor de gebruiker gedefinieerde functies en roltoewijzingen [handleiding voor het gebruik van de gebruiker gedefinieerde functies](how-to-user-defined-functions.md).

<!-- Images -->
[1]: media/concepts/digital-twins-data-processing-flow.png
[2]: media/concepts/digital-twins-user-defined-functions.png
