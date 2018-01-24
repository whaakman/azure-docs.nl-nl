---
title: "Gegevens kopiëren van ODBC-bronnen met behulp van Azure Data Factory | Microsoft Docs"
description: "Ontdek hoe u gegevens uit OData-bronnen naar gegevensarchieven ondersteunde sink kopiëren met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn."
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
ms.openlocfilehash: 14f654979f004186e81b2f18578ced3c9aab3815
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-and-to-odbc-data-stores-using-azure-data-factory"></a>Gegevens kopiëren van en naar gegevensarchieven ODBC met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-odbc-connector.md)
> * [Versie 2 - Preview](connector-odbc.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar een ODBC-gegevensarchief. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [ODBC-connector in V1](v1/data-factory-odata-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens kopiëren van ODBC-gegevensbron naar een ondersteunde sink-gegevensarchief of kopiëren van een ondersteunde brongegevensarchief naar ODBC sink. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder kopiëren van gegevens van/naar biedt ondersteuning voor deze connector ODBC **een ODBC-compatibele gegevensarchieven** met **Basic** of **anoniem** verificatie.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van deze connector ODBC, moet u:

- Stel een Self-hosted integratie-Runtime. Zie [Self-hosted integratie Runtime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.
- Het ODBC-stuurprogramma voor de gegevensopslag op de machine Runtime-integratie installeren.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met ODBC-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor ODBC-gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Odbc** | Ja |
| connectionString | De verbindingstekenreeks met uitzondering van het gedeelte referentie. U kunt de verbindingsreeks opgeven met patroon zoals `"Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;"`, of gebruik de systeem-DSN (gegevensbronnaam) die u hebt ingesteld op de machine Runtime-integratie met `"DSN=<name of the DSN on IR machine>;"` (u moet nog steeds opgeven het gedeelte referenties in de gekoppelde service dienovereenkomstig).| Ja |
| authenticationType | Het soort verificatie gebruikt voor verbinding met het beheerprogramma voor ODBC-gegevensarchief.<br/>Toegestane waarden zijn: **Basic** en **anoniem**. | Ja |
| userName | Geef de gebruikersnaam als u basisverificatie gebruikt. | Nee |
| wachtwoord | Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. Dit veld markeren als een SecureString. | Nee |
| referentie | Het gedeelte van de referentie toegang van de verbindingsreeks die is opgegeven in de indeling van de eigenschapswaarde specifieke stuurprogramma's. Voorbeeld: `"RefreshToken=<secret refresh token>;"`. Dit veld markeren als een SecureString. | Nee |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. Een Runtime Self-hosted-integratie is vereist zoals vermeld in [vereisten](#prerequisites). |Ja |

**Voorbeeld 1: met behulp van basisverificatie**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "<connection string>"
            },
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

**Voorbeeld 2: anonieme verificatie gebruikt**

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "<connection string>"
            },
            "authenticationType": "Anonymous",
            "credential": {
                "type": "SecureString",
                "value": "RefreshToken=<secret refresh token>;"
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door het ODBC-gegevensset.

Om gegevens te kopiëren van/met ODBC compatibele gegevensopslag, stel de eigenschap type van de gegevensset **RelationalTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **RelationalTable** | Ja |
| tableName | De naam van de tabel in het beheerprogramma voor ODBC-gegevensarchief. | Er is geen voor bron (als 'vragen' in de activiteitbron is opgegeven);<br/>Ja voor sink |

**Voorbeeld**

```json
{
    "name": "ODBCDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<ODBC linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door het ODBC-gegevensbron.

### <a name="odbc-as-source"></a>ODBC als bron

Om gegevens te kopiëren uit ODBC compatibele gegevensopslag, stelt u het brontype in de kopieeractiviteit om te **RelationalSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **RelationalSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM MyTable"`. | Nee (als 'tableName' in de gegevensset is opgegeven) |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ODBC input dataset name>",
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
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="odbc-as-sink"></a>ODBC als sink

Om gegevens te kopiëren met ODBC compatibele gegevensopslag, stelt u het sink-type in de kopieerbewerking naar **OdbcSink**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **sink** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de activiteit kopiëren sink moet worden ingesteld op: **OdbcSink** | Ja |
| writeBatchTimeout |Wachttijd voor de batch-insert-bewerking te voltooien voordat er een optreedt time-out.<br/>Toegestane waarden zijn: timespan. Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |
| writeBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt.<br/>Toegestane waarden zijn: geheel getal (aantal rijen). |Nee (standaardwaarde is 0 - automatisch gedetecteerd) |
| preCopyScript |Geef een SQL-query voor de Kopieeractiviteit worden uitgevoerd voordat het schrijven van gegevens in de gegevensopslag in elke uitvoering. U kunt deze eigenschap gebruiken om de vooraf geladen gegevens op te schonen. |Nee |

> [!NOTE]
> Voor 'writeBatchSize' als deze niet is ingesteld (automatische detectie), detecteert kopieeractiviteit eerst of het stuurprogramma batchbewerkingen, ondersteunt en stel deze in op 10000 als dat zo is, of ingesteld op 1, dat niet het geval. Als u expliciet de andere waarde dan 0 instelt, worden de kopieeractiviteit zich houdt aan de waarde en mislukt tijdens runtime als batchbewerkingen biedt geen ondersteuning voor het stuurprogramma.

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToODBC",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ODBC output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OdbcSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

## <a name="ibm-informix-source"></a>IBM Informix-bron

U kunt gegevens uit met de algemene ODBC-connector voor IBM Informix-database kopiëren.

Stel een Self-hosted integratie Runtime op een computer met toegang tot uw data store. Het ODBC-stuurprogramma voor Informix de Runtime-integratie gebruikt om verbinding met de gegevensopslag. Installeer het stuurprogramma daarom als deze nog niet is geïnstalleerd op dezelfde machine. U kunt bijvoorbeeld stuurprogramma 'IBM INFORMIX ODBC-stuurprogramma (64-bits)'. Zie [vereisten](#prerequisites) sectie voor meer informatie.

Voordat u met de Informix-bron in een Data Factory-oplossing, moet u controleren of de Runtime-integratie verbinding kan maken met de gegevensopslag van instructies in [oplossen van problemen met de netwerkverbinding](#troubleshoot-connectivity-issues) sectie.

Maak een service gekoppelde ODBC-om een IBM Informix data store koppelen aan een Azure data factory, zoals wordt weergegeven in het volgende voorbeeld:

```json
{
    "name": "InformixLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "<Informix connection string or DSN>"
            },
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

Lees het artikel vanaf het begin voor een gedetailleerd overzicht van het gebruik van ODBC-gegevens worden opgeslagen als de bron/sink gegevensarchieven een kopieerbewerking.

## <a name="microsoft-access-source"></a>Microsoft Access-bron

U kunt gegevens kopiëren van Microsoft Access-database met de algemene ODBC-connector.

Stel een Self-hosted integratie Runtime op een computer met toegang tot uw data store. Het ODBC-stuurprogramma voor Microsoft Access de Runtime-integratie gebruikt om verbinding met de gegevensopslag. Installeer het stuurprogramma daarom als deze nog niet is geïnstalleerd op dezelfde machine. Zie [vereisten](#prerequisites) sectie voor meer informatie.

Voordat u de Microsoft Access-bron in een Data Factory-oplossing gebruiken, moet u controleren of de Runtime-integratie verbinding kan maken met de gegevensopslag van instructies in [oplossen van problemen met de netwerkverbinding](#troubleshoot-connectivity-issues) sectie.

Maak een ODBC-gekoppelde service voor Microsoft Access-database koppelen aan een Azure data factory, zoals wordt weergegeven in het volgende voorbeeld:

```json
{
    "name": "MicrosoftAccessLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Driver={Microsoft Access Driver (*.mdb, *.accdb)};Dbq=<path to your DB file e.g. C:\\mydatabase.accdb>;"
            },
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

Lees het artikel vanaf het begin voor een gedetailleerd overzicht van het gebruik van ODBC-gegevens worden opgeslagen als de bron/sink gegevensarchieven een kopieerbewerking.

## <a name="ge-historian-source"></a>GE Historian bron

U kunt gegevens kopiëren van GE Historian met de algemene ODBC-connector.

Stel een Self-hosted integratie Runtime op een computer met toegang tot uw data store. Het ODBC-stuurprogramma voor GE Historian de Runtime-integratie gebruikt om verbinding met de gegevensopslag. Installeer het stuurprogramma daarom als deze nog niet is geïnstalleerd op dezelfde machine. Zie [vereisten](#prerequisites) sectie voor meer informatie.

Voordat u de bron GE Historian in een Data Factory-oplossing gebruiken, moet u controleren of de Runtime-integratie verbinding kan maken met de gegevensopslag van instructies in [oplossen van problemen met de netwerkverbinding](#troubleshoot-connectivity-issues) sectie.

Maak een ODBC-gekoppelde service voor Microsoft Access-database koppelen aan een Azure data factory, zoals wordt weergegeven in het volgende voorbeeld:

```json
{
    "name": "HistorianLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "<GE Historian store connection string or DSN>"
            },
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

Lees het artikel vanaf het begin voor een gedetailleerd overzicht van het gebruik van ODBC-gegevens worden opgeslagen als de bron/sink gegevensarchieven een kopieerbewerking.

## <a name="sap-hana-sink"></a>SAP HANA sink

>[!NOTE]
>Om gegevens te kopiëren uit SAP HANA-gegevensopslag, verwijzen naar systeemeigen [SAP HANA-connector](connector-sap-hana.md). Volg deze instructies om ODBC-connector te gebruiken om gegevens te kopiëren naar een SAP HANA. De gekoppelde services voor SAP HANA-connector en ODBC-connector zijn met een ander type dus kan niet opnieuw worden gebruikt.
>

U kunt gegevens kopiëren naar SAP HANA-database met de algemene ODBC-connector.

Stel een Self-hosted integratie Runtime op een computer met toegang tot uw data store. Het ODBC-stuurprogramma voor SAP HANA de Runtime-integratie gebruikt om verbinding met de gegevensopslag. Installeer het stuurprogramma daarom als deze nog niet is geïnstalleerd op dezelfde machine. Zie [vereisten](#prerequisites) sectie voor meer informatie.

Voordat u de SAP HANA-sink in een Data Factory-oplossing gebruiken, moet u controleren of de Runtime-integratie verbinding kan maken met de gegevensopslag van instructies in [oplossen van problemen met de netwerkverbinding](#troubleshoot-connectivity-issues) sectie.

Maak een ODBC-gekoppelde service voor een SAP HANA-gegevensarchief koppelen aan een Azure data factory, zoals wordt weergegeven in het volgende voorbeeld:

```json
{
    "name": "SAPHANAViaODBCLinkedService",
    "properties": {
        "type": "Odbc",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Driver={HDBODBC};servernode=<HANA server>.clouddatahub-int.net:30015"
            },
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

Lees het artikel vanaf het begin voor een gedetailleerd overzicht van het gebruik van ODBC-gegevens worden opgeslagen als de bron/sink gegevensarchieven een kopieerbewerking.

## <a name="troubleshoot-connectivity-issues"></a>Verbindingsproblemen oplossen

Gebruik voor het oplossen van problemen met de **Diagnostics** tabblad van **integratie Runtime Configuration Manager**.

1. Start **integratie Runtime Configuration Manager**.
2. Overschakelen naar de **Diagnostics** tabblad.
3. Selecteer onder de sectie 'Verbinding testen' de **type** gegevens opslaan (gekoppelde service).
4. Geef de **verbindingsreeks** die wordt gebruikt om verbinding maken met de gegevensopslag, kiest u de **verificatie** en voer **gebruikersnaam**, **wachtwoord**, en/of **referenties**.
5. Klik op **verbinding testen** voor het testen van de verbinding met het gegevensarchief.

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).