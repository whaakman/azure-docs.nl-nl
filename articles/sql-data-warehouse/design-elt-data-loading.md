---
title: In plaats van ETL, ELT-ontwerp voor Azure SQL Data Warehouse | Microsoft Docs
description: In plaats van ETL, ontwerpen van een Extract, Load en Transform (ELT)-proces voor het laden van gegevens of Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ckarst
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: 33e4a405547fcdd797ddfdf6aba6c6c1c126b742
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2018
ms.locfileid: "42054246"
---
# <a name="designing-extract-load-and-transform-elt-for-azure-sql-data-warehouse"></a>Het ontwerpen van Extract, Load en Transform (ELT) voor Azure SQL datawarehouse

Ontwerp in plaats van extraheren, transformeren en laden (ETL), een Extract, Load en Transform (ELT)-proces voor het laden van gegevens in Azure SQL Data Warehouse. Dit artikel worden manieren voor het ontwerpen van een ELT-proces die gegevens naar een Azure datawarehouse verplaatst geïntroduceerd.

## <a name="what-is-elt"></a>Wat is ELT?

Extraheren, laden en Load en Transform (ELT) is een proces waarmee gegevens worden verplaatst van een bronsysteem naar een bestemming datawarehouse. Dit proces wordt uitgevoerd op een geregelde, bijvoorbeeld elk uur of dagelijks, als u het pas de gegenereerde gegevens in het datawarehouse. De ideale manier om gegevens van bron naar datawarehouse is voor het ontwikkelen van een ELT-proces dat gebruikmaakt van PolyBase om gegevens te laden in SQL Data Warehouse.

ELT wordt eerst zijn geladen en vervolgens de gegevens worden getransformeerd terwijl de gegevens extraheren, transformeren en laden (ETL) worden getransformeerd voordat deze worden geladen. Uitvoeren van ELT in plaats van ETL Hiermee slaat u de kosten van het leveren van uw eigen resources voor het transformeren van de gegevens voordat deze worden geladen. Wanneer u SQL Data Warehouse gebruikt, is ELT maakt gebruik van de MPP-systeem om uit te voeren van de transformaties.

Hoewel er veel variaties voor het implementeren van ELT voor SQL Data Warehouse, zijn dit de eenvoudige stappen:  

1. Pak de brongegevens in tekstbestanden.
2. Land van de gegevens in Azure Blob storage of Azure Data Lake Store.
3. De gegevens worden voorbereid voor het laden.
2. De gegevens laden in SQL Data Warehouse faseringstabellen met behulp van PolyBase.
3. De gegevens te transformeren.
4. De gegevens invoegen in de Productietabellen.


Zie voor een zelfstudie laden [gebruik PolyBase om gegevens te laden vanuit Azure blob storage naar Azure SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md).

