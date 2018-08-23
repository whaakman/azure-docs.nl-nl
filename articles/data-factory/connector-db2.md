---
title: Gegevens kopiëren naar DB2 met Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van DB2 naar ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: jingwang
ms.openlocfilehash: f9d1d2181649cf24784dc7ad11638946c9ee4406
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "42058725"
---
# <a name="copy-data-from-db2-by-using-azure-data-factory"></a>Gegevens kopiëren van DB2 met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-onprem-db2-connector.md)
> * [Huidige versie](connector-db2.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een DB2-database. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit een DB2-database kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies ondersteunt deze DB2-connector de volgende IBM DB2-platformen en versies met gedistribueerde relationele Database architectuur (DRDA) SQL Access Manager (SQLAM) versie 9, 10 en 11:

* IBM DB2 voor 11.1 z/OS
* IBM DB2 voor z/OS 10.1
* IBM DB2 voor ik 7.2
* IBM DB2 voor ik 7.1
* IBM DB2 voor LUW 11
* IBM DB2 voor LUW 10,5
* IBM DB2 voor LUW 10.1

> [!TIP]
> Als u een foutbericht weergegeven dat ontvangt 'het pakket overeenkomt met de aanvraag voor een SQL-instructie uitvoeren is niet gevonden. SQLSTATE 51002 SQLCODE =-805 = ", de reden is het benodigde pakket is niet gemaakt voor normale gebruiker op dit besturingssysteem. Volg deze instructies op basis van het type van de DB2:
> - DB2 voor i (AS400): zodat de gebruiker van power-verzameling voor de aangemelde gebruiker voordat u met de kopieeractiviteit maken. Opdracht: `create collection <username>`
> - DB2 voor z/OS of LUW: gebruik een account van hoge bevoegdheid - power-gebruiker of beheerder met pakket-instanties en VERBINDT, BINDADD, verlenen uitvoeren naar openbare machtigingen - eenmaal met de kopieeractiviteit wordt uitgevoerd en vervolgens het benodigde pakket wordt automatisch gemaakt tijdens het kopiëren van. U kunt daarna Ga terug naar de normale gebruiker voor het volgende exemplaar wordt uitgevoerd.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van kopiëren van gegevens uit een DB2-database die niet openbaar toegankelijk is, moet u voor het instellen van een zelfgehoste Cloudintegratieruntime. Zie voor meer informatie over de zelf-hostende integratieruntimes, [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md) artikel. De Integration Runtime biedt een ingebouwde stuurprogramma voor DB2, dus hoeft u voor het installeren van een stuurprogramma handmatig bij het kopiëren van gegevens uit een DB2.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor DB2-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor DB2 gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **Db2** | Ja |
| server |Naam van de DB2-server. U kunt het volgende op de naam van de server gescheiden door puntkomma's, bijvoorbeeld poortnummer opgeven `server:port`. |Ja |
| database |De naam van de DB2-database. |Ja |
| authenticationType |Het type verificatie gebruikt voor verbinding met de DB2-database.<br/>De waarde is toegestaan: **Basic**. |Ja |
| gebruikersnaam |Geef de naam van de gebruiker verbinding maakt met de DB2-database. |Ja |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de zelfgehoste Cloudintegratieruntime of Azure Integration Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk zijn is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

**Voorbeeld:**

```json
{
    "name": "Db2LinkedService",
    "properties": {
        "type": "Db2",
        "typeProperties": {
            "server": "<servername:port>",
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de DB2-gegevensset.

Om gegevens te kopiëren uit een DB2, stel de eigenschap type van de gegevensset in **RelationalTable**. De volgende eigenschappen worden ondersteund:

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

Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door DB2-bron.

### <a name="db2-as-source"></a>DB2 als bron

Om gegevens te kopiëren uit een DB2, stelt u het brontype in de kopieeractiviteit naar **RelationalSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **RelationalSource** | Ja |
| query | Gebruik de aangepaste SQL-query om gegevens te lezen. Bijvoorbeeld: `"query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""`. | Nee (als de 'tableName' in de gegevensset is opgegeven) |

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

## <a name="data-type-mapping-for-db2"></a>Toewijzing voor DB2-gegevenstype

Bij het kopiëren van gegevens uit een DB2, worden de volgende toewijzingen van gegevenstypen DB2 gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe copy activity in het schema en de gegevens van een brontype aan de sink toegewezen.

| Type van de DB2-Database | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| BigInt |Int64 |
| Binair bestand |Byte[] |
| Blob |Byte[] |
| CHAR |Reeks |
| CLOB |Reeks |
| Date |Datum en tijd |
| DB2DynArray |Reeks |
| DbClob |Reeks |
| decimaal |decimaal |
| DecimalFloat |decimaal |
| Double-waarde |Double-waarde |
| Float |Double-waarde |
| Afbeelding |Reeks |
| Geheel getal |Int32 |
| LongVarBinary |Byte[] |
| LongVarChar |Reeks |
| LongVarGraphic |Reeks |
| Numeriek |decimaal |
| Real |Enkelvoudig |
| SmallInt |Int16 |
| Time |TimeSpan |
| Tijdstempel |DateTime |
| VarBinary |Byte[] |
| VarChar |Reeks |
| VarGraphic |Reeks |
| Xml |Byte[] |


## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
