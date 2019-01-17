---
title: Gegevens kopiëren van/naar SAP-Cloud voor klanten met Azure Data Factory | Microsoft Docs
description: Meer informatie over het kopiëren van gegevens van SAP-Cloud voor klant ondersteunde sink-gegevensopslag (of) van de ondersteunde bron-gegevensopslag naar SAP-Cloud voor klanten met behulp van Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: jingwang
ms.openlocfilehash: e4625b934f9e1cf98254f3dee59f9c26e8e16fb5
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54353376"
---
# <a name="copy-data-from-sap-cloud-for-customer-c4c-using-azure-data-factory"></a>Kopiëren van gegevens van SAP-Cloud voor klant (C4C) met behulp van Azure Data Factory

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van/naar SAP-Cloud voor klant (C4C). Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens kopiëren van SAP-Cloud voor klanten met een ondersteunde sink-gegevensarchief of gegevens kopiëren van een ondersteund brongegevensarchief naar SAP-Cloud voor klanten. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Deze connector kunnen met name Azure Data Factory om gegevens te kopiëren van/naar SAP-Cloud voor klanten met inbegrip van de SAP-Cloud voor verkoop, SAP-Cloud voor de Service en SAP-Cloud voor sociale Engagement oplossingen.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar SAP-Cloud voor de klant-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor SAP-Cloud voor de service van de klant die is gekoppeld:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **SapCloudForCustomer**. | Ja |
| url | De URL van de SAP C4C OData-service. | Ja |
| gebruikersnaam | Geef de naam van de gebruiker verbinding maken met de SAP-C4C. | Ja |
| wachtwoord | Geef het wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. Als niet is opgegeven, wordt de standaard Azure Integration Runtime. | Nee voor bron, Ja voor sink |

>[!IMPORTANT]
>Om gegevens te kopiëren naar SAP-Cloud voor klanten, expliciet [maken van een Azure IR](create-azure-integration-runtime.md#create-azure-ir) met een locatie in de buurt van uw SAP-Cloud voor klanten en koppelen in de gekoppelde service als in het volgende voorbeeld:

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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SAP-Cloud voor de gegevensset van de klant.

Om gegevens te kopiëren vanuit SAP-Cloud voor klanten, stel de eigenschap type van de gegevensset in **SapCloudForCustomerResource**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **SapCloudForCustomerResource** |Ja |
| pad | Geef het pad van de SAP C4C OData-entiteit. |Ja |

**Voorbeeld:**

```json
{
    "name": "SAPC4CDataset",
    "properties": {
        "type": "SapCloudForCustomerResource",
        "typeProperties": {
            "path": "<path e.g. LeadCollection>"
        },
        "linkedServiceName": {
            "referenceName": "<SAP C4C linked service>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SAP-Cloud voor de bron van de klant.

### <a name="sap-c4c-as-source"></a>SAP-C4C als bron

Om gegevens te kopiëren vanuit SAP-Cloud voor klanten, stelt u het brontype in de kopieeractiviteit naar **SapCloudForCustomerSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **SapCloudForCustomerSource**  | Ja |
| query | Geef de aangepaste OData-query voor het lezen van gegevens. | Nee |

Voorbeeld-query om gegevens te verkrijgen voor een specifieke dag: `"query": "$filter=CreatedOn ge datetimeoffset'2017-07-31T10:02:06.4202620Z' and CreatedOn le datetimeoffset'2017-08-01T10:02:06.4202620Z'"`

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
                "type": "SapCloudForCustomerSource",
                "query": "<custom query e.g. $top=10>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="sap-c4c-as-sink"></a>SAP-C4C als sink

Om gegevens te kopiëren naar SAP-Cloud voor klanten, stelt u het sink-type in de kopieeractiviteit naar **SapCloudForCustomerSink**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **sink** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **SapCloudForCustomerSink**  | Ja |
| WriteBehavior | Het gedrag van het schrijven van de bewerking. Kan worden 'Insert', 'Update'. | Nee. Standaard 'Insert'. |
| WriteBatchSize | De batchgrootte van de schrijfbewerking. De batchgrootte om de beste prestaties kan mogelijk verschillen voor andere tabel of de server. | Nee. Standaard 10. |

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
            "dataIntegrationUnits": 4,
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

## <a name="data-type-mapping-for-sap-cloud-for-customer"></a>Gegevenstypetoewijzing voor SAP-Cloud voor klanten

Bij het kopiëren van gegevens van SAP-Cloud voor klanten, worden de volgende toewijzingen gebruikt vanuit SAP-Cloud voor gegevenstypen van de klant op Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe copy activity in het schema en de gegevens van een brontype aan de sink toegewezen.

| SAP C4C OData-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| Edm.Binary | Byte[] |
| Edm.Boolean | BOOL |
| Edm.Byte | Byte[] |
| Edm.DateTime | DateTime |
| Edm.Decimal | Decimaal |
| Edm.Double | Double-waarde |
| Edm.Single | Enkelvoudig |
| Edm.Guid | GUID |
| Edm.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | Reeks |
| Edm.Time | TimeSpan |
| Edm.DateTimeOffset | DateTimeOffset |


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
