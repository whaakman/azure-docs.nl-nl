---
title: Gegevensverwerking en de gebruiker gedefinieerde functies met Azure digitale dubbels | Microsoft Docs
description: Overzicht van gegevensverwerking, vergelijkingsprogramma voor en door de gebruiker gedefinieerde functies met digitale dubbels van Azure
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: b7561848ffd0158e22e97530774112dcee2a9864
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324078"
---
# <a name="data-processing-and-user-defined-functions"></a>Gegevens verwerken en de gebruiker gedefinieerde functies

Azure van digitale dubbels biedt geavanceerde Reken-mogelijkheden. Ontwikkelaars kunnen definiëren en aangepaste functies uitvoeren in binnenkomende berichten over telemetrie te verzenden van gebeurtenissen naar vooraf gedefinieerde eindpunten.

## <a name="data-processing-flow"></a>Gegevensverwerking stroom

Zodra apparaten telemetriegegevens naar digitale Twins verzenden, ontwikkelaars gegevens kunnen verwerken in vier fasen: _valideren_, _overeenkomen met_, _compute_, en _verzending_:

![Digitale dubbels gegevensverwerking stroom][1]

1. De _valideren_ fase transformeert het binnenkomende telemetrie-bericht naar een algemeen begrepen [ `data transfer object` ](https://en.wikipedia.org/wiki/Data_transfer_object) indeling. Deze fase voert ook validatie van het apparaat en de sensor.
1. De _overeenkomen met_ fase vindt de juiste door de gebruiker gedefinieerde functie (s) om uit te voeren. Vooraf gedefinieerde vergelijkingsprogramma voor vindt dat de door de gebruiker gedefinieerde functie (s) op basis van het apparaat, sensor en ruimte gegevens uit de binnenkomende telemetrie-bericht.
1. De _compute_ fase wordt uitgevoerd voor de door de gebruiker gedefinieerde functie (s) in de vorige fase overeenkomen. Deze functie (s) kunnen lezen en bijwerken van berekende waarden op ruimtelijke graph-knooppunten en kunt aangepaste meldingen verzenden.
1. De _verzending_ fase routes aangepaste meldingen van de compute-fase naar eindpunten zijn gedefinieerd in de grafiek.

## <a name="data-processing-objects"></a>Gegevensverwerking objecten

Gegevens worden verwerkt in digitale dubbels Azure bestaat uit drie objecten te definiëren: _vergelijkingsprogramma voor_, _door gebruiker gedefinieerde functies_, en _roltoewijzingen_:

![Digitale dubbels gegevensverwerking stroom][2]

### <a name="matchers"></a>Vergelijkingsprogramma voor

_Vergelijkingsprogramma voor_ definiëren van een set voorwaarden die evalueren welke acties uitgevoerd op basis van binnenkomende telemetriegegevens van de sensor. Deze voorwaarden om te bepalen van de overeenkomst kunnen eigenschappen van de sensor, van de sensor bovenliggende apparaat en van de sensor bovenliggende ruimte bevatten. De voorwaarden worden uitgedrukt als vergelijkingen op basis van een [JSON-pad](http://jsonpath.com/) zoals wordt beschreven in het voorbeeld hieronder:

- Alle sensoren van gegevenstype `Temperature`.
- Met `01` in hun poort.
- Die deel uitmaken van apparaten met de sleutel van de uitgebreide eigenschap `Manufacturer` ingesteld op de waarde `GoodCorp`.
- Die deel uitmaken van opslagruimten van het type `Venue`.
- Welke descendanten van bovenliggende zijn `SpaceId` `DE8F06CA-1138-4AD7-89F4-F782CC6F69FD`.

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

Een _de gebruiker gedefinieerde functie_, of _UDF_, een aangepaste functie die wordt uitgevoerd in een geïsoleerde omgeving in digitale dubbels van Azure. UDF's hebben toegang tot zowel de onbewerkte sensor telemetrieberichten als deze is ontvangen, evenals garantie voor de ruimtelijke graph- en verzender-service. Zodra de UDF is geregistreerd in de grafiek, moet een matcher (Zie boven) om op te geven bij het uitvoeren van de UDF worden gemaakt. Wanneer nieuwe telemetrie digitale dubbels van een bepaalde sensor ontvangt, kunt de overeenkomende UDF een zwevend gemiddelde van de laatste paar sensorwaarden, serverlogs, bijvoorbeeld berekenen.

UDF's kunnen worden geschreven in JavaScript en kunnen ontwikkelaars aangepaste fragmenten van code op basis van de sensor telemetrieberichten uitvoeren. Er zijn ook Help-methoden om te communiceren met de grafiek in de uitvoeringsomgeving die door de gebruiker gedefinieerd. Ontwikkelaars kunnen met een UDF:

- Stel de sensor lezen rechtstreeks op de sensor-object in de grafiek.
- Een actie op basis van verschillende sensorwaarden, serverlogs, binnen een ruimte in de grafiek.
- Maak een melding wanneer bepaalde voorwaarden wordt voldaan voor een binnenkomende sensor lezen.
- De metagegevens van de grafiek koppelen aan de sensor lezen voordat u verstuurt een melding.

Raadpleeg [hoe User User-Defined functies](how-to-user-defined-functions.md) voor meer informatie.

### <a name="role-assignment"></a>Roltoewijzing

Acties van een UDF zijn afhankelijk van digitale dubbels op rollen gebaseerd toegangsbeheer voor het beveiligen van gegevens in de service. Roltoewijzingen Zorg ervoor dat een bepaalde UDF de juiste machtigingen om te communiceren met de ruimtelijke grafiek. Bijvoorbeeld, kan een UDF proberen te maken, lezen, bijwerken of verwijderen van de grafiekgegevens zijn onder de beschikbare ruimte. Het toegangsniveau van een UDF wordt gecontroleerd wanneer de UDF vraagt de grafiek om de gegevens of een actie probeert. Zie voor meer informatie, [Role-Based Access Control](security-create-manage-role-assignments.md).

Het is mogelijk dat een matcher voor het activeren van een UDF waarvoor geen roltoewijzingen. In dit geval de UDF niet lezen van gegevens in de grafiek.

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over hoe u routering gebeurtenissen en van telemetrieberichten naar andere Azure-services, [Routing gebeurtenissen en -berichten](concepts-events-routing.md).

Lees voor meer informatie over het maken van vergelijkingsprogramma voor de gebruiker gedefinieerde functies en roltoewijzingen [handleiding voor het gebruik van de gebruiker gedefinieerde functies](how-to-user-defined-functions.md).

<!-- Images -->
[1]: media/concepts/digital-twins-data-processing-flow.png
[2]: media/concepts/digital-twins-user-defined-functions.png
