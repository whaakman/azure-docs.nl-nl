---
title: Gegevens verplaatsen van Cassandra gebruik Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit een on-premises Cassandra-database met behulp van Azure Data Factory.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 085cc312-42ca-4f43-aa35-535b35a102d5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: c8f61cb165b0bfffe2f42b060cdbd666fff3a8b3
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Verplaatsen van gegevens uit een on-premises Cassandra-database met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-onprem-cassandra-connector.md)
> * [Versie 2 - Preview](../connector-cassandra.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [Cassandra-connector in V2](../connector-cassandra.md).

In dit artikel wordt uitgelegd hoe de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van een on-premises Cassandra database gebruiken. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, hetgeen een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit toont.

U kunt gegevens uit een on-premises Cassandra-gegevensopslag kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven als PUT wordt ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. Data factory ondersteunt momenteel alleen zwevend gegevens uit een gegevensopslag Cassandra tot andere gegevensarchieven, maar niet voor het verplaatsen van gegevens uit andere gegevensarchieven met Cassandra gegevensopslag. 

## <a name="supported-versions"></a>Ondersteunde versies
De connector Cassandra ondersteunt de volgende versies van Cassandra: 2.X.

## <a name="prerequisites"></a>Vereisten
Voor de Azure Data Factory-service kunnen verbinding maken met uw lokale Cassandra-database, moet u een Data Management Gateway installeren op dezelfde computer die als host fungeert voor de database of op een afzonderlijke computer om te voorkomen dat concurrentie voor resources met de database. Data Management Gateway is een onderdeel dat on-premises gegevensbronnen met cloud-services op een manier veilig en beheerd verbindt. Zie [Data Management Gateway](data-factory-data-management-gateway.md) voor meer informatie over Data Management Gateway. Zie [gegevens verplaatsen van on-premises naar cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies over het instellen van de gateway een gegevens-pijplijn om gegevens te verplaatsen.

Verbinding maken met een database Cassandra, zelfs als de database wordt gehost in de cloud, bijvoorbeeld op een virtuele machine van Azure IaaS moet u de gateway. Y u de gateway kan hebben op dezelfde virtuele machine die als host fungeert voor de database of op een afzonderlijke virtuele machine zo lang als de gateway verbinding kan maken met de database.  

Wanneer u de gateway installeert, installeert deze automatisch een verbinding maken met database Cassandra gebruikt Microsoft Cassandra ODBC-stuurprogramma. Daarom hoeft u niet elk stuurprogramma handmatig installeren op de gatewaycomputer bij het kopiëren van gegevens uit de database Cassandra. 

> [!NOTE]
> Zie [gateway problemen](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) voor tips over het oplossen van verbinding of gateway gerelateerde problemen.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens vanuit een on-premises Cassandra-gegevensopslag verplaatst met behulp van verschillende hulpprogramma's voor API's. 

- De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren. 
- U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren:

1. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory.
2. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een on-premises Cassandra-gegevensopslag [JSON-voorbeeld: gegevens kopiëren van Cassandra naar Azure Blob](#json-example-copy-data-from-cassandra-to-azure-blob) sectie van dit artikel. 

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar een gegevensopslag Cassandra:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor Cassandra gekoppelde service.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OnPremisesCassandra** |Ja |
| host |Een of meer IP-adressen of hostnamen van Cassandra servers.<br/><br/>Geef een door komma's gescheiden lijst met IP-adressen of hostnamen gelijktijdig verbinding maken met alle servers. |Ja |
| poort |De TCP-poort die de Cassandra-server gebruikt om te luisteren naar verbindingen van clients. |Nee, de standaardwaarde: 9042 |
| authenticationType |Basic- of anonieme |Ja |
| gebruikersnaam |Geef de gebruikersnaam voor het gebruikersaccount. |Ja, als authenticationType is ingesteld op Basic. |
| wachtwoord |Wachtwoord voor het gebruikersaccount opgeven. |Ja, als authenticationType is ingesteld op Basic. |
| gatewayName |De naam van de gateway die wordt gebruikt voor verbinding met de lokale Cassandra-database. |Ja |
| encryptedCredential |De referentie is versleuteld met de gateway. |Nee |

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets van de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle gegevensset typen (Azure SQL, Azure blob-, Azure-tabel, enz.).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **CassandraTable** heeft de volgende eigenschappen

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| met keyspace |Naam van de keyspace of het schema in Cassandra-database. |Ja (als **query** voor **CassandraSource** is niet gedefinieerd). |
| tableName |Naam van de tabel in Cassandra-database. |Ja (als **query** voor **CassandraSource** is niet gedefinieerd). |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten van de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle typen activiteiten.

Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren met elk activiteitstype. Voor de kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put.

Wanneer de bron is van het type **CassandraSource**, de volgende eigenschappen beschikbaar zijn in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |Gebruik de aangepaste query om gegevens te lezen. |SQL-92 query of CQL query. Zie [CQL verwijzing](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Wanneer u SQL-query gebruikt, geef **keyspace name.table naam** om weer te geven van de tabel die u wilt zoeken. |Nee (als tableName en keyspace op gegevensset zijn gedefinieerd). |
| consistencyLevel |Het consistentieniveau van de geeft het aantal replica's moeten reageren op een leesaanvraag voor het retourneren van gegevens naar de clienttoepassing. Cassandra controleert het opgegeven aantal replica's voor gegevens om te voldoen aan de leesaanvraag. |EEN, TWEE, DRIE, QUORUM, ALL, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE. Zie [gegevensconsistentie configureren](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) voor meer informatie. |Nee. Standaardwaarde is een. |

## <a name="json-example-copy-data-from-cassandra-to-azure-blob"></a>JSON-voorbeeld: gegevens kopiëren van Cassandra naar Azure Blob
In dit voorbeeld bevat definities van de voorbeeld-JSON die u een pijplijn maken kunt met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Er wordt weergegeven hoe gegevens uit een on-premises Cassandra database kopiëren naar een Azure Blob-opslag. Gegevens kunnen echter worden gekopieerd naar een van de PUT vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

> [!IMPORTANT]
> Dit voorbeeld bevat JSON-fragmenten. Dit omvat geen stapsgewijze instructies voor het maken van de gegevensfactory. Zie [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies.

Het voorbeeld heeft de volgende data factory-entiteiten:

* Een gekoppelde service van het type [OnPremisesCassandra](#linked-service-properties).
* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [CassandraTable](#dataset-properties).
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [CassandraSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

**Cassandra gekoppelde service:**

In dit voorbeeld wordt de **Cassandra** gekoppelde service. Zie [Cassandra gekoppelde service](#linked-service-properties) sectie voor de eigenschappen die worden ondersteund door deze gekoppelde service.  

```json
{
    "name": "CassandraLinkedService",
    "properties":
    {
        "type": "OnPremisesCassandra",
        "typeProperties":
        {
            "authenticationType": "Basic",
            "host": "mycassandraserver",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

**Gekoppelde Azure Storage-service:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
    "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

**Cassandra invoergegevensset:**

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "mykeyspace"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Instelling **externe** naar **true** informeert de Data Factory-service dat de dataset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

**Azure Blob-uitvoergegevensset:**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1).

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/fromcassandra"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**De kopieeractiviteit in een pijplijn met Cassandra bron en sink van Blob:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **CassandraSource** en **sink** type is ingesteld op **BlobSink**.

Zie [RelationalSource type-eigenschappen](#copy-activity-properties) voor de lijst met eigenschappen die ondersteund worden door de RelationalSource.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
        {
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra to an Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "CassandraInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
            "typeProperties": {
                "source": {
                    "type": "CassandraSource",
                    "query": "select id, firstname, lastname from mykeyspace.mytable"

                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }
        ]    
    }
}
```

### <a name="type-mapping-for-cassandra"></a>Toewijzing van het type voor Cassandra
| Type Cassandra | .NET op basis van Type |
| --- | --- |
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
>

## <a name="work-with-collections-using-virtual-table"></a>Werken met behulp van de virtuele tabel verzamelingen
Azure Data Factory gebruikt een ingebouwde ODBC-stuurprogramma voor het verbinding maken met en het kopiëren van gegevens uit de database Cassandra. Voor verzamelingtypen met inbegrip van de kaart, de verzameling en de lijst, renormalizes het stuurprogramma voor de gegevens in de bijbehorende virtuele tabellen. Als een tabel bevat de verzameling kolommen, genereert het stuurprogramma in het bijzonder de volgende virtuele tabellen:

* Een **basistabel**, die dezelfde gegevens als de echte tabel, met uitzondering van de verzameling kolommen bevat. De basistabel gebruikt dezelfde naam als de echte tabel die wordt vertegenwoordigd.
* Een **virtuele tabel** voor elke kolom collectie die geneste gegevens wordt uitgebreid. De virtuele tabellen waarbij verzamelingen worden benoemd met de naam van de echte tabel, een scheidingsteken '*vt*' en de naam van de kolom.

Virtuele tabellen verwijzen naar de gegevens in de echte tabel, waardoor het stuurprogramma voor toegang tot de gedenormaliseerd gegevens. Zie het voorbeeld gedeelte voor meer informatie. U kunt toegang tot de inhoud van Cassandra verzamelingen door het uitvoeren van query's en het toevoegen aan de virtuele tabellen.

U kunt de [Wizard kopiëren](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) intuïtief weergeven van de lijst met tabellen in Cassandra database, inclusief de virtuele tabellen en bekijk vooraf de gegevens in. U kunt ook maakt u een query in de Wizard kopiëren en valideren als u wilt het resultaat te bekijken.

### <a name="example"></a>Voorbeeld
De volgende 'ExampleTable' is bijvoorbeeld een databasetabel Cassandra die een geheel getal primaire-sleutelkolom met de naam 'pk_int', een tekstkolom benoemde waarde, een lijstkolom, een kaart kolom en een set-kolom (met de naam 'StringSet') bevat.

| pk_int | Waarde | Lijst | Kaart | StringSet |
| --- | --- | --- | --- | --- |
| 1 |'Voorbeeldwaarde 1' |["1", "2", "3"] |{"S1": "a", "S2": "b"} |{"A", "B", "C"} |
| 3 |"Voorbeeldwaarde 3" |["100", "101", "102", "105"] |{"S1": "t"} |{"A", "E"} |

Het stuurprogramma genereert meerdere virtuele tabellen te deze één tabel vertegenwoordigen. De refererende-sleutelkolommen in de virtuele-tabellen verwijzen naar de primaire-sleutelkolommen in de echte tabel en aangeven welke echte tabelrij de rij van de virtuele tabel overeen met.

De eerste virtuele tabel is de basistabel met de naam 'ExampleTable' wordt weergegeven in de volgende tabel. De basistabel bevat dezelfde gegevens als de oorspronkelijke databasetabel, met uitzondering van de verzamelingen die worden weggelaten uit deze tabel en uitgevouwen in andere virtuele tabellen.

| pk_int | Waarde |
| --- | --- |
| 1 |'Voorbeeldwaarde 1' |
| 3 |"Voorbeeldwaarde 3" |

De volgende tabellen tonen de virtuele tabellen die de gegevens uit de lijst, toewijzing en StringSet kolommen renormalize. De kolommen met namen die met '_index' of '_key eindigen' geven de positie van de gegevens in de oorspronkelijke lijst of de kaart aan. De kolommen met namen die met '_Waarde eindigen' bevatten de uitgebreide gegevens uit de verzameling.

#### <a name="table-exampletablevtlist"></a>Tabel 'ExampleTable_vt_List':
| pk_int | List_index | List_value |
| --- | --- | --- |
| 1 |0 |1 |
| 1 |1 |2 |
| 1 |2 |3 |
| 3 |0 |100 |
| 3 |1 |101 |
| 3 |2 |102 |
| 3 |3 |103 |

#### <a name="table-exampletablevtmap"></a>Tabel 'ExampleTable_vt_Map':
| pk_int | Map_key | Map_value |
| --- | --- | --- |
| 1 |S1 |A |
| 1 |S2 |b |
| 3 |S1 |t |

#### <a name="table-exampletablevtstringset"></a>Tabel 'ExampleTable_vt_StringSet':
| pk_int | StringSet_value |
| --- | --- |
| 1 |A |
| 1 |B |
| 1 |C |
| 3 |A |
| 3 |E |

## <a name="map-source-to-sink-columns"></a>Bron van de kaart opvangen kolommen
Zie voor meer informatie over het toewijzen van kolommen in gegevensset naar kolommen in gegevensset sink bron, [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Herhaalbare leesbewerking van relationele bronnen
Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, moet u herhaalbaarheid Houd in gedachten om te voorkomen dat ongewenste resultaten. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook een beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment opnieuw in beide gevallen wordt uitgevoerd, moet u ervoor zorgen dat dezelfde gegevens is gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable van relationele bronnen lezen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en het afstemmen
Zie [uitvoering van de activiteit & afstemmen handleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren.
