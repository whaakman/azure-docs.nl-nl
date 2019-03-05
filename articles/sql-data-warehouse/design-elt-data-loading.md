---
title: In plaats van ETL, ELT-ontwerp voor Azure SQL Data Warehouse | Microsoft Docs
description: In plaats van ETL, ontwerpen van een Extract, Load en Transform (ELT)-proces voor het laden van gegevens of Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: 96f6da7e081430768b5a6f8fd874e289b8256271
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57308479"
---
# <a name="designing-a-polybase-data-loading-strategy-for-azure-sql-data-warehouse"></a>Het ontwerpen van een strategie laden voor Azure SQL Data Warehouse PolyBase-gegevens

Traditionele SMP-datawarehouses gebruiken een proces extraheren, transformeren en laden (ETL) voor het laden van gegevens. Azure SQL Data Warehouse is een architectuur met krachtige parallelle verwerking (MPP) die profiteren van de schaalbaarheid en flexibiliteit van reken- en opslagresources. Met behulp van een Extract, Load en Transform (ELT)-proces kan profiteren van de MPP en verwijderen van resources die nodig zijn om te zetten van de gegevens voordat ze worden geladen. Hoewel veel methoden voor het laden, inclusief opties voor niet-Polybase zoals BCP en SQL BulkCopy API biedt ondersteuning voor SQL Data Warehouse, is de snelste en meest schaalbare manier datum laden via PolyBase.  PolyBase is een technologie die toegang heeft tot externe gegevens die zijn opgeslagen in Azure Blob storage of Azure Data Lake Store via de T-SQL-taal.

