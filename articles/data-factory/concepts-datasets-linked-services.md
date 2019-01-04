---
title: Gegevenssets en gekoppelde services in Azure Data Factory | Microsoft Docs
description: Meer informatie over gegevenssets en gekoppelde services in Data Factory. Gekoppelde services berekenen/gegevensarchieven aan data factory koppelen. Gegevenssets vertegenwoordigen invoer-en uitvoergegevens.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: shlo
ms.openlocfilehash: c9c9f07eab395df716a4575338f881f07d573b74
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54019126"
---
# <a name="datasets-and-linked-services-in-azure-data-factory"></a>Gegevenssets en gekoppelde services in Azure Data Factory 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-create-datasets.md)
> * [Huidige versie](concepts-datasets-linked-services.md)

Dit artikel wordt beschreven welke gegevenssets bent, hoe ze worden gedefinieerd in JSON-indeling, en hoe ze worden gebruikt Azure Data Factory-pijplijnen. 

Als u niet bekend bent met Data Factory, raadpleegt u [Inleiding tot Azure Data Factory](introduction.md) voor een overzicht. 

## <a name="overview"></a>Overzicht
Een gegevensfactory kan één of meer pijplijnen hebben. Een **pijplijn** is een logische groepering van **activiteiten** die samen een taak uitvoeren. Met activiteiten in een pijplijn definieert u welk acties moeten worden uitgevoerd voor uw gegevens. Bijvoorbeeld, kunt u een kopieeractiviteit om gegevens te kopiëren uit een on-premises SQL Server naar Azure Blob storage. Vervolgens kunt u een Hive-activiteit die een Hive-script uitvoert op een Azure HDInsight-cluster om gegevens te verwerken van Blob-opslag om uitvoergegevens te produceren. U kunt tot slot een tweede kopieeractiviteit gebruiken om het kopiëren van de uitvoergegevens naar Azure SQL Data Warehouse, boven op welke business intelligence (BI) reporting oplossingen zijn gemaakt. Zie voor meer informatie over pijplijnen en activiteiten [pijplijnen en activiteiten](concepts-pipelines-activities.md) in Azure Data Factory.

Nu een **gegevensset** is een benoemde weergave van gegevens die gewoon verwijst of verwijst naar de gegevens die u gebruiken wilt uw **activiteiten** als invoer en uitvoer. Met gegevenssets worden gegevens binnen andere gegevensarchieven geïdentificeerd, waaronder tabellen, bestanden, mappen en documenten. Een Azure Blob-gegevensset benoemt bijvoorbeeld de blobcontainer en -map in de Blob-opslag van waaruit de activiteit de gegevens moet lezen.

Voordat u een gegevensset maakt, moet u een **gekoppelde service** uw gegevensopslag aan de data factory koppelen. Gekoppelde services zijn te vergelijken met verbindingsreeksen, die de verbindingsinformatie bevatten die Data Factory nodig heeft om verbinding te maken met externe bronnen. Beschouw dit niet mogelijk. de structuur van de gegevens in de gekoppelde gegevensarchieven, vertegenwoordigt de gegevensset en de gekoppelde service definieert de verbinding met de gegevensbron. Bijvoorbeeld, gekoppelde een Azure Storage-service wordt een storage-account aan de data factory. Een Azure Blob-gegevensset vertegenwoordigt de blob-container en de map in het Azure storage-account met de blobs voor invoer om te worden verwerkt.

Hier volgt een voorbeeldscenario. Om gegevens te kopiëren van Blob-opslag met een SQL-database, moet u twee gekoppelde services maken: Azure Storage en Azure SQL-Database. Vervolgens maakt u twee gegevenssets: Azure Blob-gegevensset (die verwijst naar de gekoppelde Azure Storage-service) en Azure SQL Table-gegevensset (die verwijst naar de gekoppelde Azure SQL Database-service). De Azure Storage en de gekoppelde Azure SQL Database-services bevatten verbindingsreeksen die Data Factory tijdens runtime gebruikt verbinding maken met uw Azure Storage en Azure SQL Database, respectievelijk. De Azure Blob-gegevensset specificeert de blob-container en de blob-map met de blobs voor invoer in uw Blob storage. De Azure SQL Table-gegevensset bevat de SQL-tabel in uw SQL-database waarnaar de gegevens zijn om te worden gekopieerd.

Het volgende diagram toont de relaties tussen de pijplijn, activiteit, gegevensset en gekoppelde service in Data Factory:

![Relatie tussen pijplijn, activiteit, gegevensset, gekoppelde services](media/concepts-datasets-linked-services/relationship-between-data-factory-entities.png)

## <a name="linked-service-json"></a>Gekoppelde service JSON
Een gekoppelde service in Data Factory wordt in JSON-indeling als volgt gedefinieerd:

