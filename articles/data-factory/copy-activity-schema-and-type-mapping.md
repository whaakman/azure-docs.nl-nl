---
title: Schematoewijzing in de kopieeractiviteit | Microsoft Docs
description: Meer informatie over hoe de kopieeractiviteit in Azure Data Factory schema's en gegevenstypen van de brongegevens sink-gegevens bij het kopiëren van gegevens wordt toegewezen.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: jingwang
ms.openlocfilehash: 1a3855b7b95224e0f872764f6710f9fa907780a7
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54025450"
---
# <a name="schema-mapping-in-copy-activity"></a>Schematoewijzing in de kopieeractiviteit
Dit artikel wordt beschreven hoe copy activity in Azure Data Factory biedt schematoewijzing en gegevenstypetoewijzing van de brongegevens met sink-gegevens wanneer het kopiëren van gegevens uitvoert.

## <a name="column-mapping"></a>Toewijzen van kolommen

In de kolomtoewijzing is van toepassing bij het kopiëren van gegevens tussen gegevens in tabelvorm vormgegeven. Standaard kopieeractiviteit **brongegevens opvangen door de namen van kolommen toewijzen**, tenzij [expliciete kolomtoewijzing](#explicit-column-mapping) is geconfigureerd. Meer specifiek, kopieeractiviteit:

1. De gegevens niet lezen vanuit de bron en het schema van de gegevensbron bepalen

    * Voor gegevensbronnen met vooraf gedefinieerd schema in de data store/bestandsindeling, bijvoorbeeld databases/bestanden met metagegevens (Avro/ORC/Parquet/tekst met kop), die schema van de gegevensbron zijn geëxtraheerd uit de metagegevens van de query resultaat of het bestand.
    * Voor gegevensbronnen met een flexibel schema, bijvoorbeeld Azure tabel/Cosmos DB, is schema van de gegevensbron afgeleid van het queryresultaat. U kunt deze vervangen door het configureren van de 'structuur"in de gegevensset.
    * Voor bestanden met tekst zonder header, zijn standaardkolomnamen gegenereerd met patroon "Prop_0", 'Prop_1'... U kunt deze vervangen door het configureren van de 'structuur"in de gegevensset.
    * Voor Dynamics-bron moet u opgeven van de schema-informatie in de gegevensset "structuur" sectie.

2. Expliciete kolomtoewijzing van toepassing als u opgeeft.

3. Schrijven van de gegevens om op te vangen

    * Voor data-archieven met vooraf gedefinieerd schema, worden de gegevens geschreven naar de kolommen met dezelfde naam.
    * Voor gegevensarchieven zonder vaste schema en voor bestandsindelingen, worden namen/metagegevens van de kolom wordt gegenereerd op basis van het schema van de gegevensbron.

### <a name="explicit-column-mapping"></a>Expliciete kolomtoewijzing

U kunt opgeven **columnMappings** in de **typeProperties** sectie van de kopieeractiviteit expliciete kolomtoewijzing doen. In dit scenario is de sectie "structuur" vereist voor zowel invoer- en uitvoergegevenssets. Kolom toewijzing ondersteunt **toewijzing van alle of een subset van kolommen in de brongegevensset "structuur" voor alle kolommen in de sink-gegevensset "structuur"**. Hier volgen foutcondities die leiden tot een uitzondering:

* Brongegevens opgeslagen resultaat heeft geen naam van een kolom die is opgegeven in de invoergegevensset "structuur" sectie query.
* Sink-gegevensopslag (als met vooraf gedefinieerd schema) heeft geen naam van een kolom die is opgegeven in de sectie uitvoer gegevensset "structuur".
* Minder kolommen of meer kolommen in de 'structuur"van de sink-gegevensset dan opgegeven in de toewijzing.
* Dubbele toewijzing.

#### <a name="explicit-column-mapping-example"></a>Expliciete kolomtoewijzing voorbeeld

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

De volgende JSON definieert een kopieeractiviteit in een pijplijn. De kolommen uit de bron die zijn toegewezen aan kolommen in sink (**columnMappings**) met behulp van de **translator** eigenschap.

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
            "columnMappings": 
            {
                "UserId": "MyUserId",
                "Group": "MyGroup",
                "Name": "MyName"
            }
        }
    }
}
```

Als u de syntaxis van `"columnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"` om op te geven in de kolomtoewijzing, wordt nog steeds ondersteund als-is.

**Kolomtoewijzing stroom:**

![Kolom toewijzing stroom](./media/copy-activity-schema-and-type-mapping/column-mapping-sample.png)

## <a name="schema-mapping"></a>Schematoewijzing

Schematoewijzing is van toepassing wanneer kopiëren van gegevens tussen hiërarchische gevormde gegevens en gegevens in tabelvorm vormgegeven kopieert bijvoorbeeld van MongoDB/REST tekstbestand te kopiëren uit SQL naar Azure Cosmos DB MongoDB-API. De volgende eigenschappen worden ondersteund in de kopieeractiviteit `translator` sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de activiteit kopiëren translator moet worden ingesteld op: **TabularTranslator** | Ja |
| schemaMapping | Een verzameling van sleutel / waarde-paren, die de relatie toewijzing van de tabellaire kant naar hiërarchische zijde vertegenwoordigt.<br/>- **Sleutel:** de kolomnaam van de gegevens in tabelvorm als gedefinieerd in de gegevenssetstructuur.<br/>- **Waarde:** de JSON-padexpressie voor elk veld om te worden toegewezen. Voor velden onder het hoofdobject begint u met root $; voor velden binnen de matrix die is gekozen door de eigenschap `collectionReference`, begint u vanaf het element van de matrix.  | Ja |
| collectionReference | Als u wilt herhalen en gegevens ophalen uit de objecten **in een matrixveld** met hetzelfde patroon en converteren naar per rij per object geeft u het JSON-pad van die matrix voor cross-pas. Deze eigenschap wordt alleen ondersteund als hiërarchische gegevens bron is. | Nee |

**Voorbeeld: van MongoDB naar SQL kopiëren:**

Bijvoorbeeld, hebt u MongoDB-document met de volgende inhoud: 

```json
{
    "id": {
        "$oid": "592e07800000000000000000"
    },
    "number": "01",
    "date": "20170122",
    "orders": [
        {
            "prod": "p1",
            "price": 23
        },
        {
            "prod": "p2",
            "price": 13
        },
        {
            "prod": "p3",
            "price": 231
        }
    ],
    "city": [ { "name": "Seattle" } ]
}
```

en u wilt kopiëren naar een Azure SQL-tabel in de volgende indeling, door te maken van de gegevens in de matrix *(order_pd en order_price)* en cross join met de algemene root-gegevens *(getal, datum en plaats)* :

| orderNumber | Orderdatum | order_pd | order_price | city |
| --- | --- | --- | --- | --- |
| 01 | 20170122 | P1 | 23 | Seattle |
| 01 | 20170122 | P2 | 13 | Seattle |
| 01 | 20170122 | P3 | 231 | Seattle |

De schema-toewijzingsregel configureren als de volgende kopie activiteit JSON-voorbeeld:

```json
{
    "name": "CopyFromMongoDBToSqlAzure",
    "type": "Copy",
    "typeProperties": {
        "source": {
            "type": "MongoDbV2Source"
        },
        "sink": {
            "type": "SqlSink"
        },
        "translator": {
            "type": "TabularTranslator",
            "schemaMapping": {
                "orderNumber": "$.number", 
                "orderDate": "$.date", 
                "order_pd": "prod", 
                "order_price": "price",
                "city": " $.city[0].name"
            },
            "collectionReference":  "$.orders"
        }
    }
}
```

## <a name="data-type-mapping"></a>Toewijzing van het type gegevens

Kopieeractiviteit voert typen gegevensbronnen sink-type toewijzing met de volgende stappen 2-benadering:

1. Systeemeigen brontypen converteren naar tussentijdse gegevenstypen voor Azure Data Factory
2. Azure Data Factory tussentijdse gegevenstypen converteren naar systeemeigen sink-type

Hier vindt u de toewijzing tussen de systeemeigen naar tussentijdse type in de sectie "Data type toewijzing" in het onderwerp van elke connector.

### <a name="supported-data-types"></a>Ondersteunde gegevenstypen

Data Factory ondersteunt de volgende tijdelijke gegevenstypen: U kunt onderstaande waarden opgeven bij het configureren van type informatie in [gegevenssetstructuur](concepts-datasets-linked-services.md#dataset-structure) configuratie:

* Byte[]
* Booleaans
* Datum en tijd
* Datetimeoffset
* Decimaal
* Double-waarde
* GUID
* Int16
* Int32
* Int64
* Enkelvoudig
* Reeks
* Periode

### <a name="explicit-data-type-conversion"></a>Expliciete conversie van gegevenstype

Bij het kopiëren van gegevens in de gegevens opslaat met vaste schema, bijvoorbeeld SQL Server/Oracle, wanneer de bron- en sinkblobpaden heeft een ander type op dezelfde kolom, kan de expliciete conversie moet worden gedeclareerd in de bron-zijde:

* Voor bestandsbron, bijvoorbeeld CSV/Avro, de typeconversie moet worden gedeclareerd via de structuur van de gegevensbron met volledige kolomlijst (side kolom naam- en sinkblobpaden kant brontype)
* Voor relationele bron (bijvoorbeeld SQL/Oracle), moet de typeconversie worden bereikt door expliciete type casten in de query-instructie.

## <a name="when-to-specify-dataset-structure"></a>"Gegevenssetstructuur" opgeven

In onderstaande scenario's is "structuur" in de gegevensset vereist:

* Toepassen van [expliciete conversie van gegevenstype](#explicit-data-type-conversion) voor bestandsbronnen tijdens het kopiëren (invoergegevensset)
* Toepassen van [expliciete kolomtoewijzing](#explicit-column-mapping) tijdens het kopiëren (zowel invoer en uitvoer gegevensset)
* Kopiëren van Dynamics 365 / CRM bron (invoergegevensset)
* Kopiëren naar Cosmos DB als het geneste object wanneer gegevensbron geen JSON-bestanden (uitvoergegevensset)

In onderstaande scenario's, wordt "structuur" in de gegevensset aangeraden:

* Kopiëren uit een tekstbestand zonder header (invoergegevensset). U kunt de namen van de kolommen voor tekstbestand overeenkomt met de bijbehorende sink-kolommen van het configureren van expliciete kolomtoewijzing op te slaan.
* Kopiëren van gegevens worden opgeslagen met een flexibel schema, bijvoorbeeld Azure-tabel/Cosmos-DB (invoergegevensset), om te garanderen van de verwachte gegevens (kolommen) wordt gekopieerd in plaats van laten kopiëren afleiden activiteit schema op basis van de bovenste rij of rijen tijdens elke activiteit die wordt uitgevoerd.


## <a name="next-steps"></a>Volgende stappen
Zie de andere artikelen van de Kopieeractiviteit:

- [Overzicht kopieeractiviteit](copy-activity-overview.md)
- [Fouttolerantie van activiteit kopiëren](copy-activity-fault-tolerance.md)
- [Prestaties van de kopieeractiviteit](copy-activity-performance.md)
