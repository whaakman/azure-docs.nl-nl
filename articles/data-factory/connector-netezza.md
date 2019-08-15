---
title: Gegevens kopiëren van Netezza met behulp van Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van Netezza naar ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
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
ms.openlocfilehash: c3c179cfbf86c2dddfb34b46540aba8898038751
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68966491"
---
# <a name="copy-data-from-netezza-by-using-azure-data-factory"></a>Gegevens kopiëren van Netezza met behulp van Azure Data Factory

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van Netezza. Het artikel is gebaseerd op [Kopieeractiviteit in Azure Data Factory](copy-activity-overview.md), die een algemeen overzicht van Kopieeractiviteit geeft.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Netezza kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevens opslaat of Kopieeractiviteit als bronnen en sinks ondersteunt, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory biedt een ingebouwde stuurprogramma als connectiviteit wilt inschakelen. U hoeft niet te installeren op een stuurprogramma voor het gebruik van deze connector.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>Aan de slag

U kunt een pijplijn met behulp van een kopieeractiviteit met behulp van de .NET SDK, de Python-SDK, Azure PowerShell, de REST-API of een Azure Resource Manager-sjabloon maken. Zie de [zelfstudie Kopieeractiviteit](quickstart-create-data-factory-dot-net.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit.

De volgende secties bevatten meer informatie over eigenschappen die u gebruiken kunt voor het definiëren van Data Factory-entiteiten die specifiek voor de Netezza-connector zijn.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de service Netezza gekoppeld:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap moet worden ingesteld op **Netezza**. | Ja |
| connectionString | Een ODBC-verbindingsreeks verbinding maken met Netezza. <br/>Markeer dit veld als een SecureString om het veilig op te slaan in Data Factory. U kunt ook wacht woord in azure Key Vault plaatsen en de `pwd` configuratie uit de Connection String halen. Raadpleeg de volgende voor beelden en [Sla referenties op in azure Key Vault](store-credentials-in-key-vault.md) artikel met meer informatie. | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) gebruiken om te verbinden met het gegevensarchief. Meer informatie vindt u in de sectie [vereisten](#prerequisites) . Indien niet opgegeven, wordt de standaard Azure Integration Runtime wordt gebruikt. |Nee |

Een gebruikelijke verbindingsreeks is `Server=<server>;Port=<port>;Database=<database>;UID=<user name>;PWD=<password>`. De volgende tabel bevat meer eigenschappen die u kunt instellen:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| SecurityLevel | Het niveau van beveiliging (SSL/TLS) die gebruikmaakt van het stuurprogramma voor de verbinding met het gegevensarchief. Voorbeeld: `SecurityLevel=preferredSecured`. Ondersteunde waarden zijn:<br/>- **Alleen niet beveiligd** (**onlyUnSecured**): Het stuur programma maakt geen gebruik van SSL.<br/>- **Voor keur niet-beveiligd (preferredUnSecured) (standaard)** : Als de server een keuze biedt, gebruikt het stuur programma geen SSL. <br/>- **Voorkeurs beveiliging (preferredSecured)** : Als de server een keuze biedt, maakt het stuur programma gebruik van SSL. <br/>- **Alleen beveiligd (onlySecured)** : Het stuur programma maakt geen verbinding tenzij er een SSL-verbinding beschikbaar is. | Nee |
| CaCertFile | Het volledige pad naar het SSL-certificaat dat wordt gebruikt door de server. Voorbeeld: `CaCertFile=<cert path>;`| Ja, als SSL is ingeschakeld |

**Voorbeeld**

```json
{
    "name": "NetezzaLinkedService",
    "properties": {
        "type": "Netezza",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=<server>;Port=<port>;Database=<database>;UID=<user name>;PWD=<password>"
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
    "name": "NetezzaLinkedService",
    "properties": {
        "type": "Netezza",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server=<server>;Port=<port>;Database=<database>;UID=<user name>;"
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

Deze sectie bevat een lijst met eigenschappen die ondersteuning biedt voor de gegevensset Netezza.

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets en secties, [gegevenssets](concepts-datasets-linked-services.md).

Om gegevens te kopiëren van Netezza, stel de **type** eigenschap van de gegevensset in **NetezzaTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **NetezzaTable** | Ja |
| tableName | Naam van de tabel. | Nee (als 'query' in de activiteitbron is opgegeven) |

**Voorbeeld**

```json
{
    "name": "NetezzaDataset",
    "properties": {
        "type": "NetezzaTable",
        "linkedServiceName": {
            "referenceName": "<Netezza linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Deze sectie bevat een lijst met eigenschappen die ondersteuning biedt voor de bron Netezza.

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van activiteiten en secties, [pijplijnen](concepts-pipelines-activities.md).

### <a name="netezza-as-source"></a>Netezza als bron

Om gegevens te kopiëren van Netezza, stel de **bron** type in de Kopieeractiviteit naar **NetezzaSource**. De volgende eigenschappen worden ondersteund in de Kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De **type** eigenschap van de Kopieeractiviteit-bron moet worden ingesteld op **NetezzaSource**. | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Voorbeeld: `"SELECT * FROM MyTable"` | Nee (als de 'tableName' in de gegevensset is opgegeven) |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromNetezza",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Netezza input dataset name>",
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
                "type": "NetezzaSource",
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

Zie voor een lijst met gegevensarchieven die Kopieeractiviteit ondersteunt als bronnen en sinks in Azure Data Factory, [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats).
