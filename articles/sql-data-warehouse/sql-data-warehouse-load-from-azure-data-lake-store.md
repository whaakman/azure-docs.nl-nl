---
title: Load - Azure Data Lake Store met SQL datawarehouse | Microsoft Docs
description: Informatie over hoe u gegevens uit Azure Data Lake Store in Azure SQL Data Warehouse laden met PolyBase externe tabellen.
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 12/14/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: a2a7d15eb51374b828d1d641e0e6754115f7aaf6
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="load-data-from-azure-data-lake-store-into-sql-data-warehouse"></a>Gegevens uit Azure Data Lake Store laden in SQL Data Warehouse
Dit document biedt u alle stappen die u wilt gegevens van Azure Data Lake Store (ADLS) in SQL Data Warehouse laden met PolyBase.
Terwijl u zich kunt ad-hoc-query's uitvoeren via de gegevens die zijn opgeslagen in ADLS met behulp van de externe tabellen, als best practice is raadzaam de gegevens importeren in de SQL Data Warehouse.

In deze zelfstudie leert u hoe:

1. Objecten van de externe Database te laden in Azure Data Lake Store maken.
2. Verbinding maken met een Azure Data Lake Store-map.
3. Gegevens laden in Azure SQL Data Warehouse.

## <a name="before-you-begin"></a>Voordat u begint
Deze zelfstudie, hebt u nodig:

* Azure Active Directory-toepassing wilt gebruiken voor verificatie van de Service-naar-Service. Als u wilt maken, volgt u [Active directory-verificatie](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory)

>[!NOTE] 
> U moet de client-ID, de sleutel en de OAuth2.0 Token Endpoint-waarde van uw Active Directory-toepassing verbinding maken met uw Azure Data Lake van SQL Data Warehouse. Details over het ophalen van deze waarden worden in de bovenstaande koppeling.
>Opmerking voor registratie in Azure Active Directory-App gebruiken de ID van het toepassing als de Client-ID.

* Zie SQL Server Management Studio of SQL Server Data Tools voor het downloaden van SSMS en verbinden [Query SSMS](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-query-ssms)

* Een Azure SQL Data Warehouse, voor het maken van een opvolgen: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision _

* Een Azure Data Lake Store, voor het maken van een opvolgen: https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal


###  <a name="create-a-credential"></a>Een referentie maken
Voor toegang tot uw Azure Data Lake Store, moet u een databasehoofdsleutel nodig voor het versleutelen van uw geheime referentie is gebruikt in de volgende stap maakt.
Vervolgens maakt u een databasereferentie binnen het bereik van, die de service principal referenties instellen in AAD opslaat. Noteer de credential-syntaxis verschilt voor mensen die PolyBase verbinding maken met Windows Azure Storage-Blobs hebt gebruikt.
Als u wilt verbinding maken met Azure Data Lake Store, moet u **eerste** een Azure Active Directory-toepassing maken, maakt u een toegangssleutel en de App toegang verlenen tot de Azure Data Lake-resource. Instructies voor het uitvoeren van deze stappen bevinden [hier](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory).

```sql
-- A: Create a Database Master Key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.
-- For more information on Master Key: https://msdn.microsoft.com/en-us/library/ms174382.aspx?f=255&MSPPError=-2147217396

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Pass the client id and OAuth 2.0 Token Endpoint taken from your Azure Active Directory Application
-- SECRET: Provide your AAD Application Service Principal key.
-- For more information on Create Database Scoped Credential: https://msdn.microsoft.com/en-us/library/mt270260.aspx

CREATE DATABASE SCOPED CREDENTIAL ADLCredential
WITH
    IDENTITY = '<client_id>@<OAuth_2.0_Token_EndPoint>',
    SECRET = '<key>'
;

-- It should look something like this:
CREATE DATABASE SCOPED CREDENTIAL ADLCredential
WITH
    IDENTITY = '536540b4-4239-45fe-b9a3-629f97591c0c@https://login.microsoftonline.com/42f988bf-85f1-41af-91ab-2d2cd011da47/oauth2/token',
    SECRET = 'BjdIlmtKp4Fpyh9hIvr8HJlUida/seM5kQ3EpLAmeDI='
;
```


### <a name="create-the-external-data-source"></a>De externe gegevensbron maken
Gebruik deze [externe gegevensbron maken] [ CREATE EXTERNAL DATA SOURCE] opdracht voor het opslaan van de locatie van de gegevens. ADL URI niet vinden in de Azure portal, gaat u naar uw Azure Data Lake Store en zoek vervolgens naar het paneel Essentials.

```sql
-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Store.
-- LOCATION: Provide Azure Data Lake accountname and URI
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH (
    TYPE = HADOOP,
    LOCATION = 'adl://<AzureDataLake account_name>.azuredatalakestore.net',
    CREDENTIAL = ADLCredential
);
```

## <a name="configure-data-format"></a>Indeling van gegevens configureren
Als u wilt de gegevens importeren uit ADLS, moet u de externe bestandsindeling opgeven. Deze opdracht heeft een indeling-specifieke opties om uw gegevens te beschrijven.
Bekijk onze T-SQL-documentatie voor een volledige lijst met [EXTERNAL FILE FORMAT maken][CREATE EXTERNAL FILE FORMAT]

