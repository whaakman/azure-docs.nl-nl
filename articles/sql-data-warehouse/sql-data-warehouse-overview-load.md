---
title: Gegevens laden in Azure SQL Data Warehouse | Microsoft Docs
description: Meer informatie over de algemene scenario's om gegevens te laden in SQL Data Warehouse. Deze omvatten het gebruik van PolyBase, Azure blob-opslag, platte bestanden en back-ups van schijf. U kunt ook hulpprogramma's van derden gebruiken.
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 2253bf46-cf72-4de7-85ce-f267494d55fa
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: c4199a387f5cdbd477a5e348e48ba8e8b5900075
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="load-data-into-azure-sql-data-warehouse"></a>Gegevens laden in Azure SQL Data Warehouse
Een samenvatting van de scenario-opties en aanbevelingen voor het laden van gegevens in SQL Data Warehouse.

De lastigste onderdeel laden van gegevens is meestal voorbereiden van de gegevens voor de belasting. Azure vereenvoudigt laden door Azure blob-opslag gebruikt als algemene gegevensopslag voor veel van de services en Azure Data Factory voor het indelen van communicatie en gegevens die verkeer tussen de Azure-services. Deze processen worden geïntegreerd met PolyBase-technologie die gegevens parallel uit Azure blob-opslag laden in SQL Data Warehouse met behulp van massively parallelle processing (MPP). 

Zie voor zelfstudies die voorbeelddatabases worden geladen, [laden voorbeelddatabases][Load sample databases].

## <a name="load-from-azure-blob-storage"></a>Laden van Azure blob-opslag
De snelste manier om gegevens te importeren in SQL Data Warehouse wordt met PolyBase gegevens uit Azure blob-opslag laden. PolyBase gebruikt ontwerp van SQL Data Warehouse massively parallelle processing (MPP) om gegevens parallel uit Azure blob-opslag laden. U kunt T-SQL-opdrachten of een Azure Data Factory-pijplijn gebruiken voor het gebruik van PolyBase.

### <a name="1-use-polybase-and-t-sql"></a>1. Gebruik PolyBase en T-SQL
Overzicht van het laadproces:

1. Uw gegevens verplaatsen naar Azure blob-opslag- of Azure Data Lake Store en sla het in tekstbestanden.
2. Externe objecten configureren in SQL Data Warehouse de locatie en de indeling van de gegevens definiëren
3. Voer een T-SQL-opdracht voor het laden van gegevens parallel in een nieuwe databasetabel.

<!-- 5. Schedule and run a loading job. --> 

Zie voor een zelfstudie [gegevens uit Azure blob-opslag laden in SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].

### <a name="2-use-azure-data-factory"></a>2. Azure Data Factory gebruiken
U kunt een Azure Data Factory-pijplijn die gebruikmaakt van PolyBase gegevens uit Azure blob-opslag laden in SQL Data Warehouse maken voor een eenvoudigere manier PolyBase gebruiken. Dit is snel te configureren omdat u hoeft niet te definiëren van de T-SQL-objecten. Als u moet een query uitvoeren op de externe gegevens zonder het worden geïmporteerd, gebruikt u de T-SQL. 

Overzicht van het laadproces:

1. Uw gegevens naar Azure blob-opslag verplaatsen en sla het in tekstbestanden. Azure Data Factory ondersteunt momenteel geen ADLS-connectiviteit met PolyBase).
2. Maak een Azure Data Factory-pijplijn voor het opnemen van de gegevens. De optie PolyBase gebruiken.
4. Plannen en uitvoeren van de pijplijn.

Zie voor een zelfstudie [gegevens uit Azure blob-opslag laden in SQL Data Warehouse (Azure Data Factory)][Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)].

## <a name="load-from-sql-server"></a>Laden van SQL Server
Voor het laden van gegevens uit SQL Server naar SQL Data Warehouse kunt u Integration Services (SSIS), overdracht platte bestanden of schijven naar Microsoft verzenden. Lees bij Zie een samenvatting van de verschillende processen en koppelingen naar zelfstudies laden.

Om te plannen volledige gegevens migreren van SQL Server naar SQL Data Warehouse, Zie de [Migratieoverzicht][Migration overview]. 

### <a name="use-integration-services-ssis"></a>Integratieservices (SSIS) gebruiken
Als u al Integration Services (SSIS) pakketten om te laden in SQL Server gebruikt, kunt u uw pakketten om SQL Server gebruiken als de bron- en SQL Data Warehouse als doel bijwerken. Dit is snel en eenvoudig doen, is een goede keuze als u niet wilt migreren van uw laadproces als gegevens wilt gebruiken al in de cloud. Afweging is dat de belasting zijn lager dan het gebruik van PolyBase omdat deze SSIS worden de belasting niet parallel uitgevoerd.

Overzicht van het laadproces:

1. Wijzig de Integration Services-pakket om te verwijzen naar de SQL Server-exemplaar voor de bron en de SQL Data Warehouse-database voor de bestemming.
2. Migreer uw schema naar SQL Data Warehouse, indien deze nog niet er is.
3. Wijzig de toewijzing in uw pakketten gebruiken alleen de gegevenstypen die worden ondersteund door SQL Data Warehouse.
4. Plannen en uitvoeren van het pakket.

