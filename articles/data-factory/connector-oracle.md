---
title: "Gegevens kopiëren naar/van Oracle met behulp van Azure Data Factory | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens van ondersteunde bron winkels met Oracle-database (of) van Oracle voor ondersteunde sink stores met behulp van Data Factory."
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
ms.openlocfilehash: 10db7959396b4ee9927e4272dec9939ac8c13580
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="copy-data-from-and-to-oracle-using-azure-data-factory"></a>Gegevens kopiëren van en naar Azure Data Factory met Oracle
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-onprem-oracle-connector.md)
> * [Versie 2 - Preview](connector-oracle.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar een Oracle-database. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Oracle-connector in V1](v1/data-factory-onprem-oracle-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van Oracle-database kopiëren naar een ondersteunde sink-gegevensarchief of gegevens kopiëren van een ondersteunde brongegevensarchief naar Oracle-database. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder deze Oracle-connector ondersteunt de volgende versies van Oracle-database, en ondersteunt Basic of de OID-verificaties.

- Oracle 12c R1 (12.1)
- Oracle 11g R1, R2 (11.1, 11.2)
- Oracle 10g R1, R2 (10.1, 10.2)
- Oracle 9i R1, R2 (9.0.1, 9.2)
- Oracle 8i R3 (8.1.7)

## <a name="prerequisites"></a>Vereisten

Om gegevens te kopiëren van/naar een Oracle-database die niet openbaar toegankelijk is, moet u voor het instellen van een Self-hosted integratie-Runtime. Zie [Self-hosted integratie Runtime](create-self-hosted-integration-runtime.md) voor meer informatie over de integratie-runtime. De Runtime-integratie biedt een ingebouwd Oracle-stuurprogramma, dus u hoeft niet te handmatig installeren van een stuurprogramma bij het kopiëren van gegevens van/naar Oracle.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek op Oracle-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Oracle gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Oracle** | Ja |
| connectionString | Geef informatie op die nodig zijn voor het verbinding maken met de Oracle-Database-exemplaar. Dit veld markeren als een SecureString.<br><br>**Verbindingstype ondersteund**: u kunt gebruiken **Oracle SID** of **Oracle-servicenaam** voor het identificeren van de database:<br>-Met behulp van de beveiligings-id:`Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;`<br>-Met behulp van de servicenaam:`Host=<host>;Port=<port>;ServiceName=<sid>;User Id=<username>;Password=<password>;` | Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Self-hosted integratie Runtime of Azure integratie Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Oracle-gegevensset.

Om gegevens te kopiëren van/naar Oracle, stel de eigenschap type van de gegevensset **OracleTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **OracleTable** | Ja |
| tableName |De naam van de tabel in de Oracle-Database waarnaar de gekoppelde service verwijst. | Ja |

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

## <a name="copy-activity-properties"></a>Eigenschappen van de activiteit kopiëren

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die door Oracle-bron- en sink ondersteund.

### <a name="oracle-as-source"></a>Oracle als bron

Om gegevens te kopiëren uit Oracle, stelt u het brontype in de kopieerbewerking naar **OracleSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **OracleSource** | Ja |
| oracleReaderQuery | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM MyTable"`. | Nee |

Als u geen 'oracleReaderQuery' opgeeft, de kolommen die zijn gedefinieerd in de sectie 'structuur' van de gegevensset worden gebruikt om een query samen te stellen (`select column1, column2 from mytable`) om uit te voeren op de Oracle-database. Als de definitie van de gegevensset geen 'de structuur', worden alle kolommen uit de tabel geselecteerd.

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

### <a name="oracle-as-sink"></a>Oracle als sink

Om gegevens te kopiëren met Oracle, stelt u het sink-type in de kopieerbewerking naar **OracleSink**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **sink** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de activiteit kopiëren sink moet worden ingesteld op: **OracleSink** | Ja |
| writeBatchSize | Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt.<br/>Toegestane waarden zijn: geheel getal (aantal rijen). |Nee (de standaardwaarde is 10000) |
| writeBatchTimeout | Wachttijd voor de batch-insert-bewerking te voltooien voordat er een optreedt time-out.<br/>Toegestane waarden zijn: Timespan. Voorbeeld: 00:30:00 (30 minuten). | Nee |
| preCopyScript | Geef een SQL-query voor de Kopieeractiviteit worden uitgevoerd voordat het schrijven van gegevens in Oracle in elke uitvoering. U kunt deze eigenschap gebruiken om de vooraf geladen gegevens op te schonen. | Nee |

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

Bij het kopiëren van gegevens van/naar Oracle, worden de volgende toewijzingen van Oracle-gegevenstypen gebruikt voor Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

| Oracle-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| BBESTAND |Byte] |
| BLOB |Byte]<br/>(alleen ondersteund voor Oracle 10g en hoger) |
| CHAR |Tekenreeks |
| CLOB |Tekenreeks |
| DATE |Datum en tijd |
| FLOAT |Decimaal, tekenreeks (als precision > 28) |
| GEHEEL GETAL |Decimaal, tekenreeks (als precision > 28) |
| LANG |Tekenreeks |
| LANGE ONBEWERKTE |Byte] |
| NCHAR |Tekenreeks |
| NCLOB |Tekenreeks |
| AANTAL |Decimaal, tekenreeks (als precision > 28) |
| NVARCHAR2 |Tekenreeks |
| ONBEWERKTE |Byte] |
| ROWID |Tekenreeks |
| TIJDSTEMPEL |Datum en tijd |
| TIJDSTEMPEL MET DE LOKALE TIJDZONE |Tekenreeks |
| TIJDSTEMPEL MET TIJDZONE |Tekenreeks |
| NIET-ONDERTEKEND GEHEEL GETAL |Aantal |
| VARCHAR2 |Tekenreeks |
| XML |Tekenreeks |

> [!NOTE]
> Gegevenstype INTERVAL jaar, maand en INTERVAL dag aan worden tweede niet ondersteund.


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).