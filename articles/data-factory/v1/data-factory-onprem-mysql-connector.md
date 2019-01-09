---
title: Gegevens verplaatsen van MySQL met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit de MySQL-database met behulp van Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 452f4fce-9eb5-40a0-92f8-1e98691bea4c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/06/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: de1263d68e96a23bd6b5eca4297e74b56ba22e40
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54021631"
---
# <a name="move-data-from-mysql-using-azure-data-factory"></a>Gegevens verplaatsen van MySQL met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Selecteer de versie van de Data Factory-service die u gebruikt:"]
> * [Versie 1:](data-factory-onprem-mysql-connector.md)
> * [Versie 2 (huidige versie)](../connector-mysql.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [MySQL-connector in V2](../connector-mysql.md).


In dit artikel wordt uitgelegd hoe u van de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen uit een on-premises MySQL-database. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit geeft.

U kunt gegevens uit een on-premises MySQL-gegevensarchief kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als sink voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. Data factory ondersteunt momenteel alleen gegevens te verplaatsen van een MySQL-gegevensarchief naar andere gegevensarchieven, maar niet voor het verplaatsen van gegevens uit andere gegevensarchieven met een MySQL-gegevensarchief. 

## <a name="prerequisites"></a>Vereisten
Data Factory-service ondersteunt een verbinding met on-premises MySQL bronnen met behulp van de Data Management Gateway. Zie [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies over het instellen van de gateway.

Gateway is vereist, zelfs als de MySQL-database wordt gehost in een Azure IaaS-machine (VM). U kunt de gateway installeren op dezelfde virtuele machine als het gegevensarchief of een andere virtuele machine, zolang de gateway verbinding met de database maken kan.

> [!NOTE]
> Zie [oplossen van problemen met gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) voor tips over het oplossen van de verbindingsgateway/problemen met betrekking tot.

## <a name="supported-versions-and-installation"></a>Ondersteunde versies en installatie
Voor Data Management Gateway verbinding maken met de MySQL-Database, moet u voor het installeren van de [MySQL Connector/Net voor Microsoft Windows](https://dev.mysql.com/downloads/connector/net/) (versie tussen 6.6.5 en 6.10.7) op hetzelfde systeem als Data Management Gateway. Deze 32-bits stuurprogramma is compatibel met 64-bits Data Management Gateway. MySQL-versie 5.1 en hoger wordt ondersteund.

> [!TIP]
> Als u op "Verificatie is mislukt omdat de externe partij is gesloten door het transport stream." fout bereikt, kunt u overwegen de MySQL Connector/Net upgraden naar een latere versie.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens uit een on-premises Cassandra-gegevensarchief verplaatst met behulp van verschillende hulpprogramma's / API's. 

- De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren. 
- U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en  **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren:

1. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory.
2. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een on-premises MySQL-gegevensarchief, [JSON-voorbeeld: Gegevens kopiëren van MySQL naar Azure Blob](#json-example-copy-data-from-mysql-to-azure-blob) sectie van dit artikel. 

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke naar een MySQL-gegevensarchief:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor MySQL gekoppelde service.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OnPremisesMySql** |Ja |
| server |De naam van de MySQL-server. |Ja |
| database |De naam van de MySQL-database. |Ja |
| schema |De naam van het schema in de database. |Nee |
| authenticationType |Het type verificatie gebruikt voor verbinding met de MySQL-database. Mogelijke waarden zijn: `Basic`. |Ja |
| gebruikersnaam |Geef de naam van de gebruiker verbinding maken met de MySQL-database. |Ja |
| wachtwoord |Geef het wachtwoord voor het gebruikersaccount dat u hebt opgegeven. |Ja |
| gatewayName |De naam van de gateway die de Data Factory-service wordt gebruikt om verbinding met de on-premises MySQL-database te maken. |Ja |

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van gegevensset (Azure SQL, Azure-blob, Azure-tabel, enz.).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **RelationalTable** (waaronder MySQL gegevensset) heeft de volgende eigenschappen

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in de MySQL-Database-instantie waarnaar de gekoppelde service verwijst. |Nee (als **query** van **RelationalSource** is opgegeven) |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen, zijn de beleidsregels zijn beschikbaar voor alle soorten activiteiten.

Dat eigenschappen die beschikbaar zijn in de **typeProperties** sectie van de activiteit variëren met elk activiteitstype. Ze verschillen voor de kopieeractiviteit, afhankelijk van de typen van bronnen en sinks.

Wanneer u de gegevensbron in de kopieeractiviteit is van het type **RelationalSource** (waaronder MySQL), de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: Selecteer * uit MyTable. |Nee (als **tableName** van **gegevensset** is opgegeven) |


## <a name="json-example-copy-data-from-mysql-to-azure-blob"></a>JSON-voorbeeld: Gegevens kopiëren van MySQL naar Azure Blob
In dit voorbeeld biedt een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Het laat zien hoe u gegevens uit een on-premises MySQL-database kopiëren naar een Azure Blob-opslag. Echter gegevens kunnen worden gekopieerd naar een van de vermelde sinks [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

> [!IMPORTANT]
> In dit voorbeeld bevat JSON-fragmenten. Deze omvatten geen stapsgewijze instructies voor het maken van de data factory. Zie [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies.

Het voorbeeld heeft de volgende data factory-entiteiten:

1. Een gekoppelde service van het type [OnPremisesMySql](data-factory-onprem-mysql-connector.md#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [RelationalTable](data-factory-onprem-mysql-connector.md#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [RelationalSource](data-factory-onprem-mysql-connector.md#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld worden gegevens gekopieerd van de resultaten van een query in de MySQL-database naar een blob per uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

Instellen als eerste stap de data management gateway. De instructies vindt u in de [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

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

**De invoergegevensset MySQL:**

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MyTable' gemaakt in MySQL en bevat een kolom met de naam 'timestampcolumn' voor time series-gegevens.

Instellen van "extern": "true" informeert de Data Factory-service dat de tabel bevindt zich buiten de data factory en niet door een activiteit in de data factory wordt geproduceerd.

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

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1). Het pad naar de map voor de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

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

**Een pijplijn met Copy activity in:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **RelationalSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **query** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

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
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieeractiviteit wordt uitgevoerd automatisch typeconversies van typen gegevensbronnen met sink-type met de volgende methode voor verificatie in twee stappen:

1. Systeemeigen brontypen converteren naar .NET-type
2. .NET-type converteren naar systeemeigen sink-type

Bij het verplaatsen van gegevens met MySQL, worden de volgende toewijzingen van MySQL typen gebruikt om .NET-typen.

| Type van de MySQL-Database | .NET framework-type |
| --- | --- |
| niet-ondertekende bigint |Decimaal |
| bigint |Int64 |
| bits |Decimaal |
| blob |Byte[] |
| bool |Booleaans |
| CHAR |Reeks |
| date |Datum en tijd |
| datum/tijd |Datum en tijd |
| decimal |Decimaal |
| dubbele precisie |Double-waarde |
| double |Double-waarde |
| enum |Reeks |
| float |Enkelvoudig |
| niet-ondertekende int |Int64 |
| int |Int32 |
| niet-ondertekende gehele getal |Int64 |
| geheel getal |Int32 |
| lange varbinary |Byte[] |
| lange varchar |Reeks |
| longblob |Byte[] |
| LONGTEXT |Reeks |
| mediumblob |Byte[] |
| niet-ondertekende mediumint |Int64 |
| mediumint |Int32 |
| mediumtext |Reeks |
| numerieke |Decimaal |
| echte |Double-waarde |
| set |Reeks |
| niet-ondertekende smallint |Int32 |
| smallint |Int16 |
| tekst |Reeks |
| time |TimeSpan |
| tijdstempel |Datum en tijd |
| tinyblob |Byte[] |
| niet-ondertekende tinyint |Int16 |
| tinyint |Int16 |
| tinytext |Reeks |
| varchar |Reeks |
| jaar |Int |

## <a name="map-source-to-sink-columns"></a>Kaartbron met sink-kolommen
Zie voor meer informatie over het toewijzen van kolommen in de brongegevensset naar kolommen in de sink-gegevensset, [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Herhaalbare lezen van relationele bronnen
Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, houd er rekening mee om te voorkomen dat ongewenste resultaten herhaalbaarheid. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment wordt opnieuw uitgevoerd in beide gevallen, moet u om ervoor te zorgen dat de dezelfde gegevens worden gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable leesrechten voor relationele bronnen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie [prestaties kopiëren en Afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren.
