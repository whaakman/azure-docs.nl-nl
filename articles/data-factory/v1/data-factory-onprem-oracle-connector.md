---
title: Gegevens kopiëren naar of van Oracle met behulp van Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens naar/van Oracle-database die on-premises met Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 3c20aa95-a8a1-4aae-9180-a6a16d64a109
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 10535e75a32a9f95e759340cf14d693f43639473
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856838"
---
# <a name="copy-data-to-or-from-on-premises-oracle-using-azure-data-factory"></a>Gegevens kopiëren naar of van on-premises Oracle met Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-onprem-oracle-connector.md)
> * [Versie 2 (huidige versie)](../connector-oracle.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [Oracle-connector in V2](../connector-oracle.md).


In dit artikel wordt uitgelegd hoe u van de Kopieeractiviteit in Azure Data Factory om gegevens naar/van een on-premises Oracle-database te verplaatsen. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit geeft.

## <a name="supported-scenarios"></a>Ondersteunde scenario's
U kunt gegevens kopiëren **uit een Oracle-database** opgeslagen in de volgende gegevens:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

U kunt gegevens kopiëren van de volgende gegevensarchieven **met een Oracle-database**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

## <a name="prerequisites"></a>Vereisten
Data Factory ondersteunt een verbinding met on-premises Oracle-bronnen met behulp van de Data Management Gateway. Zie [Data Management Gateway](data-factory-data-management-gateway.md) artikel voor meer informatie over Data Management Gateway en [gegevens verplaatsen van on-premises naar de cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies voor het instellen van de gateway een pijplijn voor gegevens verplaatsen.

Gateway is vereist, zelfs als de Oracle wordt gehost in een Azure IaaS-VM. U kunt de gateway installeren op dezelfde IaaS-VM als het gegevensarchief of op een andere virtuele machine, zolang de gateway verbinding met de database maken kan.

> [!NOTE]
> Zie [oplossen van problemen met gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) voor tips over het oplossen van de verbindingsgateway/problemen met betrekking tot.

## <a name="supported-versions-and-installation"></a>Ondersteunde versies en installatie
Deze connector Oracle bieden ondersteuning voor twee versies van stuurprogramma's:

- **Microsoft-stuurprogramma voor Oracle (aanbevolen)**: Data Management Gateway versie 2.7, een Microsoft stuurprogramma voor Oracle is automatisch geïnstalleerd samen met de gateway, zodat u bovendien niet hoeft te verwerken van het stuurprogramma in zodat u vanaf verbinding maken met Oracle en u kunt ook betere kopieerprestaties met behulp van dit stuurprogramma ondervindt. Hieronder versies van Oracle worden databases ondersteund:
    - Oracle 12c R1 (12.1)
    - Oracle 11g R1, R2 (11.1, 11.2)
    - Oracle 10g R1, R2 (10.1, 10.2)
    - Oracle 9i R1, R2 (9.0.1, 9.2)
    - Oracle 8i R3 (8.1.7)

> [!NOTE]
> Proxyserver voor Oracle wordt niet ondersteund.

> [!IMPORTANT]
> Microsoft-stuurprogramma voor Oracle ondersteunt momenteel alleen kopiëren van gegevens van Oracle, maar niet schrijven naar Oracle. En Let op dat de mogelijkheid verbinding testen van Data Management Gateway Diagnostics tabblad biedt geen ondersteuning voor dit stuurprogramma. U kunt ook de wizard kopiëren gebruiken voor het valideren van de verbinding.
>

- **Oracle-gegevensprovider voor .NET:** kunt u ook Oracle-gegevensprovider gebruiken om gegevens te kopiëren van/naar Oracle. Dit onderdeel is opgenomen in [Oracle Data Access-componenten voor Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/). Installeer de juiste versie (32/64 bits) op de computer waarop de gateway is geïnstalleerd. [Oracle-gegevensprovider .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) toegang tot Oracle Database 10 g versie 2 of hoger.

    Als u 'Installatie XCopy' kiest, voert u de stappen in de Leesmij.htm. U wordt aangeraden u het installatieprogramma met de gebruikersinterface (niet-XCopy een).

    Na de installatie van de provider **opnieuw** de hostservice van Data Management Gateway op uw computer met Services-applet (of) Data Management Gateway Configuratiebeheer.  

Als u wizard kopiëren om de kopieerpijplijn gebruikt, is het stuurprogrammatype automatisch bepaald. Microsoft-stuurprogramma wordt standaard gebruikt, tenzij uw gatewayversie lager dan 2.7 is of Oracle als sink die u kiest.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens naar/van een on-premises Oracle-database verplaatst met behulp van verschillende hulpprogramma's / API's.

De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon** , **.NET API**, en **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit.

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren:

1. Maak een **gegevensfactory**. Een data factory kan één of meer pijplijnen bevatten. 
2. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory. Bijvoorbeeld, als u gegevens uit een database Oralce naar een Azure blob-opslag kopieert, u twee gekoppelde services om uw Oracle-database en Azure storage-account koppelen aan uw data factory. Zie voor de gekoppelde service-eigenschappen die specifiek voor Oracle zijn, [gekoppelde service-eigenschappen](#linked-service-properties) sectie.
3. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking. In het voorbeeld dat wordt vermeld in de vorige stap, maakt u een gegevensset om op te geven van de tabel in uw Oracle-database die de invoergegevens bevat. En u maakt een andere gegevensset om op te geven van de blob-container en de map waarin de gegevens van de Oracle-database gekopieerd. Zie voor de gegevensseteigenschappen die specifiek voor Oracle zijn, [gegevensseteigenschappen](#dataset-properties) sectie.
4. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. In het voorbeeld eerder vermeld, gebruikt u OracleSource als een bron- en BlobSink als een sink voor de kopieeractiviteit. Op dezelfde manier als u van Azure Blob-opslag met Oracle-Database kopiëren wilt, gebruikt u BlobSource en OracleSink in de kopieeractiviteit. Zie voor kopiëren-activiteitseigenschappen die specifiek voor Oracle-database zijn, [eigenschappen van de kopieeractiviteit](#copy-activity-properties) sectie. Klik op de koppeling in de vorige sectie voor de gegevensopslag voor meer informatie over het gebruik van een gegevensarchief als een bron of een sink. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor voorbeelden met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar/van een on-premises Oracle-database, [JSON voorbeelden](#json-examples-for-copying-data-to-and-from-oracle-database) sectie van dit artikel.

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor Oracle gekoppelde service.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OnPremisesOracle** |Ja |
| driverType | Geef op welke stuurprogramma om te gebruiken om gegevens te kopiëren van/naar Oracle-Database. Toegestane waarden zijn **Microsoft** of **ODP** (standaard). Zie [ondersteunde versie en installatie](#supported-versions-and-installation) sectie op stuurprogrammagegevens. | Nee |
| connectionString | Geef informatie op die nodig zijn voor het verbinding maken met de Oracle-Database-exemplaar voor de connectionString-eigenschap. | Ja |
| gatewayName | Naam van de gateway die wordt gebruikt voor verbinding met de on-premises Oracle-server |Ja |

**Voorbeeld: met behulp van Microsoft-stuurprogramma:**

>[!TIP]
>Als u fout uitspraak bereikt "ORA 01025: UPI parameter buiten het bereik ' en de Oracle-van versie 8i, toevoegen `WireProtocolMode=1` met de verbindingstekenreeks en probeer het opnieuw.

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
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van gegevensset (Oracle, Azure-blob, Azure-tabel, enz.).

De sectie typeProperties verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De sectie typeProperties voor de gegevensset van het type OracleTable heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in de Oracle-Database waarnaar de gekoppelde service verwijst. |Nee (als **oracleReaderQuery** van **OracleSource** is opgegeven) |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle soorten activiteiten.

> [!NOTE]
> De Kopieeractiviteit slechts één invoer en produceert slechts één uitvoer.

Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit is afhankelijk van elk activiteitstype. Ze verschillen voor de kopieeractiviteit, afhankelijk van de typen van bronnen en sinks.

### <a name="oraclesource"></a>OracleSource
In de kopieeractiviteit, wanneer de bron van het type **OracleSource** de volgende eigenschappen zijn beschikbaar in **typeProperties** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| oracleReaderQuery |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: Selecteer * uit MyTable <br/><br/>Indien niet opgegeven, de SQL-instructie die wordt uitgevoerd: Selecteer * uit MyTable |Nee (als **tableName** van **gegevensset** is opgegeven) |

### <a name="oraclesink"></a>OracleSink
**OracleSink** ondersteunt de volgende eigenschappen:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| writeBatchTimeout |Wachttijd voor de batch insert bewerking is voltooid voordat er een optreedt time-out. |TimeSpan<br/><br/> Voorbeeld: 00:30:00 (30 minuten). |Nee |
| writeBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt. |Geheel getal (aantal rijen) |Nee (standaard: 100) |
| sqlWriterCleanupScript |Geef een query voor de Kopieeractiviteit om uit te voeren waarbij de gegevens van een bepaald segment wordt opgeschoond. |Een query-instructie. |Nee |
| sliceIdentifierColumnName |Geef de naam van de kolom voor de Kopieeractiviteit in te vullen met automatisch gegenereerde segment-id, die wordt gebruikt voor het opschonen van gegevens van een bepaald segment wanneer opnieuw uitgevoerd. |De naam van de kolom van een kolom met het gegevenstype van binary(32). |Nee |

## <a name="json-examples-for-copying-data-to-and-from-oracle-database"></a>JSON-voorbeelden voor het kopiëren van gegevens naar en van Oracle-database
Het volgende voorbeeld biedt een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe het kopiëren van gegevens van/naar een Oracle-database naar/van Azure Blob Storage. Echter gegevens kunnen worden gekopieerd naar een van de vermelde sinks [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.   

## <a name="example-copy-data-from-oracle-to-azure-blob"></a>Voorbeeld: Gegevens kopiëren van Oracle naar Azure Blob

Het voorbeeld heeft de volgende data factory-entiteiten:

1. Een gekoppelde service van het type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) als bron en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) als sink.

Het voorbeeld worden gegevens gekopieerd van een tabel in een on-premises Oracle-database naar een blob per uur. Zie de documentatie in de voorbeelden van de volgende secties voor meer informatie over diverse eigenschappen die worden gebruikt in het voorbeeld.

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

**Gekoppelde Azure Blob storage-service:**

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

**De invoergegevensset Oracle:**

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MyTable' gemaakt in Oracle en bevat een kolom met de naam 'timestampcolumn' voor time series-gegevens.

Instellen van "extern": "true" informeert de Data Factory-service dat de dataset bevindt zich buiten de data factory en niet door een activiteit in de data factory gemaakt wordt.

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

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1). Pad en naam van de map voor de blob worden dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

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

**Een pijplijn met Copy activity in:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur wordt uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **OracleSource** en **sink** type is ingesteld op **BlobSink**.  De SQL-query die is opgegeven met **oracleReaderQuery** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

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

## <a name="example-copy-data-from-azure-blob-to-oracle"></a>Voorbeeld: Gegevens van Azure-Blob kopiëren naar Oracle
Dit voorbeeld laat zien hoe u gegevens uit Azure Blob Storage kopiëren naar een on-premises Oracle-database. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** uit een van de bronnen vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.  

Het voorbeeld heeft de volgende data factory-entiteiten:

1. Een gekoppelde service van het type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de kopieeractiviteit die gebruikmaakt van [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) als bron [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) als sink.

Het voorbeeld worden gegevens gekopieerd van een blob naar een tabel in een on-premises Oracle-database om het uur. Zie de documentatie in de voorbeelden van de volgende secties voor meer informatie over diverse eigenschappen die worden gebruikt in het voorbeeld.

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

**Gekoppelde Azure Blob storage-service:**
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

Gegevens wordt opgehaald uit een nieuwe blob elk uur (frequentie: uur en interval: 1). Pad en naam van de map voor de blob worden dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map gebruikt jaar, maand en dag deel uit van de begintijd en de bestandsnaam wordt gebruikt voor het uur deel van de begintijd. "extern": "true" instelling de Data Factory-service wordt geïnformeerd dat deze tabel bevindt zich buiten de data factory en niet door een activiteit in de data factory wordt geproduceerd.

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

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MyTable' gemaakt in Oracle. Maken van de tabel in Oracle met hetzelfde aantal kolommen als u verwacht dat de Blob CSV-bestand bevatten. Nieuwe rijen zijn toegevoegd aan de tabel om het uur.

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

**Een pijplijn met Copy activity in:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **BlobSource** en de **sink** type is ingesteld op **OracleSink**.  

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
### <a name="problem-1-net-framework-data-provider"></a>Probleem 1: .NET Framework Data Provider

U ziet het volgende **foutbericht**:

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable to find the requested .Net Framework Data Provider. It may not be installed”.  

**Mogelijke oorzaken:**

1. De .NET Framework Data Provider voor Oracle is niet geïnstalleerd.
2. De .NET Framework Data Provider voor Oracle naar .NET Framework 2.0 is geïnstalleerd en is niet gevonden in de .NET Framework 4.0-mappen.

**Oplossing/tijdelijke oplossing:**

1. Als u de .NET-Provider voor Oracle, nog niet hebt geïnstalleerd [installeren](http://www.oracle.com/technetwork/topics/dotnet/downloads/) en probeer het scenario.
2. Als u de foutmelding krijgt zelfs na het installeren van de provider, voert u de volgende stappen uit:
   1. Machine-configuratie van .NET 2.0 openen vanuit de map: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
   2. Zoeken naar **Oracle-gegevensprovider voor .NET**, en u moet kunnen geen vermelding vinden, zoals wordt weergegeven in het volgende voorbeeld onder **systeem.gegevens** -> **DbProviderFactories**: "<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle-gegevensprovider voor .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
3. Deze vermelding kopiëren naar het bestand machine.config in de volgende v4.0-map: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config en wijzig de versie 4.xxx.x.x.
4. '< ODP.NET geïnstalleerde pad > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll' in de global assemblycache (GAC) installeren door te voeren `gacutil /i [provider path]`. ## tips voor probleemoplossing

### <a name="problem-2-datetime-formatting"></a>Probleem 2: datum/tijd opmaken

U ziet het volgende **foutbericht**:

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

**Oplossing/tijdelijke oplossing:**

Mogelijk moet u de gebruikte queryreeks in de kopieeractiviteit die is gebaseerd op hoe datums zijn geconfigureerd in uw Oracle-database, zoals weergegeven in het volgende voorbeeld (met behulp van de functie to_date) aanpassen:

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


## <a name="type-mapping-for-oracle"></a>Toewijzing van het type voor Oracle
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieeractiviteit wordt uitgevoerd automatisch typeconversies van typen gegevensbronnen met sink-type met de volgende stappen 2-benadering:

1. Systeemeigen brontypen converteren naar .NET-type
2. .NET-type converteren naar systeemeigen sink-type

Bij het verplaatsen van gegevens van Oracle, worden de volgende toewijzingen van Oracle-gegevenstype naar .NET-type en vice versa gebruikt.

| Oracle-gegevenstype | .NET framework-gegevenstype |
| --- | --- |
| BBESTAND |Byte[] |
| BLOB |Byte[]<br/>(alleen ondersteund op Oracle 10g en hogere wanneer met behulp van Microsoft-stuurprogramma) |
| CHAR |Reeks |
| CLOB |Reeks |
| DATE |DateTime |
| DRIJVENDE KOMMA |Decimaal getal, tekenreeks (als precisie > 28) |
| GEHEEL GETAL ZIJN |Decimaal getal, tekenreeks (als precisie > 28) |
| INTERVAL VOOR JAAR TOT MAAND |Int32 |
| TWEEDE INTERVAL DAG |TimeSpan |
| LANG |Reeks |
| LANGE ONBEWERKTE |Byte[] |
| NCHAR |Reeks |
| NCLOB |Reeks |
| AANTAL |Decimaal getal, tekenreeks (als precisie > 28) |
| NVARCHAR2 |Reeks |
| ONBEWERKTE |Byte[] |
| ROWID |Reeks |
| TIJDSTEMPEL |DateTime |
| TIJDSTEMPEL MET DE LOKALE TIJDZONE |DateTime |
| TIJDSTEMPEL MET TIJDZONE |DateTime |
| GEHEEL GETAL ZONDER TEKEN |Aantal |
| VARCHAR2 |Reeks |
| XML |Reeks |

> [!NOTE]
> Gegevens van het type **INTERVAL jaar aan maand** en **INTERVAL dag aan tweede** worden niet ondersteund bij het gebruik van Microsoft-stuurprogramma.

## <a name="map-source-to-sink-columns"></a>Kaartbron met sink-kolommen
Zie voor meer informatie over het toewijzen van kolommen in de brongegevensset naar kolommen in de sink-gegevensset, [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Herhaalbare lezen van relationele bronnen
Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, houd er rekening mee om te voorkomen dat ongewenste resultaten herhaalbaarheid. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment wordt opnieuw uitgevoerd in beide gevallen, moet u om ervoor te zorgen dat de dezelfde gegevens worden gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable leesrechten voor relationele bronnen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie [prestaties kopiëren en Afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren.
