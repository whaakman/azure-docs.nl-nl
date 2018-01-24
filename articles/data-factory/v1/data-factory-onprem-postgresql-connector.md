---
title: Verplaatsen van gegevens van PostgreSQL met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit een PostgreSQL-Database met behulp van Azure Data Factory.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 888d9ebc-2500-4071-b6d1-0f6bd1b5997c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 4cec177456b007fd7c6721380c00a622b43af677
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-from-postgresql-using-azure-data-factory"></a>Verplaatsen van gegevens uit een PostgreSQL met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-onprem-postgresql-connector.md)
> * [Versie 2 - Preview](../connector-postgresql.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [PostgreSQL-connector in V2](../connector-postgresql.md).


In dit artikel wordt uitgelegd hoe de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van een on-premises PostgreSQL-database gebruiken. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, hetgeen een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit toont.

U kunt gegevens uit een on-premises PostgreSQL-gegevensopslag kopiëren naar een ondersteunde sink data store. Zie voor een lijst met gegevensarchieven als PUT wordt ondersteund door de kopieeractiviteit, [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Data factory ondersteunt momenteel zwevend gegevens uit een PostgreSQL-database op andere gegevensarchieven, maar niet voor het verplaatsen van gegevens uit andere gegevensarchieven naar een PostgreSQL-database. 

## <a name="prerequisites"></a>Vereisten

Verbinding maken met lokale PostgreSQL bronnen met behulp van Data Management Gateway biedt ondersteuning voor Data Factory-service. Zie [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies over het instellen van de gateway.

Gateway is vereist, zelfs als de PostgreSQL-database wordt gehost in een Azure IaaS VM. U kunt gateway installeren op de dezelfde IaaS VM als gegevensopslag of op een andere virtuele machine, zolang de gateway verbinding met de database maken kan.

> [!NOTE]
> Zie [gateway problemen](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) voor tips over het oplossen van verbinding of gateway gerelateerde problemen.

## <a name="supported-versions-and-installation"></a>Ondersteunde versies en installatie
Voor de Data Management Gateway verbinding maken met de PostgreSQL-Database, installeert u de [Ngpsql-gegevensprovider voor PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) met versie tussen 2.0.12 en punt 3.1.9 op hetzelfde systeem als Data Management Gateway. PostgreSQL versie 7.4 en hoger wordt ondersteund.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens vanuit een on-premises PostgreSQL-gegevensopslag verplaatst met behulp van verschillende hulpprogramma's voor API's. 

- De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren. 
- U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: 
    - Azure Portal
    - Visual Studio
    - Azure PowerShell
    - Azure Resource Manager-sjabloon
    - .NET API
    - REST-API

     Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren:

1. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory.
2. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een on-premises PostgreSQL-gegevensopslag [JSON-voorbeeld: gegevens kopiëren van PostgreSQL naar Azure Blob](#json-example-copy-data-from-postgresql-to-azure-blob) sectie van dit artikel. 

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar een PostgreSQL-gegevensopslag:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor PostgreSQL gekoppelde service.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OnPremisesPostgreSql** |Ja |
| server |De naam van de PostgreSQL-server. |Ja |
| database |De naam van de PostgreSQL-database. |Ja |
| schema |De naam van het schema in de database. Naam van het schema is hoofdlettergevoelig. |Nee |
| authenticationType |Het soort verificatie die wordt gebruikt voor verbinding met de PostgreSQL-database. Mogelijke waarden zijn: anoniem, basis en Windows. |Ja |
| gebruikersnaam |Geef de gebruikersnaam als u basisverificatie of Windows-verificatie gebruikt. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |
| gatewayName |Naam van de gateway die voor de Data Factory-service gebruiken moet voor verbinding met de lokale PostgreSQL-database. |Ja |

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets van de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle typen van de gegevensset.

De sectie typeProperties verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **RelationalTable** (inclusief PostgreSQL gegevensset) heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in de PostgreSQL-Database-instantie waarnaar de gekoppelde service verwijst. De tabelnaam is hoofdlettergevoelig. |Nee (als **query** van **RelationalSource** is opgegeven) |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten van de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle typen activiteiten.

Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren met elk activiteitstype. Voor de kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put.

Wanneer de bron is van het type **RelationalSource** (waaronder PostgreSQL), de volgende eigenschappen beschikbaar zijn in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `"query": "select * from \"MySchema\".\"MyTable\""`. |Nee (als **tableName** van **gegevensset** is opgegeven) |

> [!NOTE]
> Schema- en tabelnamen zijn hoofdlettergevoelig. Omsluiten in `""` (dubbele aanhalingstekens) in de query.  

**Voorbeeld:**

 `"query": "select * from \"MySchema\".\"MyTable\""`

## <a name="json-example-copy-data-from-postgresql-to-azure-blob"></a>JSON-voorbeeld: gegevens kopiëren van PostgreSQL naar Azure Blob
In dit voorbeeld bevat definities van de voorbeeld-JSON die u een pijplijn maken kunt met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe gegevens uit een PostgreSQL-database kopiëren naar Azure Blob Storage. Gegevens kunnen echter worden gekopieerd naar een van de PUT vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.   

> [!IMPORTANT]
> Dit voorbeeld bevat JSON-fragmenten. Dit omvat geen stapsgewijze instructies voor het maken van de gegevensfactory. Zie [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies.

Het voorbeeld heeft de volgende data factory-entiteiten:

1. Een gekoppelde service van het type [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. De [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld kopieert gegevens van de resultaten van een query in een PostgreSQL-database naar een blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

Instellen als eerste stap van de data management gateway. De instructies vindt u in de [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

**PostgreSQL gekoppelde service:**

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
**Azure Blob-opslag gekoppelde service:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
    "type": "AzureStorage",
    "typeProperties": {
        "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
    }
    }
}
```
**PostgreSQL invoergegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MijnTabel' gemaakt in PostgreSQL en bevat een kolom met de naam 'tijdstempel' voor de reeksgegevens.

Instelling `"external": true` informeert de Data Factory-service dat de dataset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
        "typeProperties": {},
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

**Azure Blob-uitvoergegevensset:**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Pad en naam van de map voor de blob worden dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad maakt gebruik van jaar, maand, dag en uur delen van de begintijd.

```json
{
    "name": "AzureBlobPostgreSqlDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/postgresql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Pijplijn met de kopieeractiviteit:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en is gepland voor uitvoering per uur. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **RelationalSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **query** eigenschap selecteert u de gegevens uit de tabel public.usstates in de PostgreSQL-database.

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from \"public\".\"usstates\""
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "PostgreSqlDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobPostgreSqlDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "PostgreSqlToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
## <a name="type-mapping-for-postgresql"></a>Toewijzing van het type voor PostgreSQL
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieeractiviteit voert automatische typeconversies van brontypen opvangen typen met de volgende stappen 2-benadering:

1. Systeemeigen brontypen converteren naar .NET-type
2. Converteren van .NET-type naar systeemeigen sink-type

Wanneer u gegevens naar PostgreSQL verplaatst, worden de volgende toewijzingen van PostgreSQL type gebruikt voor .NET-type.

| Type PostgreSQL-Database | PostgresSQL aliassen | .NET framework-type |
| --- | --- | --- |
| abstime | |Datum en tijd | &nbsp;
| bigint |int8 |Int64 |
| bigserial |serial8 |Int64 |
| bits [(n)] | |Byte [], String | &nbsp;
| bit uiteenlopende [(n)] |varbit |Byte [], String |
| booleaans |BOOL |Boole-waarde |
| Vak | |Byte [], String |&nbsp;
| bytea | |Byte [], String |&nbsp;
| teken [(n)] |char [(n)] |Tekenreeks |
| teken wisselende [(n)] |varchar [(n)] |Tekenreeks |
| CID | |Tekenreeks |&nbsp;
| CIDR | |Tekenreeks |&nbsp;
| cirkel | |Byte [], String |&nbsp;
| datum | |Datum en tijd |&nbsp;
| DateRange | |Tekenreeks |&nbsp;
| dubbele precisie |FLOAT8 |Double |
| INet | |Byte [], String |&nbsp;
| intarry | |Tekenreeks |&nbsp;
| int4range | |Tekenreeks |&nbsp;
| int8range | |Tekenreeks |&nbsp;
| geheel getal |int, int4 |Int32 |
| interval [velden] [(p)] | |Periode |&nbsp;
| JSON | |Tekenreeks |&nbsp;
| jsonb | |Byte[] |&nbsp;
| regel | |Byte [], String |&nbsp;
| lseg | |Byte [], String |&nbsp;
| macaddr | |Byte [], String |&nbsp;
| Money | |Decimale |&nbsp;
| numerieke [(p, s)] |Decimal [(p, s)] |Decimale |
| numrange | |Tekenreeks |&nbsp;
| OID | |Int32 |&nbsp;
| pad | |Byte [], String |&nbsp;
| pg_lsn | |Int64 |&nbsp;
| beheerpunt | |Byte [], String |&nbsp;
| veelhoek | |Byte [], String |&nbsp;
| echte |FLOAT4 |Single |
| smallint |int2 |Int16 |
| smallserial |serial2 |Int16 |
| serieel |serial4 |Int32 |
| Tekst | |Tekenreeks |&nbsp;

## <a name="map-source-to-sink-columns"></a>Bron van de kaart opvangen kolommen
Zie voor meer informatie over het toewijzen van kolommen in gegevensset naar kolommen in gegevensset sink bron, [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Herhaalbare leesbewerking van relationele bronnen
Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, moet u herhaalbaarheid Houd in gedachten om te voorkomen dat ongewenste resultaten. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook een beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment opnieuw in beide gevallen wordt uitgevoerd, moet u ervoor zorgen dat dezelfde gegevens is gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable van relationele bronnen lezen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en het afstemmen
Zie [uitvoering van de activiteit & afstemmen handleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren.
