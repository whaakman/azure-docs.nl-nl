---
title: Gegevens kopiëren van SAP ECC met behulp van Azure Data Factory | Microsoft Docs
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
ms.date: 07/02/2019
ms.author: jingwang
ms.openlocfilehash: 7ccd2e7a804c6495f6caf5e264b1f7c2a36cb02e
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827773"
---
# <a name="copy-data-from-sap-ecc-by-using-azure-data-factory"></a>Gegevens kopiëren van SAP ECC met behulp van Azure Data Factory

In dit artikel bevat een overzicht over het gebruik van de kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit SAP Enterprise centraal onderdeel (ECC). Zie voor meer informatie, [overzicht Kopieeractiviteit](copy-activity-overview.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit SAP ECC kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als gegevensbronnen of PUT voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies ondersteunt deze SAP ECC-connector:

- Het kopiëren van gegevens uit SAP ECC op SAP NetWeaver versie 7.0 en hoger.
- Het kopiëren van gegevens uit alle objecten die worden weergegeven door SAP ECC OData-services, zoals:

  - SAP-tabellen of weergaven.
  - Business Application Programming Interface [BAPI]-objecten.
  - Gegevens extractors.
  - Gegevens of tussenliggende documenten (idoc's) die naar SAP proces Integration (PI) die kunnen worden ontvangen als OData via relatieve adapters zijn verzonden.

- Kopiëren van gegevens met behulp van basisverificatie.

>[!TIP]
>Gebruiken om gegevens te kopiëren uit SAP ECC via een SAP-tabel of weergave, de [SAP tabel](connector-sap-table.md) -connector, die sneller en beter schaalbaar is.

## <a name="prerequisites"></a>Vereisten

Over het algemeen SAP ECC wordt aangegeven dat entiteiten via OData-services via SAP-Gateway. Voor het gebruik van deze SAP ECC-connector, moet u naar:

- **SAP-Gateway instellen**. Voor servers met SAP NetWeaver-versies hoger is dan 7.4, is SAP-Gateway al geïnstalleerd. Voor eerdere versies, moet u de ingesloten SAP-Gateway of het systeem van de hub SAP-Gateway installeren voordat het blootstellen van SAP ECC-gegevens via OData-services. Als u de SAP-Gateway instelt, Zie de [installatiehandleiding](https://help.sap.com/saphelp_gateway20sp12/helpdata/en/c3/424a2657aa4cf58df949578a56ba80/frameset.htm).

- **Activeren en configureren van de SAP-OData-service**. U kunt de OData-service via TCODE SICF activeren in seconden. U kunt ook configureren welke objecten moeten worden weergegeven. Zie voor meer informatie de [stapsgewijze begeleiding](https://blogs.sap.com/2012/10/26/step-by-step-guide-to-build-an-odata-service-based-on-rfcs-part-1/).

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van de specifieke Data Factory-entiteiten met de SAP ECC-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de SAP ECC gekoppelde service:

| Eigenschap | Description | Verplicht |
|:--- |:--- |:--- |
| `type` | De `type` eigenschap moet worden ingesteld op `SapEcc`. | Ja |
| `url` | De URL van de SAP ECC OData-service. | Ja |
| `username` | De gebruikersnaam die wordt gebruikt om te verbinden met SAP ECC. | Nee |
| `password` | Het leesbare wachtwoord gebruikt voor verbinding met SAP ECC. | Nee |
| `connectVia` | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt een zelf-hostende integratieruntime of de Azure integratieruntime gebruiken (als uw gegevensarchief openbaar toegankelijk zijn is). Als u geen dat een runtime opgeeft `connectVia` maakt gebruik van de standaard Azure integratieruntime. | Nee |

### <a name="example"></a>Voorbeeld

```json
{
    "name": "SapECCLinkedService",
    "properties": {
        "type": "SapEcc",
        "typeProperties": {
            "url": "<SAP ECC OData URL, e.g., http://eccsvrname:8000/sap/opu/odata/sap/zgw100_dd02l_so_srv/>",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        }
    },
    "connectVia": {
        "referenceName": "<name of integration runtime>",
        "type": "IntegrationRuntimeReference"
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, [gegevenssets](concepts-datasets-linked-services.md). De volgende sectie bevat een lijst van de eigenschappen die worden ondersteund door de SAP ECC-gegevensset.

Om gegevens te kopiëren uit SAP ECC, stel de `type` eigenschap van de gegevensset in `SapEccResource`.

De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Verplicht |
|:--- |:--- |:--- |
| `path` | Het pad van de SAP ECC OData-entiteit. | Ja |

### <a name="example"></a>Voorbeeld

```json
{
    "name": "SapEccDataset",
    "properties": {
        "type": "SapEccResource",
        "typeProperties": {
            "path": "<entity path, e.g., dd04tentitySet>"
        },
        "linkedServiceName": {
            "referenceName": "<SAP ECC linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, [pijplijnen](concepts-pipelines-activities.md). De volgende sectie bevat een lijst van de eigenschappen die worden ondersteund door de SAP ECC-bron.

### <a name="sap-ecc-as-a-source"></a>SAP ECC als een bron

Om gegevens te kopiëren uit SAP ECC, stel de `type` eigenschap in de `source` sectie van de kopieeractiviteit naar `SapEccSource`.

De volgende eigenschappen worden ondersteund in de kopieeractiviteit `source` sectie:

| Eigenschap | Description | Verplicht |
|:--- |:--- |:--- |
| `type` | De `type` eigenschap van de kopieeractiviteit `source` sectie moet worden ingesteld op `SapEccSource`. | Ja |
| `query` | De OData-queryopties om de gegevens te filteren. Bijvoorbeeld:<br/><br/>`"$select=Name,Description&$top=10"`<br/><br/>De connector SAP ECC worden gegevens gekopieerd van de gecombineerde URL:<br/><br/>`<URL specified in the linked service>/<path specified in the dataset>?<query specified in the copy activity's source section>`<br/><br/>Zie voor meer informatie, [OData-URL-onderdelen](https://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Nee |

### <a name="example"></a>Voorbeeld

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

## <a name="data-type-mappings-for-sap-ecc"></a>Toewijzingen van het gegevenstype voor SAP ECC

Wanneer u gegevens van SAP ECC kopieert, worden de volgende toewijzingen gebruikt uit OData-gegevenstypen voor SAP ECC-gegevens voor Azure Data Factory tussentijdse gegevenstypen. Zie voor meer informatie hoe de kopieerbewerking het schema en de gegevens van een brontype toegewezen aan de sink [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md).

| OData-gegevenstype | Data Factory tussentijdse gegevenstype |
|:--- |:--- |
| `Edm.Binary` | `String` |
| `Edm.Boolean` | `Bool` |
| `Edm.Byte` | `String` |
| `Edm.DateTime` | `DateTime` |
| `Edm.Decimal` | `Decimal` |
| `Edm.Double` | `Double` |
| `Edm.Single` | `Single` |
| `Edm.Guid` | `String` |
| `Edm.Int16` | `Int16` |
| `Edm.Int32` | `Int32` |
| `Edm.Int64` | `Int64` |
| `Edm.SByte` | `Int16` |
| `Edm.String` | `String` |
| `Edm.Time` | `TimeSpan` |
| `Edm.DateTimeOffset` | `DateTimeOffset` |

> [!NOTE]
> Complexe gegevenstypen worden momenteel niet ondersteund.

## <a name="next-steps"></a>Volgende stappen

Zie voor een lijst van de gegevensarchieven die door de kopieeractiviteit in Azure Data Factory ondersteund als bronnen en sinks, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
