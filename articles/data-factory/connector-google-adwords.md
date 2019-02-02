---
title: Gegevens kopiëren van Google AdWords met Azure Data Factory (Preview) | Microsoft Docs
description: Leer hoe u gegevens kopiëren van Google AdWords naar ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
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
ms.openlocfilehash: 0f68627e2db3c08049f0273045906057526bd6aa
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55568049"
---
# <a name="copy-data-from-google-adwords-using-azure-data-factory-preview"></a>Gegevens kopiëren van Google AdWords met Azure Data Factory (Preview)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren vanuit Google AdWords. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!IMPORTANT]
> Deze connector is momenteel in preview. U kunt uitproberen en feedback geven. Neem contact op met de [ondersteuning van Azure](https://azure.microsoft.com/support/) als u een afhankelijkheid van preview-connectors wilt opnemen in uw oplossing.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Google AdWords kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Azure Data Factory biedt een ingebouwde stuurprogramma als connectiviteit wilt inschakelen, dus hoeft u stuurprogramma voor gebruik van deze connector handmatig installeren.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Google AdWords-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Google AdWords service gekoppelde:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **GoogleAdWords** | Ja |
| clientCustomerID | De Client-klant-ID van het AdWords-account dat u wilt ophalen van rapportgegevens voor.  | Ja |
| developerToken | De developer-token dat is gekoppeld aan de manageraccount dat u gebruikt om toegang te verlenen aan de AdWords-API.  U kunt dit veld markeren als een SecureString veilig opslaan in ADF of wachtwoord opslaan in Azure Key Vault en laat ADF activiteit pull van daaruit kopiëren, bij het uitvoeren van het kopiëren van gegevens: meer informatie uit [referenties Store in Key Vault](store-credentials-in-key-vault.md). | Ja |
| authenticationType | Het OAuth 2.0-verificatiemechanisme voor verificatie gebruikt. ServiceAuthentication kan alleen worden gebruikt voor de zelf-hostende IR <br/>Toegestane waarden zijn: **ServiceAuthentication**, **UserAuthentication** | Ja |
| refreshToken | Het vernieuwingstoken dat is verkregen van Google voor het verlenen van toegang tot AdWords voor UserAuthentication. U kunt dit veld markeren als een SecureString veilig opslaan in ADF of wachtwoord opslaan in Azure Key Vault en laat ADF activiteit pull van daaruit kopiëren, bij het uitvoeren van het kopiëren van gegevens: meer informatie uit [referenties Store in Key Vault](store-credentials-in-key-vault.md). | Nee |
| ClientId | De client-id van het google-toepassing die wordt gebruikt om de vernieuwingstoken te verkrijgen. U kunt dit veld markeren als een SecureString veilig opslaan in ADF of wachtwoord opslaan in Azure Key Vault en laat ADF activiteit pull van daaruit kopiëren, bij het uitvoeren van het kopiëren van gegevens: meer informatie uit [referenties Store in Key Vault](store-credentials-in-key-vault.md). | Nee |
| ClientSecret | Het clientgeheim van de google-toepassing die wordt gebruikt om de vernieuwingstoken te verkrijgen. U kunt dit veld markeren als een SecureString veilig opslaan in ADF of wachtwoord opslaan in Azure Key Vault en laat ADF activiteit pull van daaruit kopiëren, bij het uitvoeren van het kopiëren van gegevens: meer informatie uit [referenties Store in Key Vault](store-credentials-in-key-vault.md). | Nee |
| e-mail | De service-account e-mail-ID die wordt gebruikt voor ServiceAuthentication en kan alleen worden gebruikt voor de zelf-hostende IR  | Nee |
| keyFilePath | Het volledige pad naar de sleutel .p12-bestand dat wordt gebruikt voor verificatie van het e-mailadres van de service-account en kan alleen worden gebruikt voor de zelf-hostende IR  | Nee |
| trustedCertPath | Het volledige pad van het .pem-bestand met de vertrouwde CA-certificaten voor het controleren van de server wanneer u verbinding maakt via SSL. Deze eigenschap kan alleen worden ingesteld wanneer u SSL op zelf-hostende IR De standaardwaarde is het bestand cacerts.pem is geïnstalleerd met de IR.  | Nee |
| useSystemTrustStore | Hiermee bepaalt u of u een CA-certificaat uit het archief van de vertrouwensrelatie systeem- of uit een opgegeven PEM-bestand. De standaardwaarde is false.  | Nee |

**Voorbeeld:**

```json
{
    "name": "GoogleAdWordsLinkedService",
    "properties": {
        "type": "GoogleAdWords",
        "typeProperties": {
            "clientCustomerID" : "<clientCustomerID>",
            "developerToken": {
                "type": "SecureString",
                "value": "<developerToken>"
            },
            "authenticationType" : "ServiceAuthentication",
            "refreshToken": {
                "type": "SecureString",
                "value": "<refreshToken>"
            },
            "clientId": {
                "type": "SecureString",
                "value": "<clientId>"
            },
            "clientSecret": {
                "type": "SecureString",
                "value": "<clientSecret>"
            },
            "email" : "<email>",
            "keyFilePath" : "<keyFilePath>",
            "trustedCertPath" : "<trustedCertPath>",
            "useSystemTrustStore" : true,
        }
    }
}

```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Google AdWords gegevensset.

Als u wilt kopiëren van gegevens uit Google AdWords, stel de eigenschap type van de gegevensset in **GoogleAdWordsObject**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **GoogleAdWordsObject** | Ja |
| tableName | Naam van de tabel. | Nee (als 'query' in de activiteitbron is opgegeven) |

**Voorbeeld**

```json
{
    "name": "GoogleAdWordsDataset",
    "properties": {
        "type": "GoogleAdWordsObject",
        "linkedServiceName": {
            "referenceName": "<GoogleAdWords linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}

```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door Google AdWords bron.

### <a name="google-adwords-as-source"></a>Google AdWords als bron

Om gegevens te kopiëren vanuit Google AdWords, stelt u het brontype in de kopieeractiviteit naar **GoogleAdWordsSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **GoogleAdWordsSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM MyTable"`. | Nee (als de 'tableName' in de gegevensset is opgegeven) |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromGoogleAdWords",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<GoogleAdWords input dataset name>",
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
                "type": "GoogleAdWordsSource",
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
