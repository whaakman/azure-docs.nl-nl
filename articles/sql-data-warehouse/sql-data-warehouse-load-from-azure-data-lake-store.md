---
title: 'Zelfstudie: Laden uit Azure Data Lake Storage Gen1 met Azure SQL datawarehouse | Microsoft Docs'
description: Externe tabellen PolyBase gebruiken om gegevens te laden uit Azure Data Lake Storage Gen1 in Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: 32ac5b0841365acfc0a52e343eafc4f3760dffaa
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55472221"
---
# <a name="load-data-from-azure-data-lake-storage-gen1-to-sql-data-warehouse"></a>Gegevens uit Azure Data Lake Storage Gen1 laden in SQL Data Warehouse
Externe tabellen PolyBase gebruiken om gegevens te laden uit Azure Data Lake Storage Gen1 in Azure SQL Data Warehouse. Hoewel u ad-hoc-query's op gegevens die zijn opgeslagen in Data Lake Storage Gen1 uitvoeren kunt, wordt u aangeraden de gegevens worden geïmporteerd in de SQL Data Warehouse voor de beste prestaties.

> [!div class="checklist"]
> * Databaseobjecten die zijn vereist om te laden uit Data Lake Storage Gen1 maken.
> * Verbinding maken met een Data Lake Storage Gen1-map.
> * Gegevens laden in Azure SQL Data Warehouse.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="before-you-begin"></a>Voordat u begint
Download en installeer voordat u met deze zelfstudie begint de nieuwste versie van [SSMS](/sql/ssms/download-sql-server-management-studio-ssms) (SQL Server Management Studio).

Als u wilt uitvoeren in deze zelfstudie, hebt u het volgende nodig:

* Azure Active Directory-toepassing moet worden gebruikt voor Service-naar-serviceverificatie. Als u wilt maken, gaat u als volgt [Active directory-verificatie](../data-lake-store/data-lake-store-authenticate-using-active-directory.md)

>[!NOTE] 
> U moet de client-ID, de sleutel en de OAuth 2.0 Token Endpoint-waarde van uw Active Directory-toepassing verbinding maken met uw Gen1 van Data Lake Storage-account van SQL Data Warehouse. Details voor informatie over het ophalen van deze waarden zijn in de bovenstaande koppeling. Voor registratie in Azure Active Directory-App kunt u de toepassings-ID gebruiken als de Client-ID.
> 

