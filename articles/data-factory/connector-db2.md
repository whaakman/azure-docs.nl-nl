---
title: "Kopiëren van gegevens van DB2 met behulp van Azure Data Factory | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens van DB2 naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn."
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
ms.date: 
ms.author: jingwang
ms.openlocfilehash: a96de1814afc7947205a0dc7ed005f7cadff20bc
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-db2-by-using-azure-data-factory"></a>Gegevens kopiëren van DB2 met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-onprem-db2-connector.md)
> * [Versie 2 - Preview](connector-db2.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een DB2-database. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [DB2-connector in V1](v1/data-factory-onprem-db2-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit een DB2-database kopiëren naar een ondersteunde sink data store. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze DB2-connector de volgende IBM DB2-platforms en versies met gedistribueerd relationele Database architectuur (DRDA) SQL toegang Manager (SQLAM) versie 9, 10 en 11:

* IBM DB2 voor z/OS 11.1
* IBM DB2 voor z/OS 10.1
* IBM DB2 voor i 7.2
* IBM DB2 voor i 7.1
* IBM DB2 voor LUW 11
* IBM DB2 voor LUW 10.5
* IBM DB2 voor LUW 10.1

> [!TIP]
> Als u een waarin wordt aangegeven foutbericht "het pakket overeenkomt met de aanvraag voor een SQL-instructie uitvoeren is niet gevonden. SQLSTATE 51002 SQLCODE =-805 = ', de reden hiervoor is een benodigde pakket is niet gemaakt voor normale gebruiker op dit besturingssysteem. Volg deze instructies op basis van het type DB2:
> - DB2 voor i (AS400): laten hoofdgebruiker verzameling voor de aanmeldingsgebruiker voordat u kopieeractiviteit maken. Opdracht:`create collection <username>`
> - DB2 voor z-/ OS of LUW: een account met hoge bevoegdheden - Hoofdgebruikers- of -beheerder met pakket-instanties en BIND BINDADD, machtigingen verlenen uitvoeren voor openbare - met de kopieeractiviteit eenmaal uitvoeren en klik vervolgens het benodigde pakket wordt automatisch gemaakt tijdens het kopiëren. U kunt daarna terug naar de normale gebruiker schakelen voor uw volgende kopie wordt uitgevoerd.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van de gegevens kopiëren uit een DB2-database die niet openbaar toegankelijk is, moet u voor het instellen van een Self-hosted integratie-Runtime. Zie voor meer informatie over host zichzelf integratie runtimes, [Self-hosted integratie Runtime](create-self-hosted-integration-runtime.md) artikel. De Runtime-integratie biedt een ingebouwd DB2-stuurprogramma, dus u hoeft niet te handmatig installeren van een stuurprogramma bij het kopiëren van gegevens uit een DB2.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar DB2-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor DB2 gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Db2** | Ja |
| server |De naam van de DB2-server. |Ja |
| database |Naam van de DB2-database. |Ja |
| schema |De naam van het schema in de database. Naam van het schema is hoofdlettergevoelig. |Nee |
| authenticationType |Het soort verificatie die verbinding maken met de DB2-database wordt gebruikt.<br/>De waarde is toegestaan: **Basic**. |Ja |
| gebruikersnaam |Geef de naam van de gebruiker verbinding maken met de DB2-database. |Ja |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. Dit veld markeren als SecureString. |Ja |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Self-hosted integratie Runtime of Azure integratie Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

**Voorbeeld:**

```json
{
    "name": "Db2LinkedService",
    "properties": {
        "type": "Db2",
        "typeProperties": {
            "server": "<servername>",
            "database": "<dbname>",
            "authenticationType": "Basic",
            "username": "<username>",
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door DB2 dataset.

Om gegevens te kopiëren van DB2, stel de eigenschap type van de gegevensset **RelationalTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **RelationalTable** | Ja |
| tableName | De naam van de tabel in de DB2-database. | Nee (als 'query' in de activiteitbron is opgegeven) |

**Voorbeeld**

```json
{
    "name": "DB2Dataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<DB2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door DB2-bron.

### <a name="db2-as-source"></a>DB2 als bron

Om gegevens te kopiëren van DB2, stelt u het brontype in de kopieerbewerking naar **RelationalSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **RelationalSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""`. | Nee (als 'tableName' in de gegevensset is opgegeven) |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromDB2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<DB2 input dataset name>",
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
                "query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-db2"></a>Gegevenstype toewijzing voor DB2

Bij het kopiëren van gegevens uit een DB2, worden de volgende toewijzingen van gegevenstypen DB2 gebruikt om tussentijdse Azure Data Factory-gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

| Type DB2-Database | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| BigInt |Int64 |
| Binair bestand |Byte[] |
| Blob |Byte[] |
| CHAR |Tekenreeks |
| CLOB |Tekenreeks |
| Date |Datum en tijd |
| DB2DynArray |Tekenreeks |
| DbClob |Tekenreeks |
| Decimale |Decimale |
| DecimalFloat |Decimale |
| Double |Double |
| Float |Double |
| Afbeelding |Tekenreeks |
| Geheel getal |Int32 |
| LongVarBinary |Byte[] |
| LongVarChar |Tekenreeks |
| LongVarGraphic |Tekenreeks |
| Numeriek |Decimale |
| Real |Single |
| SmallInt |Int16 |
| Time |TimeSpan |
| Timestamp |Datum en tijd |
| VarBinary |Byte[] |
| VarChar |Tekenreeks |
| VarGraphic |Tekenreeks |
| Xml |Byte[] |


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).