---
title: Gegevens kopiëren van SAP Business Warehouse via Open-Hub met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het kopiëren van gegevens van SAP Business Warehouse (BW) via Open-Hub aan ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: jingwang
ms.openlocfilehash: c64842dc89c9519c738701558f510940f4cc148d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58103907"
---
# <a name="copy-data-from-sap-business-warehouse-via-open-hub-using-azure-data-factory"></a>Gegevens kopiëren van SAP Business Warehouse via Open-Hub met behulp van Azure Data Factory

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een SAP Business Warehouse (BW) via Open-Hub. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens van SAP Business Warehouse via Open Hub kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Deze connector SAP Business Warehouse Open Hub ondersteunt name:

- SAP Business Warehouse **versie 7.01 of hoger (in een recente SAP ondersteuning voor pakket-Stack die na het jaar 2015 zijn uitgebracht)**.
- Kopiëren van gegevens via Open Hub lokale doeltabel die onder DSO, InfoCube, MultiProvider, DataSource, enzovoort.
- Kopiëren van gegevens met behulp van basisverificatie.
- Verbinding maken met toepassingsserver.

## <a name="sap-bw-open-hub-integration"></a>SAP BW Open Hub-integratie 

[SAP BW Open Hub Service](https://wiki.scn.sap.com/wiki/display/BI/Overview+of+Open+Hub+Service) is een efficiënte manier gegevens te extraheren uit SAP BW. Het volgende diagram toont een van de typische stromen die klanten in hun SAP-systeem hebben, in welk geval gegevensstromen van SAP ECC -> PSA -> DSO kubus ->.

SAP BW Open Hub bestemming (OHD) definieert het doel waarnaar de SAP-gegevens wordt doorgegeven. Eventuele objecten die wordt ondersteund door SAP Data Transfer proces (DTP) kunnen worden gebruikt als open hub-gegevensbronnen, bijvoorbeeld DSO, InfoCube, DataSource, enzovoort. Open type van de bestemming van de Hub - waar de relayed gegevens worden opgeslagen - databasetabellen (lokaal of extern) kan worden en platte bestanden. Deze connector SAP BW Open Hub ondersteuning het kopiëren van gegevens uit de lokale tabel OHD in BW. Als u van andere typen gebruikmaakt, kunt u rechtstreeks verbinding maken met de database of bestand system met behulp van andere connectors.

![SAP BW Open Hub](./media/connector-sap-business-warehouse-open-hub/sap-bw-open-hub.png)

## <a name="delta-extraction-flow"></a>Delta-extractie stroom

ADF SAP BW Open Hub Connector biedt twee optionele eigenschappen: `excludeLastRequest` en `baseRequestId` die kan worden gebruikt voor het afhandelen van delta-laden van Open-Hub. 

- **excludeLastRequestId**: Of u wilt uitsluiten van de records van de laatste aanvraag. Standaardwaarde is true. 
- **baseRequestId**: De ID van de aanvraag voor de delta-laden. Zodra deze is ingesteld, kunt u alleen gegevens met aanvraag-id groter is dan de waarde van deze eigenschap wordt opgehaald. 

Over het algemeen bestaat de extractie van InfoProviders SAP op Azure Data Factory (ADF) uit 2 stappen: 

1. **SAP BW Data Transfer proces (DTP)** in deze stap worden de gegevens uit een SAP BW InfoProvider gekopieerd naar een tabel met SAP BW Open Hub 

1. **Het kopiëren van gegevens ADF** In deze stap in de tabel Open Hub kan worden gelezen door de ADF-Connector 

![Delta-extractie stroom](media/connector-sap-business-warehouse-open-hub/delta-extraction-flow.png)

In de eerste stap van wordt een DTP uitgevoerd. Elke uitvoering wordt gemaakt van een nieuwe SAP-aanvraag-ID. De aanvraag-ID is opgeslagen in de tabel Open Hub en wordt vervolgens gebruikt door de ADF-connector voor het identificeren van de verschillen. De twee stappen asynchroon uitvoeren: de DTP wordt geactiveerd door SAP en het kopiëren van de ADF-gegevens wordt geactiveerd door ADF. 

Standaard ADF is niet lezen van de meest recente delta uit de tabel Open Hub (de optie "laatste aanvraag uitsluiten" is true). De gegevens in ADF is hierbij, niet 100% up-to-date zijn met de gegevens in de tabel van de Open Hub (de laatste delta ontbreekt). Hierna deze procedure zorgt ervoor dat er geen gegevensrijen veroorzaakt door de asynchrone extractie ophalen verloren. Het werkt prima, zelfs wanneer ADF de tabel Open Hub lezen is terwijl de DTP nog steeds schrijven in dezelfde tabel is. 

U doorgaans opslaan de maximale gekopieerde aanvraag-ID in de laatste uitvoering door ADF in een tijdelijke gegevensopslag (zoals Azure Blob in bovenstaande diagram). Daarom is dezelfde aanvraag niet gelezen een tweede keer door ADF in de volgende uitvoering. In de tussentijd zorgen, Let op: de gegevens wordt niet automatisch verwijderd uit de tabel Open Hub.

Voor de juiste delta is behandeling van het niet toegestaan dat de aanvraag-id's van verschillende DTPs in dezelfde tabel Open Hub. Daarom moet u niet maken meer dan één DTP voor elke Open Hub bestemming (OHD). Wanneer u volledige statistieken en Deltastatistieken extractie van de dezelfde InfoProvider, moet u twee OHDs maken voor de dezelfde InfoProvider. 

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van deze connector SAP Business Warehouse Open Hub, moet u naar:

- Instellen van een zelfgehoste Integration Runtime met versie 3.13 of hoger. Zie [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.

- Download de **64-bits [SAP .NET Connector 3.0](https://support.sap.com/en/product/connectors/msnet.html)**  van de SAP-website, en op de machine zelfgehoste IR installeren. Wanneer u installeert, in het instellingenvenster voor optionele stappen Zorg ervoor dat u selecteert de **assembly's installeren in GAC** optie zoals wordt weergegeven in de volgende afbeelding. 

    ![SAP .NET Connector installeren](./media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

- SAP-gebruiker wordt gebruikt in de Data Factory BW-connector moet de volgende bevoegdheden hebben: 

    - Autorisatie voor RFC en SAP BW. 
    - Machtigingen voor de activiteit "Uitvoeren" van autorisatie-Object 'S_SDSAUTH'.

- Maken van SAP Open Hub doeltype als **databasetabel** met 'Technische Key'-optie is ingeschakeld.  Het is ook raadzaam de gegevens verwijderen verlaten van de tabel als dit selectievakje is uitgeschakeld, maar dit niet vereist is. Maak gebruik van de DTP (rechtstreeks uitvoeren of integreren met bestaande procesketen) op grond van gegevens uit het bronobject (zoals kubus) die u hebt gekozen voor de doeltabel open hub.

## <a name="getting-started"></a>Aan de slag

> [!TIP]
>
> Zie voor een overzicht van het gebruik van SAP BW Open Hub connector [laden van gegevens van SAP Business Warehouse (BW) met behulp van Azure Data Factory](load-sap-bw-data.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Open Hub van SAP Business Warehouse-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor SAP Business Warehouse Open Hub gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **SapOpenHub** | Ja |
| server | Naam van de server waarop de SAP BW-instantie zich bevindt. | Ja |
| systemNumber | Het systeemnummer van de SAP BW-systeem.<br/>Toegestane waarde: decimaal getal van twee cijfers weergegeven als een tekenreeks. | Ja |
| ClientId | Client-ID van de client in het W SAP-systeem.<br/>Toegestane waarde: decimaal getal van drie cijfers wordt weergegeven als een tekenreeks. | Ja |
| language | De taal die gebruikmaakt van de SAP-systeem. | Nee (standaardwaarde is **EN**)|
| Gebruikersnaam | De naam van de gebruiker die toegang tot de SAP-server heeft. | Ja |
| wachtwoord | Het wachtwoord voor de gebruiker. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. Er is een zelfgehoste Cloudintegratieruntime vereist zoals vermeld in [vereisten](#prerequisites). |Ja |

**Voorbeeld:**

```json
{
    "name": "SapBwOpenHubLinkedService",
    "properties": {
        "type": "SapOpenHub",
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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets](concepts-datasets-linked-services.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Salesforce-gegevensset.

Als u wilt kopiëren van gegevens van en naar SAP BW Open Hub, stel de eigenschap type van de gegevensset in **SapOpenHubTable**. De volgende eigenschappen worden ondersteund.

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **SapOpenHubTable**.  | Ja |
| openHubDestinationName | De naam van de Open Hub bestemming gegevens kopiëren naar. | Ja |
| excludeLastRequest | Of u wilt uitsluiten van de records van de laatste aanvraag. | Nee (de standaardwaarde is **waar**) |
| baseRequestId | De ID van de aanvraag voor de delta-laden. Zodra deze is ingesteld, alleen gegevens met aanvraag-id **groter is dan** de waarde van deze eigenschap wordt opgehaald.  | Nee |

>[!TIP]
>Als uw tabel Open Hub alleen de gegevens die worden gegenereerd door één aanvraag-ID bevat, bijvoorbeeld, u altijd volledig geladen en overschrijven de bestaande gegevens in de tabel, of u alleen de DTP eenmaal voor testen, uitvoeren moet u Schakel de optie 'excludeLastRequest' om te kopiëren van de d ATA uit.

**Voorbeeld:**

```json
{
    "name": "SAPBWOpenHubDataset",
    "properties": {
        "type": "SapOpenHubTable",
        "linkedServiceName": {
            "referenceName": "<SAP BW Open Hub linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "openHubDestinationName": "<open hub destination name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door SAP BW Open Hub bron.

### <a name="sap-bw-open-hub-as-source"></a>SAP BW Open Hub als bron

Om gegevens te kopiëren van SAP BW Open Hub, stelt u het brontype in de kopieeractiviteit naar **SapOpenHubSource**. Er zijn geen extra typespecifieke-eigenschappen die nodig zijn in de kopieeractiviteit **bron** sectie.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromSAPBWOpenHub",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP BW Open Hub input dataset name>",
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
                "type": "SapOpenHubSource"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-bw-open-hub"></a>De gegevenstypetoewijzing voor SAP BW Open Hub

Het kopiëren van gegevens van SAP BW Open Hub, worden de volgende toewijzingen van SAP BW-gegevenstypen gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe copy activity in het schema en de gegevens van een brontype aan de sink toegewezen.

| SAP ABAP-Type | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| C (String) | String |
| I (integer) | Int32 |
| F (drijvende komma) | Double |
| D (Date) | String |
| T (Time) | String |
| P (BCD verpakt, valuta, decimaal, hoeveelheid) | Decimal |
| N (Numc) | String |
| X (binaire en onbewerkte) | String |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats).
