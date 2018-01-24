---
title: Ophalen van metagegevens van activiteit in Azure Data Factory | Microsoft Docs
description: Meer informatie over hoe u de SQL Server opgeslagen Procedure activiteit kunt gebruiken om aan te roepen, een opgeslagen procedure in een Azure SQL Database of een Azure SQL Data Warehouse van een Data Factory-pijplijn.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 20f3d4bb876a46b67385dd4435296e149641149e
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="get-metadata-activity-in-azure-data-factory"></a>Ophalen van metagegevens van activiteit in Azure Data Factory
De Get Metadata Activity kan worden gebruikt voor het ophalen van metagegevens van gegevens in Azure Data Factory. Deze activiteit wordt alleen ondersteund voor data Factory van versie 2. Het kan worden gebruikt in de volgende scenario's:

- De informatie over de metagegevens van alle gegevens valideren
- Een pijplijn wordt geactiveerd wanneer gegevens gereed / beschikbaar

De volgende functionaliteit is beschikbaar in de Controlestroom:
- De uitvoer van activiteit GetMetadata kan worden gebruikt in voorwaardelijke expressies validatie uitvoeren.
- Een pijplijn kan worden geactiveerd als voorwaarde wordt voldaan via komen-totdat herhalen

Het duurt GetMetadata activiteit een gegevensset als een vereiste invoer en uitvoer metagegevensinformatie beschikbaar als uitvoer. Alleen Azure blob-gegevensset wordt momenteel ondersteund. De metagegevensvelden ondersteunde zijn grootte, structuur en lastModified-tijd.  

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Data Factory V1 documentatie](v1/data-factory-introduction.md).


## <a name="syntax"></a>Syntaxis

### <a name="get-metadata-activity-definition"></a>De definitie van metagegevens activiteit ophalen:
In het volgende voorbeeld retourneert de activiteit GetMetadata metagegevens over de gegevens die wordt vertegenwoordigd door de MyDataset. 

```json
{
    "name": "MyActivity",
    "type": "GetMetadata",
    "typeProperties": {
        "fieldList" : ["size", "lastModified", "structure"],
        "dataset": {
            "referenceName": "MyDataset",
            "type": "DatasetReference"
        }
    }
}
```
### <a name="dataset-definition"></a>De definitie van de gegevensset:

```json
{
    "name": "MyDataset",
    "properties": {
    "type": "AzureBlob",
        "linkedService": {
            "referenceName": "StorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath":"container/folder",
            "Filename": "file.json",
            "format":{
                "type":"JsonFormat"
                "nestedSeperator": ","
            }
        }
    }
}
```

### <a name="output"></a>Uitvoer
```json
{
    "size": 1024,
    "structure": [
        {
            "name": "id",
            "type": "Int64"
        }, 
    ],
    "lastModified": "2016-07-12T00:00:00Z"
}
```

## <a name="type-properties"></a>Type-eigenschappen
GetMetadata activiteit kunt momenteel de volgende soorten informatie over metagegevens ophalen van een Azure-opslag-gegevensset.

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
Veldenlijst | Bevat de typen metagegevens die zijn vereist.  | <ul><li>grootte</li><li>structuur</li><li>lastModified</li></ul> |    Nee<br/>Als dit leeg is, retourneert activiteit alle 3 ondersteunde metagegevens. 
dataset | De verwijzingsgegevensset waarvan activiteit metagegevens worden opgehaald door de activiteit GetMetadata is. <br/><br/>Type momenteel ondersteunde dataset is Azure-Blob. Er zijn twee sub-eigenschappen: <ul><li><b>naamverwijzing</b>: verwijzing naar een bestaande Azure Blob-gegevensset</li><li><b>type</b>: omdat de gegevensset wordt verwezen, is van het type 'DatasetReference'</li></ul> |    <ul><li>Tekenreeks</li><li>DatasetReference</li></ul> | Ja

## <a name="next-steps"></a>Volgende stappen
Zie andere controlestroomactiviteiten die door Data Factory worden ondersteund: 

- [Execute Pipeline Activity](control-flow-execute-pipeline-activity.md)
- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Lookup Activity](control-flow-lookup-activity.md)
- [Webactiviteit](control-flow-web-activity.md)