---
title: Voorbeeld gegevens opnemen in azure Data Explorer
description: Meer informatie over het opnemen (laden) van weer gave-gerelateerde voorbeeld gegevens in azure Data Explorer.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 08/12/2019
ms.openlocfilehash: c803de599f6be98512b15e927c6d15f1c7d95ff1
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69515738"
---
# <a name="quickstart-ingest-sample-data-into-azure-data-explorer"></a>Quickstart: Voorbeeld gegevens opnemen in azure Data Explorer

In dit artikel wordt beschreven hoe u voorbeeld gegevens opneemt in een Azure Data Explorer-data base. Er zijn [verschillende manieren om gegevens op te](ingest-data-overview.md)nemen; Dit artikel richt zich op een basis benadering die geschikt is voor test doeleinden.

> [!NOTE]
> U hebt deze gegevens al als u [gegevens opname hebt gemaakt met behulp van de Azure Data Explorer python-bibliotheek](python-ingest-data.md).

## <a name="prerequisites"></a>Vereisten

[Een cluster en database voor testdoeleinden](create-cluster-database-portal.md)

## <a name="ingest-data"></a>Gegevens opnemen

De set met voorbeeldgegevens **StormEvents** bevat gegevens van het weer afkomstig van de [National Centers for Environmental Information](https://www.ncdc.noaa.gov/stormevents/).

1. Meld u aan bij [https://dataexplorer.azure.com](https://dataexplorer.azure.com).

1. Selecteer in de linkerbovenhoek van de toepassing **Add Cluster**.

1. Voer in het dialoog venster **cluster toevoegen** uw cluster-URL in het formulier `https://<ClusterName>.<Region>.kusto.windows.net/`in en selecteer vervolgens **toevoegen**.

1. Plak de volgende opdracht en selecteer **uitvoeren**.

    ```Kusto
    .create table StormEvents (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)

    .ingest into table StormEvents h'https://kustosamplefiles.blob.core.windows.net/samplefiles/StormEvents.csv?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (ignoreFirstRecord=true)
    ```

1. Wanneer de opname is voltooid, plakt u de volgende query in het venster en selecteert u **uitvoeren**.

    ```Kusto
    StormEvents
    | sort by StartTime desc
    | take 10
    ```
    De query retourneert de volgende resultaten van de opgenomen voorbeeld gegevens.

    ![Queryresultaten](media/ingest-sample-data/query-results.png)

## <a name="next-steps"></a>Volgende stappen

* [Gegevens opname van Azure Data Explorer](ingest-data-overview.md) voor meer informatie over opname methoden.
* [Snelstart: Query's uitvoeren op gegevens in](web-query-data.md) Azure Data Explorer web-UI.
* [Schrijf query's](write-queries.md) met een Kusto-query taal.
