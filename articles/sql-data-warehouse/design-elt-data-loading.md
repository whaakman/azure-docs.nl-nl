---
title: ELT ontwerpen voor Azure SQL datawarehouse | Microsoft Docs
description: "Technologieën voor het verplaatsen van gegevens naar Azure en laden van gegevens in SQL Data Warehouse voor het ontwerpen van een proces extraheren, laden en transformeren (ELT) voor Azure SQL Data Warehouse worden gecombineerd."
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
ms.date: 12/12/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: e94dca69c77c46034e318205279be5188e1371f5
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="designing-extract-load-and-transform-elt-for-azure-sql-data-warehouse"></a>Ontwerpen extraheren, laden en transformeren (ELT) voor Azure SQL datawarehouse

Combineer de technologieën voor landingspagina gegevens in Azure storage en de gegevens in SQL Data Warehouse voor het ontwerpen van een proces extraheren, laden en transformeren (ELT) voor Azure SQL Data Warehouse te laden. Dit artikel bevat de technologieën die ondersteuning voor laden met Polybase en vervolgens is gericht op het ontwerpen van een proces ELT die gebruikmaakt van PolyBase gegevens laadt in SQL Data Warehouse uit Azure Storage met T-SQL.

## <a name="what-is-elt"></a>Wat is ELT?

Extraheren, laden, en transformeren (ELT) is een proces waarmee gegevens worden verplaatst uit een bronsysteem bij een datawarehouse bestemming. Dit proces wordt uitgevoerd op een regelmatige basis, bijvoorbeeld per uur of dagelijks, als u zojuist de gegenereerde gegevens in het datawarehouse. De ideale methode om gegevens ophalen uit de bron met DataWarehouse is voor het ontwikkelen van een proces ELT die gebruikmaakt van PolyBase gegevens laadt in SQL Data Warehouse.

ELT wordt eerst zijn geladen en vervolgens de gegevens worden getransformeerd dat uitpakken, transformeren en Load (ETL) transformeert de gegevens voordat deze worden geladen. Uitvoeren van ELT in plaats van ETL Hiermee slaat u de kosten van het bieden van uw eigen resources voor het transformeren van de gegevens voordat deze is geladen. Wanneer u SQL Data Warehouse, ELT borduurt voort op het systeem MPP de transformaties uitvoeren.

Hoewel er veel variaties voor het implementeren van ELT voor SQL Data Warehouse, zijn dit de eenvoudige stappen:  

1. Het uitpakken van de brongegevens naar tekstbestanden.
2. Vaste de gegevens in Azure Blob storage of Azure Data Lake Store.
3. Bereid de gegevens voor het laden.
2. De gegevens laden in SQL Data Warehouse faseringstabellen met behulp van PolyBase.
3. De gegevens transformeren.
4. De gegevens invoegen in productie tabellen.


Zie voor een zelfstudie laden [gebruik PolyBase om gegevens te laden uit Azure blob storage met Azure SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md).

