---
title: Schematoewijzing in kopieeractiviteit | Microsoft Docs
description: "Meer informatie over hoe kopieeractiviteit in Azure Data Factory-schema's en gegevenstypen van de brongegevens als u wilt gegevens opvangen bij het kopiëren van gegevens wordt toegewezen."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: jingwang
ms.openlocfilehash: c1fde0abd2f5fa8e36c71707d14cd5afeb3a0d2d
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="schema-mapping-in-copy-activity"></a>Schematoewijzing in de kopieerbewerking
Dit artikel wordt beschreven hoe Azure Data Factory kopieeractiviteit schematoewijzing en gegevenstypetoewijzing van brongegevens opvangen gegevens gaat uitvoeren wanneer de gegevens opnieuw te kopiëren.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-introduction.md).


## <a name="column-mapping"></a>Kolomtoewijzing

Standaard kopieeractiviteit **brongegevens opvangen door kolomnamen toewijzen**, tenzij [expliciete kolomtoewijzing](#explicit-column-mapping) is geconfigureerd. Meer specifiek, kopieeractiviteit:

1. De gegevens niet lezen vanuit de bron en het schema van de gegevensbron bepalen

    * Voor gegevensbronnen met vooraf gedefinieerde schema in de data store/bestandsindeling, bijvoorbeeld databasebestanden met metagegevens (Avro/ORC/parketvloeren/tekst met koptekst), wordt het schema van gegevensbron opgehaald uit de metagegevens van de query resultaat of het bestand.
    * Voor gegevensbronnen met flexibele schema, bijvoorbeeld Azure tabel/Cosmos DB is gegevensbronschema afgeleid van het queryresultaat. U kunt deze vervangen door "structuur" in de gegevensset.
    * Voor tekstbestand zonder header zijn standaardkolomnamen gegenereerd met patroon 'Prop_0', 'Prop_1'... U kunt deze vervangen door "structuur" in de gegevensset.
    * U hebt de schema-informatie in de gegevensset "structuur" sectie voor Dynamics-bron.

2. Expliciete kolomtoewijzing van toepassing als opgegeven.

3. De gegevens opvangen schrijven

    * Voor de gegevensopslag met vooraf gedefinieerde schema, worden de gegevens worden geschreven naar de kolommen met dezelfde naam.
    * Voor gegevensarchieven zonder vaste schema en een voor indelingen van logboekbestanden, de kolom namen/metagegevens op basis van het schema van de gegevensbron gegenereerd.

### <a name="explicit-column-mapping"></a>Expliciete kolomtoewijzing

U kunt opgeven **columnMappings** in de **typeProperties** sectie van de kopieeractiviteit expliciete kolomtoewijzing doen. In dit scenario is de sectie 'structuur' vereist voor zowel invoer- en uitvoergegevenssets. Kolom toewijzing ondersteunt **toewijzing alle of een subset van kolommen in de gegevensset voor bron "structuur" op alle kolommen in de gegevensset sink "structuur"**. Hier volgen de fouten die ertoe leiden dat een uitzondering:

* Brongegevens opgeslagen resultaat heeft geen naam van een kolom die is opgegeven in de sectie van de "structuur" invoergegevensset query.
* Sink-gegevensarchief (als met vooraf gedefinieerde schema) heeft geen naam van een kolom die is opgegeven in de sectie uitvoer gegevensset 'structuur'.
* Minder kolommen of meer kolommen in de 'structuur' sink gegevensset dan opgegeven in de toewijzing.
* Dubbele toewijzing.

#### <a name="explicit-column-mapping-example"></a>Voorbeeld van de expliciete column-toewijzing

In dit voorbeeld heeft een structuur van de invoertabel en of deze verwijst naar een tabel in een on-premises SQL-database.

```json
{
    "name": "SqlServerInput",
    "properties": {
        "structure":
         [
            { "name": "UserId"},
            { "name": "Name"},
            { "name": "Group"}
         ],
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "SqlServerLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SourceTable"
        }
    }
}
```

In dit voorbeeld wordt de uitvoertabel heeft een structuur en of deze verwijst naar een tabel in een Azure SQL Database.

```json
{
    "name": "AzureSqlOutput",
    "properties": {
        "structure":
        [
            { "name": "MyUserId"},
            { "name": "MyName" },
            { "name": "MyGroup"}
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "AzureSqlLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "SinkTable"
        }
    }
}
```

De volgende JSON definieert een kopieeractiviteit in een pijplijn. De kolommen van bron toegewezen aan kolommen in sink (**columnMappings**) met behulp van de **conversieprogramma** eigenschap.

```json
{
    "name": "CopyActivity",
    "type": "Copy",
    "inputs": [
        {
            "referenceName": "SqlServerInput",
            "type": "DatasetReference"
        }
    ],
    "outputs": [
        {
            "referenceName": "AzureSqlOutput",
            "type": "DatasetReference"
        }
    ],
    "typeProperties":    {
        "source": { "type": "SqlSource" },
        "sink": { "type": "SqlSink" },
        "translator":
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
        }
    }
}
```

**Stroom van de toewijzing van kolom:**

![Kolom toewijzing stroom](./media/copy-activity-schema-and-type-mapping/column-mapping-sample.png)

## <a name="data-type-mapping"></a>Gegevenstypetoewijzing

Kopieerbewerking wordt de brontypen als u wilt toewijzen met de volgende stappen 2-benadering typen opvangen uitgevoerd:

1. Systeemeigen brontypen converteren naar Azure Data Factory tussentijdse-gegevenstypen
2. Azure Data Factory tussentijdse gegevenstypen converteren naar systeemeigen sink-type

U vindt de toewijzing tussen native type van tussentijdse type in de sectie 'Data type toewijzing' in het onderwerp van elke connector.

### <a name="supported-data-types"></a>Ondersteunde gegevenstypen

Data Factory ondersteunt de volgende tussentijdse gegevenstypen: U kunt onderstaande waarden opgeven bij het opgeven type-informatie in [gegevenssetstructuur](concepts-datasets-linked-services.md#dataset-structure) configuratie:

* Byte[]
* Boole-waarde
* Datum en tijd
* Datetimeoffset
* Decimale
* Double
* GUID
* Int16
* Int32
* Int64
* Single
* Tekenreeks
* Periode

### <a name="explicit-data-type-conversion"></a>Expliciete conversie van gegevenstype

Bij het kopiëren van gegevens in gegevens opslaat met vaste schema, bijvoorbeeld SQL Server/Oracle, wanneer bron en sink heeft een ander type voor dezelfde kolom de expliciete conversie moet worden gedeclareerd als aan de bronkant:

* Voor bestandsbron, bijvoorbeeld, CSV/Avro de typeconversie aangegeven via de structuur van de gegevensbron met volledige kolomlijst (side kolom naam- en sink side brontype)
* Voor relationele bron (bijvoorbeeld SQL/Oracle), moet de typeconversie worden bereikt door het casten expliciet type in de query-instructie.

## <a name="when-to-specify-dataset-structure"></a>'Gegevenssetstructuur' opgeven

In onderstaande scenario's is 'structuur' in de gegevensset vereist:

* Toepassen van [expliciete conversie van gegevenstype](#explicit-data-type-conversion) voor bestandsbronnen tijdens het kopiëren (invoergegevensset)
* Toepassen van [expliciete kolomtoewijzing](#explicit-column-mapping) tijdens het kopiëren (zowel invoer en uitvoer gegevensset)
* Kopiëren van Dynamics 365 / CRM bron (invoergegevensset)
* Kopiëren naar Cosmos DB als geneste object als bron niet JSON-bestanden (uitvoergegevensset)

In onderstaande scenario's, wordt 'structuur' in de gegevensset voorgesteld:

* Kopiëren van een tekstbestand zonder header (invoergegevensset). U kunt opgeven dat de kolomnamen voor tekstbestand afstemmen met de overeenkomstige kolommen sink van expliciete kolomtoewijzing bieden op te slaan.
* Kopiëren van gegevens worden opgeslagen met flexibele schema, bijvoorbeeld Azure-tabel/Cosmos-DB (invoergegevensset), om te garanderen van de verwachte gegevens (kolommen) wordt gekopieerd in plaats van laten kopiëren activiteit schemadeductie naar op basis van de eerste rij(en) tijdens elke activiteit die wordt uitgevoerd.


## <a name="next-steps"></a>Volgende stappen
Zie de andere artikelen voor de Kopieeractiviteit:

- [Activiteit-overzicht](copy-activity-overview.md)
- [Activiteit fouttolerantie kopiëren](copy-activity-fault-tolerance.md)
- [Uitvoering van de activiteit](copy-activity-performance.md)
