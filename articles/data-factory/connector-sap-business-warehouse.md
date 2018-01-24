---
title: "Gegevens kopiëren van SAP BW met behulp van Azure Data Factory | Microsoft Docs"
description: "Ontdek hoe u gegevens kopiëren van een SAP Business Warehouse naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn."
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
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: 7f494cff1e8dc57a41467cd722fdf224e10c9dec
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-sap-business-warehouse-using-azure-data-factory"></a>Gegevens kopiëren van een SAP Business Warehouse met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-sap-business-warehouse-connector.md)
> * [Versie 2 - Preview](connector-sap-business-warehouse.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een SAP Business Warehouse (BW). Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [SAP BW-connector in V1](v1/data-factory-sap-business-warehouse-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit een SAP Business Warehouse kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze SAP Business Warehouse-connector:

- SAP Business Warehouse **versie 7.x**.
- Kopiëren van gegevens van **InfoCubes en QueryCubes** (inclusief BEx query's) met MDX-query's.
- Kopiëren van gegevens met behulp van basisverificatie.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van deze SAP Business Warehouse-connector, moet u:

- Stel een Self-hosted integratie-Runtime. Zie [Self-hosted integratie Runtime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.
- Installeer de **SAP NetWeaver bibliotheek** op de machine integratie Runtime. U kunt de bibliotheek SAP Netweaver ophalen uit de SAP-beheerder of rechtstreeks uit de [SAP Software Download Center](https://support.sap.com/swdc). Zoeken naar de **SAP-notitie #1025361** ophalen van de downloadlocatie voor de meest recente versie. Zorg ervoor dat u kiest de **64-bits** SAP NetWeaver library, die overeenkomt met de integratie Runtime-installatie. Installeer vervolgens alle bestanden die zijn opgenomen in de SDK SAP NetWeaver RFC volgens de SAP-notitie. De bibliotheek SAP NetWeaver is ook opgenomen in de clienthulpprogramma's voor SAP-installatie.

> [!TIP]
> De DLL's van de NetWeaver RFC-SDK hebt uitgepakt in de map system32 plaatsen.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met SAP Business Warehouse-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor SAP Business Warehouse (BW) gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **SapBw** | Ja |
| server | Naam van de server waarop het exemplaar SAP BW zich bevindt. | Ja |
| systemNumber | Systeemnummer van het SAP BW-systeem.<br/>Toegestane waarde: decimaal getal zijn twee cijfers weergegeven als een tekenreeks. | Ja |
| clientId | Client-ID van de client in het systeem SAP-W.<br/>Toegestane waarde: decimaal nummer met drie cijfers weergegeven als een tekenreeks. | Ja |
| userName | De naam van de gebruiker die toegang tot de SAP-server heeft. | Ja |
| wachtwoord | Wachtwoord voor de gebruiker. Dit veld markeren als een SecureString. | Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. Een Runtime Self-hosted-integratie is vereist zoals vermeld in [vereisten](#prerequisites). |Ja |

**Voorbeeld:**

```json
{
    "name": "SapBwLinkedService",
    "properties": {
        "type": "SapBw",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SAP BW-gegevensset.

Om gegevens te kopiëren uit SAP BW, stel de eigenschap type van de gegevensset **RelationalTable**. Typ RelationalTable terwijl er geen type-specifieke eigenschappen voor de gegevensset SAP BW van ondersteund zijn.

**Voorbeeld:**

```json
{
    "name": "SAPBWDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<SAP BW linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door SAP BW-bron.

### <a name="sap-bw-as-source"></a>SAP BW als bron

Om gegevens te kopiëren uit SAP BW, stelt u het brontype in de kopieerbewerking naar **RelationalSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **RelationalSource** | Ja |
| query | Hiermee geeft u de MDX-query om te lezen van gegevens uit de SAP BW-exemplaar. | Ja |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromSAPBW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP BW input dataset name>",
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
                "type": "RelationalSource",
                "query": "<MDX query for SAP BW>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-bw"></a>De gegevenstypetoewijzing voor SAP BW

Bij het kopiëren van gegevens uit SAP BW, worden de volgende toewijzingen van gegevenstypen voor SAP BW gebruikt voor Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

| Gegevenstype voor SAP BW | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| ACCP | Int |
| CHAR | Tekenreeks |
| CLNT | Tekenreeks |
| CURR | Decimale |
| CUKY | Tekenreeks |
| DEC | Decimale |
| FLTP | Double |
| INT1 | Byte |
| INT2 | Int16 |
| INT4 | Int |
| LANG | Tekenreeks |
| LCHR | Tekenreeks |
| LRAW | Byte[] |
| PREC | Int16 |
| QUAN | Decimale |
| RAW | Byte[] |
| RAWSTRING | Byte[] |
| TEKENREEKS | Tekenreeks |
| EENHEID | Tekenreeks |
| DATS | Tekenreeks |
| NUMC | Tekenreeks |
| TIMS | Tekenreeks |


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).