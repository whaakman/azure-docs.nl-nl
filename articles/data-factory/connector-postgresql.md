---
title: Kopiëren van gegevens van PostgreSQL met behulp van Azure Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens van PostgreSQL naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/23/2018
ms.author: jingwang
ms.openlocfilehash: a4f300666d0ab5345274d69d9ad6ad6871ce85e3
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36334037"
---
# <a name="copy-data-from-postgresql-by-using-azure-data-factory"></a>Gegevens kopiëren van PostgreSQL met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-onprem-postgresql-connector.md)
> * [Versie 2 - Preview](connector-postgresql.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een PostgreSQL-database. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.


> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [PostgreSQL-connector in V1](v1/data-factory-onprem-postgresql-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit een PostgreSQL-database kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder PostgreSQL biedt ondersteuning voor deze connector PostgreSQL **versie 7.4 en hoger**.

## <a name="prerequisites"></a>Vereisten

Als uw PostgreSQL-database is niet openbaar toegankelijk is, moet u voor het instellen van een Self-hosted integratie-Runtime. Zie voor meer informatie over host zichzelf integratie runtimes, [Self-hosted integratie Runtime](create-self-hosted-integration-runtime.md) artikel. De Runtime-integratie biedt een ingebouwd PostgreSQL-stuurprogramma vanaf versie 3.7, dus u hoeft niet te eventuele stuurprogramma handmatig installeren.

Voor Self-hosted IR-versie lager is dan 3.7, moet u voor het installeren van de [Ngpsql-gegevensprovider voor PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) met versie tussen 2.0.12 en punt 3.1.9 op de machine integratie Runtime.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar PostgreSQL-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor PostgreSQL gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **PostgreSql** | Ja |
| connectionString | Een ODBC-verbindingsreeks verbinding maken met Azure-Database voor PostgreSQL. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of Self-hosted integratie Runtime gebruiken (indien de gegevensopslag bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

Een typische verbindingsreeks is `Server=<server>;Database=<database>;Port=<port>;UID=<username>;Password=<Password>`. Meer eigenschappen die u per uw aanvraag instellen kunt:

| Eigenschap | Beschrijving | Opties | Vereist |
|:--- |:--- |:--- |:--- |:--- |
| EncryptionMethod (EM)| De methode het stuurprogramma gebruikt voor het versleutelen van gegevens die tussen het stuurprogramma en de database-server verzonden. Bijvoorbeeld `ValidateServerCertificate=<0/1/6>;`| 0 (geen versleuteling) **(standaard)** / 1 (SSL) / 6 (RequestSSL) | Nee |
| ValidateServerCertificate (VSC) | Hiermee wordt bepaald of het certificaat dat wordt verzonden door de database-server als SSL-versleuteling is ingeschakeld wordt gevalideerd door het stuurprogramma (versleutelingsmethode = 1). Bijvoorbeeld `ValidateServerCertificate=<0/1>;`| 0 (uitgeschakeld) **(standaard)** / 1 (ingeschakeld) | Nee |

**Voorbeeld:**

```json
{
    "name": "PostgreSqlLinkedService",
    "properties": {
        "type": "PostgreSql",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server=<server>;Database=<database>;Port=<port>;UID=<username>;Password=<Password>"
            }
        }
    }
}
```

Als u PostgreSQL gekoppelde service met de nettolading van de volgende gebruikte, nog steeds wordt ondersteund als-is, terwijl u het gebruik van de nieuwe knop voortaan worden voorgesteld.

**De nettolading van de vorige:**

```json
{
    "name": "PostgreSqlLinkedService",
    "properties": {
        "type": "PostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door PostgreSQL-gegevensset.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren uit een PostgreSQL, **RelationalTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **RelationalTable** | Ja |
| tableName | De naam van de tabel in de PostgreSQL-database. | Nee (als 'query' in de activiteitbron is opgegeven) |

**Voorbeeld**

```json
{
    "name": "PostgreSQLDataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<PostgreSQL linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door PostgreSQL-bron.

### <a name="postgresql-as-source"></a>PostgreSQL als bron

Om gegevens te kopiëren uit een PostgreSQL, stelt u het brontype in de kopieerbewerking naar **RelationalSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **RelationalSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"query": "SELECT * FROM \"MySchema\".\"MyTable\""`. | Nee (als 'tableName' in de gegevensset is opgegeven) |

> [!NOTE]
> Schema- en tabelnamen zijn hoofdlettergevoelig. Omsluiten in `""` (dubbele aanhalingstekens) in de query.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromPostgreSQL",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<PostgreSQL input dataset name>",
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
                "type": "RelationalSource",
                "query": "SELECT * FROM \"MySchema\".\"MyTable\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-postgresql"></a>Toewijzing voor PostgreSQL-gegevenstype

Bij het kopiëren van gegevens uit een PostgreSQL, worden de volgende toewijzingen van gegevenstypen PostgreSQL gebruikt voor Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

| PostgreSQL-gegevenstype | PostgresSQL aliassen | Data factory tussentijdse gegevenstype |
|:--- |:--- |:--- |
| `abstime` | |`Datetime` | &nbsp;
| `bigint` | `int8` | `Int64` |
| `bigserial` | `serial8` | `Int64` |
| `bit [ (n) ]` | | `Byte[], String` | &nbsp;
| `bit varying [ (n) ]` | ' varbit |Byte [] tekenreeks ' |
| `boolean` | `bool` | `Boolean` |
| `box` | | `Byte[], String` | &nbsp;
| `bytea` | | `Byte[], String` |&nbsp;
| `character [ (n) ]` | `char [ (n) ]` | `String` |
| `character varying [ (n) ]` | `varchar [ (n) ]` | `String` |
| `cid` | | `String` |&nbsp;
| `cidr` | | `String` |&nbsp;
| `circle` | |`Byte[], String` |&nbsp;
| `date` | |`Datetime` |&nbsp;
| `daterange` | |`String` |&nbsp;
| `double precision` |`float8` |`Double` |
| `inet` | |`Byte[], String` |&nbsp;
| `intarry` | |`String` |&nbsp;
| `int4range` | |`String` |&nbsp;
| `int8range` | |`String` |&nbsp;
| `integer` | ' int, int4 |Int32` |
| `interval [ fields ] [ (p) ]` | | `Timespan` |&nbsp;
| `json` | | `String` |&nbsp;
| `jsonb` | | `Byte[]` |&nbsp;
| `line` | | `Byte[], String` |&nbsp;
| `lseg` | | `Byte[], String` |&nbsp;
| `macaddr` | | `Byte[], String` |&nbsp;
| `money` | | `Decimal` |&nbsp;
| `numeric [ (p, s) ]`|`decimal [ (p, s) ]` |`Decimal` |
| `numrange` | |`String` |&nbsp;
| `oid` | |`Int32` |&nbsp;
| `path` | |`Byte[], String` |&nbsp;
| `pg_lsn` | |`Int64` |&nbsp;
| `point` | |`Byte[], String` |&nbsp;
| `polygon` | |`Byte[], String` |&nbsp;
| `real` |`float4` |`Single` |
| `smallint` |`int2` |`Int16` |
| `smallserial` |`serial2` |`Int16` |
| `serial` |`serial4` |`Int32` |
| `text` | |`String` |&nbsp;


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