```json
{
    "name": "<Name of the linked service>",
    "properties": {
        "type": "<Type of the linked service>",
        "typeProperties": {
              "<data store or compute-specific type properties>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

De volgende tabel beschrijft de eigenschappen in de bovenstaande JSON:

Eigenschap | Description | Vereist |
-------- | ----------- | -------- |
naam | De naam van de gekoppelde service. Zie [Azure Data Factory - naamgevingsregels](naming-rules.md). |  Ja |
type | Het type van de gekoppelde service. Bijvoorbeeld: AzureStorage (gegevensopslag) of AzureBatch (rekenen). Zie de beschrijving voor typeProperties. | Ja |
typeProperties | De type-eigenschappen zijn verschillend voor elk gegevensarchief of compute. <br/><br/> Voor de ondersteunde gegevens opslaat typen en hun type-eigenschappen, Zie de [gegevenssettype](#dataset-type) tabel in dit artikel. Ga naar het data store connector artikel voor meer informatie over de type-eigenschappen die specifiek zijn voor een gegevensarchief. <br/><br/> Zie voor de typen ondersteunde compute en hun eigenschappen [gekoppelde services berekenen](compute-linked-services.md). | Ja |
connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken (als het gegevensarchief bevindt zich in een particulier netwerk). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. | Nee

## <a name="linked-service-example"></a>Voorbeeld van de gekoppelde service
De volgende gekoppelde service is een gekoppelde Azure Storage-service. U ziet dat het type is ingesteld voor opslag van Azureverzonden. De eigenschappen van het type voor de gekoppelde Azure Storage-service zijn een verbindingsreeks. De Data Factory-service gebruikt deze verbindingsreeks verbinding maken met het gegevensarchief tijdens runtime. 

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-json"></a>JSON van de gegevensset
Een gegevensset in Data Factory wordt in JSON-indeling als volgt gedefinieerd:

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "linkedServiceName": {
                "referenceName": "<name of linked service>",
                 "type": "LinkedServiceReference",
        },
        "structure": [
            {
                "name": "<Name of the column>",
                "type": "<Name of the type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        }
    }
}

```
De volgende tabel beschrijft de eigenschappen in de bovenstaande JSON:

