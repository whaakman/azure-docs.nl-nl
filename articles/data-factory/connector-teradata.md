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
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: e9fd818990c8a985a77c2e7eeea19bf63c440e4e
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54018990"
---
# <a name="copy-data-from-teradata-using-azure-data-factory"></a>Gegevens kopiëren van Teradata met Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-onprem-teradata-connector.md)
> * [Huidige versie](connector-teradata.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een Teradata-database. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Teradata-database kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies ondersteunt deze Teradata-connector:

- Teradata **versie 12 en hoger**.
- Kopiëren van gegevens met **Basic** of **Windows** verificatie.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van deze Teradata-connector, moet u naar:

- Instellen van een zelfgehoste Cloudintegratieruntime. Zie [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.
- Installeer de [.NET Data Provider voor Teradata](https://go.microsoft.com/fwlink/?LinkId=278886) versie 14 of hoger op de machine Integration Runtime.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met Teradata-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Teradata gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Teradata** | Ja |
| server | Naam van de Teradata-server. | Ja |
| authenticationType | Het type verificatie gebruikt voor verbinding met de Teradata-database.<br/>Toegestane waarden zijn: **Basic**, en **Windows**. | Ja |
| gebruikersnaam | Geef de naam van de gebruiker verbinding maken met de Teradata-database. | Ja |
| wachtwoord | Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. Er is een zelfgehoste Cloudintegratieruntime vereist zoals vermeld in [vereisten](#prerequisites). |Ja |

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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund voor Teradata-gegevensset.

Om gegevens te kopiëren uit Teradata, stel de eigenschap type van de gegevensset in **RelationalTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Teradata-bron.

### <a name="teradata-as-source"></a>Teradata als bron

Om gegevens te kopiëren uit Teradata, stelt u het brontype in de kopieeractiviteit naar **RelationalSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **RelationalSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM MyTable"`. | Nee (als de 'tableName' in de gegevensset is opgegeven) |

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

## <a name="data-type-mapping-for-teradata"></a>Toewijzing voor Teradata-gegevenstype

Het kopiëren van gegevens uit Teradata, worden de volgende toewijzingen uit Teradata-gegevenstypen gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe copy activity in het schema en de gegevens van een brontype aan de sink toegewezen.

| Teradata-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| BigInt |Int64 |
| Blob |Byte[] |
| Byte |Byte[] |
| ByteInt |Int16 |
| CHAR |Reeks |
| CLOB |Reeks |
| Date |DateTime |
| Decimaal |Decimaal |
| Double-waarde |Double-waarde |
| Afbeelding |Reeks |
| Geheel getal |Int32 |
| Interval dag |TimeSpan |
| Interval voor dag uur |TimeSpan |
| Interval voor dag minuut |TimeSpan |
| Tweede interval dag |TimeSpan |
| Interval uur |TimeSpan |
| Interval voor uur minuut |TimeSpan |
| Tweede interval uur |TimeSpan |
| Interval minuut |TimeSpan |
| Tweede interval minuut |TimeSpan |
| Interval van maand |Reeks |
| Interval voor tweede |TimeSpan |
| Interval van jaar |Reeks |
| Interval voor jaar tot maand |Reeks |
| Aantal |Double-waarde |
| Period(date) |Reeks |
| Period(Time) |Reeks |
| Periode (tijd met tijdzone) |Reeks |
| Period(timestamp) |Reeks |
| Periode (Timestamp met tijdzone) |Reeks |
| SmallInt |Int16 |
| Time |TimeSpan |
| Tijd met de tijdzone |Reeks |
| Tijdstempel |DateTime |
| Tijdstempel met tijdzone |DateTimeOffset |
| VarByte |Byte[] |
| VarChar |Reeks |
| VarGraphic |Reeks |
| Xml |Reeks |


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
