---
title: Azure SQL Data Warehouse opmerkingen bij de Release | Microsoft Docs
description: Opmerkingen bij de release voor Azure SQL Data Warehouse.
services: sql-data-warehouse
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 02/09/2019
author: mlee3gsd
ms.author: anumjs
ms.reviewer: jrasnick
manager: craigg
ms.openlocfilehash: 2de7c335e56117f2a99db5150575ed94616467e9
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56455582"
---
# <a name="azure-sql-data-warehouse-release-notes"></a>Releaseopmerkingen voor Azure SQL Data Warehouse
In dit artikel bevat een overzicht van de nieuwe functies en verbeteringen in de recente versies van [Azure SQL Data Warehouse](sql-data-warehouse-overview-what-is.md). Het artikel bevat ook belangrijke updates van inhoud die niet direct verband houden met de release maar gepubliceerd in dezelfde periode. Zie voor verbeteringen aan andere Azure-services, [Service-updates](https://azure.microsoft.com/updates)

## <a name="sql-data-warehouse-version-100101060-january"></a>SQL Data Warehouse versie 10.0.10106.0 (januari)

### <a name="service-improvements"></a>Verbeteringen van de services

| Verbeteringen van de services | Details |
| --- | --- |
|**Order By-optimalisatie retourneren**|SELECTEREN... ORDER BY-query's krijgen prestaties in deze release.   Alle rekenbronnen nu knooppunten verzendt dat de resultaten naar een enkele rekenknooppunt te selecteren, die wordt samengevoegd en sorteert de resultaten, terug naar de gebruiker via het rekenknooppunt keert.  Samengevoegd door de resultaten van een enkele compute-knooppunt in een aanzienlijke prestatieverbetering te bereiken wanneer de queryresultaatset een groot aantal rijen bevat. De engine voor het uitvoeren van query zou eerder, bestellen resultaten op elk rekenknooppunt en kunt streamen naar het beheerknooppunt, die zouden de resultaten vervolgens samenvoegen.|
|**Gegevens verplaatsen verbeteringen voor PartitionMove en BroadcastMove**|Gebruik in Azure SQL Data Warehouse Gen2, data movement stappen van het type ShuffleMove, instant data movement technieken beschreven in de [prestaties verbeteringen blog](https://azure.microsoft.com/blog/lightning-fast-query-performance-with-azure-sql-data-warehouse/). Met deze release, worden gegevenstypen van verkeer PartitionMove en BroadcastMove nu ook aangestuurd door de dezelfde technieken voor directe gegevensverplaatsing. Gebruiker-query's die gebruikmaken van deze typen gegevens verkeer stappen worden uitgevoerd met verbeterde prestaties. Er zijn geen codewijzigingen is vereist om te profiteren van deze verbeterde prestaties.|
|**Fouten die zijn die aandacht vereisen**|Onjuiste versie van Azure SQL Data Warehouse - ' Selecteer@VERSION' kan de juiste versie, 10.0.9999.0 retourneren. De juiste versie van de huidige versie is 10.0.10106.0. Deze fout is gemeld en is onder revisie.

### <a name="documentation-improvements"></a>Verbeteringen in de documentatie bij

| Verbeteringen in de documentatie bij | Details |
| --- | --- |
|geen | |
| | |

## <a name="next-steps"></a>Volgende stappen
- [een SQL Data Warehouse maakt](./create-data-warehouse-portal.md)

## <a name="more-information"></a>Meer informatie
- [Blog - Azure SQL datawarehouse](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
- [Teamblogs met adviezen voor klanten](https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/)
- [Succesverhalen van klanten](https://azure.microsoft.com/case-studies/?service=sql-data-warehouse)
- [Stack Overflow-forum](http://stackoverflow.com/questions/tagged/azure-sqldw)
- [Twitter](https://twitter.com/hashtag/SQLDW)
- [Video's](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)
- [Azure-woordenlijst](../azure-glossary-cloud-terminology.md)
