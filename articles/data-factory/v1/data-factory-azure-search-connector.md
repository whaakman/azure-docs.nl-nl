---
title: Push-gegevens naar de Search-index met behulp van de Data Factory | Microsoft Docs
description: Meer informatie over de push-gegevens naar Azure Search-Index met behulp van Azure Data Factory.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f8d46e1e-5c37-4408-80fb-c54be532a4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: d8848f93518392333df16c9c7bf07bd0b2529034
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="push-data-to-an-azure-search-index-by-using-azure-data-factory"></a>Push-gegevens naar een Azure Search-index met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-azure-search-connector.md)
> * [Versie 2 - Preview](../connector-azure-search.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [Azure Search-connector in V2](../connector-azure-search.md).

In dit artikel wordt beschreven hoe de Kopieeractiviteit gebruiken om gegevens te pushen van een gegevensarchief ondersteunde bron voor Azure Search-index. Ondersteunde bron gegevensarchieven worden vermeld in de kolom bron van de [ondersteunde gegevensbronnen en put](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. In dit artikel is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, waarin Kopieeractiviteit en combinaties van ondersteunde gegevens store een algemeen overzicht van de verplaatsing van gegevens biedt.

## <a name="enabling-connectivity"></a>Connectiviteit inschakelen
Als u Data Factory-service verbinding maken met een on-premises gegevensopslag, moet u Data Management Gateway installeren in uw on-premises omgeving. U kunt de gateway installeren op dezelfde computer als hosts de brongegevens opslaan of op een afzonderlijke computer om te voorkomen dat concurrentie voor resources met de gegevensopslag.

Data Management Gateway verbindt on-premises gegevensbronnen met cloud-services in een veilige en beheerde manier. Zie [gegevens verplaatsen tussen on-premises en cloud](data-factory-move-data-between-onprem-and-cloud.md) voor meer informatie over Data Management Gateway.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens uit een gegevensopslag bron verstuurd naar Azure Search-index met behulp van verschillende hulpprogramma's voor API's.

De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren: 

1. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory.
2. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar Azure Search-index [JSON-voorbeeld: gegevens kopiëren van on-premises SQL-Server naar Azure Search-index](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) sectie van dit artikel. 

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor Azure Search-Index:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende tabel bevat beschrijvingen van JSON-elementen die specifiek voor de service Azure Search is gekoppeld zijn.

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| type | De eigenschap type moet worden ingesteld op: **AzureSearch**. | Ja |
| url | De URL voor de Azure Search-service. | Ja |
| sleutel | Administrator-code voor de Azure Search-service. | Ja |

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst met eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets en secties, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle typen van de gegevensset. De **typeProperties** sectie verschilt voor elk type dataset. De typeProperties sectie voor een gegevensset van het type **AzureSearchIndex** heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| type | De eigenschap type moet worden ingesteld op **AzureSearchIndex**.| Ja |
| indexName | De naam van de Azure Search-index. Data Factory maakt niet de index. De index moet bestaan in Azure Search. | Ja |


## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst met eigenschappen die beschikbaar zijn voor het definiëren van activiteiten en secties, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen op, zoals naam, beschrijving, invoer en uitvoertabellen en verschillende beleidsregels zijn beschikbaar voor alle typen activiteiten. Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties variëren met elk activiteitstype. Voor de Kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put.

Voor de Kopieeractiviteit, wanneer de sink van het type is **AzureSearchIndexSink**, de volgende eigenschappen beschikbaar zijn in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------------- | -------- |
| WriteBehavior | Geeft aan of samenvoegen of wanneer een document al in de index bestaat te vervangen. Zie de [WriteBehavior eigenschap](#writebehavior-property).| samenvoegen (standaard)<br/>Uploaden| Nee |
| WriteBatchSize | Gegevens geüpload naar de Azure Search-index wanneer de buffergrootte writeBatchSize bereikt. Zie de [WriteBatchSize eigenschap](#writebatchsize-property) voor meer informatie. | 1-1000. Standaardwaarde is 1000. | Nee |

### <a name="writebehavior-property"></a>De eigenschap WriteBehavior
AzureSearchSink upserts bij het schrijven van gegevens. Bij het schrijven van een document, als de documentsleutel al in de Azure Search-index bestaat, updates Azure Search met andere woorden, het bestaande document in plaats van er een conflict uitzondering is opgetreden.

De AzureSearchSink biedt de volgende twee upsert problemen (met behulp van AzureSearch SDK):

- **Samenvoegen**: alle kolommen in het nieuwe document met de huidige combineren. Voor kolommen met null-waarde in het nieuwe document, wordt de waarde in de bestaande waarde bewaard.
- **Uploaden**: nieuw document vervangt de bestaande waarde. Voor de kolommen is niet opgegeven in het nieuwe document, wordt de waarde ingesteld op null of er een niet-null-waarde in een bestaand document of niet is.

Het standaardgedrag **samenvoegen**.

### <a name="writebatchsize-property"></a>De eigenschap WriteBatchSize
Azure Search-service ondersteunt documenten schrijven als een batch. Een batch kunt 1-1000 acties bevatten. Een actie verwerkt één document als de upload/merge-bewerking wilt uitvoeren.

### <a name="data-type-support"></a>Ondersteuning voor gegevenstype
De volgende tabel geeft aan of een Azure Search-gegevenstype of niet wordt ondersteund.

| Azure Search-gegevenstype | Ondersteund in Azure Search Sink |
| ---------------------- | ------------------------------ |
| Tekenreeks | J |
| Int32 | J |
| Int64 | J |
| Double | J |
| Boole-waarde | J |
| DataTimeOffset | J |
| Tekenreeksmatrix | N |
| GeographyPoint | N |

## <a name="json-example-copy-data-from-on-premises-sql-server-to-azure-search-index"></a>JSON-voorbeeld: gegevens kopiëren van on-premises SQL-Server naar Azure Search-index

Het volgende voorbeeld toont:

1.  Een gekoppelde service van het type [AzureSearch](#linked-service-properties).
2.  Een gekoppelde service van het type [OnPremisesSqlServer](data-factory-sqlserver-connector.md#linked-service-properties).
3.  Invoer [gegevensset](data-factory-create-datasets.md) van het type [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).
4.  Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureSearchIndex](#dataset-properties).
4.  Een [pijplijn](data-factory-create-pipelines.md) met een kopieeractiviteit die gebruikmaakt van [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) en [AzureSearchIndexSink](#copy-activity-properties).

Het voorbeeld kopieert tijdreeks gegevens van een on-premises SQL Server database naar een Azure Search-index per uur. De JSON-eigenschappen die in dit voorbeeld worden in voorbeelden van de volgende secties beschreven.

Als een eerste stap het instellen van de data management gateway op uw on-premises machine. De instructies vindt u in de [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

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

**SQL Server gekoppeld-service**

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

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MijnTabel' gemaakt in SQL Server en deze bevat een kolom met de naam 'timestampcolumn' voor de reeksgegevens. U kunt een query over meerdere tabellen binnen dezelfde database met behulp van een één gegevensset, maar één tabel moet worden gebruikt voor de gegevensset tableName typeProperty.

Instelling 'extern': 'true' informeert Data Factory-service dat de dataset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

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

**Azure Search uitvoergegevensset:**

Het voorbeeld worden gegevens gekopieerd naar een Azure Search-index met de naam **producten**. Data Factory maakt niet de index. Als u wilt testen van de steekproef, moet u een index maken met deze naam. De Azure Search-index maken met hetzelfde aantal kolommen in de invoergegevensset. Nieuwe items worden toegevoegd aan de Azure Search-index om het uur.

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

**De kopieeractiviteit in een pijplijn met de SQL-bron en sink van Azure Search-Index:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **SqlSource** en **sink** type is ingesteld op **AzureSearchIndexSink**. De SQL-query die is opgegeven voor de **SqlReaderQuery** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

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

Als u gegevens uit een cloud-gegevensarchief in Azure Search kopiëren wilt `executionLocation` eigenschap is vereist. De volgende JSON-fragment toont de wijziging nodig onder Kopieeractiviteit `typeProperties` als voorbeeld. Controleer [kopiëren van gegevens tussen gegevensarchieven cloud](data-factory-data-movement-activities.md#global) sectie voor meer details en de ondersteunde waarden.

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


## <a name="copy-from-a-cloud-source"></a>Kopiëren van een cloud-bron
Als u gegevens uit een cloud-gegevensarchief in Azure Search kopiëren wilt `executionLocation` eigenschap is vereist. De volgende JSON-fragment toont de wijziging nodig onder Kopieeractiviteit `typeProperties` als voorbeeld. Controleer [kopiëren van gegevens tussen gegevensarchieven cloud](data-factory-data-movement-activities.md#global) sectie voor meer details en de ondersteunde waarden.

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

U kunt ook kolommen uit de bron-gegevensset naar kolommen uit sink gegevensset in de definitie van de activiteit kopiëren toewijzen. Zie voor meer informatie [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen  
Zie de [Kopieeractiviteit prestaties en prestatieafstemming handleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) en verschillende manieren om te optimaliseren.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

* [Zelfstudie voor kopiëren-activiteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een Kopieeractiviteit.
