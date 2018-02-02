---
title: "Gegevens kopiëren naar en van Oracle met behulp van Azure Data Factory | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens van ondersteunde bron winkels met een Oracle-database of van Oracle voor ondersteunde sink stores met behulp van Data Factory."
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
ms.date: 01/05/2018
ms.author: jingwang
ms.openlocfilehash: d6b96bc40325d398c91e293ec6ca8f8cc2993e58
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="copy-data-from-and-to-oracle-by-using-azure-data-factory"></a>Gegevens kopiëren van en naar Oracle met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - algemeen beschikbaar](v1/data-factory-onprem-oracle-connector.md)
> * [Versie 2 - Preview](connector-oracle.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar een Oracle-database. Dit is gebaseerd op de [Kopieeractiviteit overzicht](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory, die in het algemeen beschikbaar is, Zie [Oracle-connector in versie 1](v1/data-factory-onprem-oracle-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit een Oracle-database kopiëren naar een ondersteunde sink-gegevensarchief. U kunt ook gegevens uit alle ondersteunde brongegevensarchief kopiëren naar een Oracle-database. Zie voor een lijst met gegevensarchieven die als bronnen of PUT worden ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze Oracle-connector de volgende versies van een Oracle-database. Het ondersteunt ook Basic of de OID-verificaties:

- Oracle 12c R1 (12.1)
- Oracle 11g R1, R2 (11.1, 11.2)
- Oracle 10g R1, R2 (10.1, 10.2)
- Oracle 9i R1, R2 (9.0.1, 9.2)
- Oracle 8i R3 (8.1.7)

## <a name="prerequisites"></a>Vereisten

Om gegevens te kopiëren van en naar een Oracle-database die niet openbaar toegankelijk is, moet u voor het instellen van een Self-hosted integratie-Runtime. Zie voor meer informatie over de integratie runtime [Self-hosted integratie Runtime](create-self-hosted-integration-runtime.md). De runtime integratie biedt een ingebouwd Oracle-stuurprogramma. Daarom hoeft u niet handmatig een stuurprogramma installeren wanneer u gegevens van en naar Oracle kopiëren.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met de Oracle-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de Oracle gekoppelde service.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **Oracle**. | Ja |
| connectionString | Hiermee geeft u de benodigde informatie om verbinding maken met de Oracle-Database-exemplaar. Dit veld markeren als SecureString.<br><br>**Verbindingstype ondersteund**: kunt u **Oracle SID** of **Oracle-servicenaam** voor het identificeren van de database:<br>-Als u de SID gebruikt:`Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;`<br>-Als u de naam van de Service gebruikt:`Host=<host>;Port=<port>;ServiceName=<sid>;User Id=<username>;Password=<password>;` | Ja |
| connectVia | De [integratie runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Self-hosted integratie Runtime of Azure integratie Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

**Voorbeeld:**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;"
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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Oracle-gegevensset.

Om gegevens te kopiëren van en naar Oracle, stel de eigenschap type van de gegevensset **OracleTable**. De volgende eigenschappen worden ondersteund.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op **OracleTable**. | Ja |
| tableName |De naam van de tabel in de Oracle-database waarnaar de gekoppelde service verwijst. | Ja |

**Voorbeeld:**

```json
{
    "name": "OracleDataset",
    "properties":
    {
        "type": "OracleTable",
        "linkedServiceName": {
            "referenceName": "<Oracle linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die door de Oracle-bron- en sink ondersteund.

### <a name="oracle-as-a-source-type"></a>Oracle als een type gegevensbron

Om gegevens te kopiëren uit Oracle, stelt u het brontype in de kopieerbewerking naar **OracleSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **OracleSource**. | Ja |
| oracleReaderQuery | Gebruik de aangepaste SQL-query om gegevens te lezen. Een voorbeeld is `"SELECT * FROM MyTable"`. | Nee |

Als u geen 'oracleReaderQuery' opgeeft, de kolommen die zijn gedefinieerd in de sectie 'structuur' van de gegevensset gebruikt een query wilt maken (`select column1, column2 from mytable`) om uit te voeren op de Oracle-database. Als de definitie van de gegevensset geen 'structure', worden alle kolommen uit de tabel geselecteerd.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Oracle input dataset name>",
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
                "type": "OracleSource",
                "oracleReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="oracle-as-a-sink-type"></a>Oracle als sink-type

Om gegevens te kopiëren met Oracle, stelt u het sink-type in de kopieerbewerking naar **OracleSink**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **sink** sectie.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de activiteit kopiëren sink moet worden ingesteld op **OracleSink**. | Ja |
| writeBatchSize | Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt.<br/>Toegestane waarden zijn geheel getal (aantal rijen). |Nee (de standaardwaarde is 10.000) |
| writeBatchTimeout | Wachttijd voor de batch-insert-bewerking te voltooien voordat er een optreedt time-out.<br/>Toegestane waarden zijn Timespan. Een voorbeeld is 00:30:00 (30 minuten). | Nee |
| preCopyScript | Geef een SQL-query voor de kopieeractiviteit worden uitgevoerd voordat het schrijven van gegevens in Oracle in elke uitvoering. U kunt deze eigenschap gebruiken om de vooraf geladen gegevens op te schonen. | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Oracle output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OracleSink"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-oracle"></a>Toewijzing voor Oracle-gegevenstype

Wanneer u gegevens van en naar Oracle kopiëren, worden de volgende toewijzingen van Oracle-gegevenstypen gebruikt voor Data Factory tussentijdse gegevenstypen. Zie voor meer informatie over hoe het brontype schema en de gegevens in de kopieerbewerking wordt toegewezen aan de sink, [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md).

| Oracle-gegevenstype | Data Factory tussentijdse gegevenstype |
|:--- |:--- |
| BBESTAND |Byte[] |
| BLOB |Byte[]<br/>(alleen ondersteund voor Oracle 10g en hoger) |
| CHAR |Tekenreeks |
| CLOB |Tekenreeks |
| DATE |Datum en tijd |
| FLOAT |Decimaal, tekenreeks (als precision > 28) |
| GEHEEL GETAL |Decimaal, tekenreeks (als precision > 28) |
| LANG |Tekenreeks |
| LANGE ONBEWERKTE |Byte[] |
| NCHAR |Tekenreeks |
| NCLOB |Tekenreeks |
| AANTAL |Decimaal, tekenreeks (als precision > 28) |
| NVARCHAR2 |Tekenreeks |
| RAW |Byte[] |
| ROWID |Tekenreeks |
| TIJDSTEMPEL |Datum en tijd |
| TIJDSTEMPEL MET DE LOKALE TIJDZONE |Tekenreeks |
| TIJDSTEMPEL MET TIJDZONE |Tekenreeks |
| NIET-ONDERTEKEND GEHEEL GETAL |Aantal |
| VARCHAR2 |Tekenreeks |
| XML |Tekenreeks |

> [!NOTE]
> De gegevenstypen INTERVAL jaar aan maand en het INTERVAL dag naar worden niet het tweede ondersteund.


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst van gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Gegevensfactory [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).