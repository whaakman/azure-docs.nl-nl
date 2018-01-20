---
title: "Gegevens kopiëren van Spark met behulp van Azure Data Factory | Microsoft Docs"
description: "Ontdek hoe u gegevens kopiëren van Spark naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn."
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
ms.date: 11/30/2017
ms.author: jingwang
ms.openlocfilehash: 0107c0e02e7158ad73671ae8e4599e1e998f20fc
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="copy-data-from-spark-using-azure-data-factory"></a>Gegevens kopiëren van Spark met behulp van Azure Data Factory 

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van Spark. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Kopieeractiviteit in V1](v1/data-factory-data-movement-activities.md).


## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Spark kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Azure Data Factory biedt een ingebouwde stuurprogramma's zodat connectiviteit, dus u hoeft niet te gebruik van deze connector stuurprogramma handmatig installeren.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met Spark-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Spark gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Spark** | Ja |
| host | IP-adres of de host-naam van de Spark-server  | Ja |
| poort | De TCP-poort die de Spark-server gebruikt om te luisteren naar verbindingen van clients.  | Ja |
| serverType | Het type van Spark-server. <br/>Toegestane waarden zijn: **SharkServer**, **SharkServer2**, **SparkThriftServer** | Nee |
| thriftTransportProtocol | Het te gebruiken in de laag Thrift-transportprotocol. <br/>Toegestane waarden zijn: **binaire**, **SASL**, ** HTTP ** | Nee |
| authenticationType | De verificatiemethode die wordt gebruikt voor toegang tot de Spark-server. <br/>Toegestane waarden zijn: **anoniem**, **gebruikersnaam**, **UsernameAndPassword**, **WindowsAzureHDInsightService** | Ja |
| gebruikersnaam | De gebruikersnaam die u gebruikt voor toegang tot Spark-Server.  | Nee |
| wachtwoord | Het wachtwoord dat overeenkomt met de naam van de gebruiker die u hebt opgegeven in het veld Username kunt u dit veld als een SecureString te bewaren in ADF, veilig of wachtwoord in Azure Key Vault opslaan en kunt de kopie activiteit pull daar bij het uitvoeren van de gegevens opnieuw te kopiëren - markeert l meer uit RN [referenties opgeslagen in de Sleutelkluis](store-credentials-in-key-vault.md). | Nee |
| httpPath | De gedeeltelijke URL overeenkomt met de Spark-server.  | Nee |
| enableSsl | Geeft aan of de verbindingen met de server zijn versleuteld met SSL. De standaardwaarde is ingesteld op false.  | Nee |
| trustedCertPath | Het volledige pad van het .pem-bestand met de vertrouwde CA-certificaten voor het controleren van de server om verbinding te maken via SSL. Deze eigenschap kan alleen worden ingesteld wanneer u SSL op host zichzelf IR De standaardwaarde is het bestand cacerts.pem is geïnstalleerd met de IR  | Nee |
| useSystemTrustStore | Hiermee geeft u op of u een CA-certificaat uit het archief van de vertrouwensrelatie system of vanuit een opgegeven PEM-bestand. De standaardwaarde is ingesteld op false.  | Nee |
| allowHostNameCNMismatch | Geeft aan of moet de naam van een Certificeringsinstantie uitgegeven SSL-certificaat overeenkomen met de hostnaam van de server om verbinding te maken via SSL. De standaardwaarde is ingesteld op false.  | Nee |
| allowSelfSignedServerCert | Hiermee bepaalt u of zelfondertekende certificaten van de server. De standaardwaarde is ingesteld op false.  | Nee |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Self-hosted integratie Runtime of Azure integratie Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

**Voorbeeld:**

```json
{
    "name": "SparkLinkedService",
    "properties": {
        "type": "Spark",
        "typeProperties": {
            "host" : "<cluster>.azurehdinsight.net",
            "port" : "<port>",
            "authenticationType" : "WindowsAzureHDInsightService",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            },
            "httpPath" : "gateway/sandbox/spark"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Spark dataset.

Om gegevens te kopiëren van Spark, stel de eigenschap type van de gegevensset **SparkObject**. Er is geen aanvullende typespecifieke-eigenschap in dit type dataset.

**Voorbeeld**

```json
{
    "name": "SparkDataset",
    "properties": {
        "type": "SparkObject",
        "linkedServiceName": {
            "referenceName": "<Spark linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de activiteit kopiëren

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Spark-bron.

### <a name="sparksource-as-source"></a>SparkSource als bron

Om gegevens te kopiëren van Spark, stelt u het brontype in de kopieerbewerking naar **SparkSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **SparkSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM MyTable"`. | Ja |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromSpark",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Spark input dataset name>",
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
                "type": "SparkSource",
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
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
