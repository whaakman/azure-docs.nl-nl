---
title: Gegevens verplaatsen van MySQL met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit een MySQL-database met behulp van Azure Data Factory.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 452f4fce-9eb5-40a0-92f8-1e98691bea4c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: f04a3b8c7bb744e3a9d539f6d3a392bc59702758
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-from-mysql-using-azure-data-factory"></a>Verplaatsen van gegevens van MySQL met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-onprem-mysql-connector.md)
> * [Versie 2 - Preview](../connector-mysql.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [MySQL-connector in V2](../connector-mysql.md).


In dit artikel wordt uitgelegd hoe de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van een lokale MySQL-database gebruiken. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, hetgeen een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit toont.

U kunt gegevens uit een on-premises MySQL-gegevensopslag kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven als PUT wordt ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. Data factory ondersteunt momenteel alleen zwevend gegevens uit een MySQL-gegevensarchief tot andere gegevensarchieven, maar niet voor het verplaatsen van gegevens uit andere gegevensarchieven naar een MySQL-gegevensarchief. 

## <a name="prerequisites"></a>Vereisten
Verbinding maken met lokale MySQL bronnen met behulp van Data Management Gateway biedt ondersteuning voor Data Factory-service. Zie [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies over het instellen van de gateway.

Gateway is vereist, zelfs als de MySQL-database wordt gehost in een Azure IaaS virtuele machine (VM). U kunt de gateway installeren op dezelfde virtuele machine als gegevensopslag of op een andere virtuele machine, zolang de gateway verbinding met de database maken kan.

> [!NOTE]
> Zie [gateway problemen](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) voor tips over het oplossen van verbinding of gateway gerelateerde problemen.

## <a name="supported-versions-and-installation"></a>Ondersteunde versies en installatie
Voor de Data Management Gateway verbinding maken met de MySQL-Database, moet u voor het installeren van de [MySQL Connector/Net voor Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) (versie 6.6.5 of hoger) op hetzelfde systeem als Data Management Gateway. Deze 32-bits stuurprogramma is compatibel met 64-bits Data Management Gateway. MySQL versie 5.1 en hoger wordt ondersteund.

> [!TIP]
> Als u raakt-fout bij het 'Verificatie is mislukt omdat de externe partij heeft gesloten het transport-stroom.', kunt u de Connector MySQL/Net upgraden naar hogere versie.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens vanuit een on-premises Cassandra-gegevensopslag verplaatst met behulp van verschillende hulpprogramma's voor API's. 

- De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren. 
- U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren:

1. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory.
2. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een on-premises MySQL-gegevensopslag [JSON-voorbeeld: gegevens kopiëren van MySQL naar Azure Blob](#json-example-copy-data-from-mysql-to-azure-blob) sectie van dit artikel. 

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van specifieke Data Factory-entiteiten in een MySQL-gegevensarchief:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor MySQL gekoppelde service.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OnPremisesMySql** |Ja |
| server |Naam van de MySQL-server. |Ja |
| database |Naam van de MySQL-database. |Ja |
| schema |De naam van het schema in de database. |Nee |
| authenticationType |Het soort verificatie die verbinding maken met de MySQL-database wordt gebruikt. Mogelijke waarden zijn: `Basic`. |Ja |
| gebruikersnaam |Geef de naam van de gebruiker verbinding maken met de MySQL-database. |Ja |
| wachtwoord |Geef het wachtwoord voor het gebruikersaccount dat u hebt opgegeven. |Ja |
| gatewayName |Naam van de gateway die voor de Data Factory-service gebruiken moet voor verbinding met de lokale MySQL-database. |Ja |

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets van de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle gegevensset typen (Azure SQL, Azure blob-, Azure-tabel, enz.).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **RelationalTable** (inclusief MySQL gegevensset) heeft de volgende eigenschappen

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in de MySQL-Database-instantie waarnaar de gekoppelde service verwijst. |Nee (als **query** van **RelationalSource** is opgegeven) |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten van de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen, zijn de beleidsregels zijn beschikbaar voor alle typen activiteiten.

Dat eigenschappen beschikbaar zijn in de **typeProperties** sectie van de activiteit variëren met elk activiteitstype. Voor de kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put.

Wanneer u de gegevensbron in de kopieerbewerking is van het type **RelationalSource** (waaronder MySQL), de volgende eigenschappen beschikbaar zijn in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: Selecteer * from MijnTabel. |Nee (als **tableName** van **gegevensset** is opgegeven) |


## <a name="json-example-copy-data-from-mysql-to-azure-blob"></a>JSON-voorbeeld: gegevens kopiëren van MySQL naar Azure Blob
In dit voorbeeld bevat definities van de voorbeeld-JSON die u een pijplijn maken kunt met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Er wordt weergegeven hoe gegevens kopiëren van een lokale MySQL-database naar een Azure Blob Storage. Gegevens kunnen echter worden gekopieerd naar een van de PUT vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

> [!IMPORTANT]
> Dit voorbeeld bevat JSON-fragmenten. Dit omvat geen stapsgewijze instructies voor het maken van de gegevensfactory. Zie [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies.

Het voorbeeld heeft de volgende data factory-entiteiten:

1. Een gekoppelde service van het type [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [RelationalTable](data-factory-onprem-mysql-connector.md#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld kopieert gegevens van de resultaten van een query in een MySQL-database naar een blob per uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

Het instellen van de data management gateway als eerste stap. De instructies vindt u in de [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

**MySQL gekoppelde service:**

```JSON
    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
        }
      }
    }
```

**Gekoppelde Azure Storage-service:**

```JSON
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

**MySQL-invoergegevensset:**

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MijnTabel' gemaakt in MySQL en bevat een kolom met de naam 'timestampcolumn' voor de reeksgegevens.

Instelling 'extern': 'true' informeert de Data Factory-service dat de tabel aan de gegevensfactory extern en niet wordt geproduceerd door een activiteit in de gegevensfactory.

```JSON
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
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

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad maakt gebruik van jaar, maand, dag en uur delen van de begintijd.

```JSON
    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **RelationalSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **query** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

```JSON
    {
        "name": "CopyMySqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }
```


### <a name="type-mapping-for-mysql"></a>Toewijzing van het type voor MySQL
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieerbewerking wordt automatische typeconversies van brontypen opvangen typen met de volgende benadering voor in twee stappen uitgevoerd:

1. Systeemeigen brontypen converteren naar .NET-type
2. Converteren van .NET-type naar systeemeigen sink-type

Wanneer u gegevens naar MySQL verplaatst, worden de volgende toewijzingen van MySQL types gebruikt voor .NET-typen.

| Type MySQL-Database | .NET framework-type |
| --- | --- |
| niet-ondertekende bigint |Decimale |
| bigint |Int64 |
| bits |Decimale |
| blob |Byte[] |
| BOOL |Boole-waarde |
| CHAR |Tekenreeks |
| datum |Datum en tijd |
| datum/tijd |Datum en tijd |
| Decimale |Decimale |
| dubbele precisie |Double |
| dubbel |Double |
| Enum |Tekenreeks |
| drijvend |Single |
| niet-ondertekende int |Int64 |
| int |Int32 |
| niet-ondertekend geheel getal |Int64 |
| geheel getal |Int32 |
| lange varbinary |Byte[] |
| lange varchar |Tekenreeks |
| longblob |Byte[] |
| longtext |Tekenreeks |
| mediumblob |Byte[] |
| niet-ondertekende mediumint |Int64 |
| mediumint |Int32 |
| mediumtext |Tekenreeks |
| numerieke |Decimale |
| echte |Double |
| instellen |Tekenreeks |
| niet-ondertekende smallint |Int32 |
| smallint |Int16 |
| Tekst |Tekenreeks |
| tijd |TimeSpan |
| tijdstempel |Datum en tijd |
| tinyblob |Byte[] |
| niet-ondertekende tinyint |Int16 |
| tinyint |Int16 |
| tinytext |Tekenreeks |
| varchar |Tekenreeks |
| jaar |Int |

## <a name="map-source-to-sink-columns"></a>Bron van de kaart opvangen kolommen
Zie voor meer informatie over het toewijzen van kolommen in gegevensset naar kolommen in gegevensset sink bron, [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Herhaalbare leesbewerking van relationele bronnen
Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, moet u herhaalbaarheid Houd in gedachten om te voorkomen dat ongewenste resultaten. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook een beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment opnieuw in beide gevallen wordt uitgevoerd, moet u ervoor zorgen dat dezelfde gegevens is gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable van relationele bronnen lezen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en het afstemmen
Zie [uitvoering van de activiteit & afstemmen handleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren.
