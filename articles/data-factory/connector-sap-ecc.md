---
title: Gegevens kopiëren van SAP ECC met Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van SAP ECC naar ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: jingwang
ms.openlocfilehash: ba268806dccb7c2671825c794383396a2ff20299
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57548810"
---
# <a name="copy-data-from-sap-ecc-using-azure-data-factory"></a>Gegevens kopiëren van SAP ECC met Azure Data Factory

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit SAP ECC (SAP Enterprise centraal onderdeel). Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit SAP ECC kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies ondersteunt deze SAP ECC-connector:

- Kopiëren van gegevens vanaf SAP ECC op SAP NetWeaver versie 7.0 en hoger. 
- Kopiëren van gegevens uit de objecten die worden weergegeven door SAP ECC OData-services (zoals SAP tabel/weergaven, BAPI, gegevens Extractors, enzovoort) of verzonden naar SAP-PI die kunnen worden ontvangen als OData via relatieve Adapters idoc's en gegevens.
- Kopiëren van gegevens met behulp van basisverificatie.

## <a name="prerequisites"></a>Vereisten

Over het algemeen SAP ECC wordt aangegeven dat entiteiten via OData-services via SAP-Gateway. Voor het gebruik van deze SAP ECC-connector, moet u naar:

- **SAP-Gateway instellen**. De SAP-Gateway is voor servers met SAP NetWeaver-versie die hoger is dan 7.4 al geïnstalleerd. Anders moet u voor het installeren van de Gateway-ingebed of Gateway hub voordat het blootstellen van SAP ECC-gegevens via OData-services. Meer informatie over het instellen van SAP-Gateway van [installatiehandleiding](https://help.sap.com/saphelp_gateway20sp12/helpdata/en/c3/424a2657aa4cf58df949578a56ba80/frameset.htm).

- **Activeren en configureren van SAP-OData-service**. U kunt het OData-Services via TCODE SICF activeren in seconden. U kunt ook configureren welke objecten dat moet worden weergegeven. Hier volgt een voorbeeld [stapsgewijze begeleiding](https://blogs.sap.com/2012/10/26/step-by-step-guide-to-build-an-odata-service-based-on-rfcs-part-1/).

## <a name="getting-started"></a>Aan de slag

U kunt een pijplijn maken met copy activity in .NET SDK, Python-SDK, Azure PowerShell, REST-API of Azure Resource Manager-sjabloon. Zie [zelfstudie Kopieeractiviteit](quickstart-create-data-factory-dot-net.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit.

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor SAP ECC-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor SAP ECC gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **SapEcc** | Ja |
| url | De url van de SAP ECC OData-service. | Ja |
| gebruikersnaam | De gebruikersnaam die wordt gebruikt voor verbinding met de SAP ECC. | Nee |
| wachtwoord | Het leesbare wachtwoord gebruikt voor verbinding met de SAP ECC. | Nee |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de zelfgehoste Cloudintegratieruntime of Azure Integration Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk zijn is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

**Voorbeeld:**

```json
{
    "name": "SapECCLinkedService",
    "properties": {
        "type": "SapEcc",
        "typeProperties": {
            "url": "<SAP ECC OData url e.g. http://eccsvrname:8000/sap/opu/odata/sap/zgw100_dd02l_so_srv/>",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        }
    },
    "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SAP ECC-gegevensset.

Om gegevens te kopiëren uit SAP ECC, stel de eigenschap type van de gegevensset in **SapEccResource**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| pad | Het pad van de SAP ECC OData-entiteit. | Ja |

**Voorbeeld**

```json
{
    "name": "SapEccDataset",
    "properties": {
        "type": "SapEccResource",
        "typeProperties": {
            "path": "<entity path e.g. dd04tentitySet>"
        },
        "linkedServiceName": {
            "referenceName": "<SAP ECC linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SAP ECC-bron.

### <a name="sap-ecc-as-source"></a>SAP ECC als bron

Om gegevens te kopiëren uit SAP ECC, stelt u het brontype in de kopieeractiviteit naar **SapEccSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **SapEccSource** | Ja |
| query | OData-queryopties om gegevens te filteren. Example: "$select=Name,Description&$top=10".<br/><br/>SAP ECC-connector worden gegevens gekopieerd van de gecombineerde URL: (url opgegeven in de gekoppelde service) / (pad opgegeven in de gegevensset)? (de query is opgegeven in de bron voor kopiëren-activiteit). Raadpleeg [OData-URL-onderdelen](https://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromSAPECC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP ECC input dataset name>",
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
                "type": "SapEccSource",
                "query": "$top=10"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-ecc"></a>De gegevenstypetoewijzing voor SAP ECC

Het kopiëren van gegevens vanaf SAP ECC, worden de volgende toewijzingen gebruikt uit OData-gegevenstypen voor SAP ECC-gegevens met Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe copy activity in het schema en de gegevens van een brontype aan de sink toegewezen.

| OData-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |:--- |
| Edm.Binary | String |
| Edm.Boolean | Bool |
| Edm.Byte | String |
| Edm.DateTime | DateTime |
| Edm.Decimal | Decimal |
| Edm.Double | Double |
| Edm.Single | Single |
| Edm.Guid | String |
| Edm.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | String |
| Edm.Time | TimeSpan |
| Edm.DateTimeOffset | DateTimeOffset |

> [!NOTE]
> Complexe gegevenstypen worden nu niet ondersteund.

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