Zie voor een zelfstudie [laden van gegevens uit SQL Server naar Azure SQL Data Warehouse (SSIS)][Load data from SQL Server to Azure SQL Data Warehouse (SSIS)].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>AZCopy (aanbevolen voor < 10 TB gegevens) gebruiken
Als de gegevensgrootte van uw < 10 TB is, kunt u de gegevens uit SQL Server exporteren naar platte bestanden, Kopieer de bestanden naar Azure blob-opslag en gebruik vervolgens PolyBase de gegevens te laden in SQL Data Warehouse

Overzicht van het laadproces:

1. Gebruik het opdrachtregelprogramma bcp om gegevens te exporteren uit SQL Server naar platte bestanden.
2. Gebruik het opdrachtregelhulpprogramma AZCopy gegevens uit platte bestanden kopiëren naar Azure blob-opslag.
3. Gebruik PolyBase in SQL Data Warehouse te laden.

Zie voor een zelfstudie [gegevens uit Azure blob-opslag laden in SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].

### <a name="use-bcp"></a>Gebruikt bcp
Als u een kleine hoeveelheid gegevens hebt, kunt u bcp kunt gebruiken om rechtstreeks in Azure SQL Data Warehouse te laden.

Overzicht van het laadproces:

1. Gebruik het opdrachtregelprogramma bcp om gegevens te exporteren uit SQL Server naar platte bestanden.
2. Gegevens uit platte bestanden direct naar SQL Data Warehouse laden met behulp van bcp.

Zie voor een zelfstudie [laden van gegevens uit SQL Server naar Azure SQL Data Warehouse (bcp)][Load data from SQL Server to Azure SQL Data Warehouse (bcp)].

### <a name="use-importexport-recommended-for--10-tb-data"></a>Gebruik voor importeren/exporteren (aanbevolen voor > 10 TB gegevens)
Als de gegevensomvang van uw > 10 TB is en u wilt verplaatsen naar Azure, raden wij aan dat u ons schijf verzend- [voor importeren/exporteren][Import/Export]. 

Samenvatting van het laadproces

1. Gebruik het opdrachtregelprogramma bcp om gegevens te exporteren uit SQL Server naar platte bestanden op naar schijven.
2. De schijven naar Microsoft verzenden.
3. Microsoft de gegevens worden geladen in SQL Data Warehouse

## <a name="load-from-hdinsight"></a>Laden van HDInsight
Laden van gegevens uit HDInsight met PolyBase biedt ondersteuning voor SQL Data Warehouse. Het proces is hetzelfde als het laden van gegevens uit Azure Blob Storage - verbinding maken met HDInsight om gegevens te laden met behulp van PolyBase. 

### <a name="1-use-polybase-and-t-sql"></a>1. Gebruik PolyBase en T-SQL
Overzicht van het laadproces:

1. Uw gegevens verplaatsen naar HDInsight en sla het in tekstbestanden, ORC of parketvloeren-indeling.
2. Configureer externe objecten in SQL Data Warehouse de locatie en de indeling van de gegevens definiëren.
3. Voer een T-SQL-opdracht voor het laden van gegevens parallel in een nieuwe databasetabel.

Zie voor een zelfstudie [gegevens uit Azure blob-opslag laden in SQL Data Warehouse (PolyBase)][Load data from Azure blob storage to SQL Data Warehouse (PolyBase)].

## <a name="recommendations"></a>Aanbevelingen
Veel van onze partners hebben bij het laden van oplossingen. Meer informatie vindt u een overzicht van onze [oplossingspartners][solution partners]. 

Als uw gegevens afkomstig is van een niet-relationele bron en u laden in SQL Data Warehouse wilt u transformeren in rijen en kolommen moet, voordat u het laden. De getransformeerde gegevens hoeven niet te worden opgeslagen in een database, het kan worden opgeslagen in tekstbestanden.

Statistieken maken voor zojuist geladen gegevens. Azure SQL Data Warehouse bevat nog geen functionaliteit voor het automatisch maken of bijwerken van statistieken.  Om de beste prestaties van uw query's, is het belangrijk dat u statistieken maken voor alle kolommen van alle tabellen nadat de eerste keer worden geladen of belangrijke wijzigingen plaatsvinden in de gegevens.  Zie voor meer informatie [statistieken][Statistics].

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor ontwikkeling, het [overzicht voor ontwikkelaars][development overview].

<!--Image references-->

<!--Article references-->
[Load data from Azure blob storage to SQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Load data from Azure blob storage to SQL Data Warehouse (Azure Data Factory)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Load data from SQL Server to Azure SQL Data Warehouse (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Load data from SQL Server to Azure SQL Data Warehouse (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Load sample databases]: ./sql-data-warehouse-load-sample-databases.md
[Migration overview]: ./sql-data-warehouse-overview-migrate.md
[solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Import/Export]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
