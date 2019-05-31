---
title: Gegevens kopiëren van SAP-tabel met behulp van Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van SAP-tabel naar een ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: jingwang
ms.openlocfilehash: 4dee0e994c9e7be9677a8f1051481850990998e9
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247167"
---
# <a name="copy-data-from-sap-table-using-azure-data-factory"></a>Gegevens kopiëren van SAP-tabel met behulp van Azure Data Factory

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een SAP-tabel. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit SAP-tabel naar een ondersteunde sink-gegevensopslag kopiëren. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Deze tabel voor SAP-connector ondersteunt name:

- Kopiëren van gegevens uit de SAP-tabel in **SAP Business Suite met versie 7.01 of hoger** (in een recente SAP ondersteuning voor pakket-Stack die zijn uitgebracht na het jaar 2015) of **S/4HANA**.
- Kopiëren van gegevens uit zowel **SAP transparante tabel** en **weergave**.
- Kopiëren van gegevens met **basisverificatie** of **SNC** (Secure Network Communications) als SNC is geconfigureerd.
- Verbinding maken met **toepassingsserver** of **Message-Server**.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van deze tabel voor SAP-connector, moet u naar:

- Instellen van een zelfgehoste Integration Runtime met versie 3.17 of hoger. Zie [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.

- Download de **64-bits [SAP .NET Connector 3.0](https://support.sap.com/en/product/connectors/msnet.html)**  van de SAP-website, en op de machine zelfgehoste IR installeren. Wanneer u installeert, in het instellingenvenster voor optionele stappen Zorg ervoor dat u selecteert de **assembly's installeren in GAC** optie. 

    ![SAP .NET Connector installeren](./media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

- SAP-gebruiker wordt gebruikt in de Data Factory-tabel SAP-connector moet de volgende bevoegdheden hebben: 

    - Autorisatie voor RFC. 
    - Machtigingen voor de activiteit "Uitvoeren" van autorisatie-Object 'S_SDSAUTH'.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar tabel van de SAP-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor SAP Business Warehouse Open Hub gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **SapTable** | Ja |
| server | Naam van de server waarop de SAP-instantie zich bevindt.<br/>Van toepassing als u verbinding maken wilt met **SAP-toepassingsserver**. | Nee |
| systemNumber | Het systeemnummer van de SAP-systeem.<br/>Van toepassing als u verbinding maken wilt met **SAP-toepassingsserver**.<br/>Toegestane waarde: decimaal getal van twee cijfers weergegeven als een tekenreeks. | Nee |
| messageServer | De hostnaam van de SAP-berichtenserver.<br/>Van toepassing als u verbinding maken wilt met **SAP-berichtenserver**. | Nee |
| messageServerService | De servicenaam van de of de poort van de bericht-Server.<br/>Van toepassing als u verbinding maken wilt met **SAP-berichtenserver**. | Nee |
| systeem-id | Systeem-id van de SAP-systeem waarin de tabel zich bevindt.<br/>Van toepassing als u verbinding maken wilt met **SAP-berichtenserver**. | Nee |
| logonGroup | De groep van de aanmelding voor de SAP-systeem.<br/>Van toepassing als u verbinding maken wilt met **SAP-berichtenserver**. | Nee |
| clientId | Client-ID van de client in de SAP-systeem.<br/>Toegestane waarde: decimaal getal van drie cijfers wordt weergegeven als een tekenreeks. | Ja |
| language | De taal die gebruikmaakt van de SAP-systeem. | Nee (standaardwaarde is **EN**)|
| userName | De naam van de gebruiker die toegang tot de SAP-server heeft. | Ja |
| password | Het wachtwoord voor de gebruiker. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| sncMode | SNC-activering indicator voor toegang tot de SAP-server waar de tabel zich bevindt.<br/>Heeft de van toepassing als u wilt gebruiken van SNC verbinding maken met SAP-server.<br/>Toegestane waarden zijn: **0** (uitgeschakeld, standaard) of **1** (aan). | Nee |
| sncMyName | Naam van de initiator van SNC voor toegang tot de SAP-server waar de tabel zich bevindt.<br/>Van toepassing wanneer `sncMode` is ingeschakeld. | Nee |
| sncPartnerName | De SNC-naam van de partner van de communicatie voor toegang tot de SAP-server waar de tabel zich bevindt.<br/>Van toepassing wanneer `sncMode` is ingeschakeld. | Nee |
| sncLibraryPath | Externe beveiliging van het product-bibliotheek voor toegang tot de SAP-server waar de tabel zich bevindt.<br/>Van toepassing wanneer `sncMode` is ingeschakeld. | Nee |
| sncQop | SNC-kwaliteit van de beveiliging.<br/>Van toepassing wanneer `sncMode` is ingeschakeld. <br/>Toegestane waarden zijn: **1** (verificatie), **2** (integriteit), **3** (Privacy), **8** (standaard), **9** (Maximum). | Nee |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. Er is een zelfgehoste Cloudintegratieruntime vereist zoals vermeld in [vereisten](#prerequisites). |Ja |

**Voorbeeld 1: verbinding maken met de SAP-toepassingsserver**

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
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

**Voorbeeld 2: verbinding maken met de SAP-berichtenserver**

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "messageServer": "<message server name>",
            "messageServerService": "<service name or port>",
            "systemId": "<system id>",
            "logonGroup": "<logon group>",
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

**Voorbeeld 3: verbinding maken met behulp van SNC**

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            },
            "sncMode": 1,
            "sncMyName": "snc myname",
            "sncPartnerName": "snc partner name",
            "sncLibraryPath": "snc library path",
            "sncQop": "8"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de SAP-Table-gegevensset.

Als u wilt kopiëren van gegevens van en naar SAP BW Open Hub, worden de volgende eigenschappen ondersteund.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **SapTableResource**. | Ja |
| tableName | De naam van de SAP-tabel om te kopiëren van gegevens uit. | Ja |

**Voorbeeld:**

```json
{
    "name": "SAPTableDataset",
    "properties": {
        "type": "SapTableResource",
        "linkedServiceName": {
            "referenceName": "<SAP Table linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<SAP table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SAP tabel bron.

### <a name="sap-table-as-source"></a>SAP-tabel als bron

Om gegevens te kopiëren uit SAP-tabel, worden de volgende eigenschappen worden ondersteund.

| Eigenschap                         | Description                                                  | Vereist |
| :------------------------------- | :----------------------------------------------------------- | :------- |
| type                             | De eigenschap type moet worden ingesteld op **SapTableSource**.       | Ja      |
| rowCount                         | Het aantal rijen moeten worden opgehaald.                              | Nee       |
| rfcTableFields                   | Velden om te kopiëren uit de SAP-tabel. Bijvoorbeeld `column0, column1`. | Nee       |
| rfcTableOptions                  | Opties voor het filteren van de rijen in de SAP-tabel. Bijvoorbeeld `COLUMN0 EQ 'SOMEVALUE'`. | Nee       |
| customRfcReadTableFunctionModule | Aangepaste RFC functiemodule die kan worden gebruikt om gegevens te lezen uit SAP-tabel. | Nee       |
| partitionOption                  | Het mechanisme voor partitie te lezen uit SAP-tabel. De ondersteunde opties zijn onder andere: <br/>- **Geen**<br/>- **PartitionOnInt** (normale geheel getal of gehele getallen met nul opvulling aan de linkerkant, zoals 0000012345)<br/>- **PartitionOnCalendarYear** (4 cijfers in de indeling "YYYY")<br/>- **PartitionOnCalendarMonth** (6 cijfers in de indeling "JJJJMM")<br/>- **PartitionOnCalendarDate** (8 cijfers in de indeling "JJJJMMDD") | Nee       |
| partitionColumnName              | De naam van de kolom voor het partitioneren van de gegevens. | Nee       |
| partitionUpperBound              | De maximale waarde van de kolom die is opgegeven `partitionColumnName` dat wordt gebruikt voor het partitioneren van u doorgaat. | Nee       |
| partitionLowerBound              | De minimumwaarde van de kolom die is opgegeven `partitionColumnName` dat wordt gebruikt voor het partitioneren van u doorgaat. | Nee       |
| maxPartitionsNumber              | Het maximale aantal partities splitsen van de gegevens in. | Nee       |

>[!TIP]
>- Als uw SAP-tabel heeft een groot volume aan gegevens, zoals verschillende miljarden rijen, gebruikt u `partitionOption` en `partitionSetting` als u wilt de gegevens splitsen in kleine partities, in welk geval gegevens worden gelezen door partities en elke partitie wordt opgehaald uit uw SAP-server via één RFC-aanroep.<br/>
>- Duurt `partitionOption` als `partitionOnInt` bijvoorbeeld het aantal rijen in elke partitie wordt berekend door (totaal aantal rijen die zich tussen *partitionUpperBound* en *partitionLowerBound*) /*maxPartitionsNumber*.<br/>
>- Als u wilt om meer partities parallel te versnellen kopiëren uit te voeren, het is raadzaam om `maxPartitionsNumber` als een veelvoud zijn van de waarde van `parallelCopies` (meer informatie uit [parallelle kopie](copy-activity-performance.md#parallel-copy)).

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromSAPTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP Table input dataset name>",
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
                "type": "SapTableSource",
                "partitionOption": "PartitionOnInt",
                "partitionSettings": {
                     "partitionColumnName": "<partition column name>",
                     "partitionUpperBound": "2000",
                     "partitionLowerBound": "1",
                     "maxPartitionsNumber": 500
                 }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-table"></a>De gegevenstypetoewijzing voor SAP-tabel

Bij het kopiëren van gegevens uit SAP-tabel, worden de volgende toewijzingen van gegevens van SAP tabeltypen gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe copy activity in het schema en de gegevens van een brontype aan de sink toegewezen.

| SAP ABAP-Type | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| C (String) | String |
| Ik (geheel getal) | Int32 |
| F (drijvende komma) | Double |
| D (Date) | String |
| T (Time) | String |
| P (BCD verpakt, valuta, decimaal, hoeveelheid) | Decimal |
| N (numeriek) | String |
| X (binaire en onbewerkte) | String |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