Zie voor meer informatie, [het laden van patronen blog](http://blogs.msdn.microsoft.com/sqlcat/2017/05/17/azure-sql-data-warehouse-loading-patterns-and-strategies/). 

## <a name="options-for-loading-with-polybase"></a>Opties voor het laden met PolyBase

PolyBase is een technologie die toegang heeft tot gegevens buiten de database via de T-SQL-taal. Dit is de beste manier om gegevens te laden in SQL Data Warehouse. Dankzij PolyBase kunnen de gegevens zijn geladen parallel uit de gegevensbron rechtstreeks naar de rekenknooppunten. 

Voor het laden van gegevens met PolyBase kunt u een van deze opties laden.

- [PolyBase met T-SQL](load-data-from-azure-blob-storage-using-polybase.md) goed werkt wanneer uw gegevens zich in Azure Blob storage of Azure Data Lake Store. Het biedt u de meeste controle over het laadproces, maar vereist ook dat u voor het definiëren van objecten van de externe gegevens. De andere methoden definieert deze objecten achter de schermen, zoals u brontabellen naar doeltabellen toewijzen.  U kunt Azure Data Factory, SSIS of Azure functions gebruiken voor de organisatie van T-SQL-belastingen. 
- [PolyBase met SSIS](/sql/integration-services/load-data-to-sql-data-warehouse) goed werkt wanneer de brongegevens is in SQL Server, SQL Server on-premises of in de cloud. SSIS definieert de bron naar doel Tabeltoewijzingen, en ook stuurt de belasting. Als u SSIS-pakketten al hebt, kunt u de pakketten om te werken met de nieuwe datawarehouse-doel wijzigen. 
- [PolyBase met Azure Data Factory (ADF)](sql-data-warehouse-load-with-data-factory.md) is een andere orchestration-hulpprogramma.  Definieert een pijplijn en plant taken. 
- [PolyBase met Azure DataBricks](../azure-databricks/databricks-extract-load-sql-data-warehouse.md) gegevens worden overgebracht van een SQL Data Warehouse-tabel naar een Databricks-dataframe en/of gegevens uit een Databricks-dataframe schrijft naar een SQL Data Warehouse-tabel.

### <a name="polybase-external-file-formats"></a>De externe bestandsindelingen PolyBase

PolyBase gegevens laadt in UTF-8- en UTF-16-gecodeerde tekstbestanden. Naast de tekstbestanden met scheidingstekens worden geladen vanuit de Hadoop-bestandsindelingen RC-bestand, ORC en Parquet. PolyBase kan gegevens laden vanuit de Gzip- en Snappy gecomprimeerde bestanden. PolyBase biedt momenteel geen ondersteuning uitgebreide ASCII-indeling vaste breedte en geneste indelingen, zoals WinZip, JSON en XML.

### <a name="non-polybase-loading-options"></a>Opties voor het laden van niet-PolyBase
Als uw gegevens niet compatibel met PolyBase is, kunt u [bcp](/sql/tools/bcp-utility) of de [SqlBulkCopy uitvoert API](https://msdn.microsoft.com/library/system.data.sqlclient.sqlbulkcopy.aspx). BCP wordt rechtstreeks naar SQL Data Warehouse wordt geladen zonder tussenkomst van Azure Blob-opslag, en is alleen bedoeld voor kleine geladen. Opmerking: de prestaties van de belasting van deze opties is aanzienlijk langzamer dan PolyBase. 


## <a name="extract-source-data"></a>Brongegevens extraheren

Ophalen van gegevens uit het bronsysteem, is afhankelijk van de bron.  Het doel is om de gegevens verplaatsen naar tekstbestanden met scheidingstekens. Als u SQL Server gebruikt, kunt u [opdrachtregelprogramma bcp](/sql/tools/bcp-utility) om de gegevens te exporteren.  

## <a name="land-data-to-azure-storage"></a>Gegevens overbrengen naar Azure storage

Als u wilt de gegevens worden neergezet in Azure storage, kunt u deze te verplaatsen [Azure Blob-opslag](../storage/blobs/storage-blobs-introduction.md) of [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md). In één van beide locaties, moeten de gegevens worden opgeslagen in tekstbestanden. Polybase kunt laden vanuit één van beide locaties.

Dit zijn hulpprogramma's en services die u gebruiken kunt om gegevens te verplaatsen naar Azure Storage.

- [Met Azure ExpressRoute](../expressroute/expressroute-introduction.md) service verbetert de netwerkdoorvoer, prestaties en voorspelbaarheid. ExpressRoute is een service die uw gegevens via een speciale persoonlijke verbinding naar Azure. ExpressRoute-verbindingen niet gegevens via het openbare internet gerouteerd. De verbindingen bieden meer betrouwbaarheid, hogere snelheden, kortere wachttijden en hogere beveiliging dan gebruikelijke verbindingen via het openbare internet.
- [AZCopy-hulpprogramma](../storage/common/storage-moving-data.md) gegevens verplaatst naar Azure Storage via het openbare internet. Dit werkt als de grootte van uw gegevens minder dan 10 TB zijn. Om uit te voeren geladen op een geregelde met AZCopy, test u de snelheid van het netwerk om te zien als het is acceptabel. 
- [Azure Data Factory (ADF)](../data-factory/introduction.md) heeft een gateway die u op uw lokale server installeren kunt. Vervolgens maakt u een pijplijn om gegevens te verplaatsen van uw lokale server tot Azure Storage. Zie voor het gebruik van Data Factory met SQL Data Warehouse, [gegevens laden in SQL Data Warehouse](/azure/data-factory/load-azure-sql-data-warehouse).

## <a name="prepare-data"></a>Gegevens voorbereiden

Mogelijk moet u voorbereiden en het opschonen van de gegevens in uw storage-account voordat deze worden geladen in SQL Data Warehouse. Voorbereiden van gegevens kan worden uitgevoerd terwijl uw gegevens zich in de bron als u de gegevens exporteren naar tekstbestanden, of nadat de gegevens zich in Azure Storage.  Het is eenvoudigst te werken met de gegevens zo vroeg in het proces mogelijk.  

### <a name="define-external-tables"></a>Externe tabellen definiëren
Voordat u gegevens laden kunt, moet u voor het definiëren van externe tabellen in uw datawarehouse. PolyBase worden externe tabellen gebruikt om te definiëren en toegang tot de gegevens in Azure Storage. De externe tabel is vergelijkbaar met een gewone tabellen. Het belangrijkste verschil is de externe tabel verwijst naar gegevens die zijn opgeslagen buiten het datawarehouse. 

Omvat het definiëren van externe tabellen op te geven van de gegevensbron, de indeling van de tekstbestanden en tabeldefinities van de. Dit zijn de T-SQL-syntaxis van de onderwerpen die u nodig hebt:
- [EXTERNE GEGEVENSBRON MAKEN](/sql/t-sql/statements/create-external-data-source-transact-sql)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql)
- [EXTERNE TABEL MAKEN](/sql/t-sql/statements/create-external-table-transact-sql)

Zie voor een voorbeeld van het maken van externe objecten, de [externe tabellen maken](load-data-from-azure-blob-storage-using-polybase.md#create-external-tables-for-the-sample-data) stap in de zelfstudie laden.

### <a name="format-text-files"></a>Indeling tekstbestanden

Nadat de externe objecten zijn gedefinieerd, moet u de rijen van de tekstbestanden met de externe tabel- en bestandsindelingdefinitie uitlijnen. De gegevens in elke rij van het tekstbestand moet uitgelijnd met de tabeldefinitie.

De tekst opmaken bestanden:

- Als uw gegevens afkomstig is van een niet-relationele gegevensbron, moet u transformeren in rijen en kolommen. Of de gegevens van een relationele of niet-relationele bron is, moet de gegevens worden getransformeerd zodat deze overeenkomt met de kolomdefinities voor de tabel waarin u van plan bent om de gegevens te laden. 
- Gegevens in het tekstbestand dat moet zijn afgestemd op de kolommen en gegevenstypen in de doeltabel van SQL Data Warehouse-indeling. Uitlijning tussen de gegevenstypen in de externe tekstbestanden en de datawarehouse-tabel zorgt ervoor dat de rijen kunnen worden geweigerd tijdens het laden.
- Afzonderlijke velden in het tekstbestand met een eindteken.  Zorg ervoor dat een teken of een tekenreeks die niet in uw gegevens gevonden is gebruiken. Gebruik de eindteken opgegeven [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql).

## <a name="load-to-a-staging-table"></a>Laden naar een faseringstabel
Als u gegevens in het datawarehouse, werkt het ook voor de eerste keer is geladen de gegevens naar een tijdelijke tabel. Met behulp van een tijdelijke tabel, kunt u fouten verwerken zonder dat de Productietabellen Hierdoor wordt beïnvloed en u te voorkomen dat het terugdraaien van de bewerkingen voor de productietabel. Een tijdelijke tabel biedt u ook de mogelijkheid om te gebruiken van SQL Data Warehouse transformaties uitvoeren voordat de gegevens in de Productietabellen invoegen.

Als u wilt laden met T-SQL, voer de [CREATE TABLE AS SELECT (CTAS)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) T-SQL-instructie. Met deze opdracht voegt de resultaten van een select-instructie in een nieuwe tabel. Als de instructie uit een externe tabel selecteert, worden de externe gegevens geïmporteerd. 

In het volgende voorbeeld, berekende Datum is een externe tabel. Alle rijen worden geïmporteerd in een nieuwe tabel met de naam dbo. De datum.

```sql
CREATE TABLE [dbo].[Date]
WITH
( 
    CLUSTERED COLUMNSTORE INDEX
)
AS SELECT * FROM [ext].[Date]
;
```

## <a name="transform-the-data"></a>De gegevens te transformeren
Terwijl de gegevens zich in de faseringstabel, uitvoeren van transformaties die uw workload. Verplaats de gegevens in een productietabel.

## <a name="insert-data-into-production-table"></a>Gegevens in productietabel invoegen

De INSERT INTO... SELECT-instructie worden de gegevens van de faseringstabel verplaatst naar de permanente tabel. 

Bij het ontwerpen van een ETL-proces, probeert u het proces uitgevoerd op een kleine test-voorbeeld. Probeer het extraheren van 1000 rijen uit de tabel naar een bestand, verplaatsen naar Azure en probeer het in een faseringstabel te laden. 

## <a name="partner-loading-solutions"></a>Het laden van partneroplossingen
Veel van onze partners hebben het laden van oplossingen. Meer informatie vindt u een overzicht van onze [oplossingspartners](sql-data-warehouse-partner-business-intelligence.md). 

## <a name="next-steps"></a>Volgende stappen
Zie voor hulp bij het laden, [richtlijnen voor gegevens laden](guidance-for-loading-data.md).