> [!VIDEO https://www.youtube.com/embed/l9-wP7OdhDk]


## <a name="what-is-elt"></a>Wat is ELT?

Extraheren, laden en Load en Transform (ELT) is een proces waarmee gegevens wordt opgehaald uit een bronsysteem, in een datawarehouse geladen en vervolgens getransformeerd. 

De basisstappen voor het implementeren van een ELT PolyBase voor SQL Data Warehouse zijn:

1. Pak de brongegevens in tekstbestanden.
2. Land van de gegevens in Azure Blob storage of Azure Data Lake Store.
3. De gegevens worden voorbereid voor het laden.
4. De gegevens laden in SQL Data Warehouse met PolyBase faseringstabellen. 
5. De gegevens te transformeren.
6. De gegevens invoegen in de Productietabellen.


Zie voor een zelfstudie laden [gebruik PolyBase om gegevens te laden vanuit Azure blob storage naar Azure SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md).

Zie voor meer informatie, [het laden van patronen blog](https://blogs.msdn.microsoft.com/sqlcat/20../../azure-sql-data-warehouse-loading-patterns-and-strategies/). 


## <a name="1-extract-the-source-data-into-text-files"></a>1. De brongegevens extraheren in tekstbestanden

Ophalen van gegevens uit het bronsysteem, is afhankelijk van de opslaglocatie.  Het doel is om te verplaatsen van de gegevens in PolyBase ondersteund tekstbestanden. 

### <a name="polybase-external-file-formats"></a>De externe bestandsindelingen PolyBase

PolyBase gegevens laadt in UTF-8- en UTF-16-gecodeerde tekstbestanden. Naast de tekstbestanden met scheidingstekens worden geladen vanuit de Hadoop-bestandsindelingen RC-bestand, ORC en Parquet. PolyBase kan ook gegevens uit Gzip- en Snappy gecomprimeerde bestanden laden. PolyBase biedt momenteel geen ondersteuning uitgebreide ASCII-indeling vaste breedte en geneste indelingen, zoals WinZip, JSON en XML. Als u van SQL Server exporteert, kunt u [opdrachtregelprogramma bcp](/sql/tools/bcp-utility) en de gegevens worden geëxporteerd in tekstbestanden met scheidingstekens.


## <a name="2-land-the-data-into-azure-blob-storage-or-azure-data-lake-store"></a>2. Land van de gegevens in Azure Blob storage of Azure Data Lake Store

Als u wilt de gegevens worden neergezet in Azure storage, kunt u deze te verplaatsen [Azure Blob-opslag](../storage/blobs/storage-blobs-introduction.md) of [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md). In één van beide locaties, moet de gegevens worden opgeslagen in de tekstbestanden. PolyBase kunt laden vanuit één van beide locaties.

Hulpprogramma's en services die u gebruiken kunt om gegevens te verplaatsen naar Azure Storage:

- [Met Azure ExpressRoute](../expressroute/expressroute-introduction.md) service verbetert de netwerkdoorvoer, prestaties en voorspelbaarheid. ExpressRoute is een service die uw gegevens via een speciale persoonlijke verbinding naar Azure. ExpressRoute-verbindingen niet gegevens via het openbare internet gerouteerd. De verbindingen bieden meer betrouwbaarheid, hogere snelheden, kortere wachttijden en hogere beveiliging dan gebruikelijke verbindingen via het openbare internet.
- [AZCopy-hulpprogramma](../storage/common/storage-moving-data.md) gegevens verplaatst naar Azure Storage via het openbare internet. Dit werkt als de grootte van uw gegevens minder dan 10 TB zijn. Om uit te voeren geladen op een geregelde met AZCopy, test u de snelheid van het netwerk om te zien als het is acceptabel. 
- [Azure Data Factory (ADF)](../data-factory/introduction.md) heeft een gateway die u op uw lokale server installeren kunt. Vervolgens maakt u een pijplijn om gegevens te verplaatsen van uw lokale server tot Azure Storage. Zie voor het gebruik van Data Factory met SQL Data Warehouse, [gegevens laden in SQL Data Warehouse](/azure/data-factory/load-azure-sql-data-warehouse).


## <a name="3-prepare-the-data-for-loading"></a>3. De gegevens worden voorbereid voor het laden

Mogelijk moet u voorbereiden en het opschonen van de gegevens in uw storage-account voordat deze worden geladen in SQL Data Warehouse. Voorbereiden van gegevens kan worden uitgevoerd terwijl uw gegevens zich in de bron als u de gegevens exporteren naar tekstbestanden, of nadat de gegevens zich in Azure Storage.  Het is eenvoudigst te werken met de gegevens zo vroeg in het proces mogelijk.  

### <a name="define-external-tables"></a>Externe tabellen definiëren

Voordat u gegevens laden kunt, moet u voor het definiëren van externe tabellen in uw datawarehouse. PolyBase worden externe tabellen gebruikt om te definiëren en toegang tot de gegevens in Azure Storage. Een externe tabel is vergelijkbaar met de weergave van een database. De externe tabel bevat het tabelschema en verwijst naar gegevens die zijn opgeslagen buiten het datawarehouse. 

Omvat het definiëren van externe tabellen op te geven van de gegevensbron, de indeling van de tekstbestanden en tabeldefinities van de. Dit zijn de T-SQL-syntaxis van de onderwerpen die u nodig hebt:
- [EXTERNE GEGEVENSBRON MAKEN](/sql/t-sql/statements/create-external-data-source-transact-sql)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql)
- [CREATE EXTERNAL TABLE](/sql/t-sql/statements/create-external-table-transact-sql)

Zie voor een voorbeeld van het maken van externe objecten, de [externe tabellen maken](load-data-from-azure-blob-storage-using-polybase.md#create-external-tables-for-the-sample-data) stap in de zelfstudie laden.

### <a name="format-text-files"></a>Indeling tekstbestanden

Nadat de externe objecten zijn gedefinieerd, moet u de rijen van de tekstbestanden met de externe tabel- en bestandsindelingdefinitie uitlijnen. De gegevens in elke rij van het tekstbestand moet uitgelijnd met de tabeldefinitie.
De tekst opmaken bestanden:

- Als uw gegevens afkomstig is van een niet-relationele gegevensbron, moet u transformeren in rijen en kolommen. Of de gegevens van een relationele of niet-relationele bron is, moet de gegevens worden getransformeerd zodat deze overeenkomt met de kolomdefinities voor de tabel waarin u van plan bent om de gegevens te laden. 
- Gegevens in het tekstbestand dat moet zijn afgestemd op de kolommen en gegevenstypen in de doeltabel van SQL Data Warehouse-indeling. Uitlijning tussen de gegevenstypen in de externe tekstbestanden en de datawarehouse-tabel zorgt ervoor dat de rijen kunnen worden geweigerd tijdens het laden.
- Afzonderlijke velden in het tekstbestand met een eindteken.  Zorg ervoor dat een teken of een tekenreeks die niet in uw gegevens gevonden is gebruiken. Gebruik de eindteken opgegeven [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql).


## <a name="4-load-the-data-into-sql-data-warehouse-staging-tables-using-polybase"></a>4. De gegevens laden in SQL Data Warehouse met PolyBase faseringstabellen

Het is best practice om gegevens te laden naar een tijdelijke tabel. Tijdelijke tabellen kunnen u voor het afhandelen van fouten zonder dat de Productietabellen Hierdoor wordt beïnvloed. Een tijdelijke tabel biedt u ook de mogelijkheid te gebruiken van SQL Data Warehouse MPP voor gegevenstransformaties voordat de gegevens in de Productietabellen invoegen.

### <a name="options-for-loading-with-polybase"></a>Opties voor het laden met PolyBase

Voor het laden van gegevens met PolyBase kunt u een van deze opties voor het laden gebruiken:

- [PolyBase met T-SQL](load-data-from-azure-blob-storage-using-polybase.md) goed werkt wanneer uw gegevens zich in Azure Blob storage of Azure Data Lake Store. Het biedt u de meeste controle over het laadproces, maar vereist ook dat u voor het definiëren van objecten van de externe gegevens. De andere methoden definieert deze objecten achter de schermen, zoals u brontabellen naar doeltabellen toewijzen.  U kunt Azure Data Factory, SSIS of Azure functions gebruiken voor de organisatie van T-SQL-belastingen. 
- [PolyBase met SSIS](/sql/integration-services/load-data-to-sql-data-warehouse) goed werkt wanneer de brongegevens is in SQL Server, SQL Server on-premises of in de cloud. SSIS definieert de bron naar doel Tabeltoewijzingen, en ook stuurt de belasting. Als u SSIS-pakketten al hebt, kunt u de pakketten om te werken met de nieuwe datawarehouse-doel wijzigen. 
- [PolyBase met Azure Data Factory (ADF)](sql-data-warehouse-load-with-data-factory.md) is een andere orchestration-hulpprogramma.  Definieert een pijplijn en plant taken. 
- [PolyBase met Azure DataBricks](../azure-databricks/databricks-extract-load-sql-data-warehouse.md) gegevens worden overgebracht van een SQL Data Warehouse-tabel naar een Databricks-dataframe en/of gegevens uit een Databricks-dataframe schrijft naar een SQL Data Warehouse-tabel.

### <a name="non-polybase-loading-options"></a>Opties voor het laden van niet-PolyBase

Als uw gegevens niet compatibel met PolyBase is, kunt u [bcp](/sql/tools/bcp-utility) of de [SqlBulkCopy uitvoert API](https://msdn.microsoft.com/library/system.data.sqlclient.sqlbulkcopy.aspx). BCP wordt rechtstreeks naar SQL Data Warehouse wordt geladen zonder tussenkomst van Azure Blob-opslag, en is alleen bedoeld voor kleine geladen. Opmerking: de prestaties van de belasting van deze opties is aanzienlijk langzamer dan PolyBase. 


## <a name="5-transform-the-data"></a>5. De gegevens te transformeren

Terwijl de gegevens zich in de faseringstabel, uitvoeren van transformaties die uw workload. Verplaats de gegevens in een productietabel.


## <a name="6-insert-the-data-into-production-tables"></a>6. De gegevens in de Productietabellen invoegen

De INSERT INTO... SELECT-instructie worden de gegevens van de faseringstabel verplaatst naar de permanente tabel. 

Bij het ontwerpen van een ETL-proces, probeert u het proces uitgevoerd op een kleine test-voorbeeld. Probeer het extraheren van 1000 rijen uit de tabel naar een bestand, verplaatsen naar Azure en probeer het in een faseringstabel te laden. 


## <a name="partner-loading-solutions"></a>Het laden van partneroplossingen

Veel van onze partners hebben het laden van oplossingen. Meer informatie vindt u een overzicht van onze [oplossingspartners](sql-data-warehouse-partner-business-intelligence.md). 


## <a name="next-steps"></a>Volgende stappen

Zie voor hulp bij het laden, [richtlijnen voor gegevens laden](guidance-for-loading-data.md).


