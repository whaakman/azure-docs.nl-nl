---
title: Gegevens verplaatsen naar/van Azure Table | Microsoft Docs
description: Informatie over het verplaatsen van gegevens van Azure Table Storage met Azure Data Factory.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 07b046b1-7884-4e57-a613-337292416319
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 765ca21c7c38fa116e0ca95b3c8dc6a6152834ce
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a>Gegevens verplaatsen en naar Azure Table met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-azure-table-connector.md)
> * [Versie 2 - Preview](../connector-azure-table-storage.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [Azure Table Storage-connector in V2](../connector-azure-table-storage.md).

Dit artikel wordt uitgelegd dat het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens uit Azure Table Storage te verplaatsen. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, hetgeen een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit toont. 

U kunt gegevens kopiëren van een ondersteunde brongegevensarchief naar Azure Table Storage of Azure Table Storage met alle ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven als bronnen of PUT wordt ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. 

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens uit een Azure-tabelopslag verplaatst met behulp van verschillende hulpprogramma's voor API's.

De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren: 

1. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory.
2. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor voorbeelden met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar/van een Azure-tabelopslag [JSON voorbeelden](#json-examples) sectie van dit artikel. 

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Azure Table Storage: 

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
Er zijn twee soorten gekoppelde services die kunt u een Azure blob storage koppelen aan een Azure data factory. Ze zijn: **AzureStorage** gekoppelde service en **AzureStorageSas** gekoppelde service. De gekoppelde Azure Storage-service biedt de gegevensfactory met globale toegang tot Azure Storage. Terwijl de Azure Storage SAS (Shared Access Signature) gekoppelde biedt service de gegevensfactory met de toegang beperkt/tijdsgebonden naar Azure Storage. Er zijn geen andere verschillen tussen deze twee gekoppelde services. Kies de gekoppelde service die bij uw behoeften past. De volgende secties bevatten meer details over deze twee gekoppelde services.

[!INCLUDE [data-factory-azure-storage-linked-services](../../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets van de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle gegevensset typen (Azure SQL, Azure blob-, Azure-tabel, enz.).

De sectie typeProperties verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De **typeProperties** sectie voor de gegevensset van het type **AzureTable** heeft de volgende eigenschappen.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in de Azure-tabel Database-instantie waarnaar de gekoppelde service verwijst. |Ja. Wanneer een tabelnaam is opgegeven zonder een azureTableSourceQuery, worden alle records uit de tabel worden gekopieerd naar de bestemming. Als een azureTableSourceQuery ook is opgegeven, worden de records uit de tabel die voldoet aan de query gekopieerd naar de bestemming. |

### <a name="schema-by-data-factory"></a>Schema door Data Factory
Voor gegevens zonder schema winkels zoals Azure Table afleidt de Data Factory-service het schema in een van de volgende manieren:

1. Als u de structuur van gegevens met behulp van opgeven de **structuur** eigenschap in de definitie van de gegevensset, de Data Factory-service zich houdt aan deze structuur als het schema. Als een rij geen waarde voor een kolom bevat, wordt in dit geval een null-waarde opgegeven voor deze.
2. Als u de structuur van gegevens niet via opgeeft de **structuur** het schema-eigenschap in de definitie van de gegevensset, Data Factory afleidt met behulp van de eerste rij in de gegevens. Als de eerste rij niet het volledige schema bevat, worden sommige kolommen in dit geval wordt opgehaald in het resultaat van de kopieerbewerking.

Daarom voor gegevensbronnen zonder schema, de aanbevolen procedure is om op te geven van de structuur van gegevens met de **structuur** eigenschap.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten van de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer gegevenssets en beleidsregels zijn beschikbaar voor alle typen activiteiten.

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit wordt aan de andere kant variëren met elk activiteitstype. Voor de kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put.

**AzureTableSource** ondersteunt de volgende eigenschappen in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| azureTableSourceQuery |Gebruik de aangepaste query om gegevens te lezen. |Azure-tabel queryreeks. Zie de voorbeelden in de volgende sectie. |Nee. Wanneer een tabelnaam is opgegeven zonder een azureTableSourceQuery, worden alle records uit de tabel worden gekopieerd naar de bestemming. Als een azureTableSourceQuery ook is opgegeven, worden de records uit de tabel die voldoet aan de query gekopieerd naar de bestemming. |
| azureTableSourceIgnoreTableNotFound |Geef aan of de uitzondering van de tabel slikken bestaat niet. |DE WAARDE TRUE<br/>DE WAARDE FALSE |Nee |

### <a name="azuretablesourcequery-examples"></a>Voorbeelden van azureTableSourceQuery
Als Azure Table-kolom van het tekenreekstype:

```JSON
azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"
```

Als Azure Table-kolom van het type datetime:

```JSON
"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"
```

**AzureTableSink** ondersteunt de volgende eigenschappen in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| azureTableDefaultPartitionKeyValue |Standaardwaarde voor de partitiesleutel die kan worden gebruikt door de sink. |Een string-waarde. |Nee |
| azureTablePartitionKeyName |Geef de naam van de kolom waarvan de waarden worden gebruikt als partitiesleutels. Als niet wordt opgegeven, wordt AzureTableDefaultPartitionKeyValue gebruikt als de partitiesleutel. |De naam van een kolom. |Nee |
| azureTableRowKeyName |Geef de naam van de kolom waarvan de kolomwaarden worden gebruikt als de rijsleutel. Als niet wordt opgegeven, gebruikt u een GUID voor elke rij. |De naam van een kolom. |Nee |
| azureTableInsertType |De modus voor gegevens in Azure-tabel invoegen.<br/><br/>Deze eigenschap bepaalt of bestaande rijen in de uitvoertabel met partitie-als rijsleutels die overeenkomt met de waarden vervangen of samenvoegen van hebben. <br/><br/>Zie voor meer informatie over de werking van deze instellingen (merge en vervangen), [invoegen of Merge entiteit](https://msdn.microsoft.com/library/azure/hh452241.aspx) en [invoegen of vervangen entiteit](https://msdn.microsoft.com/library/azure/hh452242.aspx) onderwerpen. <br/><br> Deze instelling is van toepassing op het rijniveau van niet tabelniveau, en geen van beide optie worden de rijen in de uitvoertabel die niet bestaan in de invoer verwijderd. |samenvoegen (standaard)<br/>vervangen |Nee |
| writeBatchSize |Voegt de gegevens in de Azure-tabel wanneer de writeBatchSize of writeBatchTimeout is bereikt. |Geheel getal (aantal rijen) |Nee (standaard: 10000) |
| writeBatchTimeout |Voegt de gegevens in de Azure-tabel wanneer de writeBatchSize of writeBatchTimeout is bereikt. |TimeSpan<br/><br/>Voorbeeld: "00: 20:00 ' (20 minuten) |Nee (voor opslag client standaardtime-out standaardwaarde is 90 sec) |

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Een bronkolom worden toegewezen aan een doelkolom met behulp van de vertaler JSON-eigenschap voordat u de doelkolom als de azureTablePartitionKeyName gebruiken kunt.

In het volgende voorbeeld bronkolom DivisionID is toegewezen aan de doelkolom: DivisionID.  

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
De volgende voorbeelden geven voorbeeld JSON definities die u een pijplijn maken kunt met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe om gegevens te kopiëren en naar Azure Table Storage en Azure Blob-Database. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** uit een van de bronnen aan een van de ondersteunde Put. Zie voor meer informatie de sectie 'ondersteunde gegevensarchieven en indelingen' in [verplaatsen van gegevens met behulp van de Kopieeractiviteit](data-factory-data-movement-activities.md).

## <a name="example-copy-data-from-azure-table-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren van Azure Table naar Azure Blob
Het volgende voorbeeld toont:

1. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (gebruikt voor blob & tabel).
2. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureTable](#dataset-properties).
3. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. De [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [AzureTableSource](#activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld kopieert de gegevens die behoren tot de standaardpartitie in een Azure-tabel naar een blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

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
Azure Data Factory ondersteunt twee soorten gekoppelde Azure Storage-services: **AzureStorage** en **AzureStorageSas**. U de verbindingsreeks met de sleutel van de account opgeven voor de eerste en voor de latere, geeft u de Shared Access Signature (SAS)-Uri. Zie [gekoppelde Services](#linked-service-properties) sectie voor meer informatie.  

**Azure Table invoergegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u een tabel "MijnTabel" hebt gemaakt in Azure-tabel.

Instelling 'extern': 'true' informeert de Data Factory-service dat de dataset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

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

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad maakt gebruik van jaar, maand, dag en uur delen van de begintijd.

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

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **AzureTableSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query opgegeven met **AzureTableSourceQuery** eigenschap selecteert de gegevens uit de standaardpartitie elk uur te kopiëren.

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

## <a name="example-copy-data-from-azure-blob-to-azure-table"></a>Voorbeeld: Gegevens kopiëren van Azure-Blob naar Azure Table
Het volgende voorbeeld toont:

1. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (gebruikt voor blob & tabel)
2. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
3. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureTable](#dataset-properties).
4. De [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) en [AzureTableSink](#copy-activity-properties).

De voorbeeld-kopieën timeseries gegevens van een Azure-blob naar een Azure tabel per uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**Gekoppelde Azure storage (voor Azure Table & Blob)-service:**

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

Azure Data Factory ondersteunt twee soorten gekoppelde Azure Storage-services: **AzureStorage** en **AzureStorageSas**. U de verbindingsreeks met de sleutel van de account opgeven voor de eerste en voor de latere, geeft u de Shared Access Signature (SAS)-Uri. Zie [gekoppelde Services](#linked-service-properties) sectie voor meer informatie.

**Azure Blob invoergegevensset:**

Gegevens wordt opgehaald uit een nieuwe blob elk uur (frequentie: uur, interval: 1). Pad en naam van de map voor de blob worden dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad gebruikt jaar, maand en dag deel uit van de begintijd en de bestandsnaam wordt gebruikt voor het uur deel van de begintijd. "extern": "true" instelling informeert de Data Factory-service dat de dataset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

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

**Azure-tabel uitvoergegevensset:**

Het voorbeeld worden gegevens gekopieerd naar een tabel met de naam "MijnTabel" in de Azure-tabel. Maak een Azure-tabel met hetzelfde aantal kolommen, zoals u verwacht dat de Blob-CSV-bestand bevatten. Nieuwe rijen worden toegevoegd aan de tabel om het uur.

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

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **BlobSource** en **sink** type is ingesteld op **AzureTableSink**.

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
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieerbewerking wordt automatische typeconversies van brontypen opvangen typen met de volgende benadering voor in twee stappen uitgevoerd.

1. Systeemeigen brontypen converteren naar .NET-type
2. Converteren van .NET-type naar systeemeigen sink-type

Wanneer u gegevens verplaatst naar & van Azure-tabel, de volgende [toewijzingen die zijn gedefinieerd door de service Azure Table](https://msdn.microsoft.com/library/azure/dd179338.aspx) worden gebruikt vanuit Azure tabel OData-typen aan .NET-type en vice versa.

| OData-gegevenstype | .NET-type | Details |
| --- | --- | --- |
| Edm.Binary |byte[] |Een matrix met bytes maximaal 64 KB. |
| Edm.Boolean |BOOL |Een Booleaanse waarde. |
| Edm.DateTime |Datum en tijd |Een 64-bits waarde wordt uitgedrukt als Coordinated Universal Time (UTC). Het ondersteunde bereik van de datum-/ begint vanaf 12:00 middernacht, 1 januari 1601 A.D. (C.E.) UTC. Het bereik eindigt op 31 December 9999. |
| Edm.Double |dubbel |Een 64-bits drijvende-kommawaarde. |
| Edm.Guid |GUID |Een globally unique identifier van 128-bits. |
| Edm.Int32 |Int32 |Een 32-bits geheel getal. |
| Edm.Int64 |Int64 |Een 64-bits geheel getal. |
| Edm.String |Tekenreeks |Een waarde UTF-16-codering. Tekenreekswaarden mogelijk maximaal 64 KB. |

### <a name="type-conversion-sample"></a>Type conversie voorbeeld
Het volgende voorbeeld is voor het kopiëren van gegevens van een Azure-Blob naar Azure Table met typeconversies.

Stel dat de Blob-gegevensset is CSV-indeling en drie kolommen bevat. Een van beide is een datetime-kolom met een aangepaste datum / tijdindeling met behulp van Franse afkortingen voor dag van de week.

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
Toewijzing van het type van Azure Table OData-type opgegeven voor .NET-type, zou u de tabel definiëren in Azure-tabel met het volgende schema.

**Azure tabelschema:**

| Kolomnaam | Type |
| --- | --- |
| gebruikers-id |Edm.Int64 |
| naam |Edm.String |
| lastlogindate |Edm.DateTime |

Definieer vervolgens als volgt de Azure Table-gegevensset. U hoeft niet "structuur" sectie met de type-informatie opgeven omdat de type-informatie al in het onderliggende gegevensarchief opgegeven is.

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

In dit geval Data Factory automatisch gegevenstypeconversies met inbegrip van de Datetime-veld met de aangepaste datum / tijdindeling met behulp van de cultuur "fr-fr" bij het verplaatsen van gegevens uit Blob naar Azure Table.

> [!NOTE]
> Zie het toewijzen van kolommen uit de bron-gegevensset naar kolommen uit de dataset sink [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestaties en het afstemmen
Zie voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren, [uitvoering van de activiteit & afstemmen handleiding](data-factory-copy-activity-performance.md).
