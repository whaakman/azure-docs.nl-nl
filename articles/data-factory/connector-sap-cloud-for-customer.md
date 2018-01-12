---
title: "Gegevens kopiëren van/naar SAP Cloud voor klant op basis van Azure Data Factory | Microsoft Docs"
description: "Informatie over het om gegevens te kopiëren uit SAP Cloud voor klant om ondersteunde sink gegevensarchieven (of) uit gegevensarchieven ondersteunde bron naar de Cloud SAP voor klanten met behulp van de Data Factory."
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
ms.openlocfilehash: e580c3f36ce19679d3edcf7a8861e4e492dfa9c5
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="copy-data-from-sap-cloud-for-customer-c4c-using-azure-data-factory"></a>Kopiëren van gegevens uit SAP Cloud voor klant (C4C) met behulp van Azure Data Factory

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van/naar SAP Cloud voor klant (C4C). Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Kopieeractiviteit in V1](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit SAP Cloud voor klant kopiëren naar een ondersteunde sink data store of gegevens kopiëren van een ondersteunde brongegevensarchief naar SAP Cloud voor de klant. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Deze connector kunt in het bijzonder Azure Data Factory om gegevens te kopiëren van/naar SAP Cloud voor de klant, met inbegrip van de Cloud SAP voor verkoop, SAP Cloud voor Service en SAP Cloud voor sociale Engagement oplossingen.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van specifieke Data Factory-entiteiten naar SAP Cloud voor klant-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor SAP Cloud voor klant gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **SapCloudForCustomer**. | Ja |
| url | De URL van de SAP C4C OData-service. | Ja |
| gebruikersnaam | Geef de naam van de gebruiker verbinding maken met de SAP-C4C. | Ja |
| wachtwoord | Geef het wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam. Dit veld markeren als een SecureString. | Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. | Er is geen voor bron Ja voor sink |

>[!IMPORTANT]
>Om gegevens te kopiëren naar Cloud SAP voor klant expliciet [maken van een Azure-IR](create-azure-integration-runtime.md#create-azure-ir) met een locatie in de buurt van uw Cloud SAP voor klant en koppelen in de gekoppelde service als het volgende voorbeeld:

**Voorbeeld:**

```json
{
    "name": "SAPC4CLinkedService",
    "properties": {
        "type": "SapCloudForCustomer",
        "typeProperties": {
            "url": "https://<tenantname>.crm.ondemand.com/sap/c4c/odata/v1/c4codata/" ,
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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen voor de klant gegevensset door SAP Cloud wordt ondersteund.

Om gegevens te kopiëren uit SAP Cloud voor de klant, stel de eigenschap type van de gegevensset **SapCloudForCustomerResource**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **SapCloudForCustomerResource** |Ja |
| pad | Geef het pad van de SAP C4C OData-entiteit. |Ja |

**Voorbeeld:**

```json
{
    "name": "SAPC4CDataset",
    "properties": {
        "type": "SapCloudForCustomerResource",
        "typePoperties": {
            "path": "<path e.g. LeadCollection>"
        },
        "linkedServiceName": {
            "referenceName": "<SAP C4C linked service>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de activiteit kopiëren

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen voor de gegevensbron van de klant door SAP Cloud wordt ondersteund.

### <a name="sap-c4c-as-source"></a>SAP C4C als bron

Om gegevens te kopiëren uit SAP Cloud voor de klant, stelt u het brontype in de kopieerbewerking naar **SapCloudForCustomerSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **SapCloudForCustomerSource**  | Ja |
| query | Geef de aangepaste OData-query voor het lezen van gegevens. | Nee |

Voorbeeldquery gegevens ophalen voor een specifieke dag:`"query": "$filter=CreatedOn ge datetimeoffset'2017-07-31T10:02:06.4202620Z' and CreatedOn le datetimeoffset'2017-08-01T10:02:06.4202620Z'"`

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromSAPC4C",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP C4C input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SAPC4CSource",
                "query": "<custom query e.g. $top=10>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="sap-c4c-as-sink"></a>SAP C4C als sink

Om gegevens te kopiëren naar Cloud SAP voor de klant, kunt u het sink-type instellen in de kopieeractiviteit om te **SapCloudForCustomerSink**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **sink** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **SapCloudForCustomerSink**  | Ja |
| WriteBehavior | Het gedrag van het schrijven van de bewerking. Kan worden 'Insert', 'Update'. | Nee. Standaard 'Insert'. |
| writeBatchSize | De batchgrootte van schrijfbewerking. De batchgrootte voor optimale prestaties kan afwijken van de andere tabel of de server. | Nee. De standaardwaarde is 10. |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToSapC4c",
        "type": "Copy",
        "inputs": [{
            "type": "DatasetReference",
            "referenceName": "<dataset type>"
        }],
        "outputs": [{
            "type": "DatasetReference",
            "referenceName": "SapC4cDataset"
        }],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SapCloudForCustomerSink",
                "writeBehavior": "Insert",
                "writeBatchSize": 30
            },
            "parallelCopies": 10,
            "cloudDataMovementUnits": 4,
            "enableSkipIncompatibleRow": true,
            "redirectIncompatibleRowSettings": {
                "linkedServiceName": {
                    "referenceName": "ErrorLogBlobLinkedService",
                    "type": "LinkedServiceReference"
                },
                "path": "incompatiblerows"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-cloud-for-customer"></a>Gegevenstypetoewijzing voor SAP Cloud voor klant

Bij het kopiëren van gegevens uit SAP Cloud voor de klant, worden de volgende toewijzingen gebruikt vanuit de Cloud SAP voor gegevenstypen van de klant tot Azure Data Factory tussentijdse-gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

| SAP C4C OData-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| Edm.Binary | Byte] |
| Edm.Boolean | BOOL |
| Edm.Byte | Byte] |
| Edm.DateTime | Datum en tijd |
| Edm.Decimal | Decimale |
| Edm.Double | dubbele |
| Edm.Single | Single |
| Edm.Guid | GUID |
| Edm.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | Tekenreeks |
| Edm.Time | TimeSpan |
| Edm.DateTimeOffset | DateTimeOffset |


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
