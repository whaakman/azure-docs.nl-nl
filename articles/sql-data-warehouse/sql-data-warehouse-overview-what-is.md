---
title: Wat is Azure SQL Data Warehouse? | Microsoft Docs
description: Gedistribueerde database op ondernemingsniveau die geschikt is voor het verwerken van petabytes aan relationele en niet-relationele gegevens. Het is het eerste geavanceerde clouddatawarehouse in de branche dat in enkele seconden kan worden vergroot, verkleind en onderbroken.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jenniehubbard
editor: 
ms.assetid: 4006c201-ec71-4982-b8ba-24bba879d7bb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: overview
ms.date: 10/23/2017
ms.author: jrj
ms.openlocfilehash: 7fd17c29c413905084c6b3736018d111d3b28fea
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="what-is-azure-sql-data-warehouse"></a>Wat is Azure SQL Data Warehouse?

SQL Data Warehouse is een op de cloud gebaseerde EDW (Enterprise Data Warehouse) die MPP (Massively Parallel Processing) gebruikt om snel complexe query's uit te voeren in petabytes aan gegevens. SQL Data Warehouse gebruiken als belangrijkste onderdeel van een big data-oplossing. Big data importeren in SQL Data Warehouse met eenvoudige PolyBase T-SQL-query's en vervolgens de kracht van MPP gebruiken om analyses met hoge prestaties uit te voeren. Door te integreren en analyseren, wordt Data Warehouse de centrale bron voor het verkrijgen van zakelijke inzichten door uw bedrijf.  


## <a name="key-component-of-big-data-solution"></a>Belangrijk onderdeel van big data-oplossing
SQL Data Warehouse is een belangrijk onderdeel van een end-to-end big data-oplossing in de cloud.

![Data Warehouse-oplossing](media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png) 

In een cloudgegevensoplossing zijn gegevens opgenomen in big data-archieven uit verschillende bronnen. Nadat de gegevens zijn opgenomen in een big data-archief, worden ze met behulp van Hadoop-, Spark- en machine learning-algoritmen voorbereid en getraind. Wanneer de gegevens klaar zijn voor complexe analyse, maakt SQL Data Warehouse gebruik van PolyBase om query's uit te voeren op de big data-archieven. PolyBase gebruikt standaard T-SQL-query's om de gegevens in SQL Data Warehouse op te slaan.
 
In SQL Data Warehouse worden gegevens opgeslagen in relationele tabellen met opslag in kolommen. Deze indeling beperkt de kosten voor gegevensopslag aanzienlijk en verbetert de prestaties van query's. Wanneer de gegevens zijn opgeslagen in SQL Data Warehouse, kunt u op grote schaal analyses uitvoeren. Vergeleken met traditionele databasesystemen duurt het analyseren van query's seconden in plaats van minuten, of uren in plaats van dagen. 

De analyseresultaten kunnen worden verzonden naar wereldwijde rapportagedatabases of toepassingen. Bedrijfsanalisten kunnen vervolgens inzichten verkrijgen om goed gefundeerde zakelijke beslissingen te maken.

## <a name="optimization-choices"></a>Keuzen voor optimalisatie

SQL Data Warehouse biedt [prestatielagen](performance-tiers.md) die zijn ontworpen voor flexibiliteit, om te voldoen aan uw gegevensbehoeften, of het nu om veel of om weinig gegevens gaat. U kunt een datawarehouse kiezen dat is geoptimaliseerd voor elasticiteit of voor rekenkracht. 

- In de prestatielaag **Geoptimaliseerd voor elasticiteit** worden de reken- en opslaglaag in de architectuur gescheiden. Deze optie werkt uitstekend bij werkbelastingen die optimaal profiteren van de scheiding tussen berekening en opslag, door regelmatig te schalen ter ondersteuning van korte perioden van piekactiviteit. Deze rekenlaag heeft de laagste toegangspuntprijs en kan worden geschaald ter ondersteuning van het merendeel van de klantwerkbelastingen.

- De prestatielaag **Geoptimaliseerd voor berekenen** maakt gebruik van de nieuwste Azure-hardware om een nieuwe NVMe Solid State Disk-cache te introduceren waarin de meestgebruikte gegevens dicht bij de CPU's worden opgeslagen, precies waar u deze wilt hebben. Doordat de opslag automatisch in lagen wordt verdeeld, is deze prestatielaag uitstekend geschikt voor complexe query's, omdat alle IO lokaal wordt opgeslagen in de rekenlaag. Bovendien wordt de kolomopslag uitgebreid voor het opslaan van een onbeperkte hoeveelheid gegevens in uw SQL-datawarehouse. De prestatielaag Geoptimaliseerd voor berekenen biedt het hoogste schaalbaarheidsniveau, zodat u maximaal 30.000 cDWU's (compute Data Warehouse Units) kunt schalen. Kies deze laag voor werkbelastingen waarvoor continue, zeer snelle prestaties zijn vereist.

## <a name="next-steps"></a>Volgende stappen
Nu u een en ander weet over SQL Data Warehouse, kunt u leren hoe u snel [een SQL Data Warehouse maakt][create a SQL Data Warehouse] en [voorbeeldgegevens laadt][load sample data]. Als u niet bekend bent met Azure, kan de [Azure-woordenlijst][Azure glossary] handig zijn bij het opzoeken van nieuwe terminologie. U kunt ook enkele andere SQL Data Warehouse-resources bekijken.  

* [Succesverhalen van klanten]
* [Blogs]
* [Functieverzoeken]
* [Video's]
* [Teamblogs met adviezen voor klanten]
* [Ondersteuningsticket maken]
* [MSDN-forum]
* [Stack Overflow-forum]
* [Twitter]

<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Ondersteuningsticket maken]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Succesverhalen van klanten]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Teamblogs met adviezen voor klanten]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Functieverzoeken]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-forum]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Stack Overflow-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://azure.microsoft.com/en-us/support/legal/sla/
