---
title: Activiteit metagegevens ophalen in Azure Data Factory | Microsoft Docs
description: Lees hoe u de SQL Server opgeslagen Procedure-activiteit kunt gebruiken om aan te roepen een opgeslagen procedure in een Azure SQL Database of Azure SQL Data Warehouse van een Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: shlo
ms.openlocfilehash: c24bec7366ea62b3dd8f7a301c9d2d62c6dd6c7d
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37859275"
---
# <a name="get-metadata-activity-in-azure-data-factory"></a>Activiteit metagegevens ophalen in Azure Data Factory
De GET metadata activity kan worden gebruikt om op te halen **metagegevens** van gegevens in Azure Data Factory. Deze activiteit kan worden gebruikt in de volgende scenario's:

- De informatie over de metagegevens van de gegevens valideren
- Een pijplijn activeren wanneer gegevens gereed / beschikbaar

De volgende functionaliteit is beschikbaar in de Controlestroom:

- De uitvoer van de GET metadata Activity kan worden gebruikt in voorwaardelijke expressies validatie uitvoeren.
- Een pijplijn kan worden geactiveerd wanneer de voorwaarde is voldaan via-tot lussen

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

De activiteit GetMetadata duurt een gegevensset als een vereiste invoer en uitvoer metagegevensinformatie beschikbaar als uitvoer van activiteit. Op dit moment de volgende connectors met bijbehorende ophaalbaar meatadata worden ondersteund en de grootte van de maximale ondersteunde metagegevens is tot **1MB**.

>[!NOTE]
>Als u de activiteit GetMetadata op een zelfgehoste Cloudintegratieruntime uitvoert, wordt de meest recente mogelijkheid wordt ondersteund op versie 3.6 of hoger. 

### <a name="supported-connectors"></a>Ondersteunde connectors

**Opslag van bestanden:**

| Connector/metagegevens | Itemnaam<br>(bestand/map) | itemType<br>(bestand/map) | grootte<br>(bestand) | gemaakt<br>(bestand/map) | lastModified<br>(bestand/map) |childItems<br>(map) |contentMD5<br>(bestand) | structuur<br/>(bestand) | aantal kolommen<br>(bestand) | Er bestaat<br>(bestand/map) |
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| Amazon S3 | √/√ | √/√ | √ | x/x | √ / √ * | √ | x | √ | √ | √ / √ * |
| Azure Blob | √/√ | √/√ | √ | x/x | √ / √ * | √ | √ | √ | √ | √/√ |
| Azure Data Lake Store | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| Azure File Storage | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| Bestandssysteem | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| SFTP | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| FTP | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |

- Voor Amazon S3, de `lastModified` is van toepassing op bucket en -sleutel, maar geen virtuele map; en de `exists` is van toepassing op bucket en -sleutel, maar geen voorvoegsel of virtuele map.
- Voor Azure-Blob, het `lastModified` is van toepassing op de container en blob, maar geen virtuele map.

**Relationele database:**

| Connector/metagegevens | structuur | aantal kolommen | Er bestaat |
|:--- |:--- |:--- |:--- |
| Azure SQL Database | √ | √ | √ |
| Azure SQL Data Warehouse | √ | √ | √ |
| SQL Server | √ | √ | √ |

### <a name="metadata-options"></a>Opties voor metagegevens

De volgende typen van de metagegevens kunnen worden opgegeven in de lijst met velden activiteit GetMetadata om op te halen:

| Metagegevenstype | Beschrijving |
|:--- |:--- |
| Itemnaam | De naam van het bestand of map. |
| itemType | Het type van het bestand of map. Uitvoerwaarde `File` of `Folder`. |
| grootte | Grootte van het bestand in bytes. Van toepassing op het bestand alleen. |
| gemaakt | Gemaakte datum en tijd van het bestand of map. |
| lastModified | Datum/tijd van het bestand of map het laatst is gewijzigd. |
| childItems | Lijst met bestanden in de opgegeven map en submappen. Van toepassing op de map alleen. Waarde voor uitvoer is een lijst met naam en type van elk onderliggend item. |
| contentMD5 | MD5-algoritme van het bestand. Van toepassing op het bestand alleen. |
| structuur | De gegevensstructuur in het bestand of de relationele database-tabel. Waarde voor uitvoer is een lijst met de naam van kolom en het type van kolom. |
| aantal kolommen | Het aantal kolommen in het bestand of een relationele tabel. |
| Er bestaat| Of een bestand/map/table of niet bestaat. Houd er rekening mee dat als 'bestaat' is opgegeven in de veldenlijst GetaMetadata, de activiteit wordt niet mislukt, zelfs wanneer het item (bestand/map/table) niet bestaat. in plaats daarvan het resultaat `exists: false` in de uitvoer. |

>[!TIP]
>Als u valideren wilt als een bestand/map/table of niet bestaat, geeft `exists` in de veldenlijst GetMetadata activiteit kunt u vervolgens controleren de `exists: true/false` als gevolg van de uitvoer van de activiteit. Als `exists` is niet geconfigureerd in de lijst met velden, de GetMetadata activiteit mislukt wanneer het object is niet gevonden.

## <a name="syntax"></a>Syntaxis

**De GET metadata activity:**

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

**Gegevensset:**

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
            "filename": "file.json",
            "format":{
                "type":"JsonFormat"
            }
        }
    }
}
```

## <a name="type-properties"></a>Type-eigenschappen

De activiteit GetMetadata kan momenteel de volgende typen metagegevens worden opgehaald.

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
Veldenlijst | Bevat de soorten metagegevens die zijn vereist. Zie voor meer informatie [opties voor metagegevens](#metadata-options) gedeelte over ondersteunde metagegevens. | Ja 
gegevensset | De verwijzing gegevensset waarvan de metagegevens van de activiteit moet worden opgehaald door de activiteit GetMetadata is. Zie [ondersteunde mogelijkheden](#supported-capabilities) sectie op ondersteunde connectors en verwijzen naar het onderwerp van de connector op de details van de syntaxis van de gegevensset. | Ja

## <a name="sample-output"></a>Voorbeelduitvoer

Het resultaat GetMetadata wordt weergegeven in de uitvoer van activiteit. Hieronder vindt u twee voorbeelden met uitgebreide metagegevens opties geselecteerd in de lijst met velden als verwijzing. Als u het resultaat van de volgende activiteit, gebruikt u het patroon van `@{activity('MyGetMetadataActivity').output.itemName}`.

### <a name="get-a-files-metadata"></a>De metagegevens van een bestand ophalen

```json
{
  "exists": true,
  "itemName": "test.csv",
  "itemType": "File",
  "size": 104857600,
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "contentMD5": "cMauY+Kz5zDm3eWa9VpoyQ==",
  "structure": [
    {
        "name": "id",
        "type": "Int64"
    },
    {
        "name": "name",
        "type": "String"
    }
  ],
  "columnCount": 2
}
```

### <a name="get-a-folders-metadata"></a>De metagegevens van een map ophalen

```json
{
  "exists": true,
  "itemName": "testFolder",
  "itemType": "Folder",
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "childItems": [
    {
      "name": "test.avro",
      "type": "File"
    },
    {
      "name": "folder hello",
      "type": "Folder"
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen
Zie andere controlestroomactiviteiten die door Data Factory worden ondersteund: 

- [Execute Pipeline Activity](control-flow-execute-pipeline-activity.md)
- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Lookup Activity](control-flow-lookup-activity.md)
- [Webactiviteit](control-flow-web-activity.md)
