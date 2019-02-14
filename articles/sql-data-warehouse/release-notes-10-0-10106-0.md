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
ms.openlocfilehash: 51932ebf7d5bdc6830098ce7136a3eee7255ffe1
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56245504"
---
# <a name="azure-sql-data-warehouse-release-notes"></a>Releaseopmerkingen voor Azure SQL Data Warehouse
In dit artikel bevat een overzicht van de nieuwe functies en verbeteringen in de recente versies van [SQL Server op Azure virtual machines](sql-data-warehouse-overview-what-is.md). Het artikel bevat ook belangrijke inhoud updates die niet direclty met betrekking tot de release, maar in dezelfde periode gepubliceerd. Zie voor verbeteringen aan andere Azure-services, [Service-updates](https://azure.microsoft.com/updates)

## <a name="sql-data-warehouse-version-100101060-january"></a>SQL Data Warehouse versie 10.0.10106.0 (januari)

### <a name="service-improvements"></a>Verbeteringen van de services

| Verbeteringen van de services | Details |
| --- | --- |
| **Query Restartability - CTAS en invoegen of te selecteren** | In zeldzame gevallen (dat wil zeggen, problemen met de onregelmatige netwerkverbinding, knooppuntfouten) kan query wordt uitgevoerd in Azure SQL DW mislukken. Meer instructies, zoals actief [CREATE TABLE AS SELECT (CTAS)](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-ctas) en bewerkingen INSERT-selecteren, beter zichtbaar zijn voor dit potentiële probleem. Met deze release implementeert Azure SQL DW logica voor opnieuw proberen op CTAS en INSERT SELECT-instructies, naast de SELECT-instructies eerder aangekondigd. De wijzigingen kunt de service transparant tijdelijke problemen verwerken en te voorkomen dat query's mislukken. Het aantal nieuwe pogingen en de lijst van tijdelijke fouten verwerkt worden door het systeem is geconfigureerd.|
|**Order By-optimalisatie retourneren**|SELECTEREN... ORDER BY-query's krijgen prestaties in deze release.   Alle rekenbronnen nu knooppunten verzendt dat de resultaten naar een enkele rekenknooppunt te selecteren, die wordt samengevoegd en sorteert de resultaten, terug naar de gebruiker via het rekenknooppunt keert.  Samengevoegd door de resultaten van een enkele compute-knooppunt in een aanzienlijke prestatieverbetering te bereiken wanneer de queryresultaatset een groot aantal rijen bevat. De engine voor het uitvoeren van query zou eerder, bestellen resultaten op elk rekenknooppunt en kunt streamen naar het beheerknooppunt, die zouden de resultaten vervolgens samenvoegen.|
|**Gegevens verplaatsen verbeteringen voor PartitionMove en BroadcastMove**|Gebruik in Azure SQL Data Warehouse Gen2, data movement stappen van het type ShuffleMove, instant data movement technieken beschreven in de [prestaties verbeteringen blog](https://azure.microsoft.com/blog/lightning-fast-query-performance-with-azure-sql-data-warehouse/). Met deze release, worden gegevenstypen van verkeer PartitionMove en BroadcastMove nu ook aangestuurd door de dezelfde technieken voor directe gegevensverplaatsing. Gebruiker-query's die gebruikmaken van deze typen gegevens verkeer stappen worden uitgevoerd met verbeterde prestaties. Er zijn geen codewijzigingen is vereist om te profiteren van deze verbeterde prestaties.|

### <a name="documentation-improvements"></a>Verbeteringen in de documentatie bij

| Verbeteringen in de documentatie bij | Details |
| --- | --- |
|geen | |
| | |

## <a name="next-steps"></a>Volgende stappen
[een SQL Data Warehouse maakt](./create-data-warehouse-portal.md)

## <a name="more-information"></a>Meer informatie
- [Blog - Azure SQL datawarehouse](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
- [Teamblogs met adviezen voor klanten](https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/)
- [Succesverhalen van klanten](https://azure.microsoft.com/case-studies/?service=sql-data-warehouse)
- [Stack Overflow-forum](http://stackoverflow.com/questions/tagged/azure-sqldw)
- [Twitter](https://twitter.com/hashtag/SQLDW)
- [Video's](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)
- [Azure-woordenlijst](../azure-glossary-cloud-terminology.md)