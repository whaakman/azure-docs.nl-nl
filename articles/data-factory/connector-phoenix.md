---
title: Gegevens kopiëren van Phoenix met Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van Phoenix naar ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: jingwang
ms.openlocfilehash: 012057c7d01924ab1998a010b6ea0c7d83651a4d
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54017800"
---
# <a name="copy-data-from-phoenix-using-azure-data-factory"></a>Gegevens kopiëren van Phoenix met Azure Data Factory 

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van Phoenix. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Phoenix kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Azure Data Factory biedt een ingebouwde stuurprogramma als connectiviteit wilt inschakelen, dus hoeft u stuurprogramma voor gebruik van deze connector handmatig installeren.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor Phoenix-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Phoenix gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Phoenix** | Ja |
| host | Het IP-adres of de hostnaam naam van de Phoenix-server. (dat wil zeggen, 192.168.222.160)  | Ja |
| poort | De TCP-poort die de Phoenix-server wordt gebruikt om te luisteren naar clientverbindingen. De standaardwaarde is 8765. Als u verbinding met Azure HDInsights maken, geeft u poort 443. | Nee |
| httpPath | De gedeeltelijke URL overeenkomt met de Phoenix-server. (dat wil zeggen, /gateway/sandbox/phoenix/version). Geef `/hbasephoenix0` als HDInsights cluster.  | Nee |
| authenticationType | Het verificatiemechanisme waarmee verbinding wordt gemaakt met de Phoenix-server. <br/>Toegestane waarden zijn: **Anonieme**, **UsernameAndPassword**, **WindowsAzureHDInsightService** | Ja |
| gebruikersnaam | De gebruikersnaam die wordt gebruikt voor verbinding met de Phoenix-server.  | Nee |
| wachtwoord | Het wachtwoord dat overeenkomt met de naam van de gebruiker. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Nee |
| enableSsl | Hiermee geeft u op of de verbindingen met de server zijn versleuteld met behulp van SSL. De standaardwaarde is false.  | Nee |
| trustedCertPath | Het volledige pad van het .pem-bestand met de vertrouwde CA-certificaten voor het controleren van de server wanneer u verbinding maakt via SSL. Deze eigenschap kan alleen worden ingesteld wanneer u SSL op zelf-hostende IR De standaardwaarde is het bestand cacerts.pem is geïnstalleerd met de IR.  | Nee |
| useSystemTrustStore | Hiermee bepaalt u of u een CA-certificaat uit het archief van de vertrouwensrelatie systeem- of uit een opgegeven PEM-bestand. De standaardwaarde is false.  | Nee |
| allowHostNameCNMismatch | Hiermee geeft u op of de naam van een Certificeringsinstantie uitgegeven SSL-certificaat zodat deze overeenkomen met de hostnaam van de server wanneer u verbinding maakt via SSL vereist. De standaardwaarde is false.  | Nee |
| allowSelfSignedServerCert | Hiermee geeft u op of zelfondertekende certificaten van de server is toegestaan. De standaardwaarde is false.  | Nee |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de zelfgehoste Cloudintegratieruntime of Azure Integration Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk zijn is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

>[!NOTE]
>Als uw cluster biedt geen ondersteuning voor sticky sessie zoals HDInsight, expliciet knooppuntindex toevoegen aan het einde van de instelling van de HTTP-pad, bijvoorbeeld opgeven `/hbasephoenix0` in plaats van `/hbasephoenix`.

**Voorbeeld:**

```json
{
    "name": "PhoenixLinkedService",
    "properties": {
        "type": "Phoenix",
        "typeProperties": {
            "host" : "<cluster>.azurehdinsight.net",
            "port" : "443",
            "httpPath" : "/hbasephoenix0",
            "authenticationType" : "WindowsAzureHDInsightService",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund voor Phoenix-gegevensset.

Om gegevens te kopiëren van Phoenix, stel de eigenschap type van de gegevensset in **PhoenixObject**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **PhoenixObject** | Ja |
| tableName | Naam van de tabel. | Nee (als 'query' in de activiteitbron is opgegeven) |

**Voorbeeld**

```json
{
    "name": "PhoenixDataset",
    "properties": {
        "type": "PhoenixObject",
        "linkedServiceName": {
            "referenceName": "<Phoenix linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Phoenix-bron.

### <a name="phoenix-as-source"></a>Phoenix als bron

Om gegevens te kopiëren van Phoenix, stelt u het brontype in de kopieeractiviteit naar **PhoenixSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **PhoenixSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM MyTable"`. | Nee (als de 'tableName' in de gegevensset is opgegeven) |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromPhoenix",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Phoenix input dataset name>",
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
                "type": "PhoenixSource",
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
