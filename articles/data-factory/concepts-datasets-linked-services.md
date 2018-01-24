---
title: Gegevenssets en gekoppelde services in Azure Data Factory | Microsoft Docs
description: Meer informatie over de gegevenssets en gekoppelde services gevonden in de Data Factory. Gekoppelde services gegevensarchieven compute/tot data factory. Gegevenssets vertegenwoordigen de invoer-en uitvoergegevens.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: 
ms.date: 01/22/2018
ms.author: shlo
ms.openlocfilehash: bfc95588378466fe1e83bcc4e899eca6b66b358a
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="datasets-and-linked-services-in-azure-data-factory"></a>Gegevenssets en gekoppelde services in Azure Data Factory 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-create-datasets.md)
> * [Versie 2 - Preview](concepts-datasets-linked-services.md)

In dit artikel wordt beschreven welke gegevenssets zijn, hoe ze worden gedefinieerd in JSON-indeling, en hoe ze worden gebruikt Azure Data Factory V2 pijplijnen. 

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [gegevenssets in de Data Factory V1](v1/data-factory-create-datasets.md).

Als u niet bekend met Data Factory bent, Zie [Inleiding tot Azure Data Factory](introduction.md) voor een overzicht. 

## <a name="overview"></a>Overzicht
Een gegevensfactory kan één of meer pijplijnen hebben. Een **pijplijn** is een logische groepering van **activiteiten** die samen een taak uitvoeren. Met activiteiten in een pijplijn definieert u welk acties moeten worden uitgevoerd voor uw gegevens. Bijvoorbeeld, kunt u een kopieeractiviteit om gegevens te kopiëren van een lokale SQL Server naar Azure Blob-opslag. Vervolgens kunt u een Hive-activiteit die wordt uitgevoerd van een Hive-script op een Azure HDInsight-cluster om gegevens te verwerken van Blob-opslag voor het produceren van uitvoergegevens. Ten slotte kunt u een tweede kopieeractiviteit de uitvoergegevens kopiëren naar Azure SQL Data Warehouse boven op welke business intelligence (BI) reporting oplossingen zijn gebouwd. Zie voor meer informatie over de pijplijnen en activiteiten [pijplijnen en activiteiten](concepts-pipelines-activities.md) in Azure Data Factory.

Nu een **gegevensset** is een benoemde weergave van gegevens die eenvoudig verwijst of verwijst naar de gegevens die u gebruiken wilt uw **activiteiten** als invoer en uitvoer. Met gegevenssets worden gegevens binnen andere gegevensarchieven geïdentificeerd, waaronder tabellen, bestanden, mappen en documenten. Een Azure Blob-gegevensset benoemt bijvoorbeeld de blobcontainer en -map in de Blob-opslag van waaruit de activiteit de gegevens moet lezen.

Voordat u een gegevensset maakt, moet u een **gekoppelde service** uw data store koppelen aan de gegevensfactory. Gekoppelde services zijn te vergelijken met verbindingsreeksen, die de verbindingsinformatie bevatten die Data Factory nodig heeft om verbinding te maken met externe bronnen. Beschouw dit niet mogelijk. de gegevensset vertegenwoordigt de structuur van de gegevens in de gekoppelde gegevens worden opgeslagen, en de gekoppelde service definieert de verbinding met de gegevensbron. Bijvoorbeeld, gekoppelde een Azure Storage service een opslagaccount aan de gegevensfactory. Een Azure Blob-gegevensset vertegenwoordigt de blob-container en de map binnen die Azure-opslagaccount waarin de invoer blobs moeten worden verwerkt.

Hier volgt een voorbeeldscenario. Om gegevens te kopiëren van Blob-opslag met een SQL-database, hebt u twee gekoppelde services: Azure Storage en Azure SQL Database. Vervolgens maakt u twee gegevenssets: Azure Blob-gegevensset (die verwijst naar de gekoppelde Azure Storage-service) en Azure SQL-tabel dataset (die verwijst naar de service Azure SQL Database gekoppeld). De Azure Storage en Azure SQL Database, gekoppelde services bevatten verbindingstekenreeksen die Data Factory tijdens runtime verbinding maakt met uw Azure Storage en Azure SQL Database, respectievelijk. De Azure Blob-gegevensset bevat de blob-container en de blob-map waarin de invoer blobs in de Blob-opslag. De gegevensset Azure SQL-tabel bevat de SQL-tabel in uw SQL-database die de gegevens wordt worden gekopieerd.

