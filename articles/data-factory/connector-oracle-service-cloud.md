---
title: Gegevens kopiëren van Oracle-Service-Cloud met Azure Data Factory (Preview) | Microsoft Docs
description: Leer hoe u gegevens kopiëren van Oracle-Servicecloud naar ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
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
ms.date: 09/07/2018
ms.author: jingwang
ms.openlocfilehash: a576b94881e114a97e58bf93515e372221da3346
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44096220"
---
# <a name="copy-data-from-oracle-service-cloud-using-azure-data-factory-preview"></a>Gegevens kopiëren van Oracle-Service-Cloud met Azure Data Factory (Preview)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit de Cloud voor Oracle-Service. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!IMPORTANT]
> Deze connector is momenteel in preview. U kunt uitproberen en feedback geven. Neem contact op met de [ondersteuning van Azure](https://azure.microsoft.com/support/) als u een afhankelijkheid van preview-connectors wilt opnemen in uw oplossing.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Oracle-Servicecloud kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Azure Data Factory biedt een ingebouwde stuurprogramma als connectiviteit wilt inschakelen, dus hoeft u stuurprogramma voor gebruik van deze connector handmatig installeren.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met Oracle-Service-Cloud-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Oracle-Servicecloud gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **OracleServiceCloud** | Ja |
| host | De URL van de Cloud voor Oracle-Service-exemplaar.  | Ja |
| gebruikersnaam | De gebruikersnaam die u gebruikt voor toegang tot Oracle-Service-Cloud-server.  | Ja |
| wachtwoord | Het wachtwoord dat overeenkomt met de naam van de gebruiker die u hebt opgegeven in de gebruikersnaam-sleutel. U kunt dit veld markeren als een SecureString veilig opslaan in ADF of wachtwoord opslaan in Azure Key Vault en laat ADF acitivty pull van daaruit kopiëren, bij het uitvoeren van het kopiëren van gegevens: meer informatie uit [referenties Store in Key Vault](store-credentials-in-key-vault.md). | Ja |
| useEncryptedEndpoints | Hiermee geeft u op of de eindpunten van de gegevensbron zijn versleuteld met behulp van HTTPS. De standaardwaarde is true.  | Nee |
| useHostVerification | Hiermee geeft u op of de hostnaam van de in het certificaat van de server zodat deze overeenkomen met de hostnaam van de server wanneer u verbinding maakt via SSL vereist. De standaardwaarde is true.  | Nee |
| usePeerVerification | Hiermee geeft u op of u wilt controleren of de identiteit van de server wanneer u verbinding maakt via SSL. De standaardwaarde is true.  | Nee |

**Voorbeeld:**

```json
{
    "name": "OracleServiceCloudLinkedService",
    "properties": {
        "type": "OracleServiceCloud",
        "typeProperties": {
            "host" : "<host>",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            },
            "useEncryptedEndpoints" : true,
            "useHostVerification" : true,
            "usePeerVerification" : true,
        }
    }
}

```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Oracle-Servicecloud gegevensset.

Om gegevens te kopiëren van Oracle-Servicecloud, stel de eigenschap type van de gegevensset in **OracleServiceCloudObject**. Er is geen aanvullende typespecifieke-eigenschap in dit type gegevensset.

**Voorbeeld**

```json
{
    "name": "OracleServiceCloudDataset",
    "properties": {
        "type": "OracleServiceCloudObject",
        "linkedServiceName": {
            "referenceName": "<OracleServiceCloud linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}

```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Oracle-Servicecloud bron.

### <a name="oracle-service-cloud-as-source"></a>Oracle-Servicecloud als bron

Om gegevens te kopiëren van Oracle-Servicecloud, stelt u het brontype in de kopieeractiviteit naar **OracleServiceCloudSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **OracleServiceCloudSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM MyTable"`. | Ja |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromOracleServiceCloud",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<OracleServiceCloud input dataset name>",
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
                "type": "OracleServiceCloudSource",
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
