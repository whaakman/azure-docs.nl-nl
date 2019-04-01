---
title: Dubbele gegevens aanpakken
description: Dit onderwerp wordt beschreven u verschillende manieren om op te lossen met dubbele gegevens
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 12/19/2018
ms.openlocfilehash: 7a9ccd5139168393019a70aafb9b7b705ca25919
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756360"
---
# <a name="deal-with-duplicate-data"></a>Dubbele gegevens aanpakken

Apparaten die gegevens verzenden naar de Cloud behouden een lokale cache van de gegevens. Afhankelijk van de gegevensgrootte kan kan de lokale cache worden opslag van gegevens voor dagen of zelfs maanden. Wilt u uw analytische databases van niet-functionerende apparaten die opnieuw verzenden van gegevens in de cache en ervoor zorgen dat gegevensontdubbeling in de analytische database beveiligen. In dit onderwerp worden aanbevolen procedures voor het verwerken van dubbele gegevens voor dit soort scenario's.

De beste oplossing voor het kopiëren van gegevens voorkomt dat de mate van duplicatie. Los het probleem, indien mogelijk, eerder in de gegevenspijplijn, die kosten voor gegevensverplaatsing langs de gegevenspijplijn worden opgeslagen en u voorkomt uitgaven van resources op kopieert met dubbele gegevens die zijn opgenomen in het systeem. In situaties waarbij het bronsysteem kan niet worden gewijzigd, zijn er verschillende manieren om u te bekommeren om dit scenario.

## <a name="understand-the-impact-of-duplicate-data"></a>Begrijp de gevolgen van dubbele gegevens

Het percentage van de dubbele gegevens bewaken. Wanneer het percentage van de dubbele gegevens wordt gedetecteerd, kunt u het bereik van de probleem- en business impact analyseren en kies de juiste oplossing.

Voorbeeldquery voor het identificeren van het percentage van de dubbele records:

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

## <a name="solutions-for-handling-duplicate-data"></a>Oplossingen voor de verwerking van dubbele gegevens

### <a name="solution-1-dont-remove-duplicate-data"></a>Oplossing #1: Dubbele gegevens niet verwijderen

Inzicht in uw zakelijke vereisten en tolerantie van dubbele gegevens. Sommige gegevenssets kunt beheren met een bepaald percentage van dubbele gegevens. Als de gedupliceerde gegevens geen grote gevolgen heeft, kunt u de aanwezigheid negeren. Het voordeel van het niet verwijderen van de duplicaatgegevens is geen extra overhead op de prestaties van gegevensopname proces of query.

### <a name="solution-2-handle-duplicate-rows-during-query"></a>Oplossing #2: Dubbele rijen tijdens query verwerken

Een andere optie is de dubbele rijen in de gegevens kunt uitfilteren tijdens query. De [ `arg_max()` ](/azure/kusto/query/arg-max-aggfunction) statistische functie kan worden gebruikt om de dubbele records filteren en de laatste record op basis van de tijdstempel (of een andere kolom) retourneren. Het voordeel van het gebruik van deze methode is sneller opname omdat deduplicatie treedt op tijdens het uitvoeren van query's. Bovendien worden alle records (met inbegrip van duplicaten) beschikbaar voor controle en probleemoplossing. Het nadeel van het gebruik van de `arg_max` functie is de aanvullende uitvoeren van query's en de belasting van de CPU, telkens wanneer de gegevens op de query wordt uitgevoerd. Afhankelijk van de hoeveelheid gegevens wordt opgevraagd, worden deze oplossing kan worden niet-functionele of geheugen verbruikt en moet overschakelen naar de andere opties.

In het volgende voorbeeld wordt een query we in de laatste record opgenomen voor een set kolommen die de unieke records bepalen:

```kusto
DeviceEventsAll
| where EventDateTime > ago(90d)
| summarize hint.strategy=shuffle arg_max(EventDateTime, *) by DeviceId, EventId, StationId
```

Deze query kan ook worden geplaatst in een functie in plaats van rechtstreeks uitvoeren van query's in de tabel:

```kusto
.create function DeviceEventsView
{
DeviceEventsAll
| where EventDateTime > ago(90d)
| summarize arg_max(EventDateTime, *) by DeviceId, EventId, StationId
}
```

### <a name="solution-3-filter-duplicates-during-the-ingestion-process"></a>Oplossing #3: Dubbele waarden filteren tijdens de opname

Een andere oplossing is voor het filteren van duplicaten tijdens de opname. Het systeem negeert de dubbele gegevens tijdens de opname in Kusto-tabellen. Gegevens die zijn opgenomen in een tijdelijke tabel en gekopieerd naar een andere tabel na het verwijderen van dubbele rijen. Het voordeel van deze oplossing is de queryprestaties aanzienlijk verbeterd in vergelijking met de vorige oplossing. De nadelen zijn verhoogde opname tijd en extra kosten voor gegevensopslag.

Het volgende voorbeeld ziet u deze methode:

1. Maak een andere tabel met hetzelfde schema:

    ```kusto
    .create table DeviceEventsUnique (EventDateTime: datetime, DeviceId: int, EventId: int, StationId: int)
    ```

1. Maak een functie uit de dubbele records te filteren om lid te worden de nieuwe records met de eerder opgenomen resultaten.

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
    > Samenvoegingen bewerkingen zijn afhankelijk van de CPU en het toevoegen van een extra belasting op het systeem.

1. Stel [updatebeleid](/azure/kusto/management/update-policy) op `DeviceEventsUnique` tabel. De updatebeleid wordt geactiveerd als er nieuwe gegevens in de `DeviceEventsAll` tabel. De Kusto-engine wordt automatisch uitvoeren van de functie als nieuwe [gebieden](/azure/kusto/management/extents-overview) worden gemaakt. De verwerking is afgestemd op de zojuist gemaakte gegevens. De volgende opdracht uit dan aan elkaar gehecht de brontabel (`DeviceEventsAll`), doeltabel (`DeviceEventsUnique`), en de functie `RemoveDuplicatesDeviceEvents` samen de om updatebeleid te maken.

    ```kusto
    .alter table DeviceEventsUnique policy update
    @'[{"IsEnabled": true, "Source": "DeviceEventsAll", "Query": "RemoveDuplicateDeviceEvents()", "IsTransactional": true, "PropagateIngestionProperties": true}]'
    ```

    > [!NOTE]
    > Updatebeleid breidt de duur van gegevensopname omdat de gegevens worden gefilterd tijdens de opname en vervolgens twee keer die zijn opgenomen (aan de `DeviceEventsAll` tabel en de `DeviceEventsUnique` tabel).

1. (Optioneel) Stel een lagere bewaren van gegevens op de `DeviceEventsAll` tabel om te voorkomen dat opslaan van kopieën van de gegevens. Kies het aantal dagen, afhankelijk van het gegevensvolume en de hoeveelheid tijd die u wilt bewaren van gegevens voor het oplossen van problemen. U kunt dit instellen op `0d` dagen retentie COGS opslaan en de prestaties verbeteren, omdat de gegevens niet is geüpload naar de opslag.

    ```kusto
    .alter-merge table DeviceEventsAll policy retention softdelete = 1d
    ```

## <a name="summary"></a>Samenvatting

Gegevensontdubbeling kan op verschillende manieren worden verwerkt. Evalueren van de opties zorgvuldig, waarbij rekening wordt account prijs en prestaties, om te bepalen van de juiste methode voor uw bedrijf.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Query's schrijven voor Azure Data Explorer](write-queries.md)
