---
title: Dubbele gegevens in azure Data Explorer verwerken
description: In dit onderwerp ziet u verschillende benaderingen voor het afhandelen van dubbele gegevens bij gebruik van Azure Data Explorer.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 12/19/2018
ms.openlocfilehash: 60ec2b86e0205060f907f1fe39d084dca3aac1cd
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68608225"
---
# <a name="handle-duplicate-data-in-azure-data-explorer"></a>Dubbele gegevens in azure Data Explorer verwerken

Apparaten die gegevens naar de Cloud verzenden, behouden een lokale cache van de gegevens. Afhankelijk van de grootte van de gegevens, kan de lokale cache gegevens gedurende dagen of zelfs maanden opslaan. U wilt uw analytische data bases beveiligen tegen apparaten die de gegevens in de cache opnieuw verzenden en gegevens dupliceren in de analytische data base veroorzaken. In dit onderwerp vindt u een overzicht van de aanbevolen procedures voor het afhandelen van dubbele gegevens voor deze typen scenario's.

De beste oplossing voor het dupliceren van gegevens kan de duplicatie verhinderen. Los, indien mogelijk, het probleem op dat eerder in de gegevens pijplijn is opgenomen, waarbij de kosten voor het verplaatsen van gegevens in de gegevens pijplijn worden bespaard en u geen bestedings resources op omgaan met dubbele gegevens die in het systeem zijn opgenomen. In situaties waarin het bron systeem niet kan worden gewijzigd, zijn er echter verschillende manieren om dit scenario te behandelen.

## <a name="understand-the-impact-of-duplicate-data"></a>Meer informatie over de impact van dubbele gegevens

Het percentage dubbele gegevens bewaken. Zodra het percentage dubbele gegevens is gedetecteerd, kunt u het bereik van het probleem en de impact van het bedrijf analyseren en de juiste oplossing kiezen.

Voorbeeld query voor het identificeren van het percentage dubbele records:

```kusto
let _sample = 0.01; // 1% sampling
let _data =
DeviceEventsAll
| where EventDateTime between (datetime('10-01-2018 10:00') .. datetime('10-10-2018 10:00'));
let _totalRecords = toscalar(_data | count);
_data
| where rand()<= _sample
| summarize recordsCount=count() by hash(DeviceId) + hash(EventId) + hash(StationId)  // Use all dimensions that make row unique. Combining hashes can be improved
| summarize duplicateRecords=countif(recordsCount  > 1)
| extend duplicate_percentage = (duplicateRecords / _sample) / _totalRecords  
```

## <a name="solutions-for-handling-duplicate-data"></a>Oplossingen voor het afhandelen van dubbele gegevens

### <a name="solution-1-dont-remove-duplicate-data"></a>#1 oplossing: Geen dubbele gegevens verwijderen

Inzicht in uw bedrijfs vereisten en tolerantie voor dubbele gegevens. Sommige gegevens sets kunnen worden beheerd met een bepaald percentage dubbele gegevens. Als de gedupliceerde gegevens geen grote invloed hebben, kunt u de aanwezigheid ervan negeren. Het voor deel van het niet verwijderen van de dubbele gegevens is geen extra overhead op het opname proces of de query prestaties.

### <a name="solution-2-handle-duplicate-rows-during-query"></a>#2 oplossing: Dubbele rijen verwerken tijdens query

Een andere optie is het filteren van de dubbele rijen in de gegevens tijdens de query. De [`arg_max()`](/azure/kusto/query/arg-max-aggfunction) functie Aggregated kan worden gebruikt om de dubbele records te filteren en de laatste record te retour neren op basis van de tijds tempel (of een andere kolom). Het voor deel van het gebruik van deze methode is een snellere opname omdat de-duplicatie tijdens de query tijd plaatsvindt. Daarnaast zijn alle records (inclusief duplicaten) beschikbaar voor controle en probleem oplossing. Het nadeel van het gebruik `arg_max` van de functie is de extra query tijd en belasting op de CPU telkens wanneer er een query wordt uitgevoerd op de gegevens. Afhankelijk van de hoeveelheid gegevens die wordt opgevraagd, kan deze oplossing niet-functioneel of geheugen verbruiken zijn en moeten andere opties worden overgeschakeld.

In het volgende voor beeld wordt de laatste record opgevraagd die is opgenomen in een set kolommen die de unieke records bepalen:

```kusto
DeviceEventsAll
| where EventDateTime > ago(90d)
| summarize hint.strategy=shuffle arg_max(EventDateTime, *) by DeviceId, EventId, StationId
```

