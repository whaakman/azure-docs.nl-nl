---
title: Gegevens kopiëren van Cassandra met Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens kopiëren van Cassandra naar ondersteunde sink-gegevensopslag met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: jingwang
ms.openlocfilehash: 743dad6032547f8f535543413adff416efb56ac0
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57998390"
---
# <a name="copy-data-from-cassandra-using-azure-data-factory"></a>Gegevens kopiëren van Cassandra met Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](v1/data-factory-onprem-cassandra-connector.md)
> * [Huidige versie](connector-cassandra.md)

In dit artikel bevat een overzicht over het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een Cassandra-database. Dit is gebaseerd op de [overzicht kopieeractiviteit](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit de Cassandra-database kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen/put door de kopieeractiviteit, de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

Om precies ondersteunt deze Cassandra-connector:

- Cassandra **versie 2.x en 3.x**.
- Kopiëren van gegevens met **Basic** of **anoniem** verificatie.

>[!NOTE]
>Voor de activiteit die wordt uitgevoerd op zelfgehoste Cloudintegratieruntime, Cassandra 3.x wordt ondersteund sinds IR versie 3.7 en hoger.

## <a name="prerequisites"></a>Vereisten

Om gegevens te kopiëren uit een Cassandra-database die niet openbaar toegankelijk is, moet u voor het instellen van een zelfgehoste Cloudintegratieruntime. Zie [zelfgehoste Cloudintegratieruntime](create-self-hosted-integration-runtime.md) artikel voor meer informatie. De Integration Runtime biedt een ingebouwd Cassandra-stuurprogramma, dus u hoeft voor het installeren van een stuurprogramma handmatig bij het kopiëren van gegevens van/naar Cassandra.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten meer informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met Cassandra-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Cassandra gekoppelde service:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op: **Cassandra** |Ja |
| host |Een of meer IP-adressen of hostnamen van de Cassandra-servers.<br/>Geef een door komma's gescheiden lijst met IP-adressen of hostnamen gelijktijdig verbinding maken met alle servers. |Ja |
| poort |De TCP-poort die gebruikmaakt van de Cassandra-server om te luisteren naar clientverbindingen. |Nee (de standaardinstelling is 9042) |
| authenticationType | Het type verificatie gebruikt voor verbinding met de Cassandra-database.<br/>Toegestane waarden zijn: **Basic**, en **anonieme**. |Ja |
| gebruikersnaam |Geef de gebruikersnaam op voor het gebruikersaccount. |Ja, als authenticationType is ingesteld op het Basislidmaatschap. |
| wachtwoord |Wachtwoord voor het gebruikersaccount opgeven. Dit veld markeren als een SecureString Bewaar deze zorgvuldig in Data Factory, of [verwijzen naar een geheim opgeslagen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja, als authenticationType is ingesteld op het Basislidmaatschap. |
| connectVia | De [Integration Runtime](concepts-integration-runtime.md) moet worden gebruikt verbinding maken met het gegevensarchief. U kunt de zelfgehoste Cloudintegratieruntime of Azure Integration Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk zijn is). Als niet is opgegeven, wordt de standaard Azure Integration Runtime. |Nee |

>[!NOTE]
>Verbinding met Cassandra met behulp van SSL wordt momenteel niet ondersteund.

**Voorbeeld:**

```json
{
    "name": "CassandraLinkedService",
    "properties": {
        "type": "Cassandra",
        "typeProperties": {
            "host": "<host>",
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

Zie het artikel gegevenssets voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Cassandra-gegevensset.

Als u wilt kopiëren van gegevens van Cassandra, stel de eigenschap type van de gegevensset in **CassandraTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **CassandraTable** | Ja |
| keyspace |De naam van de keyspace of het schema in Cassandra-database. |Nee (als 'query' voor 'CassandraSource' is opgegeven) |
| tableName |De naam van de tabel in de Cassandra-database. |Nee (als 'query' voor 'CassandraSource' is opgegeven) |

**Voorbeeld:**

```json
{
    "name": "CassandraDataset",
    "properties": {
        "type": "CassandraTable",
        "linkedServiceName": {
            "referenceName": "<Cassandra linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "keySpace": "<keyspace name>",
            "tableName": "<table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit


Zie voor een volledige lijst van de secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de Cassandra-bron.

### <a name="cassandra-as-source"></a>Cassandra als bron

Als u wilt kopiëren van gegevens van Cassandra, stelt u het brontype in de kopieeractiviteit naar **CassandraSource**. De volgende eigenschappen worden ondersteund in de kopieeractiviteit **bron** sectie:

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **CassandraSource** | Ja |
| query |De aangepaste query gebruiken om gegevens te lezen. SQL-92 query of CQL-query. Zie [CQL-verwijzing](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Wanneer u SQL-query gebruikt, geef **keyspace name.table naam** om weer te geven van de tabel die u wilt zoeken. |Nee (als de 'tableName' en 'keyspace' in de gegevensset zijn opgegeven). |
| consistencyLevel |Het consistentieniveau Hiermee geeft u het aantal replica's moeten reageren op een leesaanvraag alvorens gegevens naar de clienttoepassing. Cassandra wordt het opgegeven aantal replica's voor gegevens om te voldoen aan de leesaanvraag gecontroleerd. Zie [gegevensconsistentie configureren](https://docs.datastax.com/en/cassandra/2.1/cassandra/dml/dml_config_consistency_c.html) voor meer informatie.<br/><br/>Toegestane waarden zijn: **EEN**, **twee**, **drie**, **QUORUM**, **alle**, **LOCAL_QUORUM**, **EACH_QUORUM**, en **LOCAL_ONE**. |Nee (de standaardwaarde is `ONE`) |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromCassandra",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Cassandra input dataset name>",
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
                "type": "CassandraSource",
                "query": "select id, firstname, lastname from mykeyspace.mytable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-cassandra"></a>Toewijzing voor Cassandra-gegevenstype

Het kopiëren van gegevens van Cassandra, worden de volgende toewijzingen van Cassandra-gegevenstypen gebruikt om Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe copy activity in het schema en de gegevens van een brontype aan de sink toegewezen.

| Cassandra-gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| ASCII |String |
| BIGINT |Int64 |
| BLOB |Byte[] |
| BOOLEAANSE WAARDE |Booleaans |
| DECIMAL |Decimal |
| DOUBLE |Double |
| FLOAT |Single |
| INET |String |
| INT |Int32 |
| TEKST |String |
| TIMESTAMP |DateTime |
| TIMEUUID |Guid |
| UUID |Guid |
| VARCHAR |String |
| VARINT |Decimal |

> [!NOTE]
> Voor de verzameling typen (kaart, set, lijst, enzovoort), verwijzen naar [werken met de Cassandra-verzamelingtypen met behulp van de virtuele tabel](#work-with-collections-using-virtual-table) sectie.
>
> Gebruiker gedefinieerde typen worden niet ondersteund.
>
> De lengte van binaire kolom en een kolom met tekenreeksen lengte mag niet groter zijn dan 4000.
>

## <a name="work-with-collections-using-virtual-table"></a>Werken met verzamelingen met behulp van de virtuele tabel

Azure Data Factory maakt gebruik van een ingebouwde ODBC-stuurprogramma's verbinding maken met en gegevens kopiëren van de Cassandra-database. Voor verzamelingtypen met inbegrip van de kaart, instellen en lijst, renormalizes het stuurprogramma de gegevens in de bijbehorende virtuele tabellen. Als een tabel een verzameling kolommen bevat, genereert het stuurprogramma met name in de volgende virtuele tabellen:

* Een **basistabel**, die dezelfde gegevens als de echte tabel, met uitzondering van de verzameling kolommen bevat. De basistabel gebruikt dezelfde naam als de echte tabel die wordt vertegenwoordigd.
* Een **virtuele tabel** voor elke kolom verzameling die de geneste gegevens wordt uitgebreid. De virtuele tabellen die staan voor verzamelingen worden genoemd met de naam van de echte tabel, een scheidingsteken "*vt*' en de naam van de kolom.

Virtuele tabellen verwijzen naar de gegevens in de echte tabel, waardoor het stuurprogramma voor toegang tot de Gedenormaliseerde gegevens. Zie de sectie Voorbeeld voor meer informatie. U kunt toegang tot de inhoud van de Cassandra-verzamelingen door het uitvoeren van query's en deze koppelen aan de virtuele tabellen.

### <a name="example"></a>Voorbeeld

De volgende "ExampleTable" wordt bijvoorbeeld een Cassandra-database-tabel met een geheel getal zijn primaire-sleutelkolom met de naam 'pk_int', een tekstkolom de benoemde waarde, een lijstkolom, een kaart-kolom en een set-kolom (met de naam 'StringSet').

| pk_int | Value | Lijst | Kaart | StringSet |
| --- | --- | --- | --- | --- |
| 1 |"sample value 1" |["1", "2", "3"] |{"S1": "a", "S2": "b"} |{"A", "B", "C"} |
| 3 |"sample value 3" |["100", "101", "102", "105"] |{"S1": "t"} |{"A", "E"} |

Het stuurprogramma genereert meerdere virtuele tabellen om weer te geven deze enkele tabel. De refererende-sleutelkolommen in de virtuele tabellen verwijzen naar de primaire-sleutelkolommen in de echte tabel en welke rij echte tabel een rij in de virtuele tabel komt met overeen geven.

De eerste virtuele tabel is de basistabel met de naam 'ExampleTable' wordt weergegeven in de volgende tabel: 

| pk_int | Value |
| --- | --- |
| 1 |"sample value 1" |
| 3 |"sample value 3" |

De basistabel bevat dezelfde gegevens als de oorspronkelijke databasetabel, met uitzondering van de verzamelingen die zijn weggelaten uit deze tabel en uitgevouwen in andere virtuele tabellen.

De volgende tabellen ziet u de virtuele tabellen die de gegevens uit de lijst, toewijzen en StringSet kolommen opnieuw normaliseren. De kolommen met namen die eindigen op '_index' of '_key' geven de positie van de gegevens binnen de oorspronkelijke lijst of een kaart. De kolommen met namen die met "_Waarde eindigen" bevatten de uitgebreide gegevens uit de verzameling.

**Tabel 'ExampleTable_vt_List':**

| pk_int | List_index | List_value |
| --- | --- | --- |
| 1 |0 |1 |
| 1 |1 |2 |
| 1 |2 |3 |
| 3 |0 |100 |
| 3 |1 |101 |
| 3 |2 |102 |
| 3 |3 |103 |

**Tabel 'ExampleTable_vt_Map':**

| pk_int | Map_key | Map_value |
| --- | --- | --- |
| 1 |S1 |A |
| 1 |S2 |b |
| 3 |S1 |t |

**Tabel 'ExampleTable_vt_StringSet':**

| pk_int | StringSet_value |
| --- | --- |
| 1 |A |
| 1 |B |
| 1 |C |
| 3 |A |
| 3 |E |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven die worden ondersteund als bronnen en sinks door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).
