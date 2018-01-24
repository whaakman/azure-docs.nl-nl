---
title: Gegevens verplaatsen naar/van Azure DB die Cosmos | Microsoft Docs
description: Meer informatie over hoe gegevens worden verplaatst naar/van Azure DB die Cosmos-verzameling met behulp van Azure Data Factory
services: data-factory, cosmosdb
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: c9297b71-1bb4-4b29-ba3c-4cf1f5575fac
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 89b860bb4174a06c17da1db2bce2eaa11832b0b2
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-to-and-from-azure-cosmos-db-using-azure-data-factory"></a>Gegevens verplaatsen en naar Azure Cosmos DB met Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-azure-documentdb-connector.md)
> * [Versie 2 - Preview](../connector-azure-cosmos-db.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [Azure DB die Cosmos-connector in V2](../connector-azure-cosmos-db.md).

In dit artikel wordt uitgelegd hoe u met de Kopieeractiviteit in Azure Data Factory te verplaatsen van gegevens uit Azure Cosmos-DB (SQL-API). Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, hetgeen een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit toont. 

U kunt gegevens kopiëren vanuit een ondersteunde brongegevensarchief bij Azure Cosmos DB of vanuit Azure Cosmos-database moet een ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven als bronnen of PUT wordt ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. 

> [!IMPORTANT]
> Azure DB Cosmos-connector ondersteunt alleen de SQL-API.

Gegevens kopiëren-is naar/van de JSON-bestanden of een andere verzameling van de Cosmos-DB, Zie [voor importeren/exporteren JSON-documenten](#importexport-json-documents).

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens uit Azure Cosmos DB verplaatst met behulp van verschillende hulpprogramma's voor API's.

De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren: 

1. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory.
2. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor voorbeelden met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar/van de Cosmos-DB, [JSON voorbeelden](#json-examples) sectie van dit artikel. 

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke aan de Cosmos-database: 

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor de service Azure Cosmos DB gekoppeld.

| **Eigenschap** | **Beschrijving** | **Vereist** |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **DocumentDb** |Ja |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met Azure DB die Cosmos-database. |Ja |

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
Raadpleeg voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets van de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle gegevensset typen (Azure SQL, Azure blob-, Azure-tabel, enz.).

De sectie typeProperties verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **DocumentDbCollection** heeft de volgende eigenschappen.

| **Eigenschap** | **Beschrijving** | **Vereist** |
| --- | --- | --- |
| CollectionName |De naam van de Cosmos-DB-document-verzameling. |Ja |

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
Voor gegevens zonder schema winkels zoals Azure Cosmos DB afleidt de Data Factory-service het schema in een van de volgende manieren:  

1. Als u de structuur van gegevens met behulp van opgeven de **structuur** eigenschap in de definitie van de gegevensset, de Data Factory-service zich houdt aan deze structuur als het schema. Als een rij geen waarde voor een kolom bevat, wordt in dit geval een null-waarde opgegeven voor deze.
2. Als u de structuur van gegevens niet via opgeeft de **structuur** eigenschap in de definitie van de gegevensset, de Data Factory-service infereert het schema met behulp van de eerste rij in de gegevens. In dit geval als de eerste rij niet het volledige schema bevat, sommige kolommen worden ontbreekt in het resultaat van de kopieerbewerking.

Daarom voor gegevensbronnen zonder schema, de aanbevolen procedure is om op te geven van de structuur van gegevens met de **structuur** eigenschap.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Raadpleeg voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten van de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle typen activiteiten.

> [!NOTE]
> De Kopieeractiviteit slechts één invoer en slechts één uitvoer produceert.

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit aan de andere kant afwijken met elk activiteitstype en in geval van een kopieeractiviteit ze afhankelijk van de typen van bronnen en Put.

In geval van een kopieeractiviteit wanneer de bron van het type **DocumentDbCollectionSource** de volgende eigenschappen beschikbaar zijn in **typeProperties** sectie:

| **Eigenschap** | **Beschrijving** | **Toegestane waarden** | **Vereist** |
| --- | --- | --- | --- |
| query |Specificeer de query voor het lezen van gegevens. |Queryreeks wordt ondersteund door Azure Cosmos DB. <br/><br/>Voorbeeld:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Nee <br/><br/>Als niet wordt opgegeven, de SQL-instructie die is uitgevoerd:`select <columns defined in structure> from mycollection` |
| nestingSeparator |Speciaal teken om aan te geven dat het document is genest |Willekeurig teken. <br/><br/>Azure Cosmos-database is een NoSQL-opslagplaats voor JSON-documenten, waarbij geneste structuren zijn toegestaan. Azure Data Factory kan gebruiker duiden hiërarchie via nestingSeparator die "." in de bovenstaande voorbeelden. Met het scheidingsteken, met de kopieerbewerking genereert het object "Name" met de drie onderliggende elementen eerst middelste en de laatste volgens 'Name.First', 'Name.Middle' en 'Name.Last' in de tabeldefinitie. |Nee |

**DocumentDbCollectionSink** ondersteunt de volgende eigenschappen:

| **Eigenschap** | **Beschrijving** | **Toegestane waarden** | **Vereist** |
| --- | --- | --- | --- |
| nestingSeparator |Er is een speciaal teken in naam van de bronkolom om aan te geven dat geneste document nodig. <br/><br/>Zoals hierboven: `Name.First` in de uitvoer van de tabel de volgende JSON-structuur in het document Cosmos DB produceert:<br/><br/>"Name": {<br/>    ' First ': 'John'<br/>}, |Teken dat wordt gebruikt voor het scheiden van geneste niveaus.<br/><br/>Standaardwaarde is `.` (punt). |Teken dat wordt gebruikt voor het scheiden van geneste niveaus. <br/><br/>Standaardwaarde is `.` (punt). |
| writeBatchSize |Het aantal parallelle aanvragen voor Azure DB die Cosmos-service om documenten te maken.<br/><br/>U kunt de prestaties aanpassen bij het kopiëren van gegevens van Cosmos-database met behulp van deze eigenschap. U kunt een betere prestaties verwachten wanneer u writeBatchSize verhogen omdat meer parallelle aanvragen aan de Cosmos-database worden verzonden. Moet u echter om te voorkomen dat beperking kunnen genereert het foutbericht weergegeven: 'Vragen snelheid is groot'.<br/><br/>Beperking wordt bepaald door een aantal factoren, onder andere de grootte van documenten, aantal termen in documenten, indexeren beleid van de doelverzameling, enzovoort. Voor het kopiëren van, kunt u een betere verzameling (bijvoorbeeld S3) hebben de meeste beschikbare doorvoer (2500 aanvragen eenheden/seconde). |Geheel getal |Nee (standaard: 5) |
| writeBatchTimeout |Wachttijd voor de bewerking te voltooien voordat er een optreedt time-out. |TimeSpan<br/><br/> Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |

## <a name="importexport-json-documents"></a>Import/Export JSON-documenten
Met deze Cosmos-DB-connector kunt u eenvoudig

* JSON-documenten importeren uit diverse bronnen in Cosmos-DB, met inbegrip van Azure Blob Azure Data Lake, on-premises bestandssysteem of andere winkels op basis van bestanden die door Azure Data Factory worden ondersteund.
* JSON-documenten uit Cosmos DB gewijzigd exporteren in verschillende winkels op basis van bestanden.
* Gegevens migreren tussen twee Cosmos DB verzamelingen op als-is.

Als u dit schema networkdirect-exemplaar 
* Als u de wizard kopiëren, moet u de **' exporteren als-JSON-bestanden of Cosmos DB verzameling '** optie.
* Wanneer met behulp van JSON bewerkt, geen de sectie 'structuur' opgeven in Cosmos DB gegevensset (s) en ook 'nestingSeparator'-eigenschap op Cosmos DB bron/sink in de kopieerbewerking. Als u wilt van importeren / exporteren naar JSON-bestanden, notatietype in de store-gegevensset bestand opgeven als 'JsonFormat', 'filePattern' config en overslaan van de instellingen van de rest-indeling, Zie [JSON-indeling](data-factory-supported-file-and-compression-formats.md#json-format) sectie voor meer informatie.

## <a name="json-examples"></a>JSON-voorbeelden
De volgende voorbeelden geven voorbeeld JSON definities die u een pijplijn maken kunt met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe om gegevens te kopiëren naar en van Azure DB die Cosmos en Azure Blob Storage. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** uit een van de bronnen aan een van de PUT vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

## <a name="example-copy-data-from-azure-cosmos-db-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren van Azure DB die Cosmos naar Azure Blob
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
**Azure Blob-opslag gekoppelde service:**

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
**Invoergegevensset van Azure Document database:**

Het voorbeeld wordt ervan uitgegaan dat u hebt een verzameling met de naam **persoon** in een Azure DB die Cosmos-database.

Instelling 'extern': 'true' en externalData beleid gegevens over de Azure Data Factory-service die de tabel extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory op te geven.

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

Gegevens worden gekopieerd naar een nieuwe blob elk uur met het pad voor de blob als gevolg van de specifieke datum/tijd met uur granulatie.

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
Voorbeeld JSON-document in de verzameling persoon in een Cosmos-DB-database:

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
Cosmos DB biedt ondersteuning voor documentquery's die gebruikmaken van een SQL zoals syntaxis via hiërarchische JSON-documenten.

Voorbeeld: 

```sql
SELECT Person.PersonId, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person
```

De volgende pijplijn kopieert invoergegevens uit de verzameling persoon in de Azure DB die Cosmos-database naar een Azure-blob. Als onderdeel van de kopieeractiviteit de invoer en uitvoer zijn gegevenssets opgegeven.  

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
## <a name="example-copy-data-from-azure-blob-to-azure-cosmos-db"></a>Voorbeeld: Gegevens kopiëren van Azure-Blob naar Azure Cosmos-DB 
Het voorbeeld hieronder ziet:

1. Een gekoppelde service van het type [DocumentDb](#azure-documentdb-linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [DocumentDbCollection](#azure-documentdb-dataset-type-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) en [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).

Het voorbeeld kopieert gegevens van Azure blob naar Azure Cosmos DB. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

**Azure Blob-opslag gekoppelde service:**

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
**Azure Blob invoergegevensset:**

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
**Azure Cosmos DB uitvoergegevensset:**

Het voorbeeld worden gegevens gekopieerd naar een verzameling met de naam 'Persoon'.

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
De volgende pijplijn kopieert invoergegevens uit Azure Blob aan de verzameling persoon in de Cosmos-database. Als onderdeel van de kopieeractiviteit de invoer en uitvoer zijn gegevenssets opgegeven.

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
          }
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
Vervolgens wordt de uitvoer van de JSON-code in Cosmos-DB worden als:

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
Azure Cosmos-database is een NoSQL-opslagplaats voor JSON-documenten, waarbij geneste structuren zijn toegestaan. Azure Data Factory kan gebruiker duiden hiërarchie via **nestingSeparator**, namelijk '. ' in dit voorbeeld. Met het scheidingsteken, met de kopieerbewerking genereert het object "Name" met de drie onderliggende elementen eerst middelste en de laatste volgens 'Name.First', 'Name.Middle' en 'Name.Last' in de tabeldefinitie.

## <a name="appendix"></a>Bijlage
1. **Vraag:** heeft de update voor de Kopieeractiviteit ondersteuning van bestaande records?

    **Antwoord:** Nee.
2. **Vraag:** records hoe al biedt een nieuwe poging van een kopie te behandelen Azure Cosmos DB gekopieerd?

    **Antwoord:** als records een veld 'ID hebben' en de kopieerbewerking wordt geprobeerd een record met dezelfde ID in te voegen, de kopieerbewerking een fout genereert.  
3. **Vraag:** biedt ondersteuning voor Data Factory [bereik of de gegevens op basis van het hash-partitionering](../../cosmos-db/sql-api-partition-data.md)?

    **Antwoord:** Nee.
4. **Vraag:** kan ik meer dan één Azure Cosmos DB verzameling voor een tabel opgeven?

    **Antwoord:** Nee. Slechts één verzameling kan op dit moment worden opgegeven.

## <a name="performance-and-tuning"></a>Prestaties en het afstemmen
Zie [uitvoering van de activiteit & afstemmen handleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren.