Deze query kan ook in een functie worden geplaatst in plaats van de tabel direct te doorzoeken:

```kusto
.create function DeviceEventsView
{
DeviceEventsAll
| where EventDateTime > ago(90d)
| summarize arg_max(EventDateTime, *) by DeviceId, EventId, StationId
}
```

### <a name="solution-3-filter-duplicates-during-the-ingestion-process"></a>#3 oplossing: Dubbele waarden filteren tijdens het opname proces

Een andere oplossing is het filteren van dubbele items tijdens het opname proces. Het systeem negeert de dubbele gegevens tijdens opname in Kusto-tabellen. Gegevens worden opgenomen in een faserings tabel en naar een andere tabel gekopieerd nadat dubbele rijen zijn verwijderd. Het voor deel van deze oplossing is dat de prestaties van query's aanzienlijk worden verbeterd ten opzichte van de vorige oplossing. De nadelen omvatten verhoogde opname tijd en extra kosten voor gegevens opslag. Extra. deze oplossing werkt alleen als dubbele en niet-verwerkingen tegelijkertijd worden uitgevoerd. Als er meerdere gelijktijdige opname waarden met dubbele records zijn, kunnen alle gegevens worden opgenomen, omdat de ontdubbeling proces geen bestaande overeenkomende records in de tabel kan vinden.    

In het volgende voor beeld wordt deze methode getoond:

1. Maak een andere tabel met hetzelfde schema:

    ```kusto
    .create table DeviceEventsUnique (EventDateTime: datetime, DeviceId: int, EventId: int, StationId: int)
    ```

1. Maak een functie waarmee de dubbele records worden gefilterd door de nieuwe records samen te voegen met de gegevens die eerder zijn opgenomen.

    ```kusto
    .create function RemoveDuplicateDeviceEvents()
    {
    DeviceEventsAll
    | join hint.strategy=broadcast kind = anti
        (
        DeviceEventsUnique
        | where EventDateTime > ago(7d)   // filter the data for certain time frame
        | limit 1000000   //set some limitations (few million records) to avoid choking-up the system during outage recovery

        ) on DeviceId, EventId, StationId
    }
    ```

    > [!NOTE]
    > Deelname zijn CPU-gebonden bewerkingen en voegen een extra belasting toe aan het systeem.

1. [Update beleid](/azure/kusto/management/update-policy) instellen op `DeviceEventsUnique` tabel. Het update beleid wordt geactiveerd wanneer nieuwe gegevens in de `DeviceEventsAll` tabel worden geplaatst. De Kusto-engine voert de functie automatisch uit wanneer er nieuwe [gebieden](/azure/kusto/management/extents-overview) worden gemaakt. De verwerking ligt binnen het bereik van de zojuist gemaakte gegevens. De volgende opdracht wordt gebruikt voor de bron tabel`DeviceEventsAll`(), de doel`DeviceEventsUnique`tabel () en de `RemoveDuplicatesDeviceEvents` functie samen om het update beleid te maken.

    ```kusto
    .alter table DeviceEventsUnique policy update
    @'[{"IsEnabled": true, "Source": "DeviceEventsAll", "Query": "RemoveDuplicateDeviceEvents()", "IsTransactional": true, "PropagateIngestionProperties": true}]'
    ```

    > [!NOTE]
    > Het update beleid breidt de duur van opname uit omdat de gegevens tijdens opname worden gefilterd en vervolgens twee maal worden `DeviceEventsAll` opgenomen (aan de `DeviceEventsUnique` tabel en aan de tabel).

1. Beschrijving Stel een lagere Bewaar periode voor gegevens in de `DeviceEventsAll` tabel in om te voor komen dat kopieën van de gegevens worden opgeslagen. Kies het aantal dagen, afhankelijk van het gegevens volume en de tijds duur dat u gegevens wilt bewaren voor het oplossen van problemen. U kunt deze instellen op `0d` dagen retentie om KPV op te slaan en de prestaties te verbeteren, omdat de gegevens niet naar de opslag worden geüpload.

    ```kusto
    .alter-merge table DeviceEventsAll policy retention softdelete = 1d
    ```

## <a name="summary"></a>Samenvatting

Gegevens duplicatie kan op verschillende manieren worden verwerkt. Evalueer de opties zorgvuldig, waarbij rekening wordt gehouden met de prijs en prestaties, om de juiste methode voor uw bedrijf te bepalen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Query's schrijven voor Azure Data Explorer](write-queries.md)
