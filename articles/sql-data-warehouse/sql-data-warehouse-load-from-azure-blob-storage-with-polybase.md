---
title: Laden van Azure blob met Azure datawarehouse | Microsoft Docs
description: Informatie over hoe u gegevens uit Azure blob-opslag laden in SQL Data Warehouse met PolyBase. Enkele tabellen uit openbare gegevens laden in het datawarehouse van Contoso Retail-schema.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: barbkess
editor: 
ms.assetid: faca0fe7-62e7-4e1f-a86f-032b4ffcb06e
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: barbkess
ms.openlocfilehash: 4221bcd5a50fad680427a500e32837c1e75dd990
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a>Gegevens uit Azure blob-opslag laden in SQL Data Warehouse (PolyBase)
> [!div class="op_single_selector"]
> * [Data Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
> * [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
> 
> 

PolyBase en T-SQL-opdrachten gebruiken voor gegevens uit Azure blob-opslag laden in Azure SQL Data Warehouse. 

Deze zelfstudie wordt in het schema Contoso Retail Data Warehouse het gemak houden de twee tabellen van een openbare Azure Storage-Blob geladen. Het voorbeeld uitvoert voor het laden van de volledige gegevensset, [volledige Contoso Retail Data Warehouse laden] [ Load the full Contoso Retail Data Warehouse] uit de opslagplaats voor Microsoft SQL Server-voorbeelden.

In deze zelfstudie wordt u:

1. PolyBase laden uit Azure blob-opslag configureren
2. Openbare gegevens laden in de database
3. Optimalisaties uitvoeren nadat de belasting is voltooid.

## <a name="before-you-begin"></a>Voordat u begint
Als u wilt uitvoeren in deze zelfstudie, moet u een Azure-account die al een SQL Data Warehouse-database. Als u dit nog geen hebt, raadpleegt u [maken van een SQL Data Warehouse][Create a SQL Data Warehouse].

## <a name="1-configure-the-data-source"></a>1. Configureer de gegevensbron
PolyBase gebruikt externe T-SQL-objecten voor het definiëren van de locatie en de kenmerken van de externe gegevens. De externe objectdefinities worden opgeslagen in SQL Data Warehouse. De gegevens zelf is die extern zijn opgeslagen.

### <a name="11-create-a-credential"></a>1.1. Een referentie maken
**Deze stap overslaan** als u de openbare Contoso-gegevens worden geladen. U hoeft niet op veilige toegang tot openbare gegevens omdat die al toegankelijk voor iedereen.

**Deze stap niet overslaan** als u deze zelfstudie als een sjabloon voor het laden van uw eigen gegevens. Toegang tot gegevens via een referentie op die het volgende script gebruiken voor het maken van een database-scoped referentie en vervolgens worden gebruikt bij het definiëren van de locatie van de gegevensbron.

```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);
```

Verder met stap 2.

### <a name="12-create-the-external-data-source"></a>1.2. De externe gegevensbron maken
Gebruik deze [externe gegevensbron maken] [ CREATE EXTERNAL DATA SOURCE] opdracht voor het opslaan van de locatie van de gegevens en het type gegevens. 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [!IMPORTANT]
> Als u uw azure blob storage-containers om openbaar te maken kiest, houd er rekening mee dat als eigenaar van de gegevens u gefactureerd voor gegevens kosten voor uitgaande wanneer gegevens het datacenter verlaat. 
> 
> 

## <a name="2-configure-data-format"></a>2. Indeling van gegevens configureren
De gegevens worden opgeslagen in tekstbestanden in Azure blob-opslag en elk veld wordt gescheiden met een scheidingsteken. Voer deze [EXTERNAL FILE FORMAT maken] [ CREATE EXTERNAL FILE FORMAT] opdracht voor het opgeven van de indeling van de gegevens in de tekstbestanden. De Contoso-gegevens niet-gecomprimeerde en pipe gescheiden.

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH 
(   FORMAT_TYPE = DELIMITEDTEXT
,    FORMAT_OPTIONS    (   FIELD_TERMINATOR = '|'
                    ,    STRING_DELIMITER = ''
                    ,    DATE_FORMAT         = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,    USE_TYPE_DEFAULT = FALSE 
                    )
);
``` 

## <a name="3-create-the-external-tables"></a>3. De externe tabellen maken
Nu dat u hebt opgegeven dat de bron- en gegevensindeling, bent u klaar voor het maken van de externe tabellen. 

### <a name="31-create-a-schema-for-the-data"></a>3.1. Maak een schema voor de gegevens.
Maak een schema voor het maken van een plaats voor het opslaan van de Contoso-gegevens in uw database.

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-the-external-tables"></a>3.2. Maak de externe tabellen.
Voer dit script voor het maken van de DimProduct en FactOnlineSales externe tabellen. Alle we hier doen is kolomnamen en gegevenstypen definiëren, en ze te binden aan de locatie en de indeling van de Azure blob-opslag-bestanden. De definitie is opgeslagen in SQL Data Warehouse en de gegevens nog in de Azure Storage-Blob.

De **locatie** parameter is de map onder de hoofdmap in de Azure Storage-Blob. Elke tabel is in een andere map.

```sql

--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="4-load-the-data"></a>4. De gegevens laden
Er is een verschillende manieren om toegang te krijgen tot externe gegevens.  U kunt een query over gegevens rechtstreeks vanuit de externe tabel, de gegevens in de nieuwe databasetabellen laden of externe gegevens toevoegen aan bestaande databasetabellen.  

### <a name="41-create-a-new-schema"></a>4.1. Een nieuw schema maken
CTAS maakt een nieuwe tabel die gegevens bevat.  Maak eerst een schema voor de contoso-gegevens.

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-the-data-into-new-tables"></a>4.2. De gegevens in de nieuwe tabellen laden
Gebruiken om gegevens uit Azure blob-opslag laden en opslaan in een tabel in de database, de [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] instructie. Laden met CTAS maakt gebruik van de sterk getypeerde externe tabellen die u zojuist hebt gemaakt. Als u wilt de gegevens in de nieuwe tabellen laden, gebruikt u een [CTAS] [ CTAS] instructie per tabel. 
 
CTAS wordt een nieuwe tabel gemaakt en gevuld met de resultaten van een select-instructie. CTAS wordt gedefinieerd in de nieuwe tabel dezelfde kolommen en gegevenstypen hebben als de resultaten van de select-instructie. Als u alle kolommen uit een externe tabel selecteert, worden de nieuwe tabel een replica van de kolommen en gegevenstypen in de externe tabel.

We maken als u de dimensie en de feitentabel als hash-gedistribueerde tabellen in dit voorbeeld. 

```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-the-load-progress"></a>4.3 de load-voortgang bijhouden
U kunt de voortgang van uw belasting met dynamische beheerweergaven (DMV's) te volgen. 

```sql
-- To see all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- To see a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="5-optimize-columnstore-compression"></a>5. Compressie columnstore optimaliseren
Standaard worden in de tabel in SQL Data Warehouse opgeslagen als een geclusterde columnstore-index. Nadat een belasting is voltooid, kunnen sommige van de rijen met gegevens niet in de columnstore worden gecomprimeerd.  Er is een aantal redenen waarom dit kan gebeuren. Zie voor meer informatie, [columnstore-indexen beheren][manage columnstore indexes].

Voor het optimaliseren van de prestaties van query's en de compressie columnstore na een werklast in de tabel om af te dwingen de columnstore-index moeten worden gecomprimeerd alle rijen opnieuw worden opgebouwd. 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

Zie voor meer informatie over het onderhouden van de columnstore-indexen, de [columnstore-indexen beheren] [ manage columnstore indexes] artikel.

## <a name="6-optimize-statistics"></a>6. Statistieken optimaliseren
Het is raadzaam om te maken van statistieken voor één kolom onmiddellijk na een belasting. Er zijn enkele mogelijkheden voor statistieken. Bijvoorbeeld als u statistieken voor één kolom in elke kolom maakt het mogelijk lang duren voor het opnieuw samenstellen van de statistieken. Als u weet dat bepaalde kolommen niet gaan worden als in query predicaten, kunt u statistieken maken voor deze kolommen overslaan.

Als u besluit te maken van statistieken voor één kolom voor elke kolom van elke tabel, kunt u de voorbeeldcode van de opgeslagen procedure `prc_sqldw_create_stats` in de [statistieken] [ statistics] artikel.

Het volgende voorbeeld is een goed uitgangspunt voor het maken van statistieken. Statistieken voor één kolom wordt gemaakt op elke kolom in de dimensietabel en op elke gekoppelde kolom in de feitentabellen. U kunt altijd statistieken met één of meerdere kolommen naar andere kolommen van de tabel feit later toevoegen.

```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a>Bereiken ontgrendeld!
Openbare gegevens is in Azure SQL Data Warehouse geladen. Taak geweldig!

U kunt nu starten opvragen van de tabellen met behulp van query's als volgt:

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a>Volgende stappen
De volledige Contoso Retail-datawarehouse om gegevens te laden, gebruikt u het script in voor meer tips voor ontwikkeling, Zie [overzicht van SQL Data Warehouse voor ontwikkelaars][SQL Data Warehouse development overview].

<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[manage columnstore indexes]: sql-data-warehouse-tables-index.md
[Statistics]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[CREATE EXTERNAL DATA SOURCE]: https://msdn.microsoft.com/en-us/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/en-us/library/dn935026.aspx
[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Load the full Contoso Retail Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
