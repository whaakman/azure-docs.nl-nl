---
title: Gegevens kopiëren van SAP HANA met behulp van Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van SAP HANA naar ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/07/2018
ms.author: jingwang
ms.openlocfilehash: cdd83c3ff9d34a5e8b7f2c164136ab82f498ffb5
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54022968"
---
# <a name="copy-data-from-sap-hana-using-azure-data-factory"></a>Gegevens kopiëren van SAP HANA met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-sap-hana-connector.md)
> * [Huidige versie](connector-sap-hana.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een SAP HANA-database. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van SAP HANA-database kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die door de kopieeractiviteit ondersteund als bronnen/sinks, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies ondersteunt deze SAP HANA-connector:

- Kopiëren van gegevens vanuit een willekeurige versie van SAP HANA-database.
- Kopiëren van gegevens uit **HANA-informatiemodellen** (zoals de weergaven analyse en berekenen) en **rij/kolom tabellen** met behulp van SQL-query's.
- Kopiëren van gegevens met **Basic** of **Windows** verificatie.

> [!NOTE]
> Om gegevens te kopiëren **in** SAP HANA-gegevens opslaan, algemene ODBC-connector gebruiken. Zie [SAP HANA-sink](connector-odbc.md#sap-hana-sink) met details. De gekoppelde services voor SAP HANA-connector en ODBC-connector met een ander type zijn Opmerking dus kan niet opnieuw worden gebruikt.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van deze SAP HANA-connector, moet u naar:

- Instellen van een zelfgehoste Cloudintegratieruntime. Zie [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.
- De SAP HANA ODBC-stuurprogramma op de machine Integration Runtime installeren. U kunt downloaden de SAP HANA ODBC-stuurprogramma van de [SAP Software Download Center](https://support.sap.com/swdc). Zoek met het trefwoord **SAP HANA CLIENT voor Windows**.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor SAP HANA-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor SAP HANA gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **SapHana** | Ja |
| server | Naam van de server waarop de SAP HANA-instantie zich bevindt. Als uw server een aangepaste poort gebruikt is, geeft u `server:port`. | Ja |
| authenticationType | Het type verificatie gebruikt voor verbinding met de SAP HANA-database.<br/>Toegestane waarden zijn: **Basic**, en **Windows** | Ja |
| Gebruikersnaam | De naam van de gebruiker die toegang tot de SAP-server heeft. | Ja |
| wachtwoord | Het wachtwoord voor de gebruiker. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. Er is een zelfgehoste Cloudintegratieruntime vereist zoals vermeld in [vereisten](#prerequisites). |Ja |

**Voorbeeld:**

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "server": "<server>:<port (optional)>",
            "authenticationType": "Basic",
            "userName": "<username>",
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SAP HANA-gegevensset.

Als u wilt kopiëren van gegevens van SAP HANA, stel de eigenschap type van de gegevensset in **RelationalTable**. Typ RelationalTable terwijl er geen specifieke eigenschappen die worden ondersteund voor de SAP HANA-gegevensset van zijn.

**Voorbeeld:**

```json
{
    "name": "SAPHANADataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<SAP HANA linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SAP HANA-bron.

### <a name="sap-hana-as-source"></a>SAP HANA als bron

Om gegevens te kopiëren van SAP HANA, stelt u het brontype in de kopieeractiviteit naar **RelationalSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **RelationalSource** | Ja |
| query | Hiermee geeft u de SQL-query voor het lezen van gegevens van de SAP HANA-instantie. | Ja |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromSAPHANA",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP HANA input dataset name>",
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
                "query": "<SQL query for SAP HANA>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-hana"></a>De gegevenstypetoewijzing voor SAP HANA

Het kopiëren van gegevens van SAP HANA, worden de volgende toewijzingen van SAP HANA-gegevenstypen gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe copy activity in het schema en de gegevens van een brontype aan de sink toegewezen.

| SAP HANA-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| ALPHANUM | Reeks |
| BIGINT | Int64 |
| BLOB | Byte[] |
| BOOLEAANSE WAARDE | Byte |
| CLOB | Byte[] |
| DATE | DateTime |
| DECIMAAL | Decimaal |
| DOUBLE-WAARDE | Enkelvoudig |
| INT | Int32 |
| NVARCHAR | Reeks |
| ECHTE | Enkelvoudig |
| SECONDDATE | DateTime |
| SMALLINT | Int16 |
| TIME | TimeSpan |
| TIJDSTEMPEL | DateTime |
| TINYINT | Byte |
| VARCHAR | Reeks |

## <a name="known-limitations"></a>Bekende beperkingen

Er zijn enkele bekende beperkingen bij het kopiëren van gegevens van SAP HANA:

- NVARCHAR-tekenreeksen worden afgekapt tot de maximale lengte van 4000 Unicode-tekens
- SMALLDECIMAL wordt niet ondersteund
- VARBINARY wordt niet ondersteund.
- Geldige datums liggen tussen 30-12-1899 en 9999/12/31


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
