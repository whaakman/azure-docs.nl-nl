---
title: Gegevens verplaatsen van Amazon Redshift met behulp van Azure Data Factory | Microsoft Docs
description: Leer hoe u gegevens van Amazon Redshift verplaatsen met behulp van Azure Data Factory Copy-activiteit.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 01d15078-58dc-455c-9d9d-98fbdf4ea51e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 3a1497211cc42c702537cbbdfea32ff71a400c7c
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836688"
---
# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Gegevens verplaatsen van Amazon Redshift, met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van Data Factory-service die u gebruikt:"]
> * [Versie 1:](data-factory-amazon-redshift-connector.md)
> * [Versie 2 (huidige versie)](../connector-amazon-redshift.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [Amazon Redshift-connector in V2](../connector-amazon-redshift.md).

In dit artikel wordt uitgelegd hoe u van de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van Amazon Redshift. Het artikel is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit geeft.

Data Factory ondersteunt momenteel alleen gegevens te verplaatsen van Amazon Redshift naar een [ondersteunde sink-gegevensopslag](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Verplaatsen van gegevens uit andere gegevensarchieven naar Amazon Redshift wordt niet ondersteund.

> [!TIP]
> Voor het bereiken van de beste prestaties bij het kopiëren van grote hoeveelheden gegevens van Amazon Redshift, overweeg het gebruik van de ingebouwde Redshift **UNLOAD** opdracht via de Amazon Simple Storage-Service (Amazon S3). Zie voor meer informatie, [gebruik laden ongedaan maken om gegevens te kopiëren van Amazon Redshift](#use-unload-to-copy-data-from-amazon-redshift).

## <a name="prerequisites"></a>Vereisten
* Als u gegevens naar een on-premises-gegevensarchief verplaatst, installeert u [Data Management Gateway](data-factory-data-management-gateway.md) op een on-premises machine. Toegangsmachtigingen voor een gateway met de Amazon Redshift-cluster met behulp van de IP-adres van de lokale computer. Zie voor instructies [verleent toegang tot het cluster](https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html).
* Om gegevens te verplaatsen naar een Azure data store, Zie de [Compute IP-adres en de SQL-bereiken die worden gebruikt door de Microsoft Azure-Datacenters](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit om gegevens te verplaatsen van een Amazon Redshift-bron met behulp van verschillende hulpprogramma's en API's.

Er is de eenvoudigste manier om een pijplijn te maken met de Kopieerwizard van Azure Data Factory. Zie voor een snel overzicht over het maken van een pijplijn met behulp van de Wizard kopiëren, de [zelfstudie: Een pijplijn maken met behulp van de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md).

U kunt ook een pijplijn maken met behulp van Visual Studio, Azure PowerShell of andere hulpprogramma's. Azure Resource Manager-sjablonen, de .NET-API of de REST-API kan ook worden gebruikt om de pijplijn te maken. Zie voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit de [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren:

1. Maak gekoppelde services om te koppelen van de invoer en uitvoer gegevensarchieven aan uw data factory.
2. Gegevenssets vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking maken.
3. Een pijplijn maken met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer.

Wanneer u de Wizard kopiëren hebt gebruikt, worden automatisch JSON-definities voor deze Data Factory-entiteiten gemaakt. Wanneer u hulpprogramma's of API's (met uitzondering van de .NET API), kunt u de Data Factory-entiteiten definiëren met behulp van de JSON-indeling. De JSON-voorbeeld: Gegevens kopiëren van Amazon Redshift naar Azure Blob-opslag ziet u de JSON-definities voor de Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren naar een Amazon Redshift-gegevensarchief.

De volgende secties beschrijven de JSON-eigenschappen die worden gebruikt voor het definiëren van de Data Factory-entiteiten voor Amazon Redshift.

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service

De volgende tabel bevat beschrijvingen voor de JSON-elementen die specifiek voor een Amazon Redshift gekoppelde service zijn.

| Eigenschap | Description | Verplicht |
| --- | --- | --- |
| **type** |Deze eigenschap moet worden ingesteld op **AmazonRedshift**. |Ja |
| **server** |De IP-adres of de hostnaam naam van de Amazon Redshift-server. |Ja |
| **Poort** |Het nummer van de TCP-poort die gebruikmaakt van de Amazon Redshift-server om te luisteren naar clientverbindingen. |Nee (de standaardinstelling is 5439) |
| **database** |De naam van de Amazon Redshift-database. |Ja |
| **gebruikersnaam** |De naam van de gebruiker die toegang tot de database heeft. |Ja |
| **Wachtwoord** |Het wachtwoord voor het gebruikersaccount. |Ja |

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie voor een lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. De **structuur**, **beschikbaarheid**, en **beleid** secties zijn vergelijkbaar voor alle typen van de gegevensset. Voorbeelden van gegevenssettypen zijn SQL Azure, Azure Blob-opslag en Azure-tabelopslag.

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in de store. **De typeProperties** sectie voor een gegevensset van het type **RelationalTable**, waaronder de Amazon Redshift-gegevensset, heeft de volgende eigenschappen:

| Eigenschap | Description | Verplicht |
| --- | --- | --- |
| **tableName** |De naam van de tabel in de Amazon Redshift-database waarnaar de gekoppelde service verwijst. |Nee (als de **query** eigenschap van de kopieeractiviteit van een van het type **RelationalSource** is opgegeven) |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie voor een lijst van eigenschappen die beschikbaar zijn voor het definiëren van activiteiten en secties, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. De **naam**, **beschrijving**, **invoer** tabel **levert** tabel, en **beleid** eigenschappen zijn beschikbaar voor alle soorten activiteiten. De eigenschappen die beschikbaar zijn in de **typeProperties** sectie verschillen voor elk activiteitstype. Voor de Kopieeractiviteit, wordt de eigenschappen variëren afhankelijk van de typen gegevensbronnen en sinks.

Voor de Kopieeractiviteit, wanneer de bron van het type **AmazonRedshiftSource**, de volgende eigenschappen zijn beschikbaar in **typeProperties** sectie:

| Eigenschap | Description | Verplicht |
| --- | --- | --- |
| **query** | De aangepaste query gebruiken om de gegevens te lezen. |Nee (als de **tableName** eigenschap van een gegevensset is opgegeven) |
| **redshiftUnloadSettings** | Bevat de eigenschapgroep bij het gebruik van de Redshift **UNLOAD** opdracht. | Nee |
| **s3LinkedServiceName** | De Amazon S3 om te gebruiken als een tijdelijke opslag. De gekoppelde service is opgegeven met behulp van een Azure Data Factory-naam van het type **AwsAccessKey**. | Vereist als u de **redshiftUnloadSettings** eigenschap |
| **bucketName** | Geeft aan dat de Amazon S3-bucket te gebruiken voor het opslaan van de tussentijdse gegevens. Als deze eigenschap niet is opgegeven, Copy Activity auto-genereert een bucket. | Vereist als u de **redshiftUnloadSettings** eigenschap |

U kunt ook kunt u de **RelationalSource** type, waaronder Amazon Redshift, met de volgende eigenschap in de **typeProperties** sectie. Houd er rekening mee de Redshift biedt geen ondersteuning voor dit brontype **UNLOAD** opdracht.

| Eigenschap | Description | Verplicht |
| --- | --- | --- |
| **query** |De aangepaste query gebruiken om de gegevens te lezen. | Nee (als de **tableName** eigenschap van een gegevensset is opgegeven) |

## <a name="use-unload-to-copy-data-from-amazon-redshift"></a>Laden ongedaan maken om gegevens te kopiëren van Amazon Redshift gebruiken

De Amazon Redshift [ **UNLOAD** ](https://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) opdracht wordt verwijderd van de resultaten van een query op een of meer bestanden op Amazon S3. Met deze opdracht wordt door Amazon aanbevolen voor het kopiëren van grote gegevenssets van Redshift.

**Voorbeeld: Gegevens kopiëren van Amazon Redshift naar Azure SQL Data Warehouse**

In dit voorbeeld kopieert gegevens van Amazon Redshift naar Azure SQL Data Warehouse. Het voorbeeld wordt de Redshift **UNLOAD** opdracht gefaseerd kopiëren van gegevens en Microsoft PolyBase.

Voor dit voorbeeld use-case Kopieeractiviteit eerst verwijdert de gegevens van Amazon Redshift met Amazon S3 zoals geconfigureerd in de **redshiftUnloadSettings** optie. Vervolgens de gegevens vanaf Amazon S3 is gekopieerd naar Azure Blob-opslag, zoals opgegeven in de **stagingSettings** optie. Ten slotte PolyBase de gegevens in SQL Data Warehouse worden geladen. Alle van de tussentijdse indelingen worden afgehandeld door de Kopieeractiviteit.

![Werkstroom kopiëren van Amazon Redshift naar SQL Data Warehouse](media/data-factory-amazon-redshift-connector/redshift-to-sql-dw-copy-workflow.png)

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

## <a name="json-example-copy-data-from-amazon-redshift-to-azure-blob-storage"></a>JSON-voorbeeld: Gegevens kopiëren van Amazon Redshift naar Azure Blob-opslag
Dit voorbeeld laat zien hoe u gegevens kopiëren van een Amazon Redshift-database naar Azure Blob Storage. Gegevens kunnen worden gekopieerd naar een [ondersteund sink](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit.

Het voorbeeld heeft de volgende data factory-entiteiten:

* Een gekoppelde service van het type [AmazonRedshift](#linked-service-properties)
* Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Invoer [gegevensset](data-factory-create-datasets.md) van het type [RelationalTable](#dataset-properties)
* Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)
* Een [pijplijn](data-factory-create-pipelines.md) met een kopieeractiviteit die gebruikmaakt van de [RelationalSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties) eigenschappen

Het voorbeeld worden gegevens gekopieerd van de resultaten van een query in de Amazon Redshift naar een Azure-blob per uur. De JSON-eigenschappen die worden gebruikt in het voorbeeld worden beschreven in de secties die volgen op de entiteitdefinities.

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
            "port": "<The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>",
            "database": "<The database name of the Amazon Redshift database>",
            "username": "<username>",
            "password": "<password>"
        }
    }
}
```

**Azure Blob storage-gekoppelde service**

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
**Amazon Redshift-invoergegevensset**

De **externe** eigenschap is ingesteld op 'true' om te informeren over de Data Factory-service dat de dataset bevindt zich buiten de data factory. Deze instelling geeft aan dat de gegevensset niet wordt geproduceerd door een activiteit in de data factory. De eigenschap ingesteld op true voor een invoergegevensset die niet door een activiteit in de pijplijn wordt geproduceerd.

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

Gegevens worden geschreven naar een nieuwe blob elk uur door in te stellen de **frequentie** eigenschap in op 'Uur' en de **interval** eigenschap in op 1. De **folderPath** eigenschap voor de blob wordt dynamisch geëvalueerd. De eigenschapswaarde is gebaseerd op de begintijd van het segment dat wordt verwerkt. Pad naar de map maakt gebruik van het jaar, maand, dag en uur onderdelen van de begintijd.

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

**Copy-activiteit in een pijplijn met een Azure-Redshift-bron (van het type RelationalSource) en een Azure Blob-sink**

De pijplijn bevat een kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets. De pijplijn is gepland voor elk uur uitgevoerd. In de JSON-definitie voor de pijplijn de **bron** type is ingesteld op **RelationalSource** en de **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **query** eigenschap selecteert u de gegevens te kopiëren van het afgelopen uur.

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
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) Copy Activity-artikel voert automatische conversie van het brontype naar het sink-type. De typen worden geconverteerd met behulp van een benadering met twee stappen:

1. Van een systeemeigen gegevenstype niet converteren naar een .NET-type
2. Converteren van een .NET-type naar een systeemeigen sink-type

De volgende toewijzingen worden gebruikt wanneer de Kopieeractiviteit converteert de gegevens van een Amazon Redshift naar een .NET-type:

| Amazon Redshift-type | .NET-type |
| --- | --- |
| SMALLINT |Int16 |
| INTEGER |Int32 |
| BIGINT |Int64 |
| DECIMAL |Decimal |
| REAL |Single |
| DOUBLE PRECISION |Double |
| BOOLEAN |Tekenreeks |
| CHAR |Tekenreeks |
| VARCHAR |Reeks |
| DATE |Datetime |
| TIMESTAMP |Datetime |
| TEXT |Tekenreeks |

## <a name="map-source-to-sink-columns"></a>Kaartbron met sink-kolommen
Zie voor informatie over het toewijzen van kolommen in de brongegevensset naar kolommen in de sink-gegevensset, [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-reads-from-relational-sources"></a>Herhaalbare leesbewerkingen van relationele bronnen
Wanneer u gegevens van een relationele gegevensopslag kopieert, houd u herhaalbaarheid in gedachten om te voorkomen dat ongewenste resultaten. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook configureren met de nieuwe poging **beleid** voor een gegevensset naar een segment opnieuw uitgevoerd wanneer een fout optreedt. Zorg ervoor dat de dezelfde gegevens worden gelezen, ongeacht het aantal keren dat het segment wordt opnieuw uitgevoerd. Zorg ook dat de dezelfde gegevens worden gelezen, ongeacht hoe u het segment opnieuw uitvoeren. Zie voor meer informatie, [Repeatable leest van relationele bronnen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Meer informatie over de belangrijkste factoren die invloed hebben op de prestaties van Kopieeractiviteit en manieren om te optimaliseren van prestaties in de [en activiteit kopiëren Afstemmingshandleiding](data-factory-copy-activity-performance.md).

## <a name="next-steps"></a>Volgende stappen
Zie voor stapsgewijze instructies voor het maken van een pijplijn met Copy Activity in de [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
