---
title: "Gegevens kopiëren naar/van Oracle gebruik Data Factory | Microsoft Docs"
description: "Informatie over het kopiëren van gegevens van Oracle-database die on-premises met Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3c20aa95-a8a1-4aae-9180-a6a16d64a109
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 82fe637b46decfc9c8d09b5c7e03f328a8636263
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="copy-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Gegevens kopiëren van lokale Oracle met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-onprem-oracle-connector.md)
> * [Versie 2 - Preview](../connector-oracle.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [Oracle-connector in V2](../connector-oracle.md).


Dit artikel wordt uitgelegd dat het gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens uit een lokale Oracle-database te verplaatsen. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, hetgeen een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit toont.

## <a name="supported-scenarios"></a>Ondersteunde scenario's
U kunt gegevens kopiëren **uit een Oracle-database** opslaat in de volgende gegevens:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

U kunt gegevens kopiëren van de volgende gegevensarchieven **met een Oracle-database**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

## <a name="prerequisites"></a>Vereisten
Data Factory ondersteunt verbindingen met lokale Oracle bronnen met behulp van Data Management Gateway. Zie [Data Management Gateway](data-factory-data-management-gateway.md) artikel voor meer informatie over Data Management Gateway en [gegevens verplaatsen van on-premises naar cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies over het instellen van de gateway een gegevens-pijplijn om gegevens te verplaatsen.

Gateway is vereist, zelfs als de Oracle wordt gehost in een Azure IaaS VM. U kunt de gateway installeren op de dezelfde IaaS VM als gegevensopslag of op een andere virtuele machine, zolang de gateway verbinding met de database maken kan.

> [!NOTE]
> Zie [gateway problemen](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) voor tips over het oplossen van verbinding of gateway gerelateerde problemen.

## <a name="supported-versions-and-installation"></a>Ondersteunde versies en installatie
Twee versies van stuurprogramma's bieden ondersteuning voor deze connector Oracle:

- **Microsoft-stuurprogramma voor Oracle (aanbevolen)**: vanaf Data Management Gateway versie 2.7, een Microsoft-stuurprogramma voor Oracle wordt automatisch geïnstalleerd samen met de gateway, dus u hoeft niet te verwerken bovendien het stuurprogramma om vast te stellen verbinding met de Oracle en u kunt ook de prestaties beter kopie dit stuurprogramma gebruikt. Hieronder versies van Oracle worden databases ondersteund:
    - Oracle 12c R1 (12.1)
    - Oracle 11g R1, R2 (11.1, 11.2)
    - Oracle 10g R1, R2 (10.1, 10.2)
    - Oracle 9i R1, R2 (9.0.1, 9.2)
    - Oracle 8i R3 (8.1.7)

> [!IMPORTANT]
> Microsoft-stuurprogramma voor Oracle ondersteunt momenteel alleen kopiëren van gegevens uit Oracle, maar niet schrijven naar Oracle. En noteer dat de mogelijkheid van de verbinding test in het tabblad Data Management Gateway diagnostische gegevens biedt geen ondersteuning voor dit stuurprogramma. U kunt ook de wizard kopiëren gebruiken voor het valideren van de connectiviteit.
>

- **Oracle-gegevensprovider voor .NET:** u kunt ook gebruik van Oracle-gegevensprovider om gegevens te kopiëren van/naar Oracle. Dit onderdeel is opgenomen in [Oracle Data Access-onderdelen voor Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/). Installeer de juiste versie (32/64 bits) op de computer waarop de gateway is geïnstalleerd. [Oracle-gegevensprovider .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) kunnen toegang tot Oracle Database 10 g versie 2 of hoger.

    Als u 'XCopy-installatie kiest, voert u de stappen in de Leesmij.htm. U wordt aangeraden u het installatieprogramma met een gebruikersinterface (niet-XCopy een).

    Na de installatie van de provider **opnieuw** de Data Management Gateway host-service op uw machine met behulp van de Services-applet (of) Data Management Gateway Configuration Manager.  

Als u de wizard kopiëren gebruiken voor het ontwerpen van de pijplijn kopiëren, worden de stuurprogrammatype automatisch bepaald. Microsoft-stuurprogramma wordt standaard gebruikt tenzij uw versie van de gegevensgateway lager dan 2.7 is of kies van Oracle als sink.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens uit een lokale Oracle-database verplaatst met behulp van verschillende hulpprogramma's voor API's.

De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit.

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren:

1. Maak een **gegevensfactory**. Een gegevensfactory kan één of meer pijplijnen bevatten. 
2. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory. Bijvoorbeeld, als u gegevens uit een database Oralce naar een Azure blob-opslag kopiëren wilt, maakt u twee gekoppelde services als u wilt uw Oracle-database en de Azure storage-account koppelen aan uw gegevensfactory. Zie voor de gekoppelde service-eigenschappen die specifiek voor Oracle zijn, [gekoppelde service-eigenschappen](#linked-service-properties) sectie.
3. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking. In het voorbeeld in de laatste stap wordt vermeld, maakt u een gegevensset om op te geven van de tabel in de Oracle-database waarin de invoergegevens. En u een andere gegevensset opgeven van de blob-container en de map waarin de gegevens gekopieerd van de Oracle-database maken. Zie voor eigenschappen van gegevensset die specifiek voor Oracle zijn, [eigenschappen van gegevensset](#dataset-properties) sectie.
4. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. In het voorbeeld eerder vermeld, gebruikt u OracleSource als een bron- en BlobSink als een sink voor de kopieeractiviteit. Op dezelfde manier als u uit Azure Blob Storage met Oracle-Database kopiëren wilt, gebruikt u BlobSource en OracleSink in de kopieerbewerking. Zie voor activiteitseigenschappen kopiëren die specifiek voor Oracle-database zijn, [activiteitseigenschappen kopiëren](#copy-activity-properties) sectie. Klik op de koppeling in de vorige sectie voor de gegevensopslag voor meer informatie over het gebruik van een gegevensarchief als een bron of een sink. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor voorbeelden met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar/van een lokale Oracle-database, [JSON voorbeelden](#json-examples-for-copying-data-to-and-from-oracle-database) sectie van dit artikel.

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor Oracle gekoppelde service.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OnPremisesOracle** |Ja |
| driverType | Geef op welke stuurprogramma moet worden gebruikt om gegevens te kopiëren van/naar Oracle-Database. Toegestane waarden zijn **Microsoft** of **ODP** (standaard). Zie [ondersteunde versie en installatieopties](#supported-versions-and-installation) gedeelte stuurprogrammagegevens. | Nee |
| connectionString | Geef informatie op die nodig zijn voor het verbinding maken met de Oracle-Database-exemplaar voor de eigenschap connectionString. | Ja |
| gatewayName | Naam van de gateway die die wordt gebruikt voor verbinding met de lokale Oracle-server |Ja |

**Voorbeeld: met behulp van het stuurprogramma Microsoft:**
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Voorbeeld: met behulp van ODP stuurprogramma**

Raadpleeg [deze site](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/) voor de toegestane indelingen.

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets van de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle gegevensset typen (Oracle, Azure blob-, Azure-tabel, enz.).

De sectie typeProperties verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De sectie typeProperties voor de gegevensset van het type OracleTable heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in de Oracle-Database waarnaar de gekoppelde service verwijst. |Nee (als **oracleReaderQuery** van **OracleSource** is opgegeven) |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten van de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle typen activiteiten.

> [!NOTE]
> De Kopieeractiviteit slechts één invoer en slechts één uitvoer produceert.

Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren met elk activiteitstype. Voor de kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put.

### <a name="oraclesource"></a>OracleSource
In de kopieeractiviteit, wanneer de bron van het type **OracleSource** de volgende eigenschappen beschikbaar zijn in **typeProperties** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| oracleReaderQuery |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: Selecteer * from MijnTabel <br/><br/>Als niet wordt opgegeven, de SQL-instructie die is uitgevoerd: Selecteer * from MijnTabel |Nee (als **tableName** van **gegevensset** is opgegeven) |

### <a name="oraclesink"></a>OracleSink
**OracleSink** ondersteunt de volgende eigenschappen:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| writeBatchTimeout |Wachttijd voor de batch-insert-bewerking te voltooien voordat er een optreedt time-out. |TimeSpan<br/><br/> Voorbeeld: 00:30:00 (30 minuten). |Nee |
| writeBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt. |Geheel getal (aantal rijen) |Nee (standaard: 100) |
| sqlWriterCleanupScript |Geef een query voor de Kopieeractiviteit uitvoeren zodat de gegevens van een bepaald segment wordt opgeschoond. |Een query-instructie. |Nee |
| sliceIdentifierColumnName |Geef de naam van de kolom voor de Kopieeractiviteit te vullen met de segment-ID automatisch gegenereerd en die wordt gebruikt voor het opschonen van de gegevens van een bepaald segment wanneer opnieuw uitgevoerd. |De naam van de kolom van een kolom met gegevenstype binary(32). |Nee |

## <a name="json-examples-for-copying-data-to-and-from-oracle-database"></a>JSON-voorbeelden voor het kopiëren van gegevens naar en van Oracle-database
Het volgende voorbeeld bevat definities van de voorbeeld-JSON die u een pijplijn maken kunt met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe de gegevens van/naar een Oracle-database uit Azure Blob Storage kopiëren. Gegevens kunnen echter worden gekopieerd naar een van de PUT vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.   

## <a name="example-copy-data-from-oracle-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren van Oracle naar Azure Blob

Het voorbeeld heeft de volgende data factory-entiteiten:

1. Een gekoppelde service van het type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) als bron en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) als sink.

Het voorbeeld worden gegevens gekopieerd van een tabel in een lokale Oracle-database naar een blob per uur. Zie de documentatie in de voorbeelden volgende secties voor meer informatie over diverse eigenschappen die in de steekproef wordt gebruikt.

**Oracle gekoppelde service:**

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Azure Blob-opslag gekoppelde service:**

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Oracle-invoergegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MijnTabel' gemaakt in Oracle en bevat een kolom met de naam 'timestampcolumn' voor de reeksgegevens.

Instelling 'extern': 'true' informeert de Data Factory-service dat de dataset extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2014-02-27T12:00:00",
            "frequency": "Hour"
        },
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
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": "\t",
                "rowDelimiter": "\n"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Pijplijn met de kopieeractiviteit:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en is gepland voor uitvoering per uur. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **OracleSource** en **sink** type is ingesteld op **BlobSink**.  De SQL-query opgegeven met **oracleReaderQuery** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "OracletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": " OracleInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "OracleSource",
                        "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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

## <a name="example-copy-data-from-azure-blob-to-oracle"></a>Voorbeeld: Gegevens kopiëren van Azure-Blob naar Oracle
Dit voorbeeld laat zien hoe gegevens kopiëren van een Azure Blob Storage naar een lokale Oracle-database. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** uit een van de bronnen vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.  

Het voorbeeld heeft de volgende data factory-entiteiten:

1. Een gekoppelde service van het type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) als bron [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) als sink.

Het voorbeeld kopieert gegevens van een blob naar een tabel in een lokale Oracle-database om het uur. Zie de documentatie in de voorbeelden volgende secties voor meer informatie over diverse eigenschappen die in de steekproef wordt gebruikt.

**Oracle gekoppelde service:**
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
            User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Azure Blob-opslag gekoppelde service:**
```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Azure Blob-invoergegevensset**

Gegevens wordt opgehaald uit een nieuwe blob elk uur (frequentie: uur, interval: 1). Pad en naam van de map voor de blob worden dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad gebruikt jaar, maand en dag deel uit van de begintijd en de bestandsnaam wordt gebruikt voor het uur deel van de begintijd. "extern": "true" instelling informeert de Data Factory-service dat deze tabel extern is aan de gegevensfactory en niet wordt geproduceerd door een activiteit in de gegevensfactory.

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
                }
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
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

**Oracle-uitvoergegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u een tabel "MijnTabel" in Oracle hebt gemaakt. De tabel in Oracle met hetzelfde aantal kolommen maken zoals u verwacht dat de Blob-CSV-bestand bevatten. Nieuwe rijen worden toegevoegd aan de tabel om het uur.

```json
{
    "name": "OracleOutput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Day",
            "interval": "1"
        }
    }
}
```

**Pijplijn met de kopieeractiviteit:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **BlobSource** en de **sink** type is ingesteld op **OracleSink**.  

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-05T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
            {
                "name": "AzureBlobtoOracle",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "OracleOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "OracleSink"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
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


## <a name="troubleshooting-tips"></a>Tips voor probleemoplossing
### <a name="problem-1-net-framework-data-provider"></a>Probleem 1: .NET Framework-gegevensprovider

U ziet het volgende **foutbericht**:

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable to find the requested .Net Framework Data Provider. It may not be installed”.  

**Mogelijke oorzaken:**

1. De .NET Framework-gegevensprovider voor Oracle is niet geïnstalleerd.
2. De .NET Framework-gegevensprovider voor Oracle naar .NET Framework 2.0 is geïnstalleerd en is niet gevonden in de .NET Framework 4.0-mappen.

**Resolutie/tijdelijke oplossing:**

1. Als u de .NET-Provider voor Oracle, nog niet geïnstalleerd [installeren](http://www.oracle.com/technetwork/topics/dotnet/downloads/) en probeer het scenario.
2. Als u het foutbericht krijgt zelfs na het installeren van de provider, gaat u de volgende stappen uit:
   1. Configuratie van .NET 2.0 machine openen vanuit de map: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
   2. Zoeken naar **Oracle-gegevensprovider voor .NET**, en moet u een vermelding vinden, zoals wordt weergegeven in het volgende voorbeeld onder kunnen **systeem.gegevens** -> **DbProviderFactories**: '<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle-gegevensprovider voor .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
3. Dit item kopiëren naar het machine.config-bestand in de volgende v4.0 map: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config en wijzig de 4.xxx.x.x-versie.
4. '< Pad ODP.NET geïnstalleerde > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll' in de global assemblycache (GAC) installeren door te voeren `gacutil /i [provider path]`. ## tips voor probleemoplossing

### <a name="problem-2-datetime-formatting"></a>Probleem 2: opmaak voor datum/tijd

U ziet het volgende **foutbericht**:

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

**Resolutie/tijdelijke oplossing:**

U wilt aanpassen van de queryreeks in de kopieerbewerking op basis van hoe datums zijn geconfigureerd in de Oracle-database, zoals weergegeven in het volgende voorbeeld (met de functie to_date):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


## <a name="type-mapping-for-oracle"></a>Toewijzing van het type voor Oracle
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieeractiviteit voert automatische typeconversies van brontypen opvangen typen met de volgende stappen 2-benadering:

1. Systeemeigen brontypen converteren naar .NET-type
2. Converteren van .NET-type naar systeemeigen sink-type

Bij het verplaatsen van gegevens uit Oracle, worden de volgende toewijzingen gebruikt van Oracle-gegevenstype naar .NET-type en vice versa.

| Oracle-gegevenstype | .NET framework-gegevenstype |
| --- | --- |
| BBESTAND |Byte[] |
| BLOB |Byte[]<br/>(alleen ondersteund voor Oracle 10g en hoger als met Microsoft-stuurprogramma) |
| CHAR |Tekenreeks |
| CLOB |Tekenreeks |
| DATE |Datum en tijd |
| FLOAT |Decimaal, tekenreeks (als precision > 28) |
| GEHEEL GETAL |Decimaal, tekenreeks (als precision > 28) |
| INTERVAL JAAR, MAAND |Int32 |
| TWEEDE INTERVAL DAG |TimeSpan |
| LANG |Tekenreeks |
| LANGE ONBEWERKTE |Byte[] |
| NCHAR |Tekenreeks |
| NCLOB |Tekenreeks |
| AANTAL |Decimaal, tekenreeks (als precision > 28) |
| NVARCHAR2 |Tekenreeks |
| RAW |Byte[] |
| ROWID |Tekenreeks |
| TIJDSTEMPEL |Datum en tijd |
| TIJDSTEMPEL MET DE LOKALE TIJDZONE |Datum en tijd |
| TIJDSTEMPEL MET TIJDZONE |Datum en tijd |
| NIET-ONDERTEKEND GEHEEL GETAL |Aantal |
| VARCHAR2 |Tekenreeks |
| XML |Tekenreeks |

> [!NOTE]
> Gegevenstype **INTERVAL aan maand** en **INTERVAL dag naar tweede** worden niet ondersteund bij gebruik van Microsoft-stuurprogramma.

## <a name="map-source-to-sink-columns"></a>Bron van de kaart opvangen kolommen
Zie voor meer informatie over het toewijzen van kolommen in gegevensset naar kolommen in gegevensset sink bron, [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Herhaalbare leesbewerking van relationele bronnen
Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, moet u herhaalbaarheid Houd in gedachten om te voorkomen dat ongewenste resultaten. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook een beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment opnieuw in beide gevallen wordt uitgevoerd, moet u ervoor zorgen dat dezelfde gegevens is gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable van relationele bronnen lezen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en het afstemmen
Zie [uitvoering van de activiteit & afstemmen handleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren.
