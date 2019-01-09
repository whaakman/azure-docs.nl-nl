---
title: Gegevens verplaatsen van Cassandra met behulp van Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit een on-premises Cassandra-database met behulp van Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 085cc312-42ca-4f43-aa35-535b35a102d5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: dd90834a2e112effbfd6876b84dfe8b3ca87fcf3
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54015641"
---
# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Gegevens verplaatsen van een on-premises Cassandra-database met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van de Data Factory-service die u gebruikt:"]
> * [Versie 1:](data-factory-onprem-cassandra-connector.md)
> * [Versie 2 (huidige versie)](../connector-cassandra.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [Cassandra-connector in V2](../connector-cassandra.md).

In dit artikel wordt uitgelegd hoe u van de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van een on-premises Cassandra-database. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit geeft.

U kunt gegevens kopiëren naar een on-premises Cassandra-gegevensarchief naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als sink voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. Data factory ondersteunt momenteel alleen gegevens te verplaatsen van een Cassandra-gegevensarchief naar andere gegevensarchieven, maar niet voor het verplaatsen van gegevens uit andere gegevensarchieven naar een Cassandra-gegevensarchief. 

## <a name="supported-versions"></a>Ondersteunde versies
De Cassandra-connector ondersteunt de volgende versies van Cassandra: 2.x en 3.x. Voor de activiteit die wordt uitgevoerd op zelfgehoste Cloudintegratieruntime, Cassandra 3.x wordt ondersteund sinds IR versie 3.7 en hoger.

## <a name="prerequisites"></a>Vereisten
Voor de Azure Data Factory-service te kunnen verbinding maken met uw on-premises Cassandra-database, moet u een Data Management Gateway installeren op dezelfde computer die als host fungeert voor de database of op een afzonderlijke computer om te voorkomen en dingen om de resources met de database. Data Management Gateway is een onderdeel dat on-premises gegevensbronnen met cloud-services in een veilige, beheerde manier verbinding maakt. Zie [Data Management Gateway](data-factory-data-management-gateway.md) artikel voor meer informatie over Data Management Gateway. Zie [gegevens verplaatsen van on-premises naar de cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies over het instellen van de gateway van een pijplijn om gegevens te verplaatsen.

U moet de gateway verbinding maakt met een Cassandra-database, zelfs als de database wordt gehost in de cloud, bijvoorbeeld op een Azure IaaS-VM. Y u de gateway kan hebben op dezelfde virtuele machine die als host fungeert voor de database of op een afzonderlijke virtuele machine zo lang als de gateway kan verbinding maken met de database.  

Wanneer u de gateway installeert, installeert deze automatisch een Cassandra-ODBC-stuurprogramma dat is gebruikt voor verbinding met de Cassandra-database. Daarom moet u niet handmatig een stuurprogramma installeren op de gatewaycomputer bij het kopiëren van gegevens uit de Cassandra-database. 

> [!NOTE]
> Zie [oplossen van problemen met gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) voor tips over het oplossen van de verbindingsgateway/problemen met betrekking tot.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens uit een on-premises Cassandra-gegevensarchief verplaatst met behulp van verschillende hulpprogramma's / API's. 

- De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren. 
- U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en  **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren:

1. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory.
2. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar een on-premises Cassandra-gegevensarchief, [JSON-voorbeeld: Gegevens kopiëren van Cassandra naar Azure Blob](#json-example-copy-data-from-cassandra-to-azure-blob) sectie van dit artikel. 

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar een gegevensarchief Cassandra:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor Cassandra gekoppelde service.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OnPremisesCassandra** |Ja |
| host |Een of meer IP-adressen of hostnamen van de Cassandra-servers.<br/><br/>Geef een door komma's gescheiden lijst met IP-adressen of hostnamen gelijktijdig verbinding maken met alle servers. |Ja |
| poort |De TCP-poort die gebruikmaakt van de Cassandra-server om te luisteren naar clientverbindingen. |Nee, standaardwaarde: 9042 |
| authenticationType |Basis- of anonieme |Ja |
| gebruikersnaam |Geef de gebruikersnaam op voor het gebruikersaccount. |Ja, als authenticationType is ingesteld op het Basislidmaatschap. |
| wachtwoord |Wachtwoord voor het gebruikersaccount opgeven. |Ja, als authenticationType is ingesteld op het Basislidmaatschap. |
| gatewayName |De naam van de gateway die wordt gebruikt om te verbinden met de on-premises Cassandra-database. |Ja |
| encryptedCredential |Referentie versleuteld door de gateway. |Nee |

>[!NOTE]
>Verbinding met Cassandra met behulp van SSL wordt momenteel niet ondersteund.

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van gegevensset (Azure SQL, Azure-blob, Azure-tabel, enz.).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **CassandraTable** heeft de volgende eigenschappen

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| keyspace |De naam van de keyspace of het schema in Cassandra-database. |Ja (als **query** voor **CassandraSource** is niet gedefinieerd). |
| tableName |De naam van de tabel in de Cassandra-database. |Ja (als **query** voor **CassandraSource** is niet gedefinieerd). |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle soorten activiteiten.

Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit is afhankelijk van elk activiteitstype. Ze verschillen voor de kopieeractiviteit, afhankelijk van de typen van bronnen en sinks.

Wanneer de bron is van het type **CassandraSource**, de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |SQL-92 query of CQL-query. Zie [CQL-verwijzing](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Wanneer u SQL-query gebruikt, geef **keyspace name.table naam** om weer te geven van de tabel die u wilt zoeken. |Nee (als de tabelnaam en keyspace op de gegevensset zijn gedefinieerd). |
| consistencyLevel |Het consistentieniveau Hiermee geeft u het aantal replica's moeten reageren op een leesaanvraag alvorens gegevens naar de clienttoepassing. Cassandra wordt het opgegeven aantal replica's voor gegevens om te voldoen aan de leesaanvraag gecontroleerd. |EEN, TWEE, DRIE, QUORUM, ALLE, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE. Zie [gegevensconsistentie configureren](https://docs.datastax.com/en/cassandra/2.1/cassandra/dml/dml_config_consistency_c.html) voor meer informatie. |Nee. Standaardwaarde is een. |

## <a name="json-example-copy-data-from-cassandra-to-azure-blob"></a>JSON-voorbeeld: Gegevens kopiëren van Cassandra naar Azure Blob
In dit voorbeeld biedt een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Het laat zien hoe gegevens kopiëren van een on-premises Cassandra-database naar een Azure Blob-opslag. Echter gegevens kunnen worden gekopieerd naar een van de vermelde sinks [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

> [!IMPORTANT]
> In dit voorbeeld bevat JSON-fragmenten. Deze omvatten geen stapsgewijze instructies voor het maken van de data factory. Zie [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies.

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

**De invoergegevensset Cassandra:**

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

Instellen van **externe** naar **waar** informeert de Data Factory-service dat de dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory gemaakt wordt.

**Azure Blob-uitvoergegevensset:**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1).

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

**De kopieeractiviteit in een pijplijn met de Cassandra-bron- en Blob:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **CassandraSource** en **sink** type is ingesteld op **BlobSink**.

Zie [RelationalSource typeproperties](#copy-activity-properties) voor de lijst met eigenschappen die worden ondersteund door de RelationalSource.

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
| Cassandra-Type | .NET op basis van Type |
| --- | --- |
| ASCII |Reeks |
| BIGINT |Int64 |
| BLOB |Byte[] |
| BOOLEAANSE WAARDE |Booleaans |
| DECIMAAL |Decimaal |
| DOUBLE-WAARDE |Double-waarde |
| DRIJVENDE KOMMA |Enkelvoudig |
| INET |Reeks |
| INT |Int32 |
| TEKST |Reeks |
| TIJDSTEMPEL |DateTime |
| TIMEUUID |GUID |
| UUID |GUID |
| VARCHAR |Reeks |
| VARINT |Decimaal |

> [!NOTE]
> Voor de verzameling typen (kaart, set, lijst, enzovoort), verwijzen naar [werken met de Cassandra-verzamelingtypen met behulp van de virtuele tabel](#work-with-collections-using-virtual-table) sectie.
>
> Gebruiker gedefinieerde typen worden niet ondersteund.
>
> De lengte van binaire kolom en een kolom met tekenreeksen lengte mag niet groter zijn dan 4000.
>
>

## <a name="work-with-collections-using-virtual-table"></a>Werken met verzamelingen met behulp van de virtuele tabel
Azure Data Factory maakt gebruik van een ingebouwde ODBC-stuurprogramma's verbinding maken met en gegevens kopiëren van de Cassandra-database. Voor verzamelingtypen met inbegrip van de kaart, instellen en lijst, renormalizes het stuurprogramma de gegevens in de bijbehorende virtuele tabellen. Als een tabel een verzameling kolommen bevat, genereert het stuurprogramma met name in de volgende virtuele tabellen:

* Een **basistabel**, die dezelfde gegevens als de echte tabel, met uitzondering van de verzameling kolommen bevat. De basistabel gebruikt dezelfde naam als de echte tabel die wordt vertegenwoordigd.
* Een **virtuele tabel** voor elke kolom verzameling die de geneste gegevens wordt uitgebreid. De virtuele tabellen die staan voor verzamelingen worden genoemd met de naam van de echte tabel, een scheidingsteken "*vt*' en de naam van de kolom.

Virtuele tabellen verwijzen naar de gegevens in de echte tabel, waardoor het stuurprogramma voor toegang tot de Gedenormaliseerde gegevens. Zie de sectie Voorbeeld voor meer informatie. U kunt toegang tot de inhoud van de Cassandra-verzamelingen door het uitvoeren van query's en deze koppelen aan de virtuele tabellen.

U kunt de [Kopieerwizard](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) intuïtief weergeven van de lijst met tabellen in Cassandra-database met inbegrip van de virtuele tabellen en voorbeeld van de gegevens in. U kunt ook een query in de Wizard kopiëren maken en valideren als het resultaat wilt weergeven.

### <a name="example"></a>Voorbeeld
De volgende "ExampleTable" wordt bijvoorbeeld een Cassandra-database-tabel met een geheel getal zijn primaire-sleutelkolom met de naam 'pk_int', een tekstkolom de benoemde waarde, een lijstkolom, een kaart-kolom en een set-kolom (met de naam 'StringSet').

| pk_int | Waarde | Lijst | Kaart | StringSet |
| --- | --- | --- | --- | --- |
| 1 |"Voorbeeldwaarde 1" |["1", "2", "3"] |{'S1': "a", "S2": "b"} |{"A", "B", "C"} |
| 3 |"Voorbeeldwaarde 3" |["100", "101", "102", "105"] |{"S1": "t"} |{"A", "E"} |

Het stuurprogramma genereert meerdere virtuele tabellen om weer te geven deze enkele tabel. De refererende-sleutelkolommen in de virtuele tabellen verwijzen naar de primaire-sleutelkolommen in de echte tabel en welke rij echte tabel een rij in de virtuele tabel komt met overeen geven.

De eerste virtuele tabel is de basistabel met de naam 'ExampleTable' wordt weergegeven in de volgende tabel. De basistabel bevat dezelfde gegevens als de oorspronkelijke databasetabel, met uitzondering van de verzamelingen die zijn weggelaten uit deze tabel en uitgevouwen in andere virtuele tabellen.

| pk_int | Waarde |
| --- | --- |
| 1 |"Voorbeeldwaarde 1" |
| 3 |"Voorbeeldwaarde 3" |

De volgende tabellen ziet u de virtuele tabellen die de gegevens uit de lijst, toewijzen en StringSet kolommen opnieuw normaliseren. De kolommen met namen die eindigen op '_index' of '_key' geven de positie van de gegevens binnen de oorspronkelijke lijst of een kaart. De kolommen met namen die met "_Waarde eindigen" bevatten de uitgebreide gegevens uit de verzameling.

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

## <a name="map-source-to-sink-columns"></a>Kaartbron met sink-kolommen
Zie voor meer informatie over het toewijzen van kolommen in de brongegevensset naar kolommen in de sink-gegevensset, [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Herhaalbare lezen van relationele bronnen
Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, houd er rekening mee om te voorkomen dat ongewenste resultaten herhaalbaarheid. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment wordt opnieuw uitgevoerd in beide gevallen, moet u om ervoor te zorgen dat de dezelfde gegevens worden gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable leesrechten voor relationele bronnen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie [prestaties kopiëren en Afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren.
