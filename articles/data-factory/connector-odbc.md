---
title: Gegevens kopiëren van ODBC-bronnen met behulp van Azure Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens uit OData-bronnen voor ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/19/2018
ms.author: jingwang
ms.openlocfilehash: c51804748e4313d79cc3a369b659974d2d32e2e2
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54014264"
---
# <a name="copy-data-from-and-to-odbc-data-stores-using-azure-data-factory"></a>Gegevens kopiëren van en naar ODBC-gegevensopslag met Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-odbc-connector.md)
> * [Huidige versie](connector-odbc.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren van en naar een ODBC-gegevensopslag. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens kopiëren van ODBC-gegevensbron naar een ondersteunde sink-gegevensopslag of kopiëren van een ondersteund brongegevensarchief naar een ODBC-sink. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies deze ODBC-connector ondersteunt de kopiëren van gegevens van/naar **alle gegevensarchieven ODBC-compatibele** met behulp van **Basic** of **anoniem** verificatie.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van deze ODBC-connector, moet u naar:

- Instellen van een zelfgehoste Cloudintegratieruntime. Zie [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md) artikel voor meer informatie.
- Installeer het ODBC-stuurprogramma voor de gegevensopslag op de machine Integration Runtime.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met ODBC-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor ODBC-gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **ODBC** | Ja |
| connectionString | De verbindingsreeks met uitzondering van het gedeelte van de referentie. U kunt de verbindingsreeks opgeven met een patroon zoals `"Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;"`, of gebruik de systeem-DSN (gegevensbronnaam) u op de machine Integration Runtime met instellen `"DSN=<name of the DSN on IR machine>;"` (u moet nog steeds opgeven het gedeelte van de referentie in de gekoppelde service dienovereenkomstig).<br>Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md).| Ja |
| authenticationType | Het type verificatie gebruikt voor verbinding met de ODBC-gegevensopslag.<br/>Toegestane waarden zijn: **Basic** en **anonieme**. | Ja |
| Gebruikersnaam | Geef de gebruikersnaam op als u basisverificatie gebruikt. | Nee |
| wachtwoord | Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). | Nee |
| referenties | De access-referentie-gedeelte van de verbindingsreeks die is opgegeven in de indeling van de eigenschap / waarde-specifieke stuurprogramma's. Voorbeeld: `"RefreshToken=<secret refresh token>;"`. Dit veld markeert als een SecureString. | Nee |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. Er is een zelfgehoste Cloudintegratieruntime vereist zoals vermeld in [vereisten](#prerequisites). |Ja |

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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund voor ODBC-gegevensset.

Om gegevens te kopiëren van/naar compatibel is met ODBC-gegevensopslag, stel de eigenschap type van de gegevensset in **RelationalTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **RelationalTable** | Ja |
| tableName | Naam van de tabel in de ODBC-gegevensopslag. | Nee voor bron (als 'query' in de activiteitbron is opgegeven);<br/>Ja voor sink |

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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de ODBC-gegevensbron.

### <a name="odbc-as-source"></a>ODBC als bron