Eigenschap | Description | Vereist |
-------- | ----------- | -------- |
naam | Naam van de gegevensset. Zie [Azure Data Factory - naamgevingsregels](naming-rules.md). |  Ja |
type | Het type van de gegevensset. Geef een van de typen die worden ondersteund door Data Factory (bijvoorbeeld: AzureBlob, AzureSqlTable). <br/><br/>Zie voor meer informatie, [gegevenssettypen](#dataset-type). | Ja |
structuur | Het schema van de gegevensset. Zie voor meer informatie, [gegevenssetstructuur](#dataset-structure). | Nee |
typeProperties | De type-eigenschappen zijn verschillend voor elk type (bijvoorbeeld: Azure Blob-, Azure SQL-tabel). Zie voor meer informatie over de ondersteunde typen en de bijbehorende eigenschappen, [gegevenssettype](#dataset-type). | Ja |

## <a name="dataset-example"></a>Voorbeeld van de gegevensset
In het volgende voorbeeld vertegenwoordigt de gegevensset een tabel met de naam MyTable in een SQL-database.

```json
{
    "name": "DatasetSample",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": {
                "referenceName": "MyAzureSqlLinkedService",
                 "type": "LinkedServiceReference",
        },
        "typeProperties":
        {
            "tableName": "MyTable"
        },
    }
}

```
Houd rekening met de volgende punten:

- type is ingesteld op AzureSqlTable.
- (specifiek voor AzureSqlTable) tableName type-eigenschap is ingesteld op MyTable.
- linkedServiceName verwijst naar een gekoppelde service van het type AzureSqlDatabase, die is gedefinieerd in de volgende JSON-codefragment.

## <a name="dataset-type"></a>Type Brongegevensset
Er zijn veel verschillende typen gegevenssets, afhankelijk van het gegevensarchief dat u gebruikt. Zie de volgende tabel voor een lijst met gegevensarchieven die worden ondersteund door Data Factory. Klik op een gegevensarchief voor informatie over het maken van een gekoppelde service en een gegevensset voor het betreffende gegevensarchief.

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores.md)]

In het voorbeeld in de vorige sectie, het type van de gegevensset is ingesteld op **AzureSqlTable**. Op dezelfde manier het type van de gegevensset voor een Azure Blob-gegevensset is ingesteld op **AzureBlob**, zoals weergegeven in de volgende JSON:

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
                "referenceName": "MyAzureStorageLinkedService",
                 "type": "LinkedServiceReference",
        }, 
 
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        }
    }
}
```
## <a name="dataset-structure"></a>Structuur van de gegevensset
De **structuur** sectie is optioneel. Het schema van de gegevensset wordt gedefinieerd door een verzameling van namen en gegevenstypen van kolommen. De structuur sectie kunt u opgeven type-informatie die wordt gebruikt om te converteren en kolommen uit de bron naar de bestemming toewijzen.

Elke kolom in de structuur bevat de volgende eigenschappen:

Eigenschap | Description | Vereist
-------- | ----------- | --------
naam | Naam van de kolom. | Ja
type | Het gegevenstype van de kolom. Data Factory ondersteunt de volgende tijdelijke gegevenstypen als toegestane waarden: **Int16, Int32 Int64, één, Double, Decimal, Byte [], Booleaanse waarde, String, Guid, datum/tijd, Datetimeoffset en Timespan** | Nee
cultuur | . NET-gebaseerde cultuur moet worden gebruikt wanneer het type een .NET-type is: `Datetime` of `Datetimeoffset`. De standaardwaarde is `en-us`. | Nee
Indeling | Tekenreeks die moet worden gebruikt wanneer het type een .NET-type is-indeling: `Datetime` of `Datetimeoffset`. Raadpleeg [aangepaste datum en tijd opmaaktekenreeksen](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) op datum/tijd opmaken. | Nee

### <a name="example"></a>Voorbeeld
Stel in het volgende voorbeeld wordt de bron-Blob-gegevens is CSV-indeling en bevat drie kolommen: gebruikers-id, naam en lastlogindate. Ze zijn van het type Int64, tekenreeks en datum/tijd met een aangepaste datum / tijdindeling met behulp van Franse afkortingen voor de dag van de week.

De structuur van de Blob-gegevensset als volgt samen met de definities voor de kolommen te definiëren:

```json
"structure":
[
    { "name": "userid", "type": "Int64"},
    { "name": "name", "type": "String"},
    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
]
```

### <a name="guidance"></a>Richtlijnen

De volgende richtlijnen te begrijpen wanneer structuur informatie op te nemen, en wat u wilt opnemen de **structuur** sectie. Meer informatie over hoe data factory gegevens naar het sink-kaarten en bij het opgeven van informatie van de structuur van [Schema en de toewijzing van het type](copy-activity-schema-and-type-mapping.md).

- **Voor gegevensbronnen van sterke schema**, geef de structuur sectie alleen als u toewijzen bronkolommen wilt sink-kolommen, en hun namen niet hetzelfde zijn. Dit soort gestructureerde gegevens worden opgeslagen gegevens schema en het type informatie samen met de gegevens zelf. Voorbeelden van gestructureerde gegevensbronnen zijn SQL Server, Oracle en Azure SQL Database.<br/><br/>Als u informatie over het type is al beschikbaar voor gestructureerde gegevensbronnen, moet u informatie over het type niet opnemen wanneer u de sectie structuur opgeeft.
- **Voor het schema niet/weak gegevensbronnen bijvoorbeeld tekstbestand in blob-opslag**, structuur opnemen wanneer de gegevensset een invoer voor een kopieeractiviteit is en gegevenstypen van brongegevensset moeten worden geconverteerd naar native-typen voor de sink. En structuur opnemen wanneer u toewijzen bronkolommen wilt sink-kolommen...

## <a name="create-datasets"></a>Gegevenssets maken
U kunt gegevenssets maken met behulp van een van deze hulpprogramma's of de SDK's: [.NET API](quickstart-create-data-factory-dot-net.md), [PowerShell](quickstart-create-data-factory-powershell.md), [REST-API](quickstart-create-data-factory-rest-api.md), Azure Resource Manager-sjabloon en Azure-portal

## <a name="current-version-vs-version-1-datasets"></a>Huidige versie vergeleken met versie 1-gegevenssets

Hier zijn enkele verschillen tussen Data Factory en Data Factory versie 1-gegevenssets: 

- De eigenschap external wordt niet ondersteund in de huidige versie. Deze wordt vervangen door een [trigger](concepts-pipeline-execution-triggers.md).
- De eigenschappen van beleid en de beschikbaarheid worden niet ondersteund in de huidige versie. Afhankelijk van de begintijd voor een pijplijn [triggers](concepts-pipeline-execution-triggers.md).
- Scoped gegevenssets (gegevenssets die zijn gedefinieerd in een pijplijn) worden niet ondersteund in de huidige versie. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende zelfstudies voor stapsgewijze instructies voor het maken van pijplijnen en gegevenssets met behulp van een van deze hulpprogramma's of de SDK's. 

- [Snelstartgids: een gegevensfactory maken met .NET](quickstart-create-data-factory-dot-net.md)
- [Snelstartgids: een data factory maken met PowerShell](quickstart-create-data-factory-powershell.md)
- [Snelstartgids: een data factory maken met REST-API](quickstart-create-data-factory-rest-api.md)
- Snelstartgids: een data factory maken met Azure portal
