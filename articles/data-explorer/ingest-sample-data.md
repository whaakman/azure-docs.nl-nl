---
title: Voorbeeldgegevens worden opgenomen in Azure Data Explorer
description: Meer informatie over het opnemen van (load) weergerelateerde voorbeeldgegevens in Azure Data Explorer.
services: data-explorer
author: mgblythe
ms.author: mblythe
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 7eb0e48a5b66775ac97ed0cab751db0ef367f667
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46964612"
---
# <a name="ingest-sample-data-into-azure-data-explorer"></a>Voorbeeldgegevens worden opgenomen in Azure Data Explorer

Dit artikel ziet u hoe u voorbeeldgegevens (load) opnemen in de database van een Azure Data Explorer. Er zijn [verschillende manieren voor opname van gegevens](ingest-data-overview.md); in dit artikel is gericht op een eenvoudige benadering die geschikt is voor testdoeleinden.

> [!NOTE]
> U hebt al deze gegevens als u voltooid [Quick Start: opnemen van gegevens met behulp van de Azure Data Explorer Python-clientbibliotheek](python-ingest-data.md).

## <a name="prerequisites"></a>Vereisten

[Een testcluster en de database](create-cluster-database-portal.md)

## <a name="ingest-data"></a>Gegevens opnemen

De **StormEvents** verzameling voorbeeldgegevens bevat weergerelateerde gegevens van de [National Centers voor omgevingsinformatie](https://www.ncdc.noaa.gov/stormevents/).

1. Aanmelden bij [ https://dataexplorer.azure.com ](https://dataexplorer.azure.com).

1. Selecteer in de linkerbovenhoek van de toepassing, **toevoegen cluster**.

1. In de **toevoegen cluster** dialoogvenster vak, voert u uw cluster-URL in het formulier `https://<ClusterName>.<Region>.kusto.windows.net/`en selecteer vervolgens **toevoegen**.

1. Plak in de volgende opdracht uit en selecteer **uitvoeren**.

    ```Kusto
    .create table StormEvents (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)

    .ingest into table StormEvents h'https://kustosamplefiles.blob.core.windows.net/samplefiles/StormEvents.csv?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (ignoreFirstRecord=true)
    ```

1. Nadat de opname is voltooid, plak in de volgende query, selecteert u de query in het venster en selecteer **uitvoeren**.

    ```Kusto
    StormEvents
    | sort by StartTime desc
    | take 10
    ```
    De query retourneert de volgende resultaten van de opgenomen voorbeeldgegevens.

    ![Queryresultaten](media/ingest-sample-data/query-results.png)

## <a name="next-steps"></a>Volgende stappen

[Query's schrijven](write-queries.md)

[Gegevensopname met Azure Data Explorer](ingest-data-overview.md)