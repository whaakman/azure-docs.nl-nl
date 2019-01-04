---
title: Gegevens pushen naar Search-index met behulp van Data Factory | Microsoft Docs
description: Meer informatie over hoe u gegevens naar Azure Search-Index pushen met behulp van Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: f8d46e1e-5c37-4408-80fb-c54be532a4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 4d3c67974bc1dd0e52d3de457071d550a6379e36
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54023078"
---
# <a name="push-data-to-an-azure-search-index-by-using-azure-data-factory"></a>Gegevens pushen naar een Azure Search-index met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-azure-search-connector.md)
> * [Versie 2 (huidige versie)](../connector-azure-search.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [Azure Search-connector in V2](../connector-azure-search.md).

In dit artikel wordt beschreven hoe u gebruik van de Kopieeractiviteit om gegevens te pushen van een ondersteund brongegevensarchief naar Azure Search-index. Ondersteunde bron-gegevensopslag worden vermeld in de kolom bron van de [ondersteunde bronnen en sinks](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. In dit artikel is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de Kopieeractiviteit en combinaties van ondersteunde data store geeft.

## <a name="enabling-connectivity"></a>Verbindingen inschakelen
Als u wilt toestaan dat Data Factory-service verbinding maken met een on-premises gegevensarchief, kunt u Data Management Gateway installeren in uw on-premises omgeving. U kunt de gateway installeren op dezelfde computer waar de brongegevens hosts worden opgeslagen of op een afzonderlijke computer om te voorkomen en dingen om de resources met het gegevensarchief.

Data Management Gateway verbonden on-premises gegevensbronnen met cloud-services op een veilige, beheerde manier. Zie [gegevens verplaatsen tussen on-premises en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor meer informatie over Data Management Gateway.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens van een brongegevensarchief naar Azure Search-index gepusht met behulp van verschillende hulpprogramma's / API's.

De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en  **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren: 

1. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory.
2. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar Azure Search-index, [JSON-voorbeeld: Gegevens kopiëren van on-premises SQL Server naar Azure Search-index](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) sectie van dit artikel. 

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Azure Search-Index:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende tabel bevat beschrijvingen van JSON-elementen die specifiek voor de gekoppelde Azure-Search-service zijn.

| Eigenschap | Description | Vereist |
| -------- | ----------- | -------- |
| type | De eigenschap type moet worden ingesteld op: **AzureSearch**. | Ja |
| url | De URL voor de Azure Search-service. | Ja |
| sleutel | Administrator-code voor de Azure Search-service. | Ja |

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets en secties, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van de gegevensset. De **typeProperties** sectie verschilt voor elk type gegevensset. De typeProperties voor een gegevensset van het type sectie **AzureSearchIndex** heeft de volgende eigenschappen:

| Eigenschap | Description | Vereist |
| -------- | ----------- | -------- |
| type | De eigenschap type moet worden ingesteld op **AzureSearchIndex**.| Ja |
| indexName | De naam van de Azure Search-index. Data Factory maakt niet de index. De index moet bestaan in Azure Search. | Ja |


## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van activiteiten en secties, de [het maken van pijplijnen](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoertabellen en verschillende beleidsregels zijn beschikbaar voor alle soorten activiteiten. Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties variëren met elk activiteitstype. Ze verschillen voor de Kopieeractiviteit, afhankelijk van de typen van bronnen en sinks.

Voor de Kopieeractiviteit, wanneer de sink van het type is **AzureSearchIndexSink**, de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

| Eigenschap | Description | Toegestane waarden | Vereist |
| -------- | ----------- | -------------- | -------- |
| WriteBehavior | Hiermee geeft u op of u wilt samenvoegen of vervangen wanneer een document al in de index bestaat. Zie de [WriteBehavior eigenschap](#writebehavior-property).| samenvoegen (standaard)<br/>Uploaden| Nee |
| WriteBatchSize | Wanneer de buffergrootte writeBatchSize bereikt, uploadt u gegevens in de Azure Search-index. Zie de [WriteBatchSize eigenschap](#writebatchsize-property) voor meer informatie. | 1-1000. Standaardwaarde is 1000. | Nee |

### <a name="writebehavior-property"></a>De eigenschap WriteBehavior
AzureSearchSink upsert-bewerking bij het schrijven van gegevens. Bij het schrijven van een document, als de documentsleutel al in de Azure Search-index bestaat, werkt Azure Search met andere woorden, het bestaande document in plaats van die een conflict uitzondering veroorzaakt.

De AzureSearchSink biedt de volgende twee upsert gedrag (met behulp van SDK AzureSearch):

- **Samenvoegen**: alle kolommen in het nieuwe document met de bestaande combineren. Voor kolommen met een null-waarde in het nieuwe document, wordt de waarde in het bestaande bestand behouden.
- **Uploaden**: Het nieuwe document wordt het bestaande bestand vervangen. Voor de kolommen is niet opgegeven in het nieuwe document, wordt de waarde ingesteld op null of er een niet-null-waarde in het bestaande document of niet is.

Het standaardgedrag **samenvoegen**.

### <a name="writebatchsize-property"></a>De eigenschap WriteBatchSize
Azure Search-service ondersteunt documenten schrijven als een batch. Een batch kan 1 naar 1000 acties bevatten. Een actie verwerkt één document als het uploaden/merge-bewerking wilt uitvoeren.

### <a name="data-type-support"></a>Ondersteuning voor gegevenstype
De volgende tabel geeft aan of een Azure Search-gegevenstype of niet wordt ondersteund.

| Azure Search-gegevenstype | Ondersteund in Azure Search-Sink |
| ---------------------- | ------------------------------ |
| Reeks | J |
| Int32 | J |
| Int64 | J |
| Double-waarde | J |
| Booleaans | J |
| DataTimeOffset | J |
| String-matrix | N |
| GeographyPoint | N |

## <a name="json-example-copy-data-from-on-premises-sql-server-to-azure-search-index"></a>JSON-voorbeeld: Gegevens kopiëren van on-premises SQL Server naar Azure Search-index

Het volgende voorbeeld laat zien:

1.  Een gekoppelde service van het type [AzureSearch](#linked-service-properties).
2.  Een gekoppelde service van het type [OnPremisesSqlServer](data-factory-sqlserver-connector.md#linked-service-properties).
3.  Invoer [gegevensset](data-factory-create-datasets.md) van het type [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).
4.  Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureSearchIndex](#dataset-properties).
4.  Een [pijplijn](data-factory-create-pipelines.md) met een kopieeractiviteit die gebruikmaakt van [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) en [AzureSearchIndexSink](#copy-activity-properties).

Het voorbeeld tijdreeksen worden gegevens gekopieerd van een on-premises SQL Server-database naar een Azure Search-index per uur. De JSON-eigenschappen die in dit voorbeeld worden beschreven in de secties na de voorbeelden.

Instellen als eerste stap de data management gateway op uw on-premises computer. De instructies vindt u in de [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

**Azure Search gekoppelde service:**

```JSON
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

**Gekoppelde SQL Server-service**

```JSON
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

**SQL Server-invoergegevensset**

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MyTable' gemaakt in SQL Server en bevat een kolom met de naam 'timestampcolumn' voor time series-gegevens. U kunt een query over meerdere tabellen binnen dezelfde database met behulp van een enkele gegevensset, maar één tabel moet worden gebruikt voor van de gegevensset tableName typeProperty.

Instellen van "extern": "true" wordt geïnformeerd Data Factory-service dat de dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory wordt geproduceerd.

```JSON
{
  "name": "SqlServerDataset",
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

**Azure Search-uitvoergegevensset:**

Het voorbeeld worden gegevens gekopieerd naar een Azure Search-index met de naam **producten**. Data Factory maakt niet de index. Als u wilt testen van het voorbeeld, een index te maken met deze naam. De Azure Search-index maken met hetzelfde aantal kolommen in de invoergegevensset. Nieuwe vermeldingen toegevoegd aan de Azure Search-index elk uur.

```JSON
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties" : {
            "indexName": "products",
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
   }
}
```

**De kopieeractiviteit in een pijplijn met de SQL-bron en sink voor Azure Search-Index:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **SqlSource** en **sink** type is ingesteld op **AzureSearchIndexSink**. De SQL-query die is opgegeven voor de **SqlReaderQuery** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "SqlServertoAzureSearchIndex",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSearchIndexDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "AzureSearchIndexSink"
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

Als u gegevens uit een cloudgegevensopslag in Azure Search kopiëren wilt, `executionLocation` eigenschap is vereist. De volgende JSON-fragment ziet u de wijziging die nodig zijn bij de Kopieeractiviteit `typeProperties` als voorbeeld. Controleer [kopiëren van gegevens tussen gegevensarchieven in cloud](data-factory-data-movement-activities.md#global) sectie voor ondersteunde waarden en meer details.

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```


## <a name="copy-from-a-cloud-source"></a>Kopiëren van een cloudbron
Als u gegevens uit een cloudgegevensopslag in Azure Search kopiëren wilt, `executionLocation` eigenschap is vereist. De volgende JSON-fragment ziet u de wijziging die nodig zijn bij de Kopieeractiviteit `typeProperties` als voorbeeld. Controleer [kopiëren van gegevens tussen gegevensarchieven in cloud](data-factory-data-movement-activities.md#global) sectie voor ondersteunde waarden en meer details.

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```

Ook kunt u kolommen uit de brongegevensset op kolommen uit de sink-gegevensset in het definitie van de activiteit kopiëren toewijzen. Zie voor meer informatie, [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen  
Zie de [Kopieeractiviteit prestatie- en afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) en de verschillende manieren om te optimaliseren.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

* [Zelfstudie kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een Kopieeractiviteit.
