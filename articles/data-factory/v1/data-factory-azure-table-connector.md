---
title: Gegevens verplaatsen naar/van Azure Table | Microsoft Docs
description: Informatie over het verplaatsen van gegevens naar/van Azure Table Storage met Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 07b046b1-7884-4e57-a613-337292416319
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 6ab3e918feda3dcf898928f159ebf8e317a95527
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54331840"
---
# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a>Gegevens verplaatsen naar en van Azure-tabel met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-azure-table-connector.md)
> * [Versie 2 (huidige versie)](../connector-azure-table-storage.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [Azure Table Storage-connector in V2](../connector-azure-table-storage.md).

In dit artikel wordt uitgelegd hoe u de Kopieeractiviteit in Azure Data Factory gebruiken om gegevens naar/van Azure Table Storage te verplaatsen. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit geeft. 

U kunt gegevens kopiëren van een ondersteunde bron-gegevensopslag met Azure Table Storage of Azure Table Storage met alle ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven die worden ondersteund als gegevensbronnen of PUT voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. 

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens naar/van een Azure-tabelopslag verplaatst met behulp van verschillende hulpprogramma's / API's.

De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en  **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren: 

1. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory.
2. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling. Zie voor voorbeelden met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar/van een Azure-tabelopslag [JSON voorbeelden](#json-examples) sectie van dit artikel.

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor Azure Table Storage: 

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
Er zijn twee soorten gekoppelde services, die kunt u een Azure blob-opslag aan een Azure data factory koppelen. Dit zijn: **AzureStorage** gekoppelde service en **AzureStorageSas** gekoppelde service. De gekoppelde Azure Storage-service biedt de data factory met wereldwijde toegang tot Azure Storage. Terwijl de Azure Storage SAS (Shared Access Signature) gekoppelde biedt service de data factory met beperkte/tijdelijke toegang naar Azure Storage. Er zijn geen andere verschillen tussen de twee gekoppelde services. Kies de gekoppelde service die tegemoetkomt aan uw behoeften. De volgende secties vindt u meer informatie over deze twee gekoppelde services.

[!INCLUDE [data-factory-azure-storage-linked-services](../../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van gegevensset (Azure SQL, Azure-blob, Azure-tabel, enz.).

De sectie typeProperties verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De **typeProperties** sectie voor de gegevensset van het type **AzureTable** heeft de volgende eigenschappen.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in de Azure Table-Database-instantie waarnaar de gekoppelde service verwijst. |Ja. Wanneer een tabelnaam wordt opgegeven zonder een azureTableSourceQuery, worden alle records uit de tabel worden gekopieerd naar de bestemming. Als een azureTableSourceQuery ook is opgegeven, worden records uit de tabel die voldoet aan de query worden gekopieerd naar de bestemming. |

### <a name="schema-by-data-factory"></a>Schema door Data Factory
Voor de winkels schemavrije gegevens, zoals Azure Table bepaalt de Data Factory-service het schema in een van de volgende manieren:

1. Als u de structuur van gegevens met behulp van de **structuur** eigenschap in de definitie van de Data Factory-service zich houdt aan deze structuur als het schema. In dit geval, als een rij geen waarde voor een kolom bevat, wordt een null-waarde opgegeven voor deze.
2. Als u de structuur van de gegevens niet via opgeeft de **structuur** eigenschap in de definitie van de Data Factory bepaalt welk schema met behulp van de eerste rij in de gegevens. Als de eerste rij niet het volledige schema bevat, worden sommige kolommen in dit geval ontbreekt in het resultaat van de kopieerbewerking.

Daarom voor gegevensbronnen zonder schema, de aanbevolen procedure is om op te geven van de structuur van het gebruik van gegevens de **structuur** eigenschap.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer gegevenssets en beleidsregels zijn beschikbaar voor alle soorten activiteiten.

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit afhankelijk aan de andere kant van elk activiteitstype. Ze verschillen voor de kopieeractiviteit, afhankelijk van de typen van bronnen en sinks.

**AzureTableSource** ondersteunt de volgende eigenschappen in de sectie typeProperties:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| azureTableSourceQuery |De aangepaste query gebruiken om gegevens te lezen. |Azure-tabel query-tekenreeks. Zie de voorbeelden in de volgende sectie. |Nee. Wanneer een tabelnaam wordt opgegeven zonder een azureTableSourceQuery, worden alle records uit de tabel worden gekopieerd naar de bestemming. Als een azureTableSourceQuery ook is opgegeven, worden records uit de tabel die voldoet aan de query worden gekopieerd naar de bestemming. |
| azureTableSourceIgnoreTableNotFound |Aangeven of de uitzondering van de tabel slikken niet bestaat. |DE WAARDE TRUE<br/>DE WAARDE FALSE |Nee |

### <a name="azuretablesourcequery-examples"></a>azureTableSourceQuery-voorbeelden
Als Azure Table-kolom van het tekenreekstype:

```JSON
azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"
```

Als Azure Table-kolom van het type datum/tijd:

```JSON
"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"
```

**AzureTableSink** ondersteunt de volgende eigenschappen in de sectie typeProperties:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| azureTableDefaultPartitionKeyValue |Standaard partitiesleutelwaarde die kan worden gebruikt door de sink. |Een string-waarde. |Nee |
| azureTablePartitionKeyName |Geef de naam van de kolom waarvan de waarden worden gebruikt als partitiesleutels. Als niet is opgegeven, wordt AzureTableDefaultPartitionKeyValue gebruikt als de partitiesleutel. |De naam van een kolom. |Nee |
| azureTableRowKeyName |Geef de naam van de kolom waarvan de kolomwaarden worden gebruikt als de rijsleutel. Als niet is opgegeven, gebruikt u een GUID voor elke rij. |De naam van een kolom. |Nee |
| azureTableInsertType |De modus invoegen van gegevens in Azure-tabel.<br/><br/>Deze eigenschap bepaalt of bestaande rijen in de uitvoertabel met de bijbehorende partitie-en recordsleutels hebben hun waarden vervangen of samenvoegen. <br/><br/>Zie voor meer informatie over de werking van deze instellingen (samenvoegen en vervangen), [invoegen of samenvoegen entiteit](https://msdn.microsoft.com/library/azure/hh452241.aspx) en [invoegen of vervangen entiteit](https://msdn.microsoft.com/library/azure/hh452242.aspx) onderwerpen. <br/><br> Deze instelling is van toepassing op het rijniveau van de, niet in de tabelniveau en geen van beide optie verwijdert rijen in de uitvoertabel die niet zijn opgenomen in de invoer. |samenvoegen (standaard)<br/>vervangen |Nee |
| WriteBatchSize |Voegt de gegevens in de Azure-tabel wanneer de writeBatchSize of writeBatchTimeout is bereikt. |Geheel getal (aantal rijen) |Nee (standaard: 10000) |
| writeBatchTimeout |Voegt de gegevens in de Azure-tabel wanneer de writeBatchSize of writeBatchTimeout is bereikt |timespan<br/><br/>Voorbeeld: "00: 20:00" (20 minuten) |Nee (standaard ingesteld op de standaardtime-out opslag client waarde 90 sec.) |

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Een bronkolom worden toegewezen aan een doelkolom met de translator JSON-eigenschap voordat u de doelkolom als de azureTablePartitionKeyName gebruiken kunt.

In het volgende voorbeeld is de bronkolom DivisionID toegewezen aan de doelkolom: DivisionID.  

```JSON
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```
De DivisionID is opgegeven als de partitiesleutel.

```JSON
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID",
    "writeBatchSize": 100,
    "writeBatchTimeout": "01:00:00"
}
```
## <a name="json-examples"></a>JSON-voorbeelden
De volgende voorbeelden geven een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe het kopiëren van gegevens naar en vanuit Azure Table Storage en Azure Blob-Database. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** uit een van de bronnen aan een van de ondersteunde Put. Zie voor meer informatie de sectie 'ondersteunde gegevensarchieven en indelingen' in [gegevens verplaatsen met behulp van Kopieeractiviteit](data-factory-data-movement-activities.md).

## <a name="example-copy-data-from-azure-table-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren van Azure Table naar Azure Blob
Het volgende voorbeeld laat zien:

1. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (gebruikt voor tabel- en blob).
2. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureTable](#dataset-properties).
3. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. De [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [AzureTableSource](#activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld worden gegevens die behoren tot de standaardpartitie in een Azure-tabel naar een blob eenmaal per uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**Gekoppelde Azure storage-service:**

```JSON
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
Azure Data Factory ondersteunt twee typen gekoppelde Azure Storage-services: **AzureStorage** en **AzureStorageSas**. U de verbindingsreeks met de accountsleutel opgeven voor de eerste versie, en voor de latere versie, geeft u de Shared Access Signature (SAS)-Uri. Zie [gekoppelde Services](#linked-service-properties) sectie voor meer informatie.  

**Azure Table-invoer gegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MyTable' gemaakt in Azure-tabel.

Instellen van "extern": "true" informeert de Data Factory-service dat de dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory gemaakt wordt.

```JSON
{
  "name": "AzureTableInput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

**Azure Blob-uitvoergegevensset:**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1). Het pad naar de map voor de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

```JSON
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

**De kopieeractiviteit in een pijplijn met AzureTableSource en BlobSink:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **AzureTableSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven met **AzureTableSourceQuery** eigenschap selecteert u de gegevens uit de standaardpartitie elk uur om te kopiëren.

```JSON
{
    "name":"SamplePipeline",
    "properties":{
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[
            {
                "name": "AzureTabletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureTableInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "AzureTableSource",
                        "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
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

## <a name="example-copy-data-from-azure-blob-to-azure-table"></a>Voorbeeld: Gegevens kopiëren van Azure-Blob naar Azure-tabel
Het volgende voorbeeld laat zien:

1. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (gebruikt voor tabel- en blob)
2. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
3. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureTable](#dataset-properties).
4. De [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) en [AzureTableSink](#copy-activity-properties).

De voorbeeld-kopieën time series-gegevens van een Azure blob naar een Azure tabel per uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**Gekoppelde Azure storage (voor Azure Table en Blob)-service:**

```JSON
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

Azure Data Factory ondersteunt twee typen gekoppelde Azure Storage-services: **AzureStorage** en **AzureStorageSas**. U de verbindingsreeks met de accountsleutel opgeven voor de eerste versie, en voor de latere versie, geeft u de Shared Access Signature (SAS)-Uri. Zie [gekoppelde Services](#linked-service-properties) sectie voor meer informatie.

**Azure Blob-invoergegevensset:**

Gegevens wordt opgehaald uit een nieuwe blob elk uur (frequentie: uur en interval: 1). Pad en naam van de map voor de blob worden dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map gebruikt jaar, maand en dag deel uit van de begintijd en de bestandsnaam wordt gebruikt voor het uur deel van de begintijd. "extern": "true" instelling informeert de Data Factory-service dat de dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory gemaakt wordt.

```JSON
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

**Azure Table-uitvoergegevensset:**

Het voorbeeld worden gegevens gekopieerd naar een tabel met de naam 'MyTable' in de Azure-tabel. Zoals u verwacht dat de Blob-CSV-bestand bevat, moet u een Azure-tabel maken met hetzelfde aantal kolommen. Nieuwe rijen zijn toegevoegd aan de tabel om het uur.

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

**De kopieeractiviteit in een pijplijn met BlobSource en AzureTableSink:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **BlobSource** en **sink** type is ingesteld op **AzureTableSink**.

```JSON
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[
      {
        "name": "AzureBlobtoTable",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureTableOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "AzureTableSink",
            "writeBatchSize": 100,
            "writeBatchTimeout": "01:00:00"
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
## <a name="type-mapping-for-azure-table"></a>Toewijzing van het type voor Azure-tabel
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieeractiviteit wordt uitgevoerd automatisch typeconversies van typen gegevensbronnen met sink-type met de volgende methode voor verificatie in twee stappen.

1. Systeemeigen brontypen converteren naar .NET-type
2. .NET-type converteren naar systeemeigen sink-type

Wanneer het verplaatsen van gegevens naar en van Azure Table, de volgende [toewijzingen die zijn gedefinieerd door Azure Table-service](https://msdn.microsoft.com/library/azure/dd179338.aspx) worden gebruikt vanaf Azure tabel OData typen .NET-type en vice versa.

| OData-gegevenstype | .NET-type | Details |
| --- | --- | --- |
| Edm.Binary |byte[] |Een matrix van bytes maximaal 64 KB. |
| Edm.Boolean |bool |Een Booleaanse waarde. |
| Edm.DateTime |DateTime |Een 64-bits waarde wordt uitgedrukt als Coordinated Universal Time (UTC). Het ondersteunde bereik voor de datum/tijd van kracht vanaf 12:00 middernacht, 1 januari 1601 A.D. (C.E.), UTC. Het bereik eindigt op 31 December 9999. |
| Edm.Double |double |Een 64-bits drijvende-kommawaarde. |
| Edm.Guid |GUID |Een globaal unieke id van 128-bits. |
| Edm.Int32 |Int32 |Een 32-bits geheel getal zijn. |
| Edm.Int64 |Int64 |Een 64-bits geheel getal zijn. |
| Edm.String |Reeks |Een waarde UTF-16-codering. Tekenreekswaarden kunnen maximaal 64 KB zijn. |

### <a name="type-conversion-sample"></a>Type conversie voorbeeld
Het volgende voorbeeld is voor het kopiëren van gegevens uit een Azure-Blob naar Azure-tabel met typeconversies.

Stel dat u de Blob-gegevensset in CSV-indeling en bevat drie kolommen. Een van beide is een datum/tijd-kolom met een aangepaste datum / tijdindeling met behulp van Franse afkortingen voor de dag van de week.

Definieer de bron van de Blob-gegevensset als volgt samen met de definities voor de kolommen.

```JSON
{
    "name": " AzureBlobInput",
    "properties":
    {
        "structure":
        [
            { "name": "userid", "type": "Int64"},
            { "name": "name", "type": "String"},
            { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
        ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName":"myfile.csv",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "external": true,
        "availability":
        {
            "frequency": "Hour",
            "interval": 1,
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
Toewijzing van het type van Azure Table OData-type opgegeven voor .NET-type, zou u de tabel in Azure-tabel definiëren met het volgende schema.

**Azure Table schema:**

| Kolomnaam | Type |
| --- | --- |
| gebruikers-id |Edm.Int64 |
| naam |Edm.String |
| lastlogindate |Edm.DateTime |

Vervolgens definieert u de Azure Table-gegevensset als volgt. U hoeft niet opgeven voor "structuur" sectie met informatie over het omdat informatie over het al is opgegeven in het onderliggende gegevensarchief.

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

In dit geval Data Factory automatisch gegevenstypeconversies met inbegrip van de datum/tijd-veld met de aangepaste datum / tijdindeling met behulp van de cultuur "fr-fr" bij het verplaatsen van gegevens uit Blob naar Azure-tabel.

> [!NOTE]
> Zie het toewijzen van kolommen in de brongegevensset op kolommen uit de sink-gegevensset [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren, [prestaties kopiëren en Afstemmingshandleiding](data-factory-copy-activity-performance.md).
