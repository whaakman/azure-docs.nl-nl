---
title: Gegevens uit Azure-Database kopiëren voor gebruik van Azure Data Factory PostgreSQL | Microsoft Docs
description: Ontdek hoe u gegevens kopiëren van Azure-Database voor PostgreSQL naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/28/2018
ms.author: jingwang
ms.openlocfilehash: 365775f840f85efe1792f376880145c7e7db1312
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="copy-data-from-azure-database-for-postgresql-using-azure-data-factory"></a>Gegevens kopiëren van Azure-Database voor PostgreSQL met behulp van Azure Data Factory 

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit Azure-Database voor PostgreSQL. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Kopieeractiviteit in V1](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Azure-Database voor PostgreSQL kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Azure Data Factory biedt een ingebouwde stuurprogramma's zodat connectiviteit, dus u hoeft niet te gebruik van deze connector stuurprogramma handmatig installeren.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met Azure-Database voor PostgreSQL-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Azure-Database voor PostgreSQL gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AzurePostgreSql** | Ja |
| connectionString | Een ODBC-verbindingsreeks verbinding maken met Azure-Database voor PostgreSQL. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Azure integratie Runtime of Self-hosted integratie Runtime gebruiken (indien de gegevensopslag bevindt zich in een particulier netwerk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

Een typische verbindingsreeks is `Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>@admstest;Password=<Password>`. Meer eigenschappen die u per uw aanvraag instellen kunt:

| Eigenschap | Beschrijving | Opties | Vereist |
|:--- |:--- |:--- |:--- |:--- |
| EncryptionMethod (EM)| De methode het stuurprogramma gebruikt voor het versleutelen van gegevens die tussen het stuurprogramma en de database-server verzonden. Bijvoorbeeld `ValidateServerCertificate=<0/1/6>;`| 0 (geen versleuteling) **(standaard)** / 1 (SSL) / 6 (RequestSSL) | Nee |
| ValidateServerCertificate (VSC) | Hiermee wordt bepaald of het certificaat dat wordt verzonden door de database-server als SSL-versleuteling is ingeschakeld wordt gevalideerd door het stuurprogramma (versleutelingsmethode = 1). Bijvoorbeeld `ValidateServerCertificate=<0/1>;`| 0 (uitgeschakeld) **(standaard)** / 1 (ingeschakeld) | Nee |

**Voorbeeld:**

```json
{
    "name": "AzurePostgreSqlLinkedService",
    "properties": {
        "type": "AzurePostgreSql",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>@admstest;Password=<Password>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen voor PostgreSQL-gegevensset wordt ondersteund door Azure-Database.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren uit Azure-Database voor PostgreSQL, **AzurePostgreSqlTable**. Er is geen aanvullende typespecifieke-eigenschap in dit type dataset.

**Voorbeeld**

```json
{
    "name": "AzurePostgreSqlDataset",
    "properties": {
        "type": "AzurePostgreSqlTable",
        "linkedServiceName": {
            "referenceName": "<AzurePostgreSql linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen voor PostgreSQL bron wordt ondersteund door Azure-Database.

### <a name="azurepostgresqlsource-as-source"></a>AzurePostgreSqlSource als bron

Om gegevens te kopiëren uit Azure-Database voor PostgreSQL, stelt u het brontype in de kopieerbewerking naar **AzurePostgreSqlSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **AzurePostgreSqlSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM MyTable"`. | Ja |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromAzurePostgreSql",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<AzurePostgreSql input dataset name>",
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
                "type": "AzurePostgreSqlSource",
                "query": "<custom query e.g. SELECT * FROM MyTable>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
