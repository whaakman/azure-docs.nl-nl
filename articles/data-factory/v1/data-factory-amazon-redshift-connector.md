---
title: Gegevens verplaatsen van Amazon Redshift met behulp van Azure Data Factory | Microsoft Docs
description: Informatie over het verplaatsen van gegevens uit Amazon Redshift met behulp van Azure Data Factory-Kopieeractiviteit.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 01d15078-58dc-455c-9d9d-98fbdf4ea51e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 47a9feaa692eaf048371b4e534e6b2e8c4086997
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Verplaatsen van gegevens vanaf Amazon Redshift, met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-amazon-redshift-connector.md)
> * [Versie 2 - Preview](../connector-amazon-redshift.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [Redshift Amazon-connector in V2](../connector-amazon-redshift.md).

In dit artikel wordt uitgelegd hoe de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van Amazon Redshift gebruiken. Het artikel is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, hetgeen een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit toont. 

Data Factory ondersteunt momenteel alleen zwevend gegevens vanaf Amazon Redshift naar een [ondersteunde sink gegevensarchief](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Verplaatsen van gegevens uit andere gegevensarchieven naar Amazon Redshift wordt niet ondersteund.

> [!TIP]
> Als u wilt de beste prestaties bereiken bij het kopiëren van grote hoeveelheden gegevens vanaf Amazon Redshift, overweeg het gebruik van de ingebouwde Redshift **UNLOAD** opdracht via Amazon eenvoudige Storage-Service (Amazon S3). Zie voor meer informatie [UNLOAD gebruiken om gegevens te kopiëren van Amazon Redshift](#use-unload-to-copy-data-from-amazon-redshift).

## <a name="prerequisites"></a>Vereisten
* Als u gegevens naar een on-premises gegevensopslag verplaatst, installeert u [Data Management Gateway](data-factory-data-management-gateway.md) op een on-premises machine. Toegang voor een gateway aan het cluster Amazon Redshift verlenen met behulp van de IP-adres van de lokale computer. Zie voor instructies [toegang verlenen aan het cluster](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html).
* Om gegevens te verplaatsen naar een Azure data store, Zie de [Compute IP-adres en de SQL-bereiken die worden gebruikt door de Microsoft Azure-Datacenters](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit om gegevens te verplaatsen van een bron Amazon Redshift met behulp van verschillende hulpprogramma's en API's.

Er is de eenvoudigste manier om een pijplijn maken met de Wizard kopiëren van Azure Data Factory. Zie voor een snel overzicht over het maken van een pijplijn met behulp van de Wizard kopiëren, de [zelfstudie: een pijplijn maken met behulp van de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md).

U kunt ook een pijplijn maken met behulp van de Azure-portal, Visual Studio, Azure PowerShell of andere hulpprogramma's. Azure Resource Manager-sjablonen, de .NET API of de REST-API kan ook worden gebruikt voor het maken van de pijplijn. Zie voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit de [Kopieeractiviteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren: 

1. Gekoppelde services om te koppelen van de invoer en uitvoer van de opgeslagen gegevens aan uw gegevensfactory maken.
2. Maak gegevenssets vertegenwoordigen de invoer- en -gegevens voor de kopieerbewerking. 
3. Een pijplijn maken met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de Wizard kopiëren gebruikt, worden JSON-definities voor deze Data Factory-entiteiten worden automatisch gemaakt. Wanneer u hulpprogramma's of API's (met uitzondering van de .NET API) gebruikt, kunt u de Data Factory-entiteiten definiëren met behulp van de JSON-indeling. De [JSON-voorbeeld: gegevens kopiëren van Amazon Redshift naar Azure Blob storage](#json-example-copy-data-from-amazon-redshift-to-azure-blob) ziet u de JSON-definities voor de Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een gegevensopslag Amazon Redshift.

De volgende secties beschrijven de JSON-eigenschappen die worden gebruikt voor het definiëren van de Data Factory-entiteiten voor Amazon Redshift.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende tabel bevat beschrijvingen van de JSON-elementen die specifiek voor een service Amazon Redshift gekoppeld zijn.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| **type** |Deze eigenschap moet worden ingesteld op **AmazonRedshift**. |Ja |
| **server** |Het IP-adres of de hostnaam naam van de Amazon Redshift-server. |Ja |
| **poort** |Het nummer van de TCP-poort die de Amazon Redshift-server gebruikt om te luisteren naar verbindingen van clients. |Nee (de standaardwaarde is 5439) |
| **database** |De naam van de Amazon Redshift-database. |Ja |
| **gebruikersnaam** |De naam van de gebruiker die toegang tot de database heeft. |Ja |
| **wachtwoord** |Het wachtwoord voor het gebruikersaccount. |Ja |

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. De **structuur**, **beschikbaarheid**, en **beleid** secties zijn identiek voor alle typen van de gegevensset. Voorbeelden van typen gegevensset zijn Azure SQL Azure Blob storage en Azure Table storage.

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het archief. **De typeProperties** sectie voor een gegevensset van het type **RelationalTable**, waaronder de gegevensset Amazon Redshift heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| **tableName** |De naam van de tabel in de Amazon Redshift database waarnaar de gekoppelde service verwijst. |Nee (als de **query** eigenschap van de kopieeractiviteit van een van het type **RelationalSource** is opgegeven) |

## <a name="copy-activity-properties"></a>Eigenschappen van de activiteit kopiëren

Zie voor een lijst met eigenschappen die beschikbaar zijn voor het definiëren van activiteiten en secties, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. De **naam**, **beschrijving**, **invoer** tabel **levert** tabel en **beleid** eigenschappen zijn beschikbaar voor alle typen activiteiten. De eigenschappen die beschikbaar zijn in de **typeProperties** sectie verschillen voor elk activiteitstype. Voor de Kopieeractiviteit, wordt de eigenschappen variëren afhankelijk van de soorten gegevensbronnen en Put.

Voor de Kopieeractiviteit, wanneer de bron van het type **AmazonRedshiftSource**, de volgende eigenschappen beschikbaar zijn in **typeProperties** sectie:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| **query** | Gebruik de aangepaste query om de gegevens te lezen. |Nee (als de **tableName** eigenschap van een dataset is opgegeven) |
| **redshiftUnloadSettings** | Bevat de eigenschappengroep bij gebruik van de Redshift **UNLOAD** opdracht. | Nee |
| **s3LinkedServiceName** | De Amazon S3 om te gebruiken als een tussentijdse store. De gekoppelde service wordt opgegeven met behulp van een Azure Data Factory-naam van het type **AwsAccessKey**. | Vereist bij gebruik van de **redshiftUnloadSettings** eigenschap |
| **bucketName** | Hiermee geeft u de Amazon S3-bucket te gebruiken voor het opslaan van de tijdelijke gegevens. Als deze eigenschap niet is opgegeven, Kopieeractiviteit auto-genereert een bucket. | Vereist bij gebruik van de **redshiftUnloadSettings** eigenschap |

U kunt ook de **RelationalSource** type, waaronder Redshift Amazon, met de volgende eigenschap in de **typeProperties** sectie. Dit type gegevensbron biedt geen ondersteuning voor de Redshift Opmerking **UNLOAD** opdracht.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| **query** |Gebruik de aangepaste query om de gegevens te lezen. | Nee (als de **tableName** eigenschap van een dataset is opgegeven) |

## <a name="use-unload-to-copy-data-from-amazon-redshift"></a>UNLOAD gebruiken om gegevens van Amazon Redshift kopiëren

De Amazon-Redshift [ **UNLOAD** ](http://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) opdracht wordt de resultaten van een query naar een of meer bestanden op de Amazon S3 verwijderd. Met deze opdracht wordt aanbevolen door Amazon voor het kopiëren van grote gegevenssets uit Redshift.

**Voorbeeld: Gegevens kopiëren van Amazon Redshift naar Azure SQL Data Warehouse**

In dit voorbeeld kopieert gegevens van Amazon Redshift naar Azure SQL Data Warehouse. In het voorbeeld wordt de Redshift **UNLOAD** opdracht, gegevens gefaseerde kopiëren en Microsoft PolyBase.

Voor deze gebruiksvoorbeeld voorbeeld Kopieeractiviteit eerst verwijderd de gegevens van Amazon Redshift met Amazon S3 zoals geconfigureerd in de **redshiftUnloadSettings** optie. Vervolgens de gegevens vanaf Amazon S3 wordt gekopieerd naar Azure Blob-opslag, zoals opgegeven in de **stagingSettings** optie. Ten slotte PolyBase de gegevens geladen in SQL Data Warehouse. Alle van de tijdelijke indelingen worden verwerkt door de kopieerbewerking.

![Werkstroom van Amazon Redshift kopiëren naar SQL Data Warehouse](media\data-factory-amazon-redshift-connector\redshift-to-sql-dw-copy-workflow.png)

```json
{
    "name": "CopyFromRedshiftToSQLDW",
    "type": "Copy",
    "typeProperties": {
        "source": {
            "type": "AmazonRedshiftSource",
            "query": "select * from MyTable",
            "redshiftUnloadSettings": {
                "s3LinkedServiceName":"MyAmazonS3StorageLinkedService",
                "bucketName": "bucketForUnload"
            }
        },
        "sink": {
            "type": "SqlDWSink",
            "allowPolyBase": true
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyAzureStorageLinkedService",
            "path": "adfstagingcopydata"
        },
        "cloudDataMovementUnits": 32
        .....
    }
}
```

## <a name="json-example-copy-data-from-amazon-redshift-to-azure-blob-storage"></a>JSON-voorbeeld: gegevens kopiëren van Amazon Redshift naar Azure Blob-opslag
Dit voorbeeld laat zien hoe gegevens uit een database Amazon Redshift kopiëren naar Azure Blob Storage. Gegevens kunnen worden gekopieerd naar een [sink ondersteund](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit.  

Het voorbeeld heeft de volgende data factory-entiteiten:

* Een gekoppelde service van het type [AmazonRedshift](#linked-service-properties)
* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [RelationalTable](#dataset-properties)
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* Een [pijplijn](data-factory-create-pipelines.md) met een kopieeractiviteit die gebruikmaakt van de [RelationalSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties) eigenschappen

Het voorbeeld kopieert gegevens van de resultaten van een query in Amazon Redshift naar een Azure-blob per uur. De JSON-eigenschappen die worden gebruikt in de steekproef die worden beschreven in de secties die, de definities van de entiteit volgen.

**Amazon Redshift gekoppelde service**

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "< The IP address or host name of the Amazon Redshift server >",
            "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
            "database": "<The database name of the Amazon Redshift database>",
            "username": "<username>",
            "password": "<password>"
        }
    }
}
```

**Azure Blob-opslag gekoppelde service**

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
**Amazon Redshift invoergegevensset**

De **externe** eigenschap is ingesteld op 'true' om te informeren over de Data Factory-service dat de dataset buiten de gegevensfactory is. Deze instelling geeft aan dat de gegevensset niet wordt geproduceerd door een activiteit in de gegevensfactory. De eigenschap instelt op true in een invoergegevensset die niet wordt geproduceerd door een activiteit in de pijplijn.

```json
{
    "name": "AmazonRedshiftInputDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "AmazonRedshiftLinkedService",
        "typeProperties": {
            "tableName": "<Table name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

**Azure Blob-uitvoergegevensset**

Gegevens worden geschreven naar een nieuwe blob elk uur door in te stellen de **frequentie** eigenschap in op "Uur" en de **interval** eigenschap in op 1. De **folderPath** eigenschap voor de blob dynamisch wordt geëvalueerd. De eigenschapswaarde is gebaseerd op de begintijd van het segment dat wordt verwerkt. Het mappad maakt gebruik van het jaar, maand, dag en uur delen van de begintijd.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kopieeractiviteit in een pijplijn met een bron Azure Redshift (van het type RelationalSource) en een Azure Blob-sink**

De pijplijn bevat een kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets. De pijplijn is gepland voor elk uur uitgevoerd. In de JSON-definitie voor de pijplijn de **bron** type is ingesteld op **RelationalSource** en de **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **query** eigenschap selecteert u de gegevens voor het kopiëren van het afgelopen uur.

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "AmazonRedshiftSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)",
                        "redshiftUnloadSettings": {
                            "s3LinkedServiceName":"myS3Storage",
                            "bucketName": "bucketForUnload"
                        }
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    },
                    "cloudDataMovementUnits": 32
                },
                "inputs": [
                    {
                        "name": "AmazonRedshiftInputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
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
                "name": "AmazonRedshiftToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-amazon-redshift"></a>Toewijzing van het type voor Amazon Redshift
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel Kopieeractiviteit voert automatische typeconversies van brontype naar sink-type. De typen worden geconverteerd met behulp van een benadering in twee stappen:

1. Converteren van een systeemeigen brontype naar een .NET-type
2. Converteren van een .NET-type naar een systeemeigen sink-type

De volgende toewijzingen worden gebruikt wanneer de Kopieeractiviteit converteert de gegevens van een type Amazon Redshift naar een .NET-type:

| Amazon Redshift type | .NET-type |
| --- | --- |
| SMALLINT |Int16 |
| GEHEEL GETAL |Int32 |
| BIGINT |Int64 |
| DECIMAL |Decimale |
| ECHTE |Single |
| DUBBELE PRECISIE |Double |
| BOOLEAANSE WAARDE |Tekenreeks |
| CHAR |Tekenreeks |
| VARCHAR |Tekenreeks |
| DATE |Datum en tijd |
| TIJDSTEMPEL |Datum en tijd |
| TEXT |Tekenreeks |

## <a name="map-source-to-sink-columns"></a>Bron van de kaart opvangen kolommen
Zie voor informatie over het toewijzen van kolommen in de bron-gegevensset aan kolommen in de gegevensset sink, [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-reads-from-relational-sources"></a>Herhaalbare leesbewerkingen van relationele bronnen
Wanneer u gegevens uit een relationele gegevensopslag kopiëren, moet rekening houden om te voorkomen dat ongewenste resultaten herhaalbaarheid. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook configureren met de nieuwe poging **beleid** voor een gegevensset naar een segment opnieuw uitvoeren wanneer een fout optreedt. Zorg ervoor dat de dezelfde gegevens gelezen, ongeacht hoe vaak het segment opnieuw uit. Zorg ook dat dezelfde gegevens is gelezen ongeacht hoe u het segment opnieuw uitvoeren. Zie voor meer informatie [Repeatable leest uit relationele bronnen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Meer informatie over de belangrijkste factoren die invloed hebben op de prestaties van de Kopieeractiviteit en manieren om te optimaliseren in de [handleiding afstemmen en uitvoering van de activiteit](data-factory-copy-activity-performance.md). 

## <a name="next-steps"></a>Volgende stappen
Zie voor stapsgewijze instructies voor het maken van een pijplijn met de Kopieeractiviteit de [Kopieeractiviteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
