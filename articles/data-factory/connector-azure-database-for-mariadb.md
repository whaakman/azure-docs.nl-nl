---
title: Gegevens kopiëren van Azure Database for MariaDB met behulp van Azure Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens van Azure Database for MariaDB naar ondersteunde Sink-gegevens archieven door gebruik te maken van een Kopieer activiteit in een Azure Data Factory-pijp lijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/12/2019
ms.author: jingwang
ms.openlocfilehash: cb783630b32b4cc28d4e4f1cfb33027da3b8d2e0
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68966568"
---
# <a name="copy-data-from-azure-database-for-mariadb-using-azure-data-factory"></a>Gegevens kopiëren van Azure Database for MariaDB met behulp van Azure Data Factory 

In dit artikel wordt beschreven hoe u de Kopieer activiteit in Azure Data Factory kunt gebruiken om gegevens uit Azure Database for MariaDB te kopiëren. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van Azure Database for MariaDB kopiëren naar elk ondersteund Sink-gegevens archief. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Azure Data Factory biedt een ingebouwde stuurprogramma als connectiviteit wilt inschakelen, dus hoeft u stuurprogramma voor gebruik van deze connector handmatig installeren.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory entiteiten die specifiek zijn voor Azure Database for MariaDB-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Azure Database for MariaDB gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **MariaDB** | Ja |
| connectionString | Een connection string om verbinding te maken met Azure Database for MariaDB. U kunt dit vinden in de Azure Portal-> uw Azure Database for MariaDB-> verbindings reeksen-> ADO.NET één. <br/>Markeer dit veld als een SecureString om het veilig op te slaan in Data Factory. U kunt ook wacht woord in azure Key Vault plaatsen en de `pwd` configuratie uit de Connection String halen. Raadpleeg de volgende voor beelden en [Sla referenties op in azure Key Vault](store-credentials-in-key-vault.md) artikel met meer informatie. | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

**Voorbeeld:**

```json
{
    "name": "AzureDatabaseForMariaDBLinkedService",
    "properties": {
        "type": "MariaDB",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server={your_server}.mariadb.database.azure.com; Port=3306; Database={your_database}; Uid={your_user}@{your_server}; Pwd={your_password}; SslMode=Preferred;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Voor beeld: wacht woord opslaan in Azure Key Vault**

```json
{
    "name": "AzureDatabaseForMariaDBLinkedService",
    "properties": {
        "type": "MariaDB",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server={your_server}.mariadb.database.azure.com; Port=3306; Database={your_database}; Uid={your_user}@{your_server}; SslMode=Preferred;"
            },
            "pwd": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die door Azure Database for MariaDB DataSet worden ondersteund.

Als u gegevens wilt kopiëren uit Azure Database for MariaDB, stelt u de eigenschap type van de gegevensset in op **MariaDBTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **MariaDBTable** | Ja |
| tableName | Naam van de tabel. | Nee (als 'query' in de activiteitbron is opgegeven) |

**Voorbeeld**

```json
{
    "name": "AzureDatabaseForMariaDBDataset",
    "properties": {
        "type": "MariaDBTable",
        "linkedServiceName": {
            "referenceName": "<Azure Database for MariaDB linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die door Azure Database for MariaDB bron worden ondersteund.

### <a name="azure-database-for-mariadb-as-source"></a>Azure Database for MariaDB als bron

Als u gegevens wilt kopiëren uit Azure Database for MariaDB, stelt u het bron type in de Kopieer activiteit in op **MariaDBSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **source** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron van de Kopieer activiteit moet worden ingesteld op: **MariaDBSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM MyTable"`. | Nee (als de 'tableName' in de gegevensset is opgegeven) |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromAzureDatabaseForMariaDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure Database for MariaDB input dataset name>",
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
                "type": "MariaDBSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
