---
title: Gegevens kopiëren van Teradata met Azure Data Factory | Microsoft Docs
description: Meer informatie over de Teradata-Connector van de Data Factory-service waarmee u gegevens kopiëren van Teradata-database naar gegevensarchieven die worden ondersteund door Data Factory als sink.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: jingwang
ms.openlocfilehash: 63f28c8b6eaceed12e1f76e9c0c5984e3b63b500
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561432"
---
# <a name="copy-data-from-teradata-using-azure-data-factory"></a>Gegevens kopiëren van Teradata met Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van Data Factory-service die u gebruikt:"]
>
> * [Versie 1:](v1/data-factory-onprem-teradata-connector.md)
> * [Huidige versie](connector-teradata.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een Teradata-database. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Teradata-database kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies ondersteunt deze Teradata-connector:

- Teradata **versie 14.10, 15.0 15.10, 16,0, 16,10 en 16.20**.
- Kopiëren van gegevens met **Basic** of **Windows** verificatie.
- Parallelle kopie van Teradata-bron. Zie [parallelle kopiëren van Teradata](#parallel-copy-from-teradata) gedeelte met details.

> [!NOTE]
>
> Azure Data Factory wordt de Teradata-connector bijgewerkt sinds zelfgehoste Cloudintegratieruntime v3.18, die is gemachtigd door een ingebouwde ODBC-stuurprogramma en biedt opties voor flexibele, evenals de parallelle kopie out-of-box om prestaties te verbeteren. Een bestaande workload met behulp van de vorige Teradata-connector die zijn aangewezen door .NET Data Provider voor Teradata wordt nog steeds ondersteund als-is, terwijl u gebruik van het nieuwe knooppunt voortaan worden voorgesteld. Noteer het nieuwe pad vereist een andere set gekoppelde service/gegevenssetkopie bron. Zie de betreffende sectie voor informatie over de configuratie.

## <a name="prerequisites"></a>Vereisten

Als uw Teradata niet openbaar toegankelijk is is, moet u voor het instellen van een zelfgehoste Cloudintegratieruntime. Zie voor meer informatie over integratieruntime [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md). De Integration Runtime biedt een ingebouwd Teradata-stuurprogramma vanaf versie 3,18, dus u hoeft te installeren op een stuurprogramma. Het stuurprogramma is vereist ' Visual C++ Redistributable 2012 Update 4 "op de machine zelfgehoste IR downloaden van [hier](https://www.microsoft.com/en-sg/download/details.aspx?id=30679) als u nog geïnstalleerd hebt.

Voor de versie van de zelfgehoste IR is lager dan 3,18, moet u voor het installeren van de [.NET Data Provider voor Teradata](https://go.microsoft.com/fwlink/?LinkId=278886) versie 14 of hoger op de machine Integration Runtime. 

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met Teradata-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Teradata gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Teradata** | Ja |
| connectionString | Hiermee geeft u de informatie die nodig zijn voor het verbinding maken met de Teradata-Database-exemplaar. Raadpleeg de volgende voorbeelden.<br/>U kunt ook wachtwoord plaatsen in Azure Key Vault en pull de `password` configuratie buiten de verbindingsreeks. Raadpleeg [referenties Store in Azure Key Vault](store-credentials-in-key-vault.md) artikel met meer informatie. | Ja |
| username | Geef de naam van de gebruiker verbinding maken met de Teradata-database. Is van toepassing als u Windows-verificatie. | Nee |
| password | Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. U kunt er ook voor kiezen om te [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). <br>Is van toepassing als u Windows-verificatie of verwijst naar een wachtwoord in Key Vault voor basisverificatie. | Nee |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. Er is een zelfgehoste Cloudintegratieruntime vereist zoals vermeld in [vereisten](#prerequisites). |Ja |

**Voorbeeld: met behulp van basisverificatie**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "connectionString": "DBCName=<server>;Uid=<username>;Pwd=<password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voorbeeld: met behulp van Windows-verificatie**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "connectionString": "DBCName=<server>",
            "username": "<username>",
            "password": "<password>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

> [!NOTE]
>
> Als u Teradata gekoppelde service de mogelijkheid krijgen door .NET Data Provider voor Teradata met de nettolading van de volgende, nog steeds wordt ondersteund als-is, terwijl u gebruik van het nieuwe knooppunt voortaan worden voorgesteld.

**De nettolading van de vorige:**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<Basic/Windows>",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund voor Teradata-gegevensset.

Om gegevens te kopiëren uit Teradata, worden de volgende eigenschappen ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **TeradataTable** | Ja |
| database | De naam van de Teradata-database. | Nee (als 'query' in de activiteitbron is opgegeven) |
| table | De naam van de tabel in de Teradata-database. | Nee (als 'query' in de activiteitbron is opgegeven) |

**Voorbeeld:**

```json
{
    "name": "TeradataDataset",
    "properties": {
        "type": "TeradataTable",
        "linkedServiceName": {
            "referenceName": "<Teradata linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

> [!NOTE]
>
> Als u "RelationalTable" type gegevensset zoals hieronder, nog steeds wordt ondersteund als-is, terwijl u gebruik van het nieuwe knooppunt voortaan worden voorgesteld.

**De nettolading van de vorige:**

```json
{
    "name": "TeradataDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Teradata linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Teradata-bron.

### <a name="teradata-as-source"></a>Teradata als bron

> [!TIP]
>
> Meer informatie uit [parallelle kopiëren van Teradata](#parallel-copy-from-teradata) sectie over het laden van gegevens uit Teradata efficiënt gebruik maakt van partitioneren van gegevens.

Om gegevens te kopiëren uit Teradata, de volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **TeradataSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM MyTable"`.<br>Wanneer u gepartitioneerde laden inschakelen, moet u de bijbehorende ingebouwde partitie parameter (s) van een toepassing aansluiten in uw query. Zie de voorbeelden in [parallelle kopiëren van Teradata](#parallel-copy-from-teradata) sectie. | Nee (als de tabel in de gegevensset is opgegeven) |
| partitionOptions | Hiermee geeft u de gegevens partitioneren van de opties die worden gebruikt om gegevens te laden uit Teradata. <br>Toestaan dat waarden zijn: **Geen** (standaard), **Hash** en **DynamicRange**.<br>Wanneer de partitieoptie is ingeschakeld (niet ' None'), ook Configureer **[ `parallelCopies` ](copy-activity-performance.md#parallel-copy)** instellen op kopieeractiviteit bijvoorbeeld als 4, dat bepaalt de mate van parallelle gelijktijdig om gegevens te laden uit Teradata de database. | Nee |
| partitionSettings | Geef de groep van de instellingen voor het partitioneren van gegevens. <br>Wanneer de partitieoptie is niet van toepassing `None`. | Nee |
| partitionColumnName | Geef de naam van de bronkolom **in het type geheel getal** dat wordt gebruikt door het bereik voor parallelle kopie partitioneren. Als niet is opgegeven, worden de primaire sleutel van de tabel automatisch gedetecteerd en gebruikt als partitiekolom. <br>Van toepassing wanneer de partitieoptie is `Hash` of `DynamicRange`. Als u query gebruikt om op te halen van de brongegevens, een toepassing aansluiten `?AdfHashPartitionCondition` of `?AdfRangePartitionColumnName` WHERE-component. Zie het voorbeeld in [parallelle kopiëren van Teradata](#parallel-copy-from-teradata) sectie. | Nee |
| partitionUpperBound | Maximale waarde van de partitiekolom kopiëren van gegevens. <br>Van toepassing wanneer de partitieoptie is `DynamicRange`. Als u query gebruikt om op te halen van de brongegevens, een toepassing aansluiten `?AdfRangePartitionUpbound` WHERE-component. Zie het voorbeeld in [parallelle kopiëren van Teradata](#parallel-copy-from-teradata) sectie. | Nee |
| PartitionLowerBound | De minimumwaarde van de partitiekolom om gegevens te kopiëren uit. <br>Van toepassing wanneer de partitieoptie is `DynamicRange`. Als u query gebruikt om op te halen van de brongegevens, een toepassing aansluiten `?AdfRangePartitionLowbound` WHERE-component. Zie het voorbeeld in [parallelle kopiëren van Teradata](#parallel-copy-from-teradata) sectie. | Nee |

> [!NOTE]
>
> Als u "RelationalSource" type kopieerbron zijn gebruikt, is het nog steeds ondersteund als-is, maar biedt geen ondersteuning voor de nieuwe ingebouwde parallel laden uit Teradata (opties voor partitioneren). U worden gebruik van dit nieuwe bestand voortaan voorgesteld.

**Voorbeeld: gegevens met behulp van eenvoudige query uitvoert zonder partitie kopiëren**

```json
"activities":[
    {
        "name": "CopyFromTeradata",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Teradata input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "TeradataSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="parallel-copy-from-teradata"></a>Parallelle kopiëren van Teradata

Data factory Teradata-connector biedt ingebouwde gegevens partitioneren om gegevens te kopiëren uit Teradata parallel met uitstekende prestaties. Gegevens partitioneren op kopieeractiviteit -> Teradata-bron, kunt u vinden:

![Opties voor partitioneren](./media/connector-teradata/connector-teradata-partition-options.png)

Wanneer u gepartitioneerde kopiëren inschakelt, wordt data factory parallelle query's uitgevoerd op de Teradata-bron om gegevens te laden door partitie. De mate van parallelle is geconfigureerd en beheerd **[ `parallelCopies` ](copy-activity-performance.md#parallel-copy)** instellen voor de kopieeractiviteit. Als u bijvoorbeeld `parallelCopies` als vier, data factory gelijktijdig genereert en vier query's uitgevoerd op basis van de opgegeven partitieoptie en instellingen voor elk gedeelte bij het ophalen van gegevens uit uw Teradata-database.

U worden voorgesteld om in te schakelen parallelle kopie met gegevens partitioneren met name wanneer u grote hoeveelheden gegevens vanuit de Teradata-database laden. Hier volgen de aanbevolen configuraties voor verschillende scenario's:

| Scenario                                                     | Aanbevolen instellingen                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Volledig laden van grote tabel                                   | **Partitie-optie**: Hash. <br><br/>Tijdens de uitvoering, data Factory automatisch detecteren PK-kolom, hash op basis van deze toepassing en gegevens kopiëren met partities. |
| Laden van grote hoeveelheden gegevens met behulp van aangepaste query                 | **Partitie-optie**: Hash.<br>**Query**: `SELECT * FROM <TABLENAME> WHERE ?AdfHashPartitionCondition AND <your_additional_where_clause>`.<br>**Partitiekolom**: Geef op de kolom die wordt gebruikt voor toepassen hash-partitie. Indien niet opgegeven, worden de PK-kolom van de tabel die u hebt opgegeven in Teradata gegevensset automatisch gedetecteerd door ADF.<br><br>Bij het uitvoeren van data factory vervangen `?AdfHashPartitionCondition` met de hash-partitie logica en verzenden naar Teradata. |
| Laden van grote hoeveelheden gegevens met behulp van aangepaste query, met een kolom met gehele getallen met gelijkmatige waarde voor het partitioneren van bereik | **Opties voor partitioneren**: Dynamisch Bereikpartitie.<br>**Query**: `SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>`.<br>**Partitiekolom**: Geef op de kolom die wordt gebruikt om gegevens te partitioneren. U kunt partitioneren op basis van de kolom met gegevenstype integer.<br>**Partitie bovengrens** en **partitie ondergrens**: Als u wilt filteren op basis van de partitiekolom alleen ophalen van gegevens tussen de boven- en ondergrenzen bereik opgeven<br><br>Bij het uitvoeren van data factory vervangen `?AdfRangePartitionColumnName`, `?AdfRangePartitionUpbound`, en `?AdfRangePartitionLowbound` met de kolomnaam van de werkelijke en waarde bereiken voor elke partitie en verzenden naar Teradata. <br>Bijvoorbeeld, als uw partitie kolom-ID ingesteld met ondergrens als 1 en bovengrens als 80, met parallelle kopie instellen als 4, ADF ophalen van gegevens door 4 partities met de ID tussen [1,20], [21, 40], [41, 60] en [61, 80]. |

**Voorbeeld: query's uitvoeren met hash-partitie**

```json
"source": {
    "type": "TeradataSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfHashPartitionCondition AND <your_additional_where_clause>",
    "partitionOption": "Hash",
    "partitionSettings": {
        "partitionColumnName": "<hash_partition_column_name>"
    }
}
```

**Voorbeeld: query's uitvoeren met dynamisch Bereikpartitie**

```json
"source": {
    "type": "TeradataSource",
    "query": "SELECT * FROM <TABLENAME> WHERE ?AdfRangePartitionColumnName <= ?AdfRangePartitionUpbound AND ?AdfRangePartitionColumnName >= ?AdfRangePartitionLowbound AND <your_additional_where_clause>",
    "partitionOption": "DynamicRange",
    "partitionSettings": {
        "partitionColumnName": "<dynamic_range_partition_column_name>",
        "partitionUpperBound": "<upper_value_of_partition_column>",
        "partitionLowerBound": "<lower_value_of_partition_column>"
    }
}
```

## <a name="data-type-mapping-for-teradata"></a>Toewijzing voor Teradata-gegevenstype

Het kopiëren van gegevens uit Teradata, worden de volgende toewijzingen uit Teradata-gegevenstypen gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe copy activity in het schema en de gegevens van een brontype aan de sink toegewezen.

| Teradata-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| BigInt |Int64 |
| Blob |Byte[] |
| Byte |Byte[] |
| ByteInt |Int16 |
| Char |String |
| Clob |String |
| Date |DateTime |
| Decimal |Decimal |
| Double |Double |
| Graphic |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Integer |Int32 |
| Interval Day |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Interval Day To Hour |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Interval Day To Minute |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Interval Day To Second |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Interval Hour |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Interval Hour To Minute |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Interval Hour To Second |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Interval Minute |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Interval Minute To Second |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Interval Month |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Interval Second |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Interval Year |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Interval Year To Month |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Aantal |Double |
| Periode (datum) |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Periode (tijd) |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Periode (tijd met tijdzone) |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Periode (tijdstempel) |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Periode (Timestamp met tijdzone) |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| SmallInt |Int16 |
| Time |TimeSpan |
| Time With Time Zone |TimeSpan |
| Timestamp |DateTime |
| Timestamp With Time Zone |DateTime |
| VarByte |Byte[] |
| VarChar |String |
| VarGraphic |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |
| Xml |Wordt niet ondersteund. Expliciete cast-conversie in bronquery van toepassing. |


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
