---
title: "Gegevens kopiëren van Cassandra met behulp van Azure Data Factory | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens van Cassandra naar gegevensarchieven ondersteunde sink met behulp van een kopieeractiviteit in een Azure Data Factory-pijplijn."
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
ms.openlocfilehash: 5b6a2cde9bea3d3aba9262bb9446d54773cf0297
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-from-cassandra-using-azure-data-factory"></a>Gegevens kopiëren van Cassandra met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-onprem-cassandra-connector.md)
> * [Versie 2 - Preview](connector-cassandra.md)

In dit artikel bevat een overzicht van het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te kopiëren uit een database Cassandra. Dit is gebaseerd op de [activiteit overzicht kopiëren](copy-activity-overview.md) artikel met daarin een algemeen overzicht van de kopieeractiviteit.


> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Cassandra-connector in V1](v1/data-factory-onprem-cassandra-connector.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

U kunt gegevens uit Cassandra database kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst van opgeslagen gegevens die worden ondersteund als bronnen/put door met de kopieerbewerking de [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) tabel.

In het bijzonder ondersteunt deze connector Cassandra:

- Cassandra **versies 2.X**.
- Kopiëren van gegevens met **Basic** of **anoniem** verificatie.

## <a name="prerequisites"></a>Vereisten

Om gegevens te kopiëren uit een Cassandra-database die niet openbaar toegankelijk is, moet u voor het instellen van een Self-hosted integratie-Runtime. Zie [Self-hosted integratie Runtime](create-self-hosted-integration-runtime.md) artikel voor meer informatie. De Runtime-integratie biedt een ingebouwd Cassandra stuurprogramma, dus u hoeft niet te handmatig installeren van een stuurprogramma bij het kopiëren van gegevens van/naar Cassandra.

## <a name="getting-started"></a>Aan de slag

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over de eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar Cassandra-connector.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende eigenschappen worden ondersteund voor Cassandra gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op: **Cassandra** |Ja |
| host |Een of meer IP-adressen of hostnamen van Cassandra servers.<br/>Geef een door komma's gescheiden lijst met IP-adressen of hostnamen gelijktijdig verbinding maken met alle servers. |Ja |
| poort |De TCP-poort die de Cassandra-server gebruikt om te luisteren naar verbindingen van clients. |Nee (de standaardwaarde is 9042) |
| authenticationType | Het soort verificatie die verbinding maken met de database Cassandra wordt gebruikt.<br/>Toegestane waarden zijn: **Basic**, en **anoniem**. |Ja |
| gebruikersnaam |Geef de gebruikersnaam voor het gebruikersaccount. |Ja, als authenticationType is ingesteld op Basic. |
| wachtwoord |Wachtwoord voor het gebruikersaccount opgeven. Dit veld markeren als SecureString. |Ja, als authenticationType is ingesteld op Basic. |
| connectVia | De [integratie Runtime](concepts-integration-runtime.md) moeten worden gebruikt voor het verbinding maken met het gegevensarchief. U kunt Self-hosted integratie Runtime of Azure integratie Runtime gebruiken (als uw gegevensarchief openbaar toegankelijk). Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. |Nee |

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

Zie het artikel gegevenssets voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Cassandra dataset.

Stel de eigenschap type van de gegevensset om gegevens te kopiëren van Cassandra, **RelationalTable**. De volgende eigenschappen worden ondersteund:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de gegevensset moet worden ingesteld op: **CassandraTable** | Ja |
| met keyspace |Naam van de keyspace of het schema in Cassandra-database. |Nee (als 'query"voor 'CassandraSource' is opgegeven) |
| tableName |Naam van de tabel in Cassandra-database. |Nee (als 'query"voor 'CassandraSource' is opgegeven) |

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


Zie voor een volledige lijst met secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen](concepts-pipelines-activities.md) artikel. Deze sectie bevat een lijst met eigenschappen die ondersteund worden door Cassandra bron.

### <a name="cassandra-as-source"></a>Cassandra als bron

Om gegevens te kopiëren van Cassandra, stelt u het brontype in de kopieerbewerking naar **CassandraSource**. De volgende eigenschappen worden ondersteund in de kopieerbewerking **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron voor kopiëren-activiteit moet worden ingesteld op: **CassandraSource** | Ja |
| query |Gebruik de aangepaste query om gegevens te lezen. |SQL-92 query of CQL query. Zie [CQL verwijzing](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Wanneer u SQL-query gebruikt, geef **keyspace name.table naam** om weer te geven van de tabel die u wilt zoeken. |Nee (als 'tabelnaam' en 'keyspace' in de gegevensset zijn opgegeven). |
| consistencyLevel |Het consistentieniveau van de geeft het aantal replica's moeten reageren op een leesaanvraag voor het retourneren van gegevens naar de clienttoepassing. Cassandra controleert het opgegeven aantal replica's voor gegevens om te voldoen aan de leesaanvraag. Zie [gegevensconsistentie configureren](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) voor meer informatie.<br/><br/>Toegestane waarden zijn: **één**, **twee**, **drie**, **QUORUM**, **alle**, **LOCAL_ QUORUM**, **EACH_QUORUM**, en **LOCAL_ONE**. |Nee (standaardwaarde is `ONE`) |

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

## <a name="data-type-mapping-for-cassandra"></a>Gegevenstype toewijzing voor Cassandra

Bij het kopiëren van gegevens van Cassandra, worden de volgende toewijzingen van gegevenstypen Cassandra gebruikt voor Azure Data Factory tussentijdse gegevenstypen. Zie [Schema en de gegevens typt toewijzingen](copy-activity-schema-and-type-mapping.md) voor meer informatie over hoe het brontype schema en de gegevens in kopieeractiviteit worden toegewezen aan de sink.

| Cassandra gegevenstype | Data factory tussentijdse gegevenstype |
|:--- |:--- |
| ASCII |Tekenreeks |
| BIGINT |Int64 |
| BLOB |Byte[] |
| BOOLEAANSE WAARDE |Boole-waarde |
| DECIMAL |Decimale |
| DOUBLE |Double |
| FLOAT |Single |
| INET |Tekenreeks |
| INT |Int32 |
| TEXT |Tekenreeks |
| TIJDSTEMPEL |Datum en tijd |
| TIMEUUID |GUID |
| UUID |GUID |
| VARCHAR |Tekenreeks |
| VARINT |Decimale |

> [!NOTE]
> Voor de verzameling van typen (kaart, set, lijst, enzovoort), verwijzen naar [werken met Cassandra verzamelingtypen met behulp van de virtuele tabel](#work-with-collections-using-virtual-table) sectie.
>
> Gebruiker gedefinieerde typen worden niet ondersteund.
>
> De lengte van binaire kolom en String-lengte mag niet groter zijn dan 4000.
>

## <a name="work-with-collections-using-virtual-table"></a>Werken met behulp van de virtuele tabel verzamelingen

Azure Data Factory gebruikt een ingebouwde ODBC-stuurprogramma voor het verbinding maken met en het kopiëren van gegevens uit de database Cassandra. Voor verzamelingtypen met inbegrip van de kaart, de verzameling en de lijst, renormalizes het stuurprogramma voor de gegevens in de bijbehorende virtuele tabellen. Als een tabel bevat de verzameling kolommen, genereert het stuurprogramma in het bijzonder de volgende virtuele tabellen:

* Een **basistabel**, die dezelfde gegevens als de echte tabel, met uitzondering van de verzameling kolommen bevat. De basistabel gebruikt dezelfde naam als de echte tabel die wordt vertegenwoordigd.
* Een **virtuele tabel** voor elke kolom collectie die geneste gegevens wordt uitgebreid. De virtuele tabellen waarbij verzamelingen worden benoemd met de naam van de echte tabel, een scheidingsteken '*vt*' en de naam van de kolom.

Virtuele tabellen verwijzen naar de gegevens in de echte tabel, waardoor het stuurprogramma voor toegang tot de gedenormaliseerd gegevens. Zie het voorbeeld gedeelte voor meer informatie. U kunt toegang tot de inhoud van Cassandra verzamelingen door het uitvoeren van query's en het toevoegen aan de virtuele tabellen.

### <a name="example"></a>Voorbeeld

De volgende 'ExampleTable' is bijvoorbeeld een databasetabel Cassandra die een geheel getal primaire-sleutelkolom met de naam 'pk_int', een tekstkolom benoemde waarde, een lijstkolom, een kaart kolom en een set-kolom (met de naam 'StringSet') bevat.

| pk_int | Waarde | Lijst | Kaart | StringSet |
| --- | --- | --- | --- | --- |
| 1 |'Voorbeeldwaarde 1' |["1", "2", "3"] |{"S1": "a", "S2": "b"} |{"A", "B", "C"} |
| 3 |"Voorbeeldwaarde 3" |["100", "101", "102", "105"] |{"S1": "t"} |{"A", "E"} |

Het stuurprogramma genereert meerdere virtuele tabellen te deze één tabel vertegenwoordigen. De refererende-sleutelkolommen in de virtuele-tabellen verwijzen naar de primaire-sleutelkolommen in de echte tabel en aangeven welke echte tabelrij de rij van de virtuele tabel overeen met.

De eerste virtuele tabel is de basistabel met de naam 'ExampleTable' wordt weergegeven in de volgende tabel: 

| pk_int | Waarde |
| --- | --- |
| 1 |'Voorbeeldwaarde 1' |
| 3 |"Voorbeeldwaarde 3" |

De basistabel bevat dezelfde gegevens als de oorspronkelijke databasetabel, met uitzondering van de verzamelingen die worden weggelaten uit deze tabel en uitgevouwen in andere virtuele tabellen.

De volgende tabellen tonen de virtuele tabellen die de gegevens uit de lijst, toewijzing en StringSet kolommen renormalize. De kolommen met namen die met '_index' of '_key eindigen' geven de positie van de gegevens in de oorspronkelijke lijst of de kaart aan. De kolommen met namen die met '_Waarde eindigen' bevatten de uitgebreide gegevens uit de verzameling.

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

**Table "ExampleTable_vt_Map":**

| pk_int | Map_key | Map_value |
| --- | --- | --- |
| 1 |S1 |A |
| 1 |S2 |b |
| 3 |S1 |t |

**Table "ExampleTable_vt_StringSet":**

| pk_int | StringSet_value |
| --- | --- |
| 1 |A |
| 1 |B |
| 1 |C |
| 3 |A |
| 3 |E |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst met gegevensarchieven als bronnen en put wordt ondersteund door de kopieeractiviteit in Azure Data Factory, [ondersteunde gegevensarchieven](copy-activity-overview.md##supported-data-stores-and-formats).