* An Azure SQL Data Warehouse. Zie [maken en query's en Azure SQL Data Warehouse](create-data-warehouse-portal.md).

* Een Data Lake Storage Gen1-account. Zie [aan de slag met Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-get-started-portal.md). 

##  <a name="create-a-credential"></a>Een referentie maken
Voor toegang tot uw Data Lake Storage Gen1-account, moet u een databasehoofdsleutel nodig voor het versleutelen van uw referentiegeheim dat is gebruikt in de volgende stap maakt. Vervolgens maakt u een Database Scoped Credential, die de service principal-referenties instellen in AAD opslaat. Houd er rekening mee dat de syntaxis van de referentie anders is bedoeld voor degenen die PolyBase verbinding maken met Windows Azure Storage-Blobs.

Als u wilt verbinding maken met Data Lake Storage Gen1, moet u **eerste** maken van een Azure Active Directory-toepassing, maak een toegangstoets en de App toegang verlenen tot de Data Lake Storage Gen1-resource. Zie voor instructies [verifiëren bij Azure Data Lake Storage Gen1 met behulp van Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).

```sql
-- A: Create a Database Master Key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.
-- For more information on Master Key: https://msdn.microsoft.com/library/ms174382.aspx?f=255&MSPPError=-2147217396

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Pass the client id and OAuth 2.0 Token Endpoint taken from your Azure Active Directory Application
-- SECRET: Provide your AAD Application Service Principal key.
-- For more information on Create Database Scoped Credential: https://msdn.microsoft.com/library/mt270260.aspx

CREATE DATABASE SCOPED CREDENTIAL ADLSG1Credential
WITH
    IDENTITY = '<client_id>@<OAuth_2.0_Token_EndPoint>',
    SECRET = '<key>'
;

-- It should look something like this:
CREATE DATABASE SCOPED CREDENTIAL ADLSG1Credential
WITH
    IDENTITY = '536540b4-4239-45fe-b9a3-629f97591c0c@https://login.microsoftonline.com/42f988bf-85f1-41af-91ab-2d2cd011da47/oauth2/token',
    SECRET = 'BjdIlmtKp4Fpyh9hIvr8HJlUida/seM5kQ3EpLAmeDI='
;
```

## <a name="create-the-external-data-source"></a>De externe gegevensbron maken
Gebruik deze [CREATE EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql) opdracht voor het opslaan van de locatie van de gegevens. 

```sql
-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Storage Gen1.
-- LOCATION: Provide Data Lake Storage Gen1 account name and URI
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStorageGen1
WITH (
    TYPE = HADOOP,
    LOCATION = 'adl://<datalakestoregen1accountname>.azuredatalakestore.net',
    CREDENTIAL = ADLSG1Credential
);
```

## <a name="configure-data-format"></a>Indeling van gegevens configureren
Als u wilt de gegevens importeren uit Data Lake Storage Gen1, moet u opgeven van de externe bestandsindeling. Dit object wordt gedefinieerd hoe de bestanden in Data Lake Storage Gen1 worden geschreven.
Bekijk onze T-SQL-documentatie voor de volledige lijst [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql)

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
Nu dat u de gegevensindeling voor bron- en -bestand hebt opgegeven, bent u klaar om de externe tabellen te maken. Externe tabellen zijn interactie met externe gegevens. De locatieparameter kunt opgeven voor een bestand of map. Als er een map is opgegeven, worden alle bestanden in de map worden geladen.

```sql
-- D: Create an External Table
-- LOCATION: Folder under the Data Lake Storage Gen1 root folder.
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
,   DATA_SOURCE = AzureDataLakeStorageGen1
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

```

## <a name="external-table-considerations"></a>Externe tabel overwegingen
Het maken van een externe tabel is eenvoudig, maar er zijn enkele aspecten die moeten worden besproken.

Externe tabellen zijn sterk getypeerd. Dit betekent dat elke rij van de gegevens die wordt opgenomen, moet voldoen aan de schemadefinitie voor de tabel.
Als een rij komt niet overeen met de schemadefinitie, wordt de rij van de belasting geweigerd.

De opties voor REJECT_TYPE en REJECT_VALUE kunnen u bepalen hoeveel rijen of welk percentage van de gegevens moet aanwezig zijn in de laatste tabel. Tijdens het laden, als de waarde afwijzen is bereikt, het laden is mislukt. De meest voorkomende oorzaak van geweigerde rijen is een niet-overeenkomend schema definitie. Bijvoorbeeld, als een kolom onjuist voor het schema van int opgegeven is wanneer de gegevens in het bestand een tekenreeks is, elke rij niet worden geladen.

Data Lake Storage Gen1 maakt gebruik van rollen gebaseerd toegangsbeheer (RBAC) voor het beheren van toegang tot de gegevens. Dit betekent dat de Service-Principal hebben voor de mappen die zijn gedefinieerd in de locatieparameter en de onderliggende objecten van de laatste map en bestanden leesmachtigingen moet. Hierdoor is PolyBase om te verifiëren en die gegevens te laden. 

## <a name="load-the-data"></a>De gegevens laden
Om gegevens te laden uit Data Lake Storage Gen1 gebruik de [CREATE TABLE AS SELECT (Transact-SQL)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) instructie. 

CTAS maakt een nieuwe tabel gemaakt en gevuld met de resultaten van een select-instructie. CTAS wordt gedefinieerd in de nieuwe tabel hebben dezelfde kolommen en gegevenstypen als de resultaten van de select-instructie. Als u alle kolommen uit een externe tabel selecteert, is de nieuwe tabel een replica van de kolommen en gegevenstypen in de externe tabel.

In dit voorbeeld maken we een tabel DimProduct aangeroepen vanuit de externe tabel DimProduct_external.

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```


## <a name="optimize-columnstore-compression"></a>Columnstore-compressie optimaliseren
Standaard worden in de tabel in SQL Data Warehouse opgeslagen als een geclusterde columnstore-index. Nadat een laden is voltooid, kunnen sommige van de rijen met gegevens niet worden gecomprimeerd in de columnstore.  Er is een aantal redenen waarom dit kan gebeuren. Zie voor meer informatie, [columnstore-indexen beheren](sql-data-warehouse-tables-index.md).

Opnieuw maken in de tabel om af te dwingen de columnstore-index voor het comprimeren van alle rijen voor het optimaliseren van prestaties van query's en columnstore-compressie na een laden.

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

## <a name="optimize-statistics"></a>Statistieken optimaliseren
Het is raadzaam om te maken van statistieken voor één kolom onmiddellijk na een belasting. Er zijn enkele mogelijkheden voor statistieken. Bijvoorbeeld, als u één kolom statistieken voor elke kolom maken duurt het lang alle statistische gegevens opnieuw. Als u weet dat bepaalde kolommen niet meer worden in query-predicaten, kunt u statistieken voor het maken van het overslaan van deze kolommen.

Als u besluit te maken van statistieken voor één kolom in elke kolom in elke tabel, kunt u de voorbeeldcode van de opgeslagen procedure `prc_sqldw_create_stats` in de [statistieken](sql-data-warehouse-tables-statistics.md) artikel.

Het volgende voorbeeld is een goed uitgangspunt voor het maken van statistieken. Statistieken voor één kolom wordt gemaakt op elke kolom in de dimensietabel en op elk lid te worden kolom in de feitentabellen. U kunt altijd één of meerdere kolommen statistieken voor andere kolommen in de tabel feit later toevoegen.

## <a name="achievement-unlocked"></a>Prestatie ontgrendeld.
U hebt gegevens zijn geladen in Azure SQL Data Warehouse. Uitstekend!

## <a name="next-steps"></a>Volgende stappen 
In deze zelfstudie, kunt u externe tabellen voor het definiëren van de structuur voor gegevens die zijn opgeslagen in Data Lake Storage Gen1 gemaakt en vervolgens de PolyBase CREATE TABLE AS SELECT-instructie die wordt gebruikt om gegevens te laden in uw datawarehouse. 

U hebt het volgende gedaan:
> [!div class="checklist"]
> * Objecten in de gemaakte database vereist om te laden uit Data Lake Storage Gen1.
> * Verbonden met een Data Lake Storage Gen1-map.
> * Geladen gegevens in Azure SQL Data Warehouse.
> 

Het laden van gegevens is de eerste stap bij het ontwikkelen van een datawarehouse-oplossing met behulp van SQL Data Warehouse. Bekijk onze resources voor het ontwikkelen.

> [!div class="nextstepaction"]
>[Informatie over het ontwikkelen van tabellen in SQL Data Warehouse](sql-data-warehouse-tables-overview.md)




