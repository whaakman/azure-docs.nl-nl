---
title: Gegevens kopiëren naar of van Oracle met behulp van Data Factory | Microsoft Docs
description: Informatie over het kopiëren van gegevens naar of van een on-premises Oracle-database met behulp van Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 3c20aa95-a8a1-4aae-9180-a6a16d64a109
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 1dbac37e8b3bac5ee06ac3bdc3270c04efdcd408
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57536688"
---
# <a name="copy-data-to-or-from-oracle-on-premises-by-using-azure-data-factory"></a>Naar of van Oracle on-premises gegevens kopiëren met behulp van Azure Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-onprem-oracle-connector.md)
> * [Versie 2 (huidige versie)](../connector-oracle.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Azure Data Factory. Als u de huidige versie van de Azure Data Factory-service gebruikt, raadpleegt u [Oracle-connector in V2](../connector-oracle.md).


In dit artikel wordt uitgelegd hoe u gebruik van de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen naar of van een on-premises Oracle-database. Het artikel is gebaseerd op [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md), die een algemeen overzicht van de verplaatsing van gegevens met behulp van de Kopieeractiviteit geeft.

## <a name="supported-scenarios"></a>Ondersteunde scenario's

U kunt gegevens kopiëren *uit een Oracle-database* opgeslagen in de volgende gegevens:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

U kunt gegevens kopiëren van de volgende gegevensarchieven *met een Oracle-database*:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

## <a name="prerequisites"></a>Vereisten

Data Factory ondersteunt een verbinding met on-premises Oracle bronnen met behulp van Data Management Gateway. Zie [Data Management Gateway](data-factory-data-management-gateway.md) voor meer informatie over Data Management Gateway. Zie voor stapsgewijze instructies voor het instellen van de gateway in een pijplijn om gegevens te verplaatsen, [gegevens verplaatsen van on-premises naar de cloud](data-factory-move-data-between-onprem-and-cloud.md).

De gateway is vereist, zelfs als de Oracle wordt gehost in een Azure-infrastructuur als een dienst (IaaS) virtuele machine. U kunt de gateway installeren op dezelfde IaaS-VM als het gegevensarchief of op een andere virtuele machine, zolang de gateway verbinding met de database maken kan.

> [!NOTE]
> Zie voor tips over het oplossen van problemen die gerelateerd zijn aan de verbinding en de gateway [oplossen van problemen met gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues).

## <a name="supported-versions-and-installation"></a>Ondersteunde versies en installatie

Deze connector Oracle bieden ondersteuning voor twee versies van stuurprogramma's:

- **Microsoft-stuurprogramma voor Oracle (aanbevolen)**: Een Microsoft-stuurprogramma voor Oracle, dat is vanaf Data Management Gateway versie 2.7, wordt automatisch geïnstalleerd met de gateway. U hoeft niet te installeren of bijwerken van het stuurprogramma voor het maken van verbinding met Oracle. U kunt ook betere kopieerprestaties ervaren door dit stuurprogramma. Deze versies van Oracle-databases worden ondersteund:
    - Oracle 12c R1 (12.1)
    - Oracle 11g R1, R2 (11.1, 11.2)
    - Oracle 10g R1, R2 (10.1, 10.2)
    - Oracle 9i R1, R2 (9.0.1, 9.2)
    - Oracle 8i R3 (8.1.7)

    > [!NOTE]
    > Proxyserver voor Oracle wordt niet ondersteund.

    > [!IMPORTANT]
    > De Microsoft-stuurprogramma voor Oracle ondersteunt momenteel alleen kopiëren van gegevens van Oracle. Het stuurprogramma biedt geen ondersteuning voor het schrijven naar Oracle. De mogelijkheid van de verbinding testen op de Data Management Gateway **Diagnostics** tabblad biedt geen ondersteuning voor dit stuurprogramma. U kunt ook de wizard kopiëren gebruiken te valideren.
    >

- **Oracle-gegevensprovider voor .NET**: U kunt de Oracle-gegevensprovider gebruiken om gegevens te kopiëren van of naar Oracle. Dit onderdeel is opgenomen in [Oracle Data Access-componenten voor Windows](https://www.oracle.com/technetwork/topics/dotnet/downloads/). Installeer de relevante versie (32-bits of 64-bits) op de computer waarop de gateway is geïnstalleerd. [Oracle-gegevensprovider .NET 12.1](https://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) hebben toegang tot Oracle Database 10 g Release 2 en hoger.

    Als u selecteert **XCopy installatie**, de stappen die worden beschreven in het bestand Leesmij.htm. Het is raadzaam om het installatieprogramma met de gebruikersinterface (niet de XCopy-installatieprogramma) selecteren.

    Nadat u de provider hebt geïnstalleerd, start u de Data Management Gateway-host-service op uw computer met behulp van de applet Services of Data Management Gateway Configuratiebeheer.

Als u de wizard kopiëren om de kopieerpijplijn gebruikt, is het stuurprogrammatype autodetermined. Het stuurprogramma Microsoft wordt standaard gebruikt, tenzij uw gatewayversie ouder dan versie 2.7 is of Oracle als de sink selecteren.

## <a name="get-started"></a>Aan de slag

U kunt een pijplijn met een kopieeractiviteit maken. De pijplijn verplaatst gegevens naar of van een on-premises Oracle-database met behulp van verschillende hulpprogramma's of API's.

Er is de eenvoudigste manier om een pijplijn te maken met de wizard kopiëren. Zie [zelfstudie: Een pijplijn maken met behulp van de wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren van gegevens.

U kunt een van de volgende hulpprogramma's ook gebruiken om een pijplijn te maken: de **Azure-portal**, **Visual Studio**, **Azure PowerShell**, een **Azure Resource Manager sjabloon**, wordt de **.NET API**, of de **REST-API**. Zie de [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit.

Of u de hulpprogramma's of API's gebruikt, voert u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst:

1. Maak een **gegevensfactory**. Een data factory kan één of meer pijplijnen bevatten.
2. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory. Als u gegevens van een Oracle-database naar Azure Blob-opslag kopieert, bijvoorbeeld twee gekoppelde services om uw Oracle-database en Azure storage-account koppelen aan uw data factory maken. Zie voor de gekoppelde service-eigenschappen die specifiek voor Oracle zijn, [gekoppelde service-eigenschappen](#linked-service-properties).
3. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking. In het voorbeeld in de vorige stap maakt u een gegevensset om op te geven van de tabel in uw Oracle-database die de invoergegevens bevat. U maakt een andere gegevensset om op te geven van de blob-container en de map waarin de gegevens van de Oracle-database gekopieerd. Zie voor de gegevensseteigenschappen die specifiek voor Oracle zijn, [gegevensseteigenschappen](#dataset-properties).
4. Maak een **pijplijn** waarvoor een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. In het vorige voorbeeld gebruikt u **OracleSource** als een bron en **BlobSink** als een sink voor de kopieeractiviteit. Op dezelfde manier als u van Azure Blob-opslag met een Oracle-database kopiëren wilt, u **BlobSource** en **OracleSink** in de kopieeractiviteit. Zie voor de Kopieeractiviteit-eigenschappen die specifiek voor een Oracle-database zijn, [Kopieeractiviteit eigenschappen](#copy-activity-properties). Selecteer de koppeling voor de gegevensopslag in de voorgaande sectie voor meer informatie over het gebruik van een gegevensarchief als een bron of de sink.

Wanneer u de wizard, JSON-definities voor deze Data Factory-entiteiten worden automatisch voor u gemaakt: gekoppelde services, gegevenssets en de pijplijn. Wanneer u hulpprogramma's of API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling. Zie voor voorbeelden van JSON-definities voor Data Factory-entiteiten die u gebruikt om gegevens te kopiëren naar of van een on-premises Oracle-database hebt, JSON-voorbeelden.

De volgende secties bevatten meer informatie over JSON-eigenschappen die u gebruikt voor het definiëren van Data Factory-entiteiten.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende tabel beschrijft de JSON-elementen die specifiek voor de Oracle gekoppelde service zijn:

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |De **type** eigenschap moet worden ingesteld op **OnPremisesOracle**. |Ja |
| driverType | Geef op welke stuurprogramma om te gebruiken om gegevens te kopiëren van of naar een Oracle-database. Toegestane waarden zijn **Microsoft** en **ODP** (standaard). Zie [ondersteunde versie en installatie](#supported-versions-and-installation) voor stuurprogramma voor meer informatie. | Nee |
| connectionString | Geef de informatie die nodig is om te verbinden met de Oracle-database-instantie voor het **connectionString** eigenschap. | Ja |
| gatewayName | De naam van de gateway die wordt gebruikt voor verbinding met de on-premises Oracle-server. |Ja |

**Voorbeeld: Met behulp van de Microsoft-stuurprogramma**

> [!TIP]
> Als er een fout met de tekst "ORA 01025: UPI parameter buiten het bereik' en de Oracle-versie 8i is, voegen `WireProtocolMode=1` met de verbindingstekenreeks en probeer het opnieuw:

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<service ID>;User Id=<user name>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Voorbeeld: Met behulp van het stuurprogramma ODP**

Zie voor meer informatie over de toegestane indelingen, [Oracle-gegevensprovider voor .NET ODP](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/).

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<host name>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<service ID>))); User Id=<user name>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets en secties, [gegevenssets maken](data-factory-create-datasets.md).

De secties van een gegevensset JSON-bestand, zoals de structuur, beschikbaarheid en het beleid, zijn identiek voor alle gegevenssettypen (bijvoorbeeld voor Oracle, Azure-blobopslag en Azure Table storage).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De **typeProperties** sectie voor de gegevensset van het type **OracleTable** heeft de volgende eigenschappen:

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in de Oracle-database waarnaar de gekoppelde service verwijst. |Nee (als **oracleReaderQuery** of **OracleSource** is opgegeven) |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een volledige lijst van eigenschappen die beschikbaar zijn voor het definiëren van activiteiten en secties, [het maken van pijplijnen](data-factory-create-pipelines.md).

Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle soorten activiteiten.

> [!NOTE]
> Kopieeractiviteit slechts één invoer en produceert slechts één uitvoer.

Eigenschappen die beschikbaar zijn in de **typeProperties** sectie van de activiteit variëren met elk activiteitstype. Eigenschappen van de activiteit kopiëren variëren afhankelijk van het type bron- en sinkblobpaden.

### <a name="oraclesource"></a>OracleSource

In de Kopieeractiviteit, wanneer de bron is de **OracleSource** type, de volgende eigenschappen zijn beschikbaar in de **typeProperties** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| oracleReaderQuery |De aangepaste query gebruiken om gegevens te lezen. |Een SQL-query-tekenreeks. Bijvoorbeeld, "Selecteer \* van **MyTable**'. <br/><br/>Als niet is opgegeven, wordt deze SQL-instructie uitgevoerd: ' Selecteer \* van **MyTable**" |Nee<br />(als **tableName** van **gegevensset** is opgegeven) |

### <a name="oraclesink"></a>OracleSink

**OracleSink** ondersteunt de volgende eigenschappen:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| writeBatchTimeout |De wachttijd voor de batch invoegen bewerking is voltooid voordat er een optreedt time-out. |**timespan**<br/><br/> Voorbeeld: 00:30:00 uur (30 minuten) |Nee |
| WriteBatchSize |Gegevens invoegen in de SQL-tabel wanneer de waarde van de buffergrootte is bereikt **writeBatchSize**. |Geheel getal (aantal rijen) |Nee (standaard: 100) |
| sqlWriterCleanupScript |Hiermee geeft u een query voor de Kopieeractiviteit om uit te voeren zodat de gegevens van een bepaald segment wordt opgeschoond. |Een query-instructie. |Nee |
| sliceIdentifierColumnName |Hiermee geeft u de naam van de kolom voor de Kopieeractiviteit in te vullen met een automatisch gegenereerde segment-id. De waarde voor **sliceIdentifierColumnName** wordt gebruikt voor het opschonen van gegevens van een bepaald segment wanneer opnieuw uitgevoerd. |De naam van de kolom van een kolom met gegevenstype van **binary(32)**. |Nee |

## <a name="json-examples-for-copying-data-to-and-from-the-oracle-database"></a>JSON-voorbeelden voor het kopiëren van gegevens naar en van de Oracle-database

De volgende voorbeelden geven een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van de [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). De voorbeelden laten zien hoe het kopiëren van gegevens van of met een Oracle-database en van of naar Azure Blob-opslag. Gegevens kunnen echter worden gekopieerd naar een van de sinks die worden vermeld in [ondersteunde gegevensarchieven en indelingen](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

**Voorbeeld: Gegevens kopiëren van Oracle naar Azure Blob-opslag**

Het voorbeeld heeft de volgende Data Factory-entiteiten:

* Een gekoppelde service van het type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Een [pijplijn](data-factory-create-pipelines.md) met een kopieeractiviteit die gebruikmaakt van [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) als bron en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) als sink.

Het voorbeeld worden gegevens gekopieerd van een tabel in een on-premises Oracle-database naar een blob per uur. Zie de secties die volgen op de voorbeelden voor meer informatie over diverse eigenschappen die worden gebruikt in het voorbeeld.

**Oracle gekoppelde service**

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<service ID>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Azure Blob storage-gekoppelde service**

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
        }
    }
}
```

**Oracle-invoergegevensset**

Het voorbeeld wordt ervan uitgegaan dat u hebt gemaakt aan een tabel met de naam **MyTable** in Oracle. Deze bevat een kolom met de naam **timestampcolumn** voor time series-gegevens.

Instellen van **externe**: **waar** informeert de Data Factory-service dat de gegevensset niet tot de data factory is en dat de gegevensset is niet door een activiteit in de data factory gemaakt.

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

**Azure Blob-uitvoergegevensset**

Gegevens worden geschreven naar een nieuwe blob elk uur (**frequentie**: **uur**, **interval**: **1**). Pad en naam van de map voor de blob worden dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map maakt gebruik van het jaar, maand, dag en uur deel uit van de begintijd.

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

**Pijplijn met een kopieeractiviteit**

De pijplijn bevat een kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en gepland om te worden elk uur wordt uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **OracleSource** en de **sink** type is ingesteld op **BlobSink**. De SQL-query die u met behulp van opgeeft de **oracleReaderQuery** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

```json
{
    "name":"SamplePipeline",
    "properties":{
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for a copy activity",
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

**Voorbeeld: Gegevens uit Azure Blob storage kopiëren naar Oracle**

Dit voorbeeld laat zien hoe u gegevens kopiëren van een Azure Blob storage-account naar een on-premises Oracle-database. U kunt echter gegevens kopiëren *rechtstreeks* uit een van de bronnen die worden vermeld in [ondersteunde gegevensarchieven en indelingen](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

Het voorbeeld heeft de volgende Data Factory-entiteiten:

* Een gekoppelde service van het type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
* Een [pijplijn](data-factory-create-pipelines.md) waarvoor een kopieeractiviteit die gebruikmaakt van [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) als bron [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) als sink.

Het voorbeeld worden gegevens gekopieerd van een blob naar een tabel in een on-premises Oracle-database om het uur. Zie de secties die volgen op de voorbeelden voor meer informatie over diverse eigenschappen die worden gebruikt in het voorbeeld.

**Oracle gekoppelde service**

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<host name>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<service ID>)));
            User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Azure Blob storage-gekoppelde service**

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
        }
    }
}
```

**De Azure Blob-invoergegevensset**

Gegevens wordt opgehaald uit een nieuwe blob elk uur (**frequentie**: **uur**, **interval**: **1**). Pad en naam van de map voor de blob worden dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map maakt gebruik van het jaar, maand en dag deel uit van de begintijd. Naam van het bestand maakt gebruik van het uur deel van de begintijd. De instelling **externe**: **waar** de Data Factory-service wordt geïnformeerd dat deze tabel bevindt zich buiten de data factory en niet door een activiteit in de data factory wordt geproduceerd.

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

**Oracle-uitvoergegevensset**

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel met de naam gemaakt **MyTable** in Oracle. Maak de tabel in Oracle met hetzelfde aantal kolommen die u verwacht dat de blob CSV-bestand bevatten. Nieuwe rijen zijn toegevoegd aan de tabel om het uur.

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

**Pijplijn met een kopieeractiviteit**

De pijplijn bevat een kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en gepland om te worden elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **BlobSource** en de **sink** type is ingesteld op **OracleSink**.

```json
{
    "name":"SamplePipeline",
    "properties":{
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-05T19:00:00",
        "description":"pipeline with a copy activity",
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

**Foutbericht**

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable to find the requested .NET Framework Data Provider. It may not be installed.

**Mogelijke oorzaken**

* De .NET Framework Data Provider voor Oracle is niet geïnstalleerd.
* De .NET Framework Data Provider voor Oracle naar .NET Framework 2.0 is geïnstalleerd en is niet gevonden in de .NET Framework 4.0-mappen.

**Resolutie**

* Als u de .NET-Provider voor Oracle, nog niet hebt geïnstalleerd [installeren](https://www.oracle.com/technetwork/topics/dotnet/downloads/), en probeer het scenario.
* Als u het foutbericht ziet, zelfs nadat u de provider hebt geïnstalleerd, kunt u de volgende stappen:
    1. Open het configuratiebestand van de machine voor .NET 2.0 uit de map < systeemschijf\>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
    2. Zoeken naar **Oracle-gegevensprovider voor .NET**. U moet kunnen geen vermelding vinden, zoals wordt weergegeven in het volgende voorbeeld onder **systeem.gegevens** > **DbProviderFactories**: `<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />`
* Deze vermelding kopiëren naar het bestand machine.config in de volgende .NET 4.0-map: < systeemschijf\>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config. Wijzig de versie in 4.xxx.x.x.
* Installeer < ODP.NET geïnstalleerd pad\>\11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll in de global assembly-cache (GAC) door te voeren **gacutil /i [providerpad]**.

### <a name="problem-2-datetime-formatting"></a>Probleem 2: Datum/tijd opmaken

**Foutbericht**

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

**Resolutie**

Mogelijk moet u de gebruikte queryreeks in de kopieeractiviteit op basis van hoe datums zijn geconfigureerd in uw Oracle-database aanpassen. Hier volgt een voorbeeld (met behulp van de **to_date** functie):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


## <a name="type-mapping-for-oracle"></a>Toewijzing van het type voor Oracle

Zoals vermeld in [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md), Kopieeractiviteit wordt uitgevoerd automatisch typeconversies van typen gegevensbronnen met sink-typen met behulp van de volgende benadering voor verificatie in twee stappen:

1. Converteren van systeemeigen brontypen naar het .NET-type.
2. Converteren van het .NET-type naar het systeemeigen sink-type.

Wanneer u gegevens van Oracle hebt verplaatst, worden de volgende toewijzingen van de Oracle-gegevenstype naar het .NET-type en vice versa gebruikt:

| Oracle-gegevenstype | .NET framework-gegevenstype |
| --- | --- |
| BBESTAND |Byte[] |
| BLOB |Byte[]<br/>(alleen ondersteund op Oracle 10g en latere versies wanneer u een stuurprogramma van Microsoft) |
| CHAR |String |
| CLOB |String |
| DATE |DateTime |
| FLOAT |Decimaal getal, tekenreeks (als precisie > 28) |
| GEHEEL GETAL ZIJN |Decimaal getal, tekenreeks (als precisie > 28) |
| INTERVAL VOOR JAAR TOT MAAND |Int32 |
| TWEEDE INTERVAL DAG |TimeSpan |
| LONG |String |
| LANGE ONBEWERKTE |Byte[] |
| NCHAR |String |
| NCLOB |String |
| AANTAL |Decimaal getal, tekenreeks (als precisie > 28) |
| NVARCHAR2 |String |
| RAW |Byte[] |
| ROWID |String |
| TIMESTAMP |DateTime |
| TIJDSTEMPEL MET DE LOKALE TIJDZONE |DateTime |
| TIJDSTEMPEL MET TIJDZONE |DateTime |
| GEHEEL GETAL ZONDER TEKEN |Aantal |
| VARCHAR2 |String |
| XML |String |

> [!NOTE]
> Gegevenstypen **INTERVAL jaar aan maand** en **INTERVAL dag aan tweede** worden niet ondersteund wanneer u een stuurprogramma van Microsoft.

## <a name="map-source-to-sink-columns"></a>Kaartbron met sink-kolommen

Zie voor meer informatie over het toewijzen van kolommen in de brongegevensset naar kolommen in de sink-gegevensset, [toewijzing van de kolommen van de gegevensset in Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Herhaalbare lezen van relationele bronnen

Wanneer u gegevens van op opgeslagen relationele gegevens kopieert, houd er rekening mee om te voorkomen dat ongewenste resultaten herhaalbaarheid. In Azure Data Factory, kunt u een segment handmatig opnieuw uitvoeren. U kunt ook een beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment wordt opnieuw uitgevoerd, ofwel handmatig of door een beleid voor opnieuw proberen, zorg ervoor dat dezelfde gegevens wordt lezen, ongeacht hoe vaak een segment uitgevoerd. Zie voor meer informatie, [Repeatable leesrechten voor relationele bronnen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen

Zie de [Kopieeractiviteit prestatie- en afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed hebben op de prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory. U kunt ook meer informatie over de verschillende manieren om te optimaliseren.
