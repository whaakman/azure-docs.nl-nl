---
title: Gegevens kopiëren van MySQL met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over de MySQL-connector in Azure Data Factory waarmee u gegevens kopiëren van een MySQL-database naar een gegevensarchief dat wordt ondersteund als een sink.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/23/2018
ms.author: jingwang
ms.openlocfilehash: 1f3d6434c7226465f9e054d5e5bf35fbb228b311
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54014484"
---
# <a name="copy-data-from-mysql-using-azure-data-factory"></a>Gegevens kopiëren van MySQL met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-onprem-mysql-connector.md)
> * [Huidige versie](connector-mysql.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een MySQL-database. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit de MySQL-database kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies deze MySQL-connector biedt ondersteuning voor MySQL **versie 5.1 en hoger**.

## <a name="prerequisites"></a>Vereisten

Als uw MySQL-database niet openbaar toegankelijk is is, moet u voor het instellen van een zelfgehoste Cloudintegratieruntime. Zie voor meer informatie over de zelf-hostende integratieruntimes, [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md) artikel. De Integration Runtime biedt een ingebouwde stuurprogramma voor MySQL vanaf versie 3.7, dus u hoeft te installeren op een stuurprogramma.

Voor de zelfgehoste IR-versie is lager dan 3.7, moet u voor het installeren van de [MySQL Connector/Net voor Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) versie tussen 6.6.5 en 6.10.7 op de machine Integration Runtime. Deze 32-bits stuurprogramma is compatibel met 64-bits IR.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor MySQL-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor MySQL gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **MySql** | Ja |
| connectionString | Geef informatie op die nodig zijn voor het verbinding maken met de Azure Database for MySQL-exemplaar. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de zelfgehoste Cloudintegratieruntime of Azure Integration Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk zijn is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

Een gebruikelijke verbindingsreeks is `Server=<server>;Port=<port>;Database=<database>;UID=<username>;PWD=<password>`. Meer eigenschappen die u per uw situatie instellen kunt:

| Eigenschap | Description | Opties | Vereist |
|:--- |:--- |:--- |:--- |:--- |
| SSLMode | Deze optie bepaalt u of het stuurprogramma maakt gebruik van SSL-versleuteling en verificatie bij het verbinden met MySQL. Bijvoorbeeld `SSLMode=<0/1/2/3/4>`| UITGESCHAKELD (0) / VOORKEUR (1) **(standaard)** / vereist (2) / VERIFY_CA (3) / VERIFY_IDENTITY (4) | Nee |
| useSystemTrustStore | Deze optie bepaalt u of u wilt gebruiken van een CA-certificaat uit het archief van de vertrouwensrelatie system of vanuit een opgegeven PEM-bestand. Bijvoorbeeld `UseSystemTrustStore=<0/1>;`| (1) ingeschakeld / uitgeschakeld (0) **(standaard)** | Nee |

**Voorbeeld:**

```json
{
    "name": "MySQLLinkedService",
    "properties": {
        "type": "MySql",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server=<server>;Port=<port>;Database=<database>;UID=<username>;PWD=<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Als u MySQL gekoppelde service met de nettolading van de volgende, nog steeds wordt ondersteund als-is, terwijl u gebruik van het nieuwe knooppunt voortaan worden voorgesteld.

**De nettolading van de vorige:**

```json
{
    "name": "MySQLLinkedService",
    "properties": {
        "type": "MySql",
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de MySQL-gegevensset.

Om gegevens te kopiëren van MySQL, stel de eigenschap type van de gegevensset in **RelationalTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **RelationalTable** | Ja |
| tableName | De naam van de tabel in de MySQL-database. | Nee (als 'query' in de activiteitbron is opgegeven) |

**Voorbeeld**

```json
{
    "name": "MySQLDataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<MySQL linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de MySQL-bron.

### <a name="mysql-as-source"></a>MySQL als bron

Om gegevens te kopiëren van MySQL, stelt u het brontype in de kopieeractiviteit naar **RelationalSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **RelationalSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM MyTable"`. | Nee (als de 'tableName' in de gegevensset is opgegeven) |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromMySQL",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<MySQL input dataset name>",
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
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-mysql"></a>Toewijzing voor MySQL-gegevenstype

Het kopiëren van gegevens van MySQL, worden de volgende toewijzingen van MySQL-gegevenstypen gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe copy activity in het schema en de gegevens van een brontype aan de sink toegewezen.

| MySQL-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| `bigint` |`Int64` |
| `bigint unsigned` |`Decimal` |
| `bit(1)` |`Boolean` |
| `bit(M), M>1`|`Byte[]`|
| `blob` |`Byte[]` |
| `bool` |`Int16` |
| `char` |`String` |
| `date` |`Datetime` |
| `datetime` |`Datetime` |
| `decimal` |`Decimal, String` |
| `double` |`Double` |
| `double precision` |`Double` |
| `enum` |`String` |
| `float` |`Single` |
| `int` |`Int32` |
| `int unsigned` |`Int64`|
| `integer` |`Int32` |
| `integer unsigned` |`Int64` |
| `long varbinary` |`Byte[]` |
| `long varchar` |`String` |
| `longblob` |`Byte[]` |
| `longtext` |`String` |
| `mediumblob` |`Byte[]` |
| `mediumint` |`Int32` |
| `mediumint unsigned` |`Int64` |
| `mediumtext` |`String` |
| `numeric` |`Decimal` |
| `real` |`Double` |
| `set` |`String` |
| `smallint` |`Int16` |
| `smallint unsigned` |`Int32` |
| `text` |`String` |
| `time` |`TimeSpan` |
| `timestamp` |`Datetime` |
| `tinyblob` |`Byte[]` |
| `tinyint` |`Int16` |
| `tinyint unsigned` |`Int16` |
| `tinytext` |`String` |
| `varchar` |`String` |
| `year` |`Int` |


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
