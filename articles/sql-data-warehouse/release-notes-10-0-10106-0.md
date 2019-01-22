---
title: Azure SQL Data Warehouse opmerkingen bij de Release van December 2018 | Microsoft Docs
description: Opmerkingen bij de release voor Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 12/12/2018
ms.author: mausher
ms.reviewer: twounder
ms.openlocfilehash: 074276e4550b9b2e347e5cd30c597a1d09f6cb2f
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54440222"
---
# <a name="whats-new-in-azure-sql-data-warehouse-version---100101060"></a>Wat is er nieuw in versie van Azure SQL Data Warehouse - 10.0.10106.0?
Azure SQL Data Warehouse (SQL DW) wordt continu te verbeteren. Dit artikel beschrijft de nieuwe functies en wijzigingen die zijn geïntroduceerd in SQL DW versie 10.0.10106.0.

## <a name="query-restartability---ctas-and-insertselect"></a>Query Restartability - CTAS en invoegen of te selecteren
In zeldzame gevallen (dat wil zeggen, onregelmatige problemen met de netwerkverbinding, knooppuntfouten)-query's kan uitvoeren in Azure SQL DW mislukken. Meer instructies, zoals actief [CREATE TABLE AS SELECT (CTAS)](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-ctas) en bewerkingen INSERT-selecteren, beter zichtbaar zijn voor dit potentiële probleem. Met deze release implementeert Azure SQL DW logica voor opnieuw proberen voor CTAS en INSERT SELECT-instructies (naast de SELECT-instructies eerder aangekondigd), zodat het systeem voor het afhandelen van deze tijdelijke problemen transparant en zo wordt voorkomen dat query's mislukken. Het aantal nieuwe pogingen en de lijst van tijdelijke fouten verwerkt worden door het systeem is geconfigureerd.

## <a name="return-order-by-optimization"></a>Order By-optimalisatie retourneren
SELECTEREN... ORDER BY-query's krijgen prestaties in deze release.  De engine voor het uitvoeren van query zou eerder, bestellen resultaten op elk rekenknooppunt en kunt streamen naar het beheerknooppunt, die zouden de resultaten vervolgens samenvoegen. Met deze uitbreiding rekenkracht alle knooppunten verzenden in plaats daarvan dat de resultaten naar een enkele rekenknooppunt te selecteren, die vervolgens worden deze samengevoegd en de gesorteerde resultaten worden geretourneerd aan de gebruiker via het rekenknooppunt.  Dit biedt een aanzienlijke prestatieverbetering te bereiken wanneer de queryresultaatset een groot aantal rijen bevat.

## <a name="data-movement-enhancements-for-partitionmove-and-broadcastmove"></a>Gegevens verplaatsen verbeteringen voor PartitionMove en BroadcastMove
In Azure SQL Data Warehouse Gen2 data movement stappen van het type ShuffleMove gebruikmaken van directe gegevens verkeer technieken beschreven in de [prestaties verbeteringen blog hier](https://azure.microsoft.com/blog/lightning-fast-query-performance-with-azure-sql-data-warehouse/).  Met deze release, worden gegevenstypen van verkeer PartitionMove en BroadcastMove nu ook aangestuurd door de dezelfde technieken voor directe gegevensverplaatsing.  Query's van gebruikers die gebruikmaken van deze typen gegevens verkeer stappen ziet prestaties.  Er zijn geen codewijzigingen is vereist om te profiteren van deze hogere prestaties.

## <a name="next-steps"></a>Volgende stappen
Nu u een en ander weet over SQL Data Warehouse, kunt u leren hoe u snel [maken van een SQL Data Warehouse][create a SQL Data Warehouse]. Als u niet bekend bent met Azure, vindt u de [Azure-woordenlijst] [ Azure glossary] handig zijn bij het leren van nieuwe terminologie. U kunt ook enkele andere SQL Data Warehouse-resources bekijken.  

* [Succesverhalen van klanten]
* [Blogs]
* [Functieverzoeken]
* [Video's]
* [Teamblogs met adviezen voor klanten]
* [Stack Overflow-forum]
* [Twitter]


[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Teamblogs met adviezen voor klanten]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Succesverhalen van klanten]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Functieverzoeken]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
