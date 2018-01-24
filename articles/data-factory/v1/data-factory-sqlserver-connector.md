---
title: Gegevens verplaatsen naar en van SQL Server | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit SQL Server-database on-premises of in een virtuele machine in Azure met Azure Data Factory.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 864ece28-93b5-4309-9873-b095bbe6fedd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 19398a33e17bde7f496070d1f1c84e61dbe65855
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-to-and-from-sql-server-on-premises-or-on-iaas-azure-vm-using-azure-data-factory"></a>Verplaatsen van gegevens naar en van SQL Server on-premises of op IaaS (Azure VM) met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-sqlserver-connector.md)
> * [Versie 2 - Preview](../connector-sql-server.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [SQL Server-connector in V2](../connector-sql-server.md).

Dit artikel wordt uitgelegd dat het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens uit een on-premises SQL Server database te verplaatsen. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, hetgeen een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit toont. 

## <a name="supported-scenarios"></a>Ondersteunde scenario's
U kunt gegevens kopiëren **uit een SQL Server-database** opslaat in de volgende gegevens:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

U kunt gegevens kopiëren van de volgende gegevensarchieven **met een SQL Server-database**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

## <a name="supported-sql-server-versions"></a>Ondersteunde versies van SQL Server
Deze ondersteuning van SQL Server-connector kopiëren van gegevens van/naar de volgende versies van gehost exemplaar lokaal of in Azure IaaS met behulp van zowel de SQL-verificatie en de Windows-verificatie: SQL Server 2016, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQL Server 2005

## <a name="enabling-connectivity"></a>Connectiviteit inschakelen
De concepten en de stappen die nodig zijn om verbinding te maken met SQL Server die wordt gehost on-premises of in virtuele machines van Azure IaaS (Infrastructure-as-a-Service) zijn hetzelfde. In beide gevallen moet u Data Management Gateway gebruiken voor verbinding.

Zie [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies over het instellen van de gateway. Instellen van een gatewayexemplaar is een vereiste voor het verbinden met SQL Server.

Terwijl de SQL-Server voor betere prestaties u gateway op de lokale machine, dezelfde of de cloud VM-instantie installeren kunt, raden wij u deze installeren op afzonderlijke computers. Met de gateway en de SQL Server op afzonderlijke computers, worden bronconflicten beperkt.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens uit een on-premises SQL Server database verplaatst met behulp van verschillende hulpprogramma's voor API's.

De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren: 

1. Maak een **gegevensfactory**. Een gegevensfactory kan één of meer pijplijnen bevatten. 
2. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory. Bijvoorbeeld, als u gegevens uit een SQL Server-database naar een Azure blob storage kopiëren wilt, maakt u twee gekoppelde services als u wilt uw SQL Server-database en de Azure storage-account koppelen aan uw gegevensfactory. Zie voor de gekoppelde service-eigenschappen die specifiek voor SQL Server-database zijn, [gekoppelde service-eigenschappen](#linked-service-properties) sectie. 
3. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking. In het voorbeeld in de laatste stap wordt vermeld, maakt u een gegevensset om op te geven van de SQL-tabel in uw SQL Server-database waarin de invoergegevens. En u een andere gegevensset opgeven van de blob-container en de map waarin de gegevens die zijn gekopieerd uit de SQL Server-database maken. Zie voor eigenschappen van gegevensset die specifiek voor SQL Server-database zijn, [eigenschappen van gegevensset](#dataset-properties) sectie.
4. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. In het voorbeeld eerder vermeld, gebruikt u SqlSource als een bron- en BlobSink als een sink voor de kopieeractiviteit. Op dezelfde manier als u uit Azure Blob Storage met SQL Server-Database kopiëren wilt, gebruikt u BlobSource en SqlSink in de kopieerbewerking. Zie voor activiteitseigenschappen kopiëren die specifiek voor SQL Server-Database zijn, [activiteitseigenschappen kopiëren](#copy-activity-properties) sectie. Klik op de koppeling in de vorige sectie voor de gegevensopslag voor meer informatie over het gebruik van een gegevensarchief als een bron of een sink. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor voorbeelden met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar/van een on-premises SQL Server database [JSON voorbeelden](#json-examples-for-copying-data-from-and-to-sql-server) sectie van dit artikel. 

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met SQL Server: 

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
Maken van een gekoppelde service van het type **OnPremisesSqlServer** een on-premises SQL Server database koppelen aan een gegevensfactory. De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor lokale SQL Server gekoppelde service.

De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor de service SQL Server gekoppeld.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OnPremisesSqlServer**. |Ja |
| connectionString |ConnectionString informatie die nodig zijn voor de verbinding met de lokale SQL Server-database met behulp van de SQL-verificatie of de Windows-verificatie opgeven. |Ja |
| gatewayName |Naam van de gateway die voor de Data Factory-service gebruiken moet voor verbinding met het lokale SQL Server-database. |Ja |
| gebruikersnaam |Geef de gebruikersnaam als u Windows-verificatie gebruikt. Voorbeeld: **domainname\\gebruikersnaam**. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |

U kunt versleutelen referenties met behulp van de **nieuw AzureRmDataFactoryEncryptValue** cmdlet en in de verbindingsreeks gebruiken, zoals wordt weergegeven in het volgende voorbeeld (**EncryptedCredential** eigenschap):  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

### <a name="samples"></a>Voorbeelden
**JSON voor het gebruik van SQL-verificatie**

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties":
    {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
**JSON voor het gebruik van Windows-verificatie**

Data Management Gateway wordt het opgegeven gebruikersaccount verbinding maken met de lokale SQL Server-database te imiteren. 

```json
{
     "Name": " MyOnPremisesSQLDB",
     "Properties":
     {
         "type": "OnPremisesSqlServer",
         "typeProperties": {
             "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
             "username": "<domain\\username>",
             "password": "<password>",
             "gatewayName": "<gateway name>"
        }
     }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
In de voorbeelden hebt u een gegevensset van het type gebruikt **SqlServerTable** vertegenwoordigt een tabel in een SQL Server-database.  

Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets van de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle gegevensset typen (SQL Server, Azure blob-, Azure-tabel, enz.).

De sectie typeProperties verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De **typeProperties** sectie voor de gegevensset van het type **SqlServerTable** heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel of weergave in de SQL Server-Database-instantie waarnaar de gekoppelde service verwijst. |Ja |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Als u gegevens uit een SQL Server-database verplaatst, u het brontype instellen in de kopieerbewerking naar **SqlSource**. Als u gegevens naar een SQL Server-database verplaatst, u stelt het sink-type in de kopieerbewerking naar **SqlSink**. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SqlSource en SqlSink.

Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten van de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer tabellen en beleidsregels zijn beschikbaar voor alle typen activiteiten.

> [!NOTE]
> De Kopieeractiviteit slechts één invoer en slechts één uitvoer produceert.

Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren met elk activiteitstype. Voor de kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put.

### <a name="sqlsource"></a>SqlSource
Wanneer u de gegevensbron in een kopieeractiviteit is van het type **SqlSource**, de volgende eigenschappen beschikbaar zijn in **typeProperties** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| sqlReaderQuery |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: Selecteer * from MijnTabel. Kan naar meerdere tabellen uit de database waarnaar wordt verwezen door de invoergegevensset verwijzen. Als niet wordt opgegeven, de SQL-instructie die is uitgevoerd: Selecteer een van de MyTable. |Nee |
| sqlReaderStoredProcedureName |Naam van de opgeslagen procedure die gegevens uit de brontabel leest. |Naam van de opgeslagen procedure. De laatste SQL-instructie moet een SELECT-instructie in de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. |Nee |

Als de **sqlReaderQuery** is opgegeven voor de SqlSource met de kopieerbewerking wordt deze query uitgevoerd op basis van de bron van de SQL Server-Database om de gegevens te verkrijgen.

U kunt ook een opgeslagen procedure opgeven door te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters nodig heeft).

Als u sqlReaderQuery of sqlReaderStoredProcedureName niet opgeeft, worden de kolommen die zijn gedefinieerd in de sectie structuur gebruikt voor het bouwen van een select-query uitvoeren op de SQL Server-Database. Als de definitie van de gegevensset heeft niet de structuur, worden alle kolommen uit de tabel geselecteerd.

> [!NOTE]
> Als u werkt met **sqlReaderStoredProcedureName**, moet u toch een waarde opgeven voor de **tableName** eigenschap in de JSON van de gegevensset. Er zijn geen controles uitgevoerd voor deze tabel al.

### <a name="sqlsink"></a>SqlSink
**SqlSink** ondersteunt de volgende eigenschappen:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| writeBatchTimeout |Wachttijd voor de batch-insert-bewerking te voltooien voordat er een optreedt time-out. |TimeSpan<br/><br/> Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |
| writeBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt. |Geheel getal (aantal rijen) |Nee (standaard: 10000) |
| sqlWriterCleanupScript |Query voor de Kopieeractiviteit uitvoeren zodat de gegevens van een bepaald segment wordt opgeschoond opgeven. Zie voor meer informatie [herhaalbare kopiëren](#repeatable-copy) sectie. |Een query-instructie. |Nee |
| sliceIdentifierColumnName |Geef de naam van de kolom voor de Kopieeractiviteit te vullen met de segment-ID automatisch gegenereerd en die wordt gebruikt voor het opschonen van de gegevens van een bepaald segment wanneer opnieuw uitgevoerd. Zie voor meer informatie [herhaalbare kopiëren](#repeatable-copy) sectie. |De naam van de kolom van een kolom met gegevenstype binary(32). |Nee |
| sqlWriterStoredProcedureName |Naam van de opgeslagen procedure die definieert hoe brongegevens in doeltabel, bijvoorbeeld toepast op komen upserts of transformeren met behulp van uw eigen zakelijke logica. <br/><br/>Houd er rekening mee deze opgeslagen procedure wordt **aangeroepen per batch**. Als u uitvoeren die slechts eenmaal uitgevoerd en heeft niets wilt te doen met de brongegevens bijvoorbeeld verwijderen/afkappen, gebruikt u `sqlWriterCleanupScript` eigenschap. |Naam van de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. |Nee |
| sqlWriterTableType |Type tabelnaam moet worden gebruikt in de opgeslagen procedure opgeven. Kopieeractiviteit maakt u de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Code van de opgeslagen procedure kan de gegevens wordt gekopieerd met de bestaande gegevens vervolgens samenvoegen. |Een typenaam van de tabel. |Nee |


## <a name="json-examples-for-copying-data-from-and-to-sql-server"></a>JSON-voorbeelden voor het kopiëren van gegevens van en naar SQL Server
De volgende voorbeelden geven voorbeeld JSON definities die u een pijplijn maken kunt met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). De volgende voorbeelden laten zien hoe om gegevens te kopiëren naar en van SQL Server en Azure Blob Storage. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** uit een van de bronnen aan een van de PUT vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.     

## <a name="example-copy-data-from-sql-server-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren van SQL Server naar Azure Blob
Het volgende voorbeeld toont:

1. Een gekoppelde service van het type [OnPremisesSqlServer](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [SqlServerTable](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. De [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [SqlSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld kopieert timeseries gegevens uit een tabel met SQL Server naar een Azure-blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

Het instellen van de data management gateway als eerste stap. De instructies vindt u in de [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

**SQL Server gekoppeld-service**
```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
**Azure Blob-opslag gekoppelde service**

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**SQL Server-invoergegevensset**

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MijnTabel' gemaakt in SQL Server en deze bevat een kolom met de naam 'timestampcolumn' voor de reeksgegevens. U kunt een query over meerdere tabellen binnen dezelfde database met behulp van een één gegevensset, maar één tabel moet worden gebruikt voor de gegevensset tableName typeProperty.

Instelling 'extern': 'true' informeert Data Factory-service dat de dataset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

```json
{
  "name": "SqlServerInput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**Azure Blob-uitvoergegevensset**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad maakt gebruik van jaar, maand, dag en uur delen van de begintijd.

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Pijplijn met de kopieeractiviteit**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van deze invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **SqlSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **SqlReaderQuery** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2016-06-01T18:00:00",
    "end":"2016-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "SqlServertoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```
In dit voorbeeld **sqlReaderQuery** is opgegeven voor de SqlSource. De Kopieeractiviteit wordt deze query uitgevoerd op basis van de bron van de SQL Server-Database om de gegevens te verkrijgen. U kunt ook een opgeslagen procedure opgeven door te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters nodig heeft). De sqlReaderQuery kunt verwijzen naar meerdere tabellen in de database waarnaar wordt verwezen door de invoergegevensset. Het is niet beperkt tot alleen de tabel als de dataset tableName typeProperty instellen.

Als u geen sqlReaderQuery of sqlReaderStoredProcedureName opgeeft, worden de kolommen die zijn gedefinieerd in de sectie structuur gebruikt voor het bouwen van een select-query uitvoeren op de SQL Server-Database. Als de definitie van de gegevensset heeft niet de structuur, worden alle kolommen uit de tabel geselecteerd.

Zie de [Sql-bron](#sqlsource) sectie en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) voor de lijst met eigenschappen die ondersteund worden door SqlSource en BlobSink.

## <a name="example-copy-data-from-azure-blob-to-sql-server"></a>Voorbeeld: Gegevens kopiëren van Azure-Blob naar SQL Server
Het volgende voorbeeld toont:

1. De gekoppelde service van het type [OnPremisesSqlServer](#linked-service-properties).
2. De gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).
5. De [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) en [SqlSink](#sql-server-copy-activity-type-properties).

De voorbeeld-kopieën timeseries gegevens van een Azure-blob naar een SQL Server tabel om het uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**SQL Server gekoppeld-service**

```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
**Azure Blob-opslag gekoppelde service**

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure Blob-invoergegevensset**

Gegevens wordt opgehaald uit een nieuwe blob elk uur (frequentie: uur, interval: 1). Pad en naam van de map voor de blob worden dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad gebruikt jaar, maand en dag deel uit van de begintijd en de bestandsnaam wordt gebruikt voor het uur deel van de begintijd. "extern": "true" instelling informeert de Data Factory-service dat de dataset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**Gegevensset voor SQL Server-uitvoer**

Het voorbeeld worden gegevens gekopieerd naar een tabel met de naam "MijnTabel" in SQL Server. De tabel in SQL-Server met hetzelfde aantal kolommen maken zoals u verwacht dat de Blob-CSV-bestand bevatten. Nieuwe rijen worden toegevoegd aan de tabel om het uur.

```json
{
  "name": "SqlServerOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Pijplijn met de kopieeractiviteit**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van deze invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **BlobSource** en **sink** type is ingesteld op **SqlSink**.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": " SqlServerOutput "
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```

## <a name="troubleshooting-connection-issues"></a>Verbindingsproblemen oplossen
1. Configureer uw SQL Server om externe verbindingen te accepteren. Start **SQL Server Management Studio**, met de rechtermuisknop op **server**, en klik op **eigenschappen**. Selecteer **verbindingen** uit de lijst en het selectievakje **toestaan van externe verbindingen met de server**.

    ![Externe verbindingen inschakelen](./media/data-factory-sqlserver-connector/AllowRemoteConnections.png)

    Zie [de serverconfiguratieoptie voor externe toegang configureren](https://msdn.microsoft.com/library/ms191464.aspx) voor gedetailleerde stappen.
2. Start **SQL Server Configuration Manager**. Vouw **SQL Server-netwerkconfiguratie** voor het exemplaar dat u wilt gebruiken en selecteer **protocollen voor MSSQLSERVER**. U ziet protocollen in het rechterdeelvenster. TCP/IP inschakelen met de rechtermuisknop op **TCP/IP** en te klikken op **inschakelen**.

    ![TCP/IP inschakelen](./media/data-factory-sqlserver-connector/EnableTCPProptocol.png)

    Zie [in- of uitschakelen van een Server netwerkprotocol](https://msdn.microsoft.com/library/ms191294.aspx) voor meer informatie en alternatieve manieren TCP/IP-protocol in te schakelen.
3. Dubbelklik in het venster dezelfde **TCP/IP** starten **TCP/IP-eigenschappen** venster.
4. Overschakelen naar de **IP-adressen** tabblad. Schuif helemaal naar Zie **IPAll** sectie. Noteer de ** TCP-poort ** (standaardwaarde is **1433**).
5. Maak een **regel voor de Windows Firewall** op de computer waarmee binnenkomend verkeer via deze poort.  
6. **Verbinding controleren**: voor verbinding met de volledig gekwalificeerde naam SQL-Server, SQL Server Management Studio uit een andere computer te gebruiken. Bijvoorbeeld: "<machine>.<domain>.corp.<company>.com,1433."

   > [!IMPORTANT]

   > Zie [gegevens verplaatsen tussen lokale bronnen en de cloud met Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) voor gedetailleerde informatie.
   >
   > Zie [gateway problemen](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) voor tips over het oplossen van verbinding of gateway gerelateerde problemen.
   >
   >


## <a name="identity-columns-in-the-target-database"></a>Id-kolommen in de doeldatabase
Deze sectie bevat een voorbeeld waarin gegevens uit een brontabel met geen identiteitskolom naar een doeltabel met een identiteitskolom kopieert.

**Brontabel:**

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
**Doeltabel:**

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

U ziet dat de doeltabel een identiteitskolom.

**Bron gegevensset JSON-definitie**

```json
{
    "name": "SampleSource",
    "properties": {
        "published": false,
        "type": " SqlServerTable",
        "linkedServiceName": "TestIdentitySQL",
        "typeProperties": {
            "tableName": "SourceTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```
**Doel-dataset JSON-definitie**

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "TestIdentitySQLSource",
        "typeProperties": {
            "tableName": "TargetTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

Merk op dat als de bron en doel-tabel verschillende schema's zijn (doel heeft een extra kolom met de identiteit). In dit scenario moet u opgeven **structuur** eigenschap in de definitie van de gegevensset doel, waaronder de identiteitskolom niet.

## <a name="invoke-stored-procedure-from-sql-sink"></a>Aanroepen van opgeslagen procedure uit SQL-sink
Zie [opgeslagen procedure voor SQL-sink aanroepen in de kopieerbewerking](data-factory-invoke-stored-procedure-from-copy-activity.md) artikel voor een voorbeeld van een opgeslagen procedure van SQL-sink in een kopieeractiviteit van een pijplijn wordt aangeroepen.

## <a name="type-mapping-for-sql-server"></a>Toewijzing van het type voor SQL server
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel de kopieerbewerking wordt automatische typeconversies van brontypen opvangen typen met de volgende stappen 2-benadering uitgevoerd:

1. Systeemeigen brontypen converteren naar .NET-type
2. Converteren van .NET-type naar systeemeigen sink-type

Bij het verplaatsen van gegevens naar & uit SQL server, worden de volgende toewijzingen gebruikt vanuit de SQL-type aan .NET-type en vice versa.

De toewijzing is hetzelfde als de SQL Server gegevenstypetoewijzing voor ADO.NET.

| SQL Server Database Engine-type | .NET framework-type |
| --- | --- |
| bigint |Int64 |
| Binaire |Byte[] |
| bits |Boole-waarde |
| CHAR |Tekenreeks, Char] |
| datum |Datum en tijd |
| Datum en tijd |Datum en tijd |
| datetime2 |Datum en tijd |
| Datetimeoffset |DateTimeOffset |
| Decimale |Decimale |
| FILESTREAM-kenmerk (varbinary(max)) |Byte[] |
| Float |Double |
| Afbeelding |Byte[] |
| int |Int32 |
| Money |Decimale |
| nchar |Tekenreeks, Char] |
| ntext |Tekenreeks, Char] |
| numerieke |Decimale |
| nvarchar |Tekenreeks, Char] |
| echte |Single |
| ROWVERSION |Byte[] |
| smalldatetime |Datum en tijd |
| smallint |Int16 |
| smallmoney |Decimale |
| sql_variant |Object * |
| Tekst |Tekenreeks, Char] |
| tijd |TimeSpan |
| tijdstempel |Byte[] |
| tinyint |Byte |
| uniqueidentifier |GUID |
| varbinary |Byte[] |
| varchar |Tekenreeks, Char] |
| xml |Xml |

## <a name="mapping-source-to-sink-columns"></a>Toewijzingsbron opvangen kolommen
Zie het toewijzen van kolommen uit de bron-gegevensset naar kolommen uit de dataset sink [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-copy"></a>Herhaalbare kopiëren
Bij het kopiëren van gegevens naar SQL Server-Database, de kopieeractiviteit worden gegevens toegevoegd aan de tabel sink standaard. Om uit te voeren in plaats daarvan een UPSERT, Zie [Repeatable schrijven naar SqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) artikel. 

Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, moet u herhaalbaarheid Houd in gedachten om te voorkomen dat ongewenste resultaten. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook een beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment opnieuw in beide gevallen wordt uitgevoerd, moet u ervoor zorgen dat dezelfde gegevens is gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable van relationele bronnen lezen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en het afstemmen
Zie [uitvoering van de activiteit & afstemmen handleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren.
