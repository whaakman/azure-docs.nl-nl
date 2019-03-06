---
title: Gegevens verplaatsen naar/van Azure Cosmos DB | Microsoft Docs
description: Informatie over hoe gegevens verplaatsen naar/van Azure Cosmos DB-verzameling met behulp van Azure Data Factory
services: data-factory, cosmosdb
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: c9297b71-1bb4-4b29-ba3c-4cf1f5575fac
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: bda3df3ce869d7717f572f72c38472e7eae4a0ef
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57437425"
---
# <a name="move-data-to-and-from-azure-cosmos-db-using-azure-data-factory"></a>Gegevens verplaatsen naar en van Azure Cosmos DB met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-azure-documentdb-connector.md)
> * [Versie 2 (huidige versie)](../connector-azure-cosmos-db.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [Azure Cosmos DB-connector in V2](../connector-azure-cosmos-db.md).

In dit artikel wordt uitgelegd hoe u van de Kopieeractiviteit in Azure Data Factory om gegevens naar/van Azure Cosmos DB (SQL-API) te verplaatsen. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit geeft.

U kunt gegevens kopiëren van een ondersteunde bron-gegevensopslag met Azure Cosmos DB of Azure Cosmos DB op alle ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als gegevensbronnen of PUT voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel.

> [!IMPORTANT]
> Azure Cosmos DB-connector biedt alleen ondersteuning voor de SQL-API.

Het kopiëren van gegevens als-is naar/van JSON-bestanden of een andere Cosmos DB-verzameling, Zie [Import/Export-JSON-documenten](#importexport-json-documents).

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens naar/van Azure Cosmos DB met behulp van verschillende hulpprogramma's / API's worden verplaatst.

De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en  **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit.

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren:

1. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory.
2. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking.
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer.

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling. Zie voor voorbeelden met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar/van Cosmos DB [JSON voorbeelden](#json-examples) sectie van dit artikel.

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met Cosmos DB:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor Azure Cosmos DB gekoppelde service.

| **Eigenschap** | **Beschrijving** | **Vereist** |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **DocumentDb** |Ja |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met Azure Cosmos DB-database. |Ja |

Voorbeeld:

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Raadpleeg voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van gegevensset (Azure SQL, Azure-blob, Azure-tabel, enz.).

De sectie typeProperties verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **DocumentDbCollection** heeft de volgende eigenschappen.

| **Eigenschap** | **Beschrijving** | **Vereist** |
| --- | --- | --- |
| collectionName |De naam van de Cosmos DB-documentverzameling. |Ja |

Voorbeeld:

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
### <a name="schema-by-data-factory"></a>Schema door Data Factory
Voor de winkels schemavrije gegevens, zoals Azure Cosmos DB bepaalt de Data Factory-service het schema in een van de volgende manieren:

1. Als u de structuur van gegevens met behulp van de **structuur** eigenschap in de definitie van de Data Factory-service zich houdt aan deze structuur als het schema. Als een rij geen waarde voor een kolom bevat, wordt er in dit geval een null-waarde worden opgegeven voor deze.
2. Als u de structuur van de gegevens niet via opgeeft de **structuur** eigenschap in de definitie van de Data Factory-service bepaalt welk schema met behulp van de eerste rij in de gegevens. In dit geval als de eerste rij niet het volledige schema bevat, sommige kolommen worden ontbreekt in het resultaat van de kopieerbewerking.

Daarom voor gegevensbronnen zonder schema, de aanbevolen procedure is om op te geven van de structuur van het gebruik van gegevens de **structuur** eigenschap.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Raadpleeg voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle soorten activiteiten.

> [!NOTE]
> De Kopieeractiviteit slechts één invoer en produceert slechts één uitvoer.

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit aan de andere kant variëren met elk activiteitstype en in het geval van kopieeractiviteit ze variëren, afhankelijk van de typen van bronnen en sinks.

In het geval van kopieeractiviteit als bron van het type **DocumentDbCollectionSource** de volgende eigenschappen zijn beschikbaar in **typeProperties** sectie:

| **Eigenschap** | **Beschrijving** | **Toegestane waarden** | **Vereist** |
| --- | --- | --- | --- |
| query |Geef de query voor het lezen van gegevens. |De queryreeks wordt ondersteund door Azure Cosmos DB. <br/><br/>Voorbeeld: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Nee <br/><br/>Indien niet opgegeven, de SQL-instructie die wordt uitgevoerd: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Speciaal teken om aan te geven dat het document is genest |Willekeurig teken. <br/><br/>Azure Cosmos DB is een NoSQL-archief voor JSON-documenten, waarin geneste structuren zijn toegestaan. Azure Data Factory kan de gebruiker om aan te duiden hiërarchie via nestingSeparator, namelijk '. ' in de bovenstaande voorbeelden. Bij het scheidingsteken, de kopieeractiviteit genereren het object "Naam" met de drie onderliggende elementen eerst, midden- en achternaam, op basis van "Name.First", "Name.Middle" en "Name.Last" in de tabeldefinitie. |Nee |

**DocumentDbCollectionSink** ondersteunt de volgende eigenschappen:

| **Eigenschap** | **Beschrijving** | **Toegestane waarden** | **Vereist** |
| --- | --- | --- | --- |
| nestingSeparator |Er is een speciaal teken in naam van de bronkolom om aan te geven dat geneste document nodig. <br/><br/>Zoals hierboven: `Name.First` in de uitvoer van de tabel de volgende JSON-structuur in de Cosmos DB-document maakt:<br/><br/>"Naam": {<br/>    'First': "John"<br/>}, |Teken dat wordt gebruikt voor het scheiden van geneste niveaus.<br/><br/>Standaardwaarde is `.` (punt). |Teken dat wordt gebruikt voor het scheiden van geneste niveaus. <br/><br/>Standaardwaarde is `.` (punt). |
| WriteBatchSize |Het aantal parallelle aanvragen voor Azure Cosmos DB-service om documenten te maken.<br/><br/>U kunt de prestaties afstemmen bij het kopiëren van gegevens naar/van Cosmos DB met behulp van deze eigenschap. U kunt een betere prestaties verwachten wanneer u writeBatchSize verhogen omdat meer parallelle aanvragen met Cosmos DB worden verzonden. Echter, moet u voorkomen dat beperking, kunnen het foutbericht genereren: 'Aanvraagsnelheid is hoog'.<br/><br/>Beperking wordt bepaald door een aantal factoren, onder andere de grootte van de documenten, het aantal voorwaarden in documenten, het indexeringsbeleid van verzameling, enzovoort. Voor kopieerbewerkingen, kunt u een betere verzameling (bijvoorbeeld S3) gebruiken om de meeste doorvoer beschikbaar (2.500 request units per seconde). |Geheel getal |Nee (standaard: 5) |
| writeBatchTimeout |Wachttijd voor de bewerking is voltooid voordat er een optreedt time-out. |timespan<br/><br/> Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |

## <a name="importexport-json-documents"></a>Import/Export-JSON-documenten
Met deze connector Cosmos DB kunt u eenvoudig

* JSON-documenten uit verschillende bronnen importeren in Cosmos DB, Azure-Blob, met inbegrip van Azure Data Lake, on-premises bestandssysteem of andere bestandsgebaseerde winkels die wordt ondersteund door Azure Data Factory.
* JSON-documenten uit Cosmos DB-verzameling in verschillende archieven op basis van een bestand exporteren.
* Gegevens migreren tussen twee Cosmos DB-verzamelingen as-is.

Voor dit exemplaar schema-agnostische
* Als u de wizard kopiëren, moet u de **' als exporteren-JSON-bestanden of Cosmos DB-verzameling "** optie.
* Wanneer met behulp van JSON bewerken, geen de sectie "structuur" opgeeft in Cosmos DB gegevensset (s), noch de eigenschap 'nestingSeparator' op Cosmos DB bron/sink in de kopieeractiviteit. Als u wilt van importeren / exporteren naar JSON-bestanden, in de store bestand gegevensset notatietype opgeven als 'JsonFormat","filePattern"config en overslaan van de instellingen van de rest-indeling, Zie [JSON-indeling](data-factory-supported-file-and-compression-formats.md#json-format) sectie voor meer informatie.

## <a name="json-examples"></a>JSON-voorbeelden
De volgende voorbeelden geven een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe het kopiëren van gegevens naar en van Azure Cosmos DB en Azure Blob Storage. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** uit een van de bronnen aan een van de vermelde sinks [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

## <a name="example-copy-data-from-azure-cosmos-db-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren van Azure Cosmos DB naar Azure Blob
Het voorbeeld hieronder ziet:

1. Een gekoppelde service van het type [DocumentDb](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [DocumentDbCollection](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [DocumentDbCollectionSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld worden gegevens in Azure Cosmos DB gekopieerd naar Azure-Blob. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**Azure Cosmos DB gekoppelde service:**

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
**Gekoppelde Azure Blob storage-service:**

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
**Azure Document DB invoergegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u hebt een verzameling met de naam **persoon** in een Azure Cosmos DB-database.

Instellen van "extern": "true" en op te geven externalData informatie over de Azure Data Factory-service dat de tabel bevindt zich buiten de data factory en niet die worden geproduceerd door een activiteit in de data factory.

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Azure Blob-uitvoergegevensset:**

Gegevens worden gekopieerd naar een nieuwe blob elk uur door het pad voor de blob met uur granulariteit zetten op basis van de specifieke datum/tijd.

```JSON
{
  "name": "PersonBlobTableOut",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
Voorbeeld van JSON-document in de verzameling persoon in een Cosmos DB-database:

```JSON
{
  "PersonId": 2,
  "Name": {
    "First": "Jane",
    "Middle": "",
    "Last": "Doe"
  }
}
```
Cosmos DB biedt ondersteuning voor documentquery met behulp van een SQL-achtige syntaxis via hiërarchische JSON-documenten.

Voorbeeld:

```sql
SELECT Person.PersonId, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person
```

De volgende pijplijn kopieert gegevens uit de verzameling persoon in de Azure Cosmos DB-database naar een Azure-blob. Als onderdeel van de kopieerbewerking de invoer en uitvoer zijn gegevenssets opgegeven.

```JSON
{
  "name": "DocDbToBlobPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "DocumentDbCollectionSource",
            "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
            "nestingSeparator": "."
          },
          "sink": {
            "type": "BlobSink",
            "blobWriterAddHeader": true,
            "writeBatchSize": 1000,
            "writeBatchTimeout": "00:00:59"
          }
        },
        "inputs": [
          {
            "name": "PersonCosmosDbTable"
          }
        ],
        "outputs": [
          {
            "name": "PersonBlobTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromDocDbToBlob"
      }
    ],
    "start": "2015-04-01T00:00:00Z",
    "end": "2015-04-02T00:00:00Z"
  }
}
```
## <a name="example-copy-data-from-azure-blob-to-azure-cosmos-db"></a>Voorbeeld: Gegevens kopiëren van Azure-Blob naar Azure Cosmos DB
Het voorbeeld hieronder ziet:

1. Een gekoppelde service van het type DocumentDb.
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type DocumentDbCollection.
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) en DocumentDbCollectionSink.

Het voorbeeld worden gegevens gekopieerd van Azure blob naar Azure Cosmos DB. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**Gekoppelde Azure Blob storage-service:**

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
**Azure Cosmos DB gekoppelde service:**

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
**Azure Blob-invoergegevensset:**

```JSON
{
  "name": "PersonBlobTableIn",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "MiddleName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "fileName": "input.csv",
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
**Azure Cosmos DB-uitvoergegevensset:**

Het voorbeeld worden gegevens gekopieerd naar een verzameling met de naam 'Persoon' gemaakt.

```JSON
{
  "name": "PersonCosmosDbTableOut",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "Name.First",
        "type": "String"
      },
      {
        "name": "Name.Middle",
        "type": "String"
      },
      {
        "name": "Name.Last",
        "type": "String"
      }
    ],
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
De volgende pijplijn kopieert gegevens van Azure-Blob naar de verzameling persoon in de Cosmos DB. Als onderdeel van de kopieerbewerking de invoer en uitvoer zijn gegevenssets opgegeven.

```JSON
{
  "name": "BlobToDocDbPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "DocumentDbCollectionSink",
            "nestingSeparator": ".",
            "writeBatchSize": 2,
            "writeBatchTimeout": "00:00:00"
          },
          "translator": {
              "type": "TabularTranslator",
              "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
          }
        },
        "inputs": [
          {
            "name": "PersonBlobTableIn"
          }
        ],
        "outputs": [
          {
            "name": "PersonCosmosDbTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromBlobToDocDb"
      }
    ],
    "start": "2015-04-14T00:00:00Z",
    "end": "2015-04-15T00:00:00Z"
  }
}
```
Als de invoer van de blob voorbeeld als

```
1,John,,Doe
```
De uitvoer-JSON in Cosmos DB kan vervolgens worden als:

```JSON
{
  "Id": 1,
  "Name": {
    "First": "John",
    "Middle": null,
    "Last": "Doe"
  },
  "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
}
```
Azure Cosmos DB is een NoSQL-archief voor JSON-documenten, waarin geneste structuren zijn toegestaan. Azure Data Factory kan de gebruiker om aan te duiden hiërarchie via **nestingSeparator**, namelijk '. ' in dit voorbeeld. Bij het scheidingsteken, de kopieeractiviteit genereren het object "Naam" met de drie onderliggende elementen eerst, midden- en achternaam, op basis van "Name.First", "Name.Middle" en "Name.Last" in de tabeldefinitie.

## <a name="appendix"></a>Bijlage
1. **Vraag:** Ondersteuning voor de Kopieeractiviteit bijwerken van bestaande records?

    **Antwoord:** Nee.
2. **Vraag:** Hoe een nieuwe poging van een kopie op Azure Cosmos DB behandelt al gekopieerde records?

    **Antwoord:** Als u records hebben een veld 'ID' en de kopieerbewerking wordt geprobeerd een record met dezelfde ID in te voegen, genereert de kopieerbewerking een fout.
3. **Vraag:** Biedt ondersteuning voor Data Factory [bereik of het partitioneren van gegevens op basis van een hash](../../cosmos-db/sql-api-partition-data.md)?

    **Antwoord:** Nee.
4. **Vraag:** Kan ik meer dan één Azure Cosmos DB-verzameling voor een tabel opgeven?

    **Antwoord:** Nee. Slechts één verzameling kan op dit moment worden opgegeven.

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie [prestaties kopiëren en Afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren.
