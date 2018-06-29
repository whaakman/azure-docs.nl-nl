---
title: Gegevens kopiëren van SAP ECC gebruik van Azure Data Factory | Microsoft Docs
description: Ontdek hoe u gegevens kopiëren van SAP ECC naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
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
ms.date: 04/26/2018
ms.author: jingwang
ms.openlocfilehash: f9f6d2e43fff9a3e57145f39863f66eed64869b2
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37048580"
---
# <a name="copy-data-from-sap-ecc-using-azure-data-factory"></a>Gegevens kopiëren van SAP ECC gebruik van Azure Data Factory

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit SAP ECC (SAP Enterprise centrale onderdeel). Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit SAP ECC kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze SAP ECC-connector:

- Kopiëren van gegevens van SAP ECC op SAP NetWeaver versie 7.0 en hoger. 
- Kopiëren van gegevens van alle objecten die worden weergegeven door SAP ECC OData-services (zoals SAP tabelweergaven, BAPI, gegevens extractie, enz.) of verzonden naar SAP-PI die kunnen worden ontvangen als OData via relatieve Adapters idoc's en gegevens.
- Kopiëren van gegevens met behulp van basisverificatie.

## <a name="prerequisites"></a>Vereisten

SAP ECC wordt over het algemeen entiteiten via OData-services via SAP-Gateway. Voor het gebruik van deze connector SAP ECC, moet u:

- **SAP-Gateway instellen**. De SAP-Gateway is voor servers met SAP NetWeaver versie hoger is dan 7.4 al geïnstalleerd. Anders moet u voor het installeren van ingebed Gateway of Gateway hub voordat SAP ECC-gegevens via de OData-services. Meer informatie over het instellen van SAP-Gateway van [installatiehandleiding](https://help.sap.com/saphelp_gateway20sp12/helpdata/en/c3/424a2657aa4cf58df949578a56ba80/frameset.htm).

- **Activeren en configureren van SAP OData-service**. U kunt de OData-Services via TCODE SICF activeren in seconden. U kunt ook configureren welke objecten moet worden weergegeven. Hier volgt een voorbeeld [stapsgewijze](https://blogs.sap.com/2012/10/26/step-by-step-guide-to-build-an-odata-service-based-on-rfcs-part-1/).

## <a name="getting-started"></a>Aan de slag

U kunt een pijplijn maken met de kopieeractiviteit middels de .NET SDK, Python SDK, Azure PowerShell, REST-API of Azure Resource Manager-sjabloon. Zie [kopie activiteit zelfstudie](quickstart-create-data-factory-dot-net.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit.

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar SAP ECC-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor SAP ECC gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **SapEcc** | Ja |
| url | De url van de SAP ECC OData-service. | Ja |
| gebruikersnaam | De gebruikersnaam die wordt gebruikt voor verbinding met de SAP ECC. | Nee |
| wachtwoord | Het leesbare wachtwoord waarmee verbinding met de SAP ECC. | Nee |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Self-hosted integratie Runtime of Azure integratie Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SAP ECC-gegevensset.

Om gegevens te kopiëren uit SAP ECC, stel de eigenschap type van de gegevensset **SapEccResource**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| pad | Pad van de SAP ECC OData-entiteit. | Ja |

**Voorbeeld**

```json
{
    "name": "SapEccDataset",
    "properties": {
        "type": "SapEccResource",
        "typePoperties": {
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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door SAP ECC-bron.

### <a name="sap-ecc-as-source"></a>SAP ECC als bron

Om gegevens te kopiëren uit SAP ECC, stelt u het brontype in de kopieerbewerking naar **SapEccSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **SapEccSource** | Ja |
| query | Opties voor OData-query om gegevens te filteren. Voorbeeld: ' $select = naam, beschrijving & $top = 10 ".<br/><br/>SAP ECC-connector worden gegevens gekopieerd van de gecombineerde URL: (url opgegeven in de gekoppelde service) (pad opgeven in de gegevensset)? (de query is opgegeven in de bron voor kopiëren-activiteit). Raadpleeg [OData-URL onderdelen](http://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Nee |

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

Bij het kopiëren van gegevens uit SAP ECC, worden de volgende toewijzingen gebruikt van OData-gegevenstypen voor SAP ECC-gegevens voor Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

| OData-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |:--- |
| Edm.Binary | Reeks |
| Edm.Boolean | BOOL |
| Edm.Byte | Reeks |
| Edm.DateTime | DateTime |
| Edm.Decimal | decimale |
| Edm.Double | Double |
| Edm.Single | Enkelvoudig |
| Edm.Guid | Reeks |
| Edm.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | Reeks |
| Edm.Time | TimeSpan |
| Edm.DateTimeOffset | DateTimeOffset |

> [!NOTE]
> Complexe gegevenstypen worden nu niet ondersteund.

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