```sql
-- D: Create an external file format
-- FIELD_TERMINATOR: Marks the end of each field (column) in a delimited text file
-- STRING_DELIMITER: Specifies the field terminator for data of type string in the text-delimited file.
-- DATE_FORMAT: Specifies a custom format for all date and time data that might appear in a delimited text file.
-- Use_Type_Default: Store missing values as default for datatype.

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

## <a name="create-the-external-tables"></a>De externe tabellen maken
Nu dat u hebt opgegeven dat de bron- en gegevensindeling, bent u klaar voor het maken van de externe tabellen. Externe tabellen zijn interactie met externe gegevens. De locatieparameter kunt een bestand of map opgeven. Als de opgegeven map, worden alle bestanden in de map wordt geladen.

```sql
-- D: Create an External Table
-- LOCATION: Folder under the ADLS root folder.
-- DATA_SOURCE: Specifies which Data Source Object to use.
-- FILE_FORMAT: Specifies which File Format Object to use
-- REJECT_TYPE: Specifies how you want to deal with rejected rows. Either Value or percentage of the total
-- REJECT_VALUE: Sets the Reject value based on the reject type.

-- DimProduct
CREATE EXTERNAL TABLE [dbo].[DimProduct_external] (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL
)
WITH
(
    LOCATION='/DimProduct/'
,   DATA_SOURCE = AzureDataLakeStore
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

```

## <a name="external-table-considerations"></a>Externe tabel overwegingen
Het maken van een externe tabel is eenvoudig, maar er zijn enkele nuances die moeten worden besproken.

Externe tabellen zijn sterk getypeerd. Dit betekent dat elke rij van de gegevens die wordt ingenomen moet voldoen aan de tabelschemadefinitie.
Als een rij komt niet overeen met de schemadefinitie, wordt de rij van de belasting geweigerd.

De opties voor REJECT_TYPE en REJECT_VALUE kunnen u bepalen hoeveel rijen of welk percentage van de gegevens moet aanwezig zijn in de laatste tabel. Tijdens het laden, als de waarde afwijzen is bereikt, mislukt de belasting. De meest voorkomende oorzaak van geweigerde rijen is een niet-overeenkomend schema definitie. Bijvoorbeeld, als een kolom het schema van int onjuist opgegeven wordt als de gegevens in het bestand een tekenreeks, elke rij niet worden geladen.

 Azure Data Lake store maakt gebruik van rollen gebaseerd toegangsbeheer (RBAC) om toegang tot de gegevens te beheren. Dit betekent dat de Service-Principal hebben voor de mappen die zijn gedefinieerd in de locatieparameter en de onderliggende leden van de laatste map en bestanden leesmachtigingen moet. Hierdoor PolyBase om te verifiëren en te laden die gegevens lezen. 

## <a name="load-the-data"></a>De gegevens laden
Gegevens laden van Azure Data Lake Store-gebruik de [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] instructie. 

CTAS wordt een nieuwe tabel gemaakt en gevuld met de resultaten van een select-instructie. CTAS wordt gedefinieerd in de nieuwe tabel dezelfde kolommen en gegevenstypen hebben als de resultaten van de select-instructie. Als u alle kolommen uit een externe tabel selecteert, is de nieuwe tabel een replica van de kolommen en gegevenstypen in de externe tabel.

In dit voorbeeld maakt er een hash-tabel gedistribueerde DimProduct aangeroepen vanuit de externe tabel DimProduct_external.

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```


## <a name="optimize-columnstore-compression"></a>Compressie columnstore optimaliseren
Standaard worden in de tabel in SQL Data Warehouse opgeslagen als een geclusterde columnstore-index. Nadat een belasting is voltooid, kunnen sommige van de rijen met gegevens niet in de columnstore worden gecomprimeerd.  Er is een aantal redenen waarom dit kan gebeuren. Zie voor meer informatie, [columnstore-indexen beheren][manage columnstore indexes].

Voor het optimaliseren van de prestaties van query's en de compressie columnstore na een werklast in de tabel om af te dwingen de columnstore-index moeten worden gecomprimeerd alle rijen opnieuw worden opgebouwd.

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

Zie voor meer informatie over het onderhouden van de columnstore-indexen, de [columnstore-indexen beheren] [ manage columnstore indexes] artikel.

## <a name="optimize-statistics"></a>Statistieken optimaliseren
Het is raadzaam om te maken van statistieken voor één kolom onmiddellijk na een belasting. Er zijn enkele mogelijkheden voor statistieken. Bijvoorbeeld als u statistieken voor één kolom in elke kolom maakt het mogelijk lang duren voor het opnieuw samenstellen van de statistieken. Als u weet dat bepaalde kolommen niet gaan worden als in query predicaten, kunt u statistieken maken voor deze kolommen overslaan.

Als u besluit te maken van statistieken voor één kolom voor elke kolom van elke tabel, kunt u de voorbeeldcode van de opgeslagen procedure `prc_sqldw_create_stats` in de [statistieken] [ statistics] artikel.

Het volgende voorbeeld is een goed uitgangspunt voor het maken van statistieken. Statistieken voor één kolom wordt gemaakt op elke kolom in de dimensietabel en op elke gekoppelde kolom in de feitentabellen. U kunt altijd statistieken met één of meerdere kolommen naar andere kolommen van de tabel feit later toevoegen.


## <a name="achievement-unlocked"></a>Bereiken ontgrendeld!
U hebt gegevens geladen in Azure SQL Data Warehouse. Taak geweldig!

## <a name="next-steps"></a>Volgende stappen
Laden van gegevens is de eerste stap bij het ontwikkelen van een datawarehouse-oplossing met behulp van SQL Data Warehouse. Bekijk onze ontwikkeling bronnen op [tabellen](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-overview) en [T-SQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-loops.md).


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
[CREATE EXTERNAL DATA SOURCE]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Load the full Contoso Retail Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
