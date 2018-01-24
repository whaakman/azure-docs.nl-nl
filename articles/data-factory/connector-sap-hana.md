---
title: "Gegevens kopiëren van SAP HANA met behulp van Azure Data Factory | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens van SAP HANA naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn."
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
ms.openlocfilehash: cb70b6fee5257a07dda673d6d0f6feb07ad66958
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-sap-hana-using-azure-data-factory"></a>Gegevens kopiëren van SAP HANA met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-sap-hana-connector.md)
> * [Versie 2 - Preview](connector-sap-hana.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een SAP HANA-database. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [SAP HANA-connector in V1](v1/data-factory-sap-hana-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit de SAP HANA-database kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven als bronnen/put wordt ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze SAP HANA-connector:

- Kopiëren van gegevens vanaf een versie van de SAP HANA-database.
- Kopiëren van gegevens van **HANA informatiemodellen** (zoals weergaven, analyse en berekening) en **rij/kolom tabellen** met behulp van SQL-query's.
- Kopiëren van gegevens met **Basic** of **Windows** verificatie.

> [!NOTE]
> Om gegevens te kopiëren **in** SAP HANA-gegevens opslaat, algemene beheerprogramma voor ODBC-connector gebruiken. Zie [SAP HANA sink](connector-odbc.md#sap-hana-sink) met details. De gekoppelde services voor SAP HANA-connector en ODBC-connector zijn met een ander type dus kan niet opnieuw worden gebruikt.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van deze SAP HANA-connector, moet u:

- Stel een Self-hosted integratie-Runtime. Zie [Self-hosted integratie Runtime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.
- Installeer het SAP HANA ODBC-stuurprogramma op de machine integratie Runtime. U kunt downloaden de SAP HANA ODBC-stuurprogramma van de [SAP Software Download Center](https://support.sap.com/swdc). Zoeken met het trefwoord **SAP HANA-CLIENT voor Windows**.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor SAP HANA-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor SAP HANA gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **SapHana** | Ja |
| server | Naam van de server waarop het exemplaar SAP HANA zich bevindt. Als de server een aangepaste poort gebruikt is, geeft u `server:port`. | Ja |
| authenticationType | Het soort verificatie die wordt gebruikt voor verbinding met de SAP HANA-database.<br/>Toegestane waarden zijn: **Basic**, en **Windows** | Ja |
| userName | De naam van de gebruiker die toegang tot de SAP-server heeft. | Ja |
| wachtwoord | Wachtwoord voor de gebruiker. Dit veld markeren als een SecureString. | Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. Een Runtime Self-hosted-integratie is vereist zoals vermeld in [vereisten](#prerequisites). |Ja |

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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SAP HANA-gegevensset.

Om gegevens te kopiëren uit een SAP HANA, stel de eigenschap type van de gegevensset **RelationalTable**. Typ RelationalTable terwijl er geen type-specifieke eigenschappen voor de gegevensset SAP HANA van ondersteund zijn.

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

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door SAP HANA-bron.

### <a name="sap-hana-as-source"></a>SAP HANA als bron

Om gegevens te kopiëren uit een SAP HANA, stelt u het brontype in de kopieerbewerking naar **RelationalSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **RelationalSource** | Ja |
| query | Hiermee geeft u de SQL-query voor het lezen van gegevens uit de SAP HANA-exemplaar. | Ja |

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

Bij het kopiëren van gegevens uit een SAP HANA, worden de volgende toewijzingen van SAP HANA-gegevenstypen gebruikt om tussentijdse Azure Data Factory-gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

| SAP HANA-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| ALPHANUM | Tekenreeks |
| BIGINT | Int64 |
| BLOB | Byte[] |
| BOOLEAANSE WAARDE | Byte |
| CLOB | Byte[] |
| DATE | Datum en tijd |
| DECIMAL | Decimale |
| DOUBLE | Single |
| INT | Int32 |
| NVARCHAR | Tekenreeks |
| ECHTE | Single |
| SECONDDATE | Datum en tijd |
| SMALLINT | Int16 |
| TIJD | TimeSpan |
| TIJDSTEMPEL | Datum en tijd |
| TINYINT | Byte |
| VARCHAR | Tekenreeks |

## <a name="known-limitations"></a>Bekende beperkingen

Er zijn enkele bekende beperkingen bij het kopiëren van gegevens uit een SAP HANA:

- NVARCHAR tekenreeksen worden afgekapt tot de maximale lengte van 4000 Unicode-tekens
- SMALLDECIMAL wordt niet ondersteund.
- VARBINARY wordt niet ondersteund.
- Geldige datums tussen 12-1899/30 zijn en 12-9999/31


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).