Het volgende diagram toont de relaties tussen pipeline, de activiteit, de gegevensset en de gekoppelde service in de Data Factory:

![Relatie tussen een pijplijn, activiteit, gegevensset, gekoppelde services](media/concepts-datasets-linked-services/relationship-between-data-factory-entities.png)

## <a name="linked-service-json"></a>JSON van de gekoppelde service
Een gekoppelde service in de Data Factory is gedefinieerd in JSON-indeling als volgt:

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

Eigenschap | Beschrijving | Vereist |
-------- | ----------- | -------- |
naam | Naam van de gekoppelde service. Zie [Azure Data Factory - naamgevingsregels](naming-rules.md). |  Ja |
type | Type van de gekoppelde service. Bijvoorbeeld: AzureStorage (gegevensopslag) of AzureBatch (berekenen). Zie de beschrijving voor typeProperties. | Ja |
typeProperties | Eigenschappen van het type zijn verschillend voor elke gegevensopslag of compute. <br/><br/> Opslaan voor de ondersteunde gegevens typen en hun eigenschappen, Zie de [gegevensettype](#dataset-type) tabel in dit artikel. Ga naar het data store connector artikel voor meer informatie over de type-eigenschappen die specifiek zijn voor een gegevensarchief. <br/><br/> Zie voor de ondersteunde rekentypen en hun eigenschappen [gekoppelde services berekenen](compute-linked-services.md). | Ja |
connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of Self-hosted integratie Runtime gebruiken (indien de gegevensopslag bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. | Nee

## <a name="linked-service-example"></a>Voorbeeld van de gekoppelde service
De volgende gekoppelde service is een gekoppelde Azure Storage-service. U ziet dat het type is ingesteld op AzureStorage. De eigenschappen van het type voor de gekoppelde Azure Storage-service omvatten een verbindingsreeks. De Data Factory-service gebruikt deze verbindingsreeks verbinding maken met het gegevensarchief tijdens runtime. 

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

## <a name="dataset-json"></a>Dataset JSON
Een gegevensset in Gegevensfactory is gedefinieerd in JSON-indeling als volgt:

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

Eigenschap | Beschrijving | Vereist |
-------- | ----------- | -------- |
naam | Naam van de gegevensset. Zie [Azure Data Factory - naamgevingsregels](naming-rules.md). |  Ja |
type | Het type van de gegevensset. Geef een van de typen die door Data Factory worden ondersteund (bijvoorbeeld: AzureBlob, AzureSqlTable). <br/><br/>Zie voor meer informatie [gegevensset typen](#dataset-types). | Ja |
structuur | Schema voor de gegevensset. Zie voor meer informatie [gegevenssetstructuur](#dataset-structure). | Nee |
typeProperties | De type-eigenschappen zijn verschillend voor elk type (bijvoorbeeld: Azure Blob, Azure SQL-tabel). Zie voor meer informatie over de ondersteunde typen en hun eigenschappen [gegevensettype](#dataset-type). | Ja |

## <a name="dataset-example"></a>Voorbeeld van de gegevensset
De gegevensset vertegenwoordigt in het volgende voorbeeld wordt een tabel MijnTabel in een SQL-database.

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

- type is ingesteld op azuresqltable zijn.
- tableName type eigenschap (die specifiek is voor AzureSqlTable type) is ingesteld op MyTable.
- linkedServiceName verwijst naar een gekoppelde service van het type AzureSqlDatabase die is gedefinieerd in het volgende JSON-codefragment.

## <a name="dataset-type"></a>Gegevensettype
Er zijn veel verschillende typen gegevenssets, afhankelijk van het gegevensarchief die u gebruikt. Zie de volgende tabel voor een lijst met opgeslagen gegevens die door Data Factory worden ondersteund. Klik op een gegevensarchief voor meer informatie over het maken van een gekoppelde service en een gegevensset voor die gegevensopslag.

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores.md)]

In het voorbeeld in de vorige sectie, het type van de gegevensset is ingesteld op **AzureSqlTable**. Op dezelfde manier, het type van de gegevensset voor een Azure Blob-gegevensset is ingesteld op **AzureBlob**, zoals wordt weergegeven in de volgende JSON:

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
De **structuur** sectie is optioneel. Hiermee definieert u het schema van de gegevensset door dat een verzameling van namen en gegevenstypen van de kolommen bevat. De structuur sectie kunt u type informatie dat wordt gebruikt voor het converteren van de typen en het toewijzen van kolommen van de bron naar de bestemming. In het volgende voorbeeld wordt de gegevensset heeft drie kolommen: timestamp, projectname, en pageviews. Zijn van het type String, String en Decimal, respectievelijk.

```json
[
    { "name": "timestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

Elke kolom in de structuur bevat de volgende eigenschappen:

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
naam | De naam van de kolom. | Ja
type | Het gegevenstype van de kolom. | Nee
Cultuur | . NET-gebaseerde cultuur moet worden gebruikt wanneer het type een .NET-type is: `Datetime` of `Datetimeoffset`. De standaardwaarde is `en-us`. | Nee
Indeling | Indeling van tekenreeks moet worden gebruikt wanneer het type een .NET-type is: `Datetime` of `Datetimeoffset`. | Nee

De volgende richtlijnen te bepalen wanneer gegevens van de structuur en wat u wilt opnemen de **structuur** sectie.

- **Voor gestructureerde gegevensbronnen**, de sectie structuur alleen opgeven als u toewijzen bronkolommen wilt voor het opvangen van kolommen en hun namen niet overeen komen. Dit soort gegevensbron gestructureerde slaat schema en het type informatie samen met de gegevens zelf. Voorbeelden van gestructureerde gegevensbronnen zijn SQL Server, Oracle en Azure SQL Database.<br/><br/>Als het type informatie is al beschikbaar voor gestructureerde gegevensbronnen, moet u type-informatie niet opnemen wanneer u de sectie structuur opneemt.
- **Voor schema op lezen gegevensbronnen (specifiek Blob-opslag)**, kunt u gegevens opslaan zonder een schema of het type informatie met de gegevens op te slaan. Voor deze typen gegevensbronnen, structuur opnemen wanneer u toewijzen bronkolommen wilt voor het opvangen van kolommen. Structuur ook bevatten wanneer de dataset de invoer voor een kopieeractiviteit is en gegevenstypen van de gegevensset bron moeten worden geconverteerd naar native-typen voor de sink.<br/><br/> Data Factory ondersteunt de volgende waarden voor het type informatie in de structuur: `Int16, Int32, Int64, Single, Double, Decimal, Byte[], Boolean, String, Guid, Datetime, Datetimeoffset, and Timespan`. 

Meer informatie over hoe data factory brongegevens opvangen van toegewezen [Schema en de toewijzing van het type]( copy-activity-schema-and-type-mapping.md) en het opgeven van informatie van de structuur.

## <a name="create-datasets"></a>Gegevenssets maken
U kunt de gegevenssets maken met behulp van een van deze hulpprogramma's of de SDK's: [.NET API](quickstart-create-data-factory-dot-net.md), [PowerShell](quickstart-create-data-factory-powershell.md), [REST-API](quickstart-create-data-factory-rest-api.md), Azure Resource Manager-sjabloon en de Azure-portal

## <a name="v1-vs-v2-datasets"></a>V1-tegenover. V2-gegevenssets

Hier volgen een aantal verschillen tussen de Data Factory-v1- en v2-gegevenssets: 

- De eigenschap external wordt niet ondersteund in v2. Deze vervangen door een [trigger](concepts-pipeline-execution-triggers.md).
- De beschikbaarheid en beleid-eigenschappen worden niet ondersteund in V2. Afhankelijk van de begintijd voor een pijplijn [triggers](concepts-pipeline-execution-triggers.md).
- Bereik gegevenssets (gegevenssets die zijn gedefinieerd in een pijplijn) worden niet ondersteund in V2. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende zelfstudie voor stapsgewijze instructies voor het maken van de pijplijnen en gegevenssets met behulp van een van deze hulpprogramma's of de SDK's. 

- [Snelstartgids: een gegevensfactory maken met .NET](quickstart-create-data-factory-dot-net.md)
- [Snelstartgids: Maak een gegevensfactory, met behulp van PowerShell](quickstart-create-data-factory-powershell.md)
- [Snelstartgids: een gegevensfactory met REST-API maken](quickstart-create-data-factory-rest-api.md)
- Snelstartgids: Maak een gegevensfactory met Azure portal
