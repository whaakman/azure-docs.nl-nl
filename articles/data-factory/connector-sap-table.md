---
title: Gegevens uit een SAP-tabel kopiëren met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het kopiëren van gegevens uit een SAP-tabel naar ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: jingwang
ms.openlocfilehash: 9216f5c00cbdac273b562736abdd1c812d172237
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827759"
---
# <a name="copy-data-from-an-sap-table-by-using-azure-data-factory"></a>Gegevens uit een SAP-tabel kopiëren met behulp van Azure Data Factory

In dit artikel bevat een overzicht over het gebruik van de kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een SAP-tabel. Zie voor meer informatie, [overzicht Kopieeractiviteit](copy-activity-overview.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit een SAP-tabel naar een ondersteunde sink-gegevensopslag kopiëren. Zie voor een lijst van de gegevensarchieven die worden ondersteund als gegevensbronnen of PUT voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Deze tabel SAP connector ondersteunt name:

- Kopiëren van gegevens uit een SAP-tabel in:

  - SAP ERP centraal onderdeel (SAP ECC) versie 7.01 of hoger (in een recente SAP ondersteuning voor pakket-Stack die zijn uitgebracht na 2015).
  - SAP Business Warehouse (SAP BW) versie 7.01 of hoger.
  - SAP S/4HANA.
  - Andere producten in SAP Business Suite 7.01 of hoger.

- Kopiëren van gegevens uit zowel een transparante SAP-tabel, een gegroepeerde tabel een geclusterde tabel en een weergave.
- Kopiëren van gegevens met behulp van basisverificatie of Secure Network Communications (SNC), als SNC is geconfigureerd.
- Verbinding maken met een SAP-toepassingsserver of SAP-berichtenserver.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van deze connector SAP-tabel, moet u naar:

- Instellen van een zelf-hostende integratieruntime (versie 3.17 of hoger). Zie voor meer informatie, [maken en configureren van een zelf-hostende integratieruntime](create-self-hosted-integration-runtime.md).

- Download de 64-bits [SAP-Connector voor Microsoft .NET 3.0](https://support.sap.com/en/product/connectors/msnet.html) van de SAP-website, en installeer deze op de zelf-hostende integration runtime-machine. Tijdens de installatie, zorg ervoor dat u selecteert de **assembly's installeren in GAC** optie in de **optionele installatiestappen uit** venster.

  ![SAP-Connector installeren voor .NET](./media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

- De SAP-gebruiker die wordt gebruikt in de Data Factory SAP-connector voor tabel moet de volgende machtigingen hebben:

  - Autorisatie voor het gebruik van de externe functie aanroepen (RFC) bestemmingen.
  - Machtigingen voor de activiteit uitvoeren van het S_SDSAUTH autorisatie-object.

## <a name="get-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van de specifieke Data Factory-entiteiten met de connector SAP-tabel.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor de SAP BW Open Hub gekoppelde service:

| Eigenschap | Description | Verplicht |
|:--- |:--- |:--- |
| `type` | De `type` eigenschap moet worden ingesteld op `SapTable`. | Ja |
| `server` | De naam van de server waarop de SAP-instantie zich bevindt.<br/>Gebruiken om te verbinden met een SAP-toepassingsserver. | Nee |
| `systemNumber` | Het systeemnummer van de SAP-systeem.<br/>Gebruiken om te verbinden met een SAP-toepassingsserver.<br/>Toegestane waarde: Een decimaal getal van twee cijfers weergegeven als een tekenreeks. | Nee |
| `messageServer` | De hostnaam van de SAP-berichtenserver.<br/>Verbinding maken met een SAP-berichtenserver gebruiken. | Nee |
| `messageServerService` | De servicenaam van de of de poort van de berichtenserver.<br/>Verbinding maken met een SAP-berichtenserver gebruiken. | Nee |
| `systemId` | De ID van de SAP-systeem waarin de tabel zich bevindt.<br/>Verbinding maken met een SAP-berichtenserver gebruiken. | Nee |
| `logonGroup` | De groep van de aanmelding voor de SAP-systeem.<br/>Verbinding maken met een SAP-berichtenserver gebruiken. | Nee |
| `clientId` | De ID van de client in de SAP-systeem.<br/>Toegestane waarde: Een decimaal getal van drie cijfers wordt weergegeven als een tekenreeks. | Ja |
| `language` | De taal die gebruikmaakt van de SAP-systeem.<br/>Standaardwaarde is `EN`.| Nee |
| `userName` | De naam van de gebruiker die toegang tot de SAP-server heeft. | Ja |
| `password` | Het wachtwoord voor de gebruiker. Markeer dit veld met de `SecureString` type voor het veilig opslaan in de Data Factory of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| `sncMode` | De SNC activering indicator voor toegang tot de SAP-server waar de tabel zich bevindt.<br/>Als u wilt gebruiken van SNC verbinding maken met de SAP-server gebruiken.<br/>Toegestane waarden zijn `0` (uitgeschakeld, de standaardinstelling) of `1` (aan). | Nee |
| `sncMyName` | SNC-naam van de initiator voor toegang tot de SAP-server waar de tabel zich bevindt.<br/>Is van toepassing wanneer `sncMode` is ingeschakeld. | Nee |
| `sncPartnerName` | De SNC-naam van de communicatie-partner toegang tot de SAP-server waar de tabel zich bevindt.<br/>Is van toepassing wanneer `sncMode` is ingeschakeld. | Nee |
| `sncLibraryPath` | Van het product van de externe beveiliging-bibliotheek voor toegang tot de SAP-server waar de tabel zich bevindt.<br/>Is van toepassing wanneer `sncMode` is ingeschakeld. | Nee |
| `sncQop` | Het niveau van de kwaliteit van bescherming SNC om toe te passen.<br/>Is van toepassing wanneer `sncMode` is ingeschakeld. <br/>Toegestane waarden zijn `1` (verificatie), `2` (integriteit), `3` (Privacy), `8` (standaard), `9` (Maximum). | Nee |
| `connectVia` | De [integratieruntime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. Een zelf-hostende integratieruntime is vereist, zoals eerder in vermeld [vereisten](#prerequisites). |Ja |

**Voorbeeld 1: Verbinding maken met een SAP-toepassingsserver**

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client ID>",
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

### <a name="example-2-connect-to-an-sap-message-server"></a>Voorbeeld 2: Verbinding maken met een SAP-berichtenserver

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "messageServer": "<message server name>",
            "messageServerService": "<service name or port>",
            "systemId": "<system ID>",
            "logonGroup": "<logon group>",
            "clientId": "<client ID>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            }
        },
        "connectVia": {
            "referenceName": "<name of integration runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="example-3-connect-by-using-snc"></a>Voorbeeld 3: Verbinding maken met behulp van SNC

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client ID>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            },
            "sncMode": 1,
            "sncMyName": "<SNC myname>",
            "sncPartnerName": "<SNC partner name>",
            "sncLibraryPath": "<SNC library path>",
            "sncQop": "8"
        },
        "connectVia": {
            "referenceName": "<name of integration runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van de secties en de eigenschappen voor het definiëren van gegevenssets, [gegevenssets](concepts-datasets-linked-services.md). De volgende sectie bevat een lijst van de eigenschappen die worden ondersteund door de SAP-table-gegevensset.

Als u wilt kopiëren van gegevens van en naar de service SAP BW Hub openen die is gekoppeld, worden de volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Verplicht |
|:--- |:--- |:--- |
| `type` | De `type` eigenschap moet worden ingesteld op `SapTableResource`. | Ja |
| `tableName` | De naam van de SAP-tabel om te kopiëren van gegevens uit. | Ja |

### <a name="example"></a>Voorbeeld

```json
{
    "name": "SAPTableDataset",
    "properties": {
        "type": "SapTableResource",
        "linkedServiceName": {
            "referenceName": "<SAP table linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<SAP table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en de eigenschappen voor het definiëren van activiteiten, [pijplijnen](concepts-pipelines-activities.md). De volgende sectie bevat een lijst van de eigenschappen die worden ondersteund door de bron van de SAP-tabel.

### <a name="sap-table-as-a-source"></a>SAP-tabel als een bron

Om gegevens te kopiëren uit een SAP-tabel, worden de volgende eigenschappen worden ondersteund:

| Eigenschap                         | Description                                                  | Verplicht |
| :------------------------------- | :----------------------------------------------------------- | :------- |
| `type`                             | De `type` eigenschap moet worden ingesteld op `SapTableSource`.         | Ja      |
| `rowCount`                         | Het aantal rijen moeten worden opgehaald.                              | Nee       |
| `rfcTableFields`                   | De velden (kolommen) om te kopiëren uit de SAP-tabel. Bijvoorbeeld `column0, column1`. | Nee       |
| `rfcTableOptions`                  | De opties voor het filteren van de rijen in een SAP-tabel. Bijvoorbeeld `COLUMN0 EQ 'SOMEVALUE'`. Zie ook de tabel van SAP-query-operator verderop in dit artikel. | Nee       |
| `customRfcReadTableFunctionModule` | Een aangepaste functie module voor RFC die kan worden gebruikt om gegevens te lezen uit een SAP-tabel.<br>U kunt een aangepaste module voor de RFC-functie gebruiken om te definiëren hoe de gegevens worden opgehaald uit uw SAP-systeem en geretourneerd naar de Data Factory. De module voor aangepaste functies moet beschikken over een interface geïmplementeerd (import, export, tabellen) die vergelijkbaar is met `/SAPDS/RFC_READ_TABLE2`, dit is de standaard-interface die door Data Factory worden gebruikt. | Nee       |
| `partitionOption`                  | Het mechanisme voor partitie te lezen uit een SAP-tabel. De ondersteunde opties zijn onder andere: <ul><li>`None`</li><li>`PartitionOnInt` (normaal geheel getal of een gehele getallen met nul opvulling aan de linkerkant, zoals `0000012345`)</li><li>`PartitionOnCalendarYear` (4 cijfers in de indeling "YYYY")</li><li>`PartitionOnCalendarMonth` (6 cijfers in de notatie "JJJJMM")</li><li>`PartitionOnCalendarDate` (8 cijfers in de notatie "JJJJMMDD")</li></ul> | Nee       |
| `partitionColumnName`              | De naam van de kolom die wordt gebruikt voor het partitioneren van de gegevens.                | Nee       |
| `partitionUpperBound`              | De maximale waarde van de kolom die is opgegeven `partitionColumnName` dat wordt gebruikt om door te gaan met partities. | Nee       |
| `partitionLowerBound`              | De minimumwaarde van de kolom die is opgegeven `partitionColumnName` dat wordt gebruikt om door te gaan met partities. | Nee       |
| `maxPartitionsNumber`              | Het maximale aantal partities splitsen van de gegevens in.     | Nee       |

>[!TIP]
>Als uw SAP-tabel een groot volume aan gegevens, zoals meerdere miljard rijen heeft, gebruikt u `partitionOption` en `partitionSetting` wilt de gegevens opsplitsen in kleinere partities. In dit geval de gegevens worden gelezen per partitie en elke partitie wordt opgehaald uit uw SAP-server via één RFC-aanroep.<br/>
<br/>
>Duurt `partitionOption` als `partitionOnInt` bijvoorbeeld het aantal rijen in elke partitie met deze formule wordt berekend: (totaal aantal rijen die zich tussen `partitionUpperBound` en `partitionLowerBound`) /`maxPartitionsNumber`.<br/>
<br/>
>Als u wilt partities om te kopiëren versnellen parallel worden uitgevoerd, wordt aangeraden maken `maxPartitionsNumber` een veelvoud zijn van de waarde van de `parallelCopies` eigenschap. Zie voor meer informatie, [kopie parallelle](copy-activity-performance.md#parallel-copy).

In `rfcTableOptions`, kunt u de volgende algemene SAP standaardoperators voor query's voor het filteren van de rijen:

| Operator | Description |
| :------- | :------- |
| `EQ` | Gelijk aan |
| `NE` | Niet gelijk aan |
| `LT` | Kleiner dan |
| `LE` | Kleiner dan of gelijk aan |
| `GT` | Groter dan |
| `GE` | Groter dan of gelijk aan |
| `LIKE` | As in `LIKE 'Emma%'` |

### <a name="example"></a>Voorbeeld

```json
"activities":[
    {
        "name": "CopyFromSAPTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP table input dataset name>",
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

## <a name="data-type-mappings-for-an-sap-table"></a>Toewijzingen voor de tabel van een SAP-gegevenstype

Wanneer u gegevens uit een SAP-tabel kopieert, worden de volgende toewijzingen uit de gegevenstypen van de SAP-tabel gebruikt om de Azure Data Factory tussentijdse gegevenstypen. Zie voor meer informatie hoe de kopieerbewerking het schema en de gegevens van een brontype toegewezen aan de sink [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md).

| SAP ABAP-Type | Data Factory tussentijdse gegevenstype |
|:--- |:--- |
| `C` (String) | `String` |
| `I` (Geheel getal) | `Int32` |
| `F` (Gegevenstype met drijvende komma) | `Double` |
| `D` (Datum) | `String` |
| `T` (Tijd) | `String` |
| `P` (BCD verpakt, valuta, decimaal, hoeveelheid) | `Decimal` |
| `N` (Numeriek) | `String` |
| `X` (Binaire en onbewerkte) | `String` |

## <a name="next-steps"></a>Volgende stappen

Zie voor een lijst van de gegevensarchieven die door de kopieeractiviteit in Azure Data Factory ondersteund als bronnen en sinks, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
