---
title: "Gegevens kopiëren van Teradata met behulp van Azure Data Factory | Microsoft Docs"
description: "Meer informatie over de Teradata-Connector van de Data Factory-service waarmee u gegevens kopiëren vanuit de Teradata-database naar de gegevensarchieven die worden ondersteund door Data Factory als PUT."
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
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: 905a2bf1b42819a531bc4b16dd1e6f5539e80068
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-teradata-using-azure-data-factory"></a>Gegevens kopiëren van Teradata met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-onprem-teradata-connector.md)
> * [Versie 2 - Preview](connector-teradata.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een Teradata-database. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Teradata-connector in V1](v1/data-factory-onprem-teradata-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit de Teradata-database kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze Teradata-connector:

- Teradata **versie 12 en hoger**.
- Kopiëren van gegevens met **Basic** of **Windows** verificatie.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van deze Teradata-connector, moet u:

- Stel een Self-hosted integratie-Runtime. Zie [Self-hosted integratie Runtime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.
- Installeer de [.NET-gegevensprovider voor Teradata](http://go.microsoft.com/fwlink/?LinkId=278886) versie 14 of hoger op de machine integratie Runtime.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Teradata-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Teradata gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Teradata** | Ja |
| server | De naam van de Teradata-server. | Ja |
| authenticationType | Het soort verificatie die verbinding maken met de Teradata-database wordt gebruikt.<br/>Toegestane waarden zijn: **Basic**, en **Windows**. | Ja |
| gebruikersnaam | Geef de naam van de gebruiker verbinding maken met de Teradata-database. | Ja |
| wachtwoord | Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. Dit veld markeren als een SecureString. | Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. Een Runtime Self-hosted-integratie is vereist zoals vermeld in [vereisten](#prerequisites). |Ja |

**Voorbeeld:**

```json
{
    "name": "TeradataLinkedService",
    "properties": {
        "type": "Teradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "Basic",
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Teradata-gegevensset.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren uit een Teradata, **RelationalTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **RelationalTable** | Ja |
| tableName | De naam van de tabel in de Teradata-database. | Nee (als 'query' in de activiteitbron is opgegeven) |

**Voorbeeld:**

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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Teradata-bron.

### <a name="teradata-as-source"></a>Teradata als bron

Om gegevens te kopiëren uit een Teradata, stelt u het brontype in de kopieerbewerking naar **RelationalSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **RelationalSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM MyTable"`. | Nee (als 'tableName' in de gegevensset is opgegeven) |

**Voorbeeld:**

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

## <a name="data-type-mapping-for-teradata"></a>Gegevenstype toewijzing voor Teradata

Bij het kopiëren van gegevens uit een Teradata, worden de volgende toewijzingen van gegevenstypen Teradata gebruikt om tussentijdse Azure Data Factory-gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

| Gegevenstype voor Teradata | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| BigInt |Int64 |
| Blob |Byte[] |
| Byte |Byte[] |
| ByteInt |Int16 |
| CHAR |Tekenreeks |
| CLOB |Tekenreeks |
| Date |Datum en tijd |
| Decimale |Decimale |
| Double |Double |
| Afbeelding |Tekenreeks |
| Geheel getal |Int32 |
| Interval dag |TimeSpan |
| Interval dag uur |TimeSpan |
| Interval dag minuut |TimeSpan |
| Tweede interval dag |TimeSpan |
| Interval uur |TimeSpan |
| Interval uur, minuut |TimeSpan |
| Tweede interval uur |TimeSpan |
| Interval minuut |TimeSpan |
| Minuut tweede interval |TimeSpan |
| Interval maand |Tekenreeks |
| Interval tweede |TimeSpan |
| Interval jaar |Tekenreeks |
| Interval jaar, maand |Tekenreeks |
| Aantal |Double |
| Period(Date) |Tekenreeks |
| Period(Time) |Tekenreeks |
| Periode (tijd met tijdzone) |Tekenreeks |
| Period(Timestamp) |Tekenreeks |
| Periode (tijdstempel met tijdzone) |Tekenreeks |
| SmallInt |Int16 |
| Time |TimeSpan |
| Tijd met de tijdzone |Tekenreeks |
| Timestamp |Datum en tijd |
| Tijdstempel met tijdzone |DateTimeOffset |
| VarByte |Byte[] |
| VarChar |Tekenreeks |
| VarGraphic |Tekenreeks |
| Xml |Tekenreeks |


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).