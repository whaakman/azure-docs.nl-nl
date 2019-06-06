---
title: Wat is Azure SQL Data Warehouse? | Microsoft Docs
description: Enterprise-klasse gedistribueerde database die petabytes aan relationele en niet-relationele gegevens kunt verwerken. Het is de bedrijfstak van de eerste cloud datawarehouse met de mogelijkheid om te groeien, verkleind of onderbroken in seconden.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: overview
ms.subservice: design
ms.date: 05/30/2019
ms.author: martinle
ms.reviewer: igorstan
mscustom: sqlfreshmay19
ms.openlocfilehash: a9126e9023091dd8c3df71f2aa2558a01227a8be
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428032"
---
# <a name="what-is-azure-sql-data-warehouse"></a>Wat is Azure SQL Data Warehouse?

SQL Data Warehouse is een cloud-gebaseerde Enterprise Data Warehouse (EDW) die gebruikmaakt van uiterst (Massively Parallel Processing) op snel complexe query's uitvoeren voor petabytes aan gegevens. SQL Data Warehouse gebruiken als belangrijkste onderdeel van een big data-oplossing. Big data importeren in SQL Data Warehouse met eenvoudige [PolyBase](/sql/relational-databases/polybase/polybase-guide?view=sql-server-2017&viewFallbackFrom=azure-sqldw-latest) T-SQL-query's, en gebruik de kracht van MPP te krachtige analyses uitvoeren. Door te integreren en analyseren, wordt Data Warehouse de centrale bron voor het verkrijgen van zakelijke inzichten door uw bedrijf.  

## <a name="key-component-of-big-data-solution"></a>Belangrijk onderdeel van big data-oplossing

SQL Data Warehouse is een belangrijk onderdeel van een end-to-end big data-oplossing in de cloud.

![Data Warehouse-oplossing](media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png) 

In een cloudgegevensoplossing zijn gegevens opgenomen in big data-archieven uit verschillende bronnen. Nadat de gegevens zijn opgenomen in een big data-archief, worden ze met behulp van Hadoop-, Spark- en machine learning-algoritmen voorbereid en getraind. Wanneer de gegevens klaar zijn voor complexe analyse, maakt SQL Data Warehouse gebruik van PolyBase om query's uit te voeren op de big data-archieven. PolyBase gebruikt standaard T-SQL-query's om de gegevens in SQL Data Warehouse op te slaan.
 
In SQL Data Warehouse worden gegevens opgeslagen in relationele tabellen met opslag in kolommen. Deze indeling beperkt de kosten voor gegevensopslag aanzienlijk en verbetert de prestaties van query's. Wanneer de gegevens zijn opgeslagen in SQL Data Warehouse, kunt u op grote schaal analyses uitvoeren. Vergeleken met traditionele databasesystemen duurt het analyseren van query's seconden in plaats van minuten, of uren in plaats van dagen. 

De analyseresultaten kunnen worden verzonden naar wereldwijde rapportagedatabases of toepassingen. Bedrijfsanalisten kunnen vervolgens inzichten verkrijgen om goed gefundeerde zakelijke beslissingen te maken.

## <a name="next-steps"></a>Volgende stappen

- Verken [Azure SQL Data Warehouse-architectuur](/azure/sql-data-warehouse/massively-parallel-processing-mpp-architecture)
- Snel [maken van een SQL Data Warehouse][create a SQL Data Warehouse]
- [Voorbeeldgegevens laden][load sample data].
- Verken [video's](/azure/sql-data-warehouse/sql-data-warehouse-videos)

U kunt ook enkele andere SQL Data Warehouse-resources bekijken.  
* Search [Blogs]
* Dien een [Functieverzoeken]
* Search [Teamblogs met adviezen voor klanten]
* [Een ondersteuningsticket maken]
* Search [MSDN-forum]
* Search [Stack Overflow-forum]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Een ondersteuningsticket maken]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Teamblogs met adviezen voor klanten]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Functieverzoeken]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-forum]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Stack Overflow-forum]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://azure.microsoft.com/support/legal/sla/
