---
title: Gegevens kopiëren van SAP BW met behulp van Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van SAP Business Warehouse naar ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/07/2018
ms.author: jingwang
ms.openlocfilehash: 9a0abcd70b4aeb2369604bafa924136122206e0a
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54022278"
---
# <a name="copy-data-from-sap-business-warehouse-using-azure-data-factory"></a>Gegevens kopiëren van SAP Business Warehouse met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-sap-business-warehouse-connector.md)
> * [Huidige versie](connector-sap-business-warehouse.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een SAP Business Warehouse (BW). Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van SAP Business Warehouse kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies ondersteunt deze SAP Business Warehouse-connector:

- SAP Business Warehouse **versie 7.x**.
- Kopiëren van gegevens uit **InfoCubes en QueryCubes** (met inbegrip van BEx query's) met MDX-query's.
- Kopiëren van gegevens met behulp van basisverificatie.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van deze connector SAP Business Warehouse, moet u naar:

- Instellen van een zelfgehoste Cloudintegratieruntime. Zie [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.
- Installeer de **SAP NetWeaver-bibliotheek** op de machine Integration Runtime. U kunt de SAP Netweaver-bibliotheek ophalen van uw SAP-beheerder, of rechtstreeks vanuit de [SAP Software Download Center](https://support.sap.com/swdc). Zoek de **SAP Opmerking #1025361** om op te halen van de downloadlocatie voor de meest recente versie. Zorg ervoor dat u kiest de **64-bits** SAP NetWeaver-bibliotheek die overeenkomt met de installatie van Integration Runtime. Installeer vervolgens alle bestanden die zijn opgenomen in de SAP NetWeaver RFC-SDK op basis van de SAP-notitie. De SAP NetWeaver-bibliotheek is ook opgenomen in de installatie van de SAP-clienthulpprogramma's.

>[!TIP]
>Voor het oplossen van verbindingsproblemen naar SAP BW, zorg ervoor dat:
>- Alle afhankelijkheid bibliotheken van de NetWeaver RFC-SDK hebt uitgepakt zijn aanwezig in de map %windir%\system32. Dit heeft meestal icudt34.dll, icuin34.dll, icuuc34.dll, libicudecnumber.dll, librfc32.dll, libsapucum.dll, sapcrypto.dll, sapcryto_old.dll, sapnwrfc.dll.
>- De vereiste poorten gebruikt voor verbinding met SAP-Server zijn ingeschakeld op de machine zelfgehoste IR, die meestal poort 3300 en 3201 zijn.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met SAP Business Warehouse-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor SAP Business Warehouse (BW) gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **SapBw** | Ja |
| server | Naam van de server waarop de SAP BW-instantie zich bevindt. | Ja |
| systemNumber | Het systeemnummer van de SAP BW-systeem.<br/>Toegestane waarde: decimaal getal van twee cijfers weergegeven als een tekenreeks. | Ja |
| ClientId | Client-ID van de client in het W SAP-systeem.<br/>Toegestane waarde: decimaal getal van drie cijfers wordt weergegeven als een tekenreeks. | Ja |
| Gebruikersnaam | De naam van de gebruiker die toegang tot de SAP-server heeft. | Ja |
| wachtwoord | Het wachtwoord voor de gebruiker. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. Er is een zelfgehoste Cloudintegratieruntime vereist zoals vermeld in [vereisten](#prerequisites). |Ja |

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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SAP BW-gegevensset.

Als u wilt kopiëren van gegevens van SAP BW, stel de eigenschap type van de gegevensset in **RelationalTable**. Typ RelationalTable terwijl er geen specifieke eigenschappen die worden ondersteund voor de SAP BW-gegevensset van zijn.

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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SAP BW-bron.

### <a name="sap-bw-as-source"></a>SAP BW als bron

Om gegevens te kopiëren van SAP BW, stelt u het brontype in de kopieeractiviteit naar **RelationalSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **RelationalSource** | Ja |
| query | Hiermee geeft u de MDX-query voor het lezen van gegevens van de SAP BW-exemplaar. | Ja |

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

Het kopiëren van gegevens van SAP BW, worden de volgende toewijzingen van SAP BW-gegevenstypen gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe copy activity in het schema en de gegevens van een brontype aan de sink toegewezen.

| SAP BW-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| ACCP | Int |
| CHAR | Reeks |
| CLNT | Reeks |
| VAL | Decimaal |
| CUKY | Reeks |
| DECEMBER | Decimaal |
| FLTP | Double-waarde |
| INT1 | Byte |
| INT2 | Int16 |
| INT4 | Int |
| LANG | Reeks |
| LCHR | Reeks |
| LRAW | Byte[] |
| PREC | Int16 |
| QUAN | Decimaal |
| ONBEWERKTE | Byte[] |
| RAWSTRING | Byte[] |
| TEKENREEKS | Reeks |
| EENHEID | Reeks |
| DATS | Reeks |
| NUMC | Reeks |
| TIM | Reeks |


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