Om gegevens te kopiëren uit ODBC-compatibele data store, stelt u het brontype in de kopieeractiviteit naar **RelationalSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **RelationalSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"SELECT * FROM MyTable"`. | Nee (als de 'tableName' in de gegevensset is opgegeven) |

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

Om gegevens te kopiëren met ODBC compatibele data store, stelt u het sink-type in de kopieeractiviteit naar **OdbcSink**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **sink** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de kopie-activiteit-sink moet worden ingesteld op: **OdbcSink** | Ja |
| writeBatchTimeout |Wachttijd voor de batch insert bewerking is voltooid voordat er een optreedt time-out.<br/>Toegestane waarden zijn: timespan. Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |
| WriteBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt.<br/>Toegestane waarden zijn: geheel getal (aantal rijen). |Nee (standaard is 0 - automatisch gedetecteerd) |
| preCopyScript |Geef een SQL-query voor de Kopieeractiviteit om uit te voeren voordat het schrijven van gegevens in het gegevensarchief in elke uitvoering. U kunt deze eigenschap gebruiken voor het opschonen van de vooraf geladen gegevens. |Nee |

> [!NOTE]
> Voor 'writeBatchSize', als deze niet ingesteld (automatische detectie), kopieeractiviteit eerst wordt gedetecteerd of het stuurprogramma biedt ondersteuning voor batchbewerkingen, en stel deze in op 10000 als dit het geval of ingesteld op 1 als dat niet het geval. Als u de andere waarde dan 0 expliciet instelt, worden de kopieeractiviteit zich houdt aan de waarde en mislukt tijdens runtime als het stuurprogramma biedt geen ondersteuning voor batchbewerkingen.

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

U kunt gegevens kopiëren uit IBM Informix-database met behulp van de algemene ODBC-connector.

Instellen van een zelfgehoste Integration Runtime op een computer met toegang tot uw gegevensarchief. De Integration Runtime maakt gebruik van het ODBC-stuurprogramma voor Informix verbinding maken met het gegevensarchief. Installeer het stuurprogramma daarom als deze nog niet is geïnstalleerd op dezelfde computer. U kunt bijvoorbeeld stuurprogramma 'IBM INFORMIX ODBC-stuurprogramma (64-bits)'. Zie [vereisten](#prerequisites) sectie voor meer informatie.

Voordat u de Informix-bron in een Data Factory-oplossing gebruikt, moet u controleren of de Integration Runtime verbinding kan maken met de gegevensopslag met behulp van instructies in [verbindingsproblemen oplossen](#troubleshoot-connectivity-issues) sectie.

Maak een ODBC-gekoppelde service om een koppeling van een IBM Informix-gegevensarchief naar een Azure data factory zoals wordt weergegeven in het volgende voorbeeld:

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

Lees het artikel vanaf het begin voor een gedetailleerd overzicht van het gebruik van ODBC-gegevens worden opgeslagen als bron/sink-gegevensopslag in een kopieerbewerking.

## <a name="microsoft-access-source"></a>Microsoft Access-bron

U kunt gegevens kopiëren vanuit Microsoft Access-database met behulp van de algemene ODBC-connector.

Instellen van een zelfgehoste Integration Runtime op een computer met toegang tot uw gegevensarchief. De Integration Runtime maakt gebruik van het ODBC-stuurprogramma voor Microsoft Access verbinding maken met het gegevensarchief. Installeer het stuurprogramma daarom als deze nog niet is geïnstalleerd op dezelfde computer. Zie [vereisten](#prerequisites) sectie voor meer informatie.

Voordat u de Microsoft Access-bron in een Data Factory-oplossing gebruikt, moet u controleren of de Integration Runtime verbinding kan maken met de gegevensopslag met behulp van instructies in [verbindingsproblemen oplossen](#troubleshoot-connectivity-issues) sectie.

Maak een ODBC-gekoppelde service om te koppelen van een Microsoft Access-database aan een Azure data factory, zoals wordt weergegeven in het volgende voorbeeld:

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

Lees het artikel vanaf het begin voor een gedetailleerd overzicht van het gebruik van ODBC-gegevens worden opgeslagen als bron/sink-gegevensopslag in een kopieerbewerking.

## <a name="sap-hana-sink"></a>SAP HANA-sink

>[!NOTE]
>Als u wilt kopiëren van gegevens van SAP HANA-gegevensarchief, verwijzen naar native [SAP HANA-connector](connector-sap-hana.md). Volg deze instructies om ODBC-connector te gebruiken om gegevens te kopiëren met SAP HANA. De gekoppelde services voor SAP HANA-connector en ODBC-connector met een ander type zijn Opmerking dus kan niet opnieuw worden gebruikt.
>

U kunt gegevens kopiëren met SAP HANA-database met behulp van de algemene ODBC-connector.

Instellen van een zelfgehoste Integration Runtime op een computer met toegang tot uw gegevensarchief. De Integration Runtime maakt gebruik van het ODBC-stuurprogramma voor SAP HANA verbinding maken met het gegevensarchief. Installeer het stuurprogramma daarom als deze nog niet is geïnstalleerd op dezelfde computer. Zie [vereisten](#prerequisites) sectie voor meer informatie.

Voordat u de SAP HANA-sink in een Data Factory-oplossing gebruikt, moet u controleren of de Integration Runtime verbinding kan maken met de gegevensopslag met behulp van instructies in [verbindingsproblemen oplossen](#troubleshoot-connectivity-issues) sectie.

Maak een ODBC-gekoppelde service om een koppeling van een SAP HANA-gegevensarchief naar een Azure data factory zoals wordt weergegeven in het volgende voorbeeld:

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

Lees het artikel vanaf het begin voor een gedetailleerd overzicht van het gebruik van ODBC-gegevens worden opgeslagen als bron/sink-gegevensopslag in een kopieerbewerking.

## <a name="troubleshoot-connectivity-issues"></a>Verbindingsproblemen oplossen

Voor het oplossen van problemen met verbindingen, gebruikt u de **Diagnostics** tabblad van **Integration Runtime Configuration Manager**.

1. Start **Integration Runtime Configuration Manager**.
2. Schakel over naar de **Diagnostics** tabblad.
3. Selecteer onder de sectie 'Test Connection' de **type** opslaan van gegevens (gekoppelde service).
4. Geef de **verbindingsreeks** die wordt gebruikt om verbinding maken met het gegevensarchief, kiest u de **verificatie** en voer **gebruikersnaam**, **wachtwoord**, en/of **referenties**.
5. Klik op **verbinding testen** voor het testen van de verbinding met het gegevensarchief.

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