Zie voor meer informatie [bij het laden van patronen blog](http://blogs.msdn.microsoft.com/sqlcat/2017/05/17/azure-sql-data-warehouse-loading-patterns-and-strategies/). 

## <a name="options-for-loading-with-polybase"></a>Opties voor het laden met PolyBase

PolyBase is een technologie die toegang heeft tot gegevens buiten de database via de T-SQL-taal. Dit is de beste manier om gegevens te laden in SQL Data Warehouse. Met PolyBase, worden de gegevens geladen parallel uit de gegevensbron rechtstreeks aan de rekenknooppunten. 

Om gegevens te laden met PolyBase, kunt u een van deze opties laden.

- [PolyBase met T-SQL](load-data-from-azure-blob-storage-using-polybase.md) goed werkt wanneer uw gegevens in Azure Blob storage of Azure Data Lake Store is. Biedt u de meeste controle over het laadproces, maar vereist dat u objecten van de externe gegevens definiëren. De andere methoden Definieer deze objecten achter de schermen als u brontabellen aan doeltabellen toewijzen.  U kunt Azure Data Factory, SSIS of Azure functions gebruiken voor de organisatie van T-SQL-belastingen. 
- [PolyBase met SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md) goed werkt wanneer de brongegevens is in SQL Server, SQL Server on-premises of in de cloud. SSIS definieert de bron naar doel Tabeltoewijzingen, en ook stuurt de belasting. Als u al SSIS-pakketten hebt, kunt u de pakketten werkt met de nieuwe datawarehouse doel wijzigen. 
- [PolyBase met Azure Data Factory (ADF)](sql-data-warehouse-load-with-data-factory.md) orchestration hulpprogramma.  Definieert een pijplijn en taken plant. 

### <a name="polybase-external-file-formats"></a>PolyBase externe bestandsindelingen

PolyBase gegevens laadt van UTF-8 en UTF-16 gecodeerde tekstbestanden met scheidingstekens. Naast de tekstbestanden met scheidingstekens worden geladen vanuit de Hadoop-bestandsindelingen RC-bestand, ORC en parketvloeren. PolyBase kan gegevens laden vanaf Gzip en treffende gecomprimeerde bestanden. PolyBase ondersteunt momenteel geen uitgebreide ASCII, vaste breedte indeling en geneste indelingen zoals WinZip JSON en XML.

### <a name="non-polybase-loading-options"></a>Laden van niet-PolyBase-opties
Als uw gegevens niet compatibel met PolyBase is, kunt u [bcp](sql-data-warehouse-load-with-bcp.md) of de [SQLBulkCopy API](https://msdn.microsoft.com/library/system.data.sqlclient.sqlbulkcopy.aspx). BCP wordt zonder tussenkomst van Azure Blob-opslag rechtstreeks naar SQL Data Warehouse zijn geladen en is alleen bedoeld voor kleine hoeveelheden. Let op de prestaties van de belasting van deze opties is aanzienlijk langzamer dan PolyBase. 


## <a name="extract-source-data"></a>Brongegevens ophalen

Ophalen van gegevens uit het bronsysteem, is afhankelijk van de bron.  Het doel is de gegevens worden verplaatst naar tekstbestanden met scheidingstekens. Als u SQL Server gebruikt, kunt u [opdrachtregelprogramma bcp](/sql/tools/bcp-utility) om de gegevens te exporteren.  

## <a name="land-data-to-azure-storage"></a>Land gegevens naar Azure-opslag

Als u wilt de gegevens in Azure-opslag land, kunt u deze te verplaatsen [Azure Blob storage](../storage/blobs/storage-blobs-introduction.md) of [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md). In beide locaties, moeten de gegevens worden opgeslagen in tekstbestanden. Polybase kunt laden vanaf de locatie.

Dit zijn de hulpprogramma's en services die u gebruiken kunt om gegevens te verplaatsen naar Azure Storage.

- [Azure ExpressRoute](../expressroute/expressroute-introduction.md) service verhoogt de netwerkdoorvoer, prestaties en een hoge voorspelbaarheid is vereist. ExpressRoute is een service waarmee uw gegevens via een speciale persoonlijke verbinding worden doorgestuurd naar Azure. ExpressRoute-verbindingen niet versturen van gegevens via het openbare internet. De verbindingen bieden meer betrouwbaarheid, sneller en hebben ze lagere latenties en betere beveiliging dan gewone verbindingen via het openbare internet.
- [AZCopy hulpprogramma](../storage/common/storage-use-azcopy.md) gegevens verplaatst naar Azure Storage via het openbare internet. Dit werkt als de grootten van uw gegevens minder dan 10 TB zijn. Test de netwerksnelheid om te zien als het is acceptabel om uit te voeren belasting regelmatig met AZCopy. 
- [Azure Data Factory (ADF)](../data-factory/introduction.md) heeft een gateway die u op uw lokale server installeren kunt. Vervolgens kunt u een pijplijn om gegevens te verplaatsen van de lokale server tot Azure Storage.

Zie voor meer informatie [verplaatsen van gegevens naar en van Azure Storage](../storage/common/storage-moving-data.md)


## <a name="prepare-data"></a>Gegevens voorbereiden

Mogelijk moet u voorbereid en de gegevens in uw opslagaccount opgeschoond voordat deze worden geladen in SQL Data Warehouse. Voorbereiden van gegevens kan worden uitgevoerd terwijl uw gegevens zich in de bron als u de gegevens exporteren naar tekstbestanden, of nadat de gegevens zich in Azure Storage.  Het gemakkelijkst om te werken met de gegevens zo vroeg in het proces mogelijk.  

### <a name="define-external-tables"></a>Externe tabellen definiëren
Voordat u gegevens laden kunt, moet u de externe tabellen definiëren in uw datawarehouse. PolyBase gebruikt externe tabellen voor het definiëren en toegang tot de gegevens in Azure Storage. De externe tabel is vergelijkbaar met een gewone tabellen. Het belangrijkste verschil is de externe tabel verwijst naar gegevens die buiten het datawarehouse zijn opgeslagen. 

Externe tabellen definiëren door de gegevensbron, de indeling van de tekstbestanden en de tabeldefinities op te geven. Dit zijn de T-SQL-syntaxis onderwerpen die u nodig hebt:
- [EXTERNE GEGEVENSBRON MAKEN](/sql/t-sql/statements/create-external-data-source-transact-sql)
- [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql)
- [CREATE EXTERNAL TABLE](/sql/t-sql/statements/create-external-table-transact-sql)

Zie voor een voorbeeld van het maken van externe objecten de [externe tabellen maken](load-data-from-azure-blob-storage-using-polybase.md#create-external-tables-for-the-sample-data) stap in de zelfstudie laden.

### <a name="format-text-files"></a>Tekstbestanden indeling

Nadat de externe objecten zijn gedefinieerd, moet u de rijen van de tekstbestanden met de externe tabel en de bestandsindelingdefinitie uitlijnen. De gegevens in elke rij van het tekstbestand dat moet worden uitgelijnd met de tabeldefinitie.

De tekst bestanden:

- Als uw gegevens afkomstig is van een niet-relationele gegevensbron, moet u transformeren in rijen en kolommen. Ongeacht of de gegevens uit een relationele of niet-relationele gegevensbron, moet de gegevens worden getransformeerd om te worden uitgelijnd met de kolomdefinities voor de tabel waaraan u van plan bent om de gegevens te laden. 
- Gegevens in het tekstbestand uitgelijnd met de kolommen en gegevenstypen in de doeltabel SQL Data Warehouse-indeling. Uitlijning van gegevenstypen tussen gegevenstypen in de externe tekstbestanden en de datawarehouse gegevenstabel zorgt ervoor dat de rijen kunnen worden geweigerd tijdens het laden.
- Afzonderlijke velden in het tekstbestand met een terminator.  Zorg ervoor dat een teken of een tekenreeks die niet is gevonden in de brongegevens gebruiken. Gebruik de terminator die u opgaf met [EXTERNAL FILE FORMAT maken](/sql/t-sql/statements/create-external-file-format-transact-sql).

## <a name="load-to-a-staging-table"></a>Laden naar een tijdelijke tabel
Als u gegevens in het datawarehouse, werkt het uitstekend naar eerst zijn geladen de gegevens in een tijdelijke tabel. Met behulp van een tijdelijke tabel voor het afhandelen van fouten zonder interactie aangaan met de productie-tabellen en u te voorkomen dat de rollback-bewerkingen op de productietabel. Een tijdelijke tabel kunt u bovendien de transformaties uitvoeren voordat de gegevens worden ingevoegd in productie-tabellen met SQL Data Warehouse.

Als u wilt laden met T-SQL, voer de [maken tabel AS selecteren (CTAS)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md) T-SQL-instructie. Deze opdracht voegt de resultaten van een select-instructie in een nieuwe tabel. Als de instructie uit een externe tabel selecteert, worden de externe gegevens geïmporteerd. 

In het volgende voorbeeld toest. Datum is een externe tabel. Alle rijen worden geïmporteerd in een nieuwe tabel met de naam dbo. Datum.

```sql
CREATE TABLE [dbo].[Date]
WITH
( 
    CLUSTERED COLUMNSTORE INDEX
)
AS SELECT * FROM [ext].[Date]
;
```

## <a name="transform-the-data"></a>De gegevens transformeren
Als u gegevens in de faseringstabel, transformaties waarvoor uw werkbelasting uitvoeren. Verplaats de gegevens in een productietabel.

## <a name="insert-data-into-production-table"></a>Gegevens in productietabel invoegen

De INSERT INTO... SELECT-instructie worden de gegevens van de faseringstabel verplaatst naar de permanente tabel. 

Bij het ontwerpen van een ETL-proces voert u het proces van een steekproef kleine test. Probeer 1000 rijen uit de tabel uitpakken naar een bestand, verplaatsen naar Azure en probeer bij het laden naar een tijdelijke tabel. 

## <a name="partner-loading-solutions"></a>Bij het laden van partneroplossingen
Veel van onze partners hebben bij het laden van oplossingen. Meer informatie vindt u een overzicht van onze [oplossingspartners](sql-data-warehouse-partner-business-intelligence.md). 

## <a name="next-steps"></a>Volgende stappen
Zie voor richtlijnen worden geladen, [richtlijnen voor gegevens laden](guidance-for-loading-data.md).


