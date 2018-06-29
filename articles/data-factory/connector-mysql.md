---
title: Gegevens kopiëren van MySQL met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over de MySQL-connector in Azure Data Factory waarmee u gegevens uit een MySQL-database kopiëren naar een gegevensopslag die wordt ondersteund als een sink.
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
ms.openlocfilehash: bb3179f1db077aacc7e36acf16486ee77a7f36e7
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37051260"
---
# <a name="copy-data-from-mysql-using-azure-data-factory"></a>Gegevens kopiëren van MySQL met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1](v1/data-factory-onprem-mysql-connector.md)
> * [Huidige versie](connector-mysql.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een MySQL-database. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit de MySQL-database kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder MySQL biedt ondersteuning voor deze connector MySQL **versie 5.1 en hoger**.

## <a name="prerequisites"></a>Vereisten

Als uw MySQL-database is niet openbaar toegankelijk is, moet u voor het instellen van een Self-hosted integratie-Runtime. Zie voor meer informatie over host zichzelf integratie runtimes, [Self-hosted integratie Runtime](create-self-hosted-integration-runtime.md) artikel. De Runtime-integratie biedt een ingebouwd MySQL-stuurprogramma vanaf versie 3.7, dus u hoeft niet te eventuele stuurprogramma handmatig installeren.

Voor Self-hosted IR-versie lager is dan 3.7, moet u voor het installeren van de [MySQL Connector/Net voor Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) versie tussen 6.6.5 en 6.10.7 op de machine integratie Runtime. Deze 32-bits stuurprogramma is compatibel met 64-bits IR

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar MySQL-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor MySQL gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **MySql** | Ja |
| connectionString | Geef informatie op die nodig zijn voor de verbinding met de Azure-Database voor MySQL-exemplaar. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Self-hosted integratie Runtime of Azure integratie Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

Een typische verbindingsreeks is `Server=<server>;Port=<port>;Database=<database>;UID=<username>;PWD=<password>`. Meer eigenschappen die u per uw aanvraag instellen kunt:

| Eigenschap | Beschrijving | Opties | Vereist |
|:--- |:--- |:--- |:--- |:--- |
| SSLMode | Deze optie geeft aan of het stuurprogramma voor SSL-versleuteling en verificatie gebruikt bij het verbinden met MySQL. Bijvoorbeeld `SSLMode=<0/1/2/3/4>`| UITGESCHAKELD (0) / VOORKEUR (1) **(standaard)** / vereist (2) / VERIFY_CA (3) / VERIFY_IDENTITY (4) | Nee |
| useSystemTrustStore | Deze optie geeft aan of gebruik een CA-certificaat uit het archief van de vertrouwensrelatie system of vanuit een opgegeven PEM-bestand. Bijvoorbeeld `UseSystemTrustStore=<0/1>;`| (1) ingeschakeld / uitgeschakeld (0) **(standaard)** | Nee |

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

Als u MySQL gekoppelde service met de nettolading van de volgende gebruikte, nog steeds wordt ondersteund als-is, terwijl u het gebruik van de nieuwe knop voortaan worden voorgesteld.

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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de MySQL-gegevensset.

Om gegevens te kopiëren van MySQL, stel de eigenschap type van de gegevensset **RelationalTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door MySQL-bron.

### <a name="mysql-as-source"></a>MySQL als bron

Om gegevens te kopiëren van MySQL, stelt u het brontype in de kopieerbewerking naar **RelationalSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **RelationalSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM MyTable"`. | Nee (als 'tableName' in de gegevensset is opgegeven) |

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

## <a name="data-type-mapping-for-mysql"></a>Toewijzing voor de MySQL-gegevenstype

Bij het kopiëren van gegevens van MySQL, worden de volgende toewijzingen van MySQL-gegevenstypen gebruikt voor Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

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
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
