---
title: "Gegevens kopiëren van Impala met behulp van Azure Data Factory (bèta) | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens van Impala naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een data factory-pijplijn."
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
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: e1f745fc70395f06d2eb3d98644d54c314a0ef26
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="copy-data-from-impala-by-using-azure-data-factory-beta"></a>Gegevens kopiëren van Impala met behulp van Azure Data Factory (bèta)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van Impala. Dit is gebaseerd op de [Kopieeractiviteit overzicht](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory, die in het algemeen beschikbaar is, Zie [Kopieeractiviteit in versie 1](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Deze connector is momenteel in de bètaversie. U kunt uitproberen en feedback geven. Gebruik deze niet in een productieomgeving.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Impala kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven die als bronnen of PUT worden ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

 Data Factory biedt een ingebouwde stuurprogramma's zodat de verbinding. Daarom hoeft u niet handmatig een stuurprogramma voor het gebruik van deze connector te installeren.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar de connector Impala.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Impala service gekoppelde.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **Impala**. | Ja |
| host | Het IP-adres of de hostnaam naam van de server Impala (dat wil zeggen, 192.168.222.160).  | Ja |
| poort | De TCP-poort die de Impala-server gebruikt om te luisteren naar verbindingen van clients. De standaardwaarde is 21050.  | Nee |
| authenticationType | Het verificatietype dat moet worden gebruikt. <br/>Toegestane waarden zijn **anoniem**, **SASLUsername**, en **UsernameAndPassword**. | Ja |
| gebruikersnaam | De gebruikersnaam die wordt gebruikt voor toegang tot de server Impala. De standaardwaarde is anonieme wanneer u SASLUsername gebruikt.  | Nee |
| wachtwoord | Het wachtwoord dat overeenkomt met de gebruikersnaam wanneer u UsernameAndPassword gebruikt. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory of [verwijzen naar een geheim dat is opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Nee |
| enableSsl | Geeft aan of de verbindingen met de server zijn versleuteld met behulp van SSL. De standaardwaarde is **false**.  | Nee |
| trustedCertPath | Het volledige pad van het .pem-bestand met vertrouwde CA-certificaten gebruikt om te controleren of de server wanneer u verbinding via SSL maakt. Deze eigenschap kan alleen worden ingesteld als u SSL op Self-hosted integratie-Runtime gebruiken. De standaardwaarde is het cacerts.pem-bestand met de integratie-runtime is geïnstalleerd.  | Nee |
| useSystemTrustStore | Hiermee geeft u op of u een CA-certificaat uit het archief van de vertrouwensrelatie system of vanuit een opgegeven PEM-bestand. De standaardwaarde is **false**.  | Nee |
| allowHostNameCNMismatch | Geeft aan of moet de naam van een Certificeringsinstantie uitgegeven SSL-certificaat overeenkomen met de hostnaam van de server wanneer u verbinding via SSL maakt. De standaardwaarde is **false**.  | Nee |
| allowSelfSignedServerCert | Hiermee bepaalt u of zelfondertekende certificaten van de server. De standaardwaarde is **false**.  | Nee |
| connectVia | De [integratie runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Self-hosted integratie Runtime of Azure integratie Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

**Voorbeeld:**

```json
{
    "name": "ImpalaLinkedService",
    "properties": {
        "type": "Impala",
        "typeProperties": {
            "host" : "<host>",
            "port" : "<port>",
            "authenticationType" : "UsernameAndPassword",
            "username" : "<username>",
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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de gegevensset Impala.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren van Impala, **ImpalaObject**. Er is geen aanvullende typespecifieke-eigenschap in dit type dataset.

**Voorbeeld**

```json
{
    "name": "ImpalaDataset",
    "properties": {
        "type": "ImpalaObject",
        "linkedServiceName": {
            "referenceName": "<Impala linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door het brontype Impala.

### <a name="impala-as-a-source-type"></a>Impala als een type gegevensbron

Om gegevens te kopiëren van Impala, stelt u het brontype in de kopieerbewerking naar **ImpalaSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie.

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op **ImpalaSource**. | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Een voorbeeld is `"SELECT * FROM MyTable"`. | Ja |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromImpala",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Impala input dataset name>",
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
                "type": "ImpalaSource",
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
Zie voor een lijst van gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Gegevensfactory [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
