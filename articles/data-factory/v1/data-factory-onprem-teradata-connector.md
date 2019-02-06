---
title: Gegevens verplaatsen van Teradata met Azure Data Factory | Microsoft Docs
description: Meer informatie over de Teradata-Connector voor de Data Factory-service waarmee u gegevens van Teradata-Database verplaatsen
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 98eb76d8-5f3d-4667-b76e-e59ed3eea3ae
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: d22318f4d9e233a57d521fe36f0827b9fc3af3e0
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55746333"
---
# <a name="move-data-from-teradata-using-azure-data-factory"></a>Gegevens verplaatsen van Teradata met Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-onprem-teradata-connector.md)
> * [Versie 2 (huidige versie)](../connector-teradata.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [Teradata-connector in V2](../connector-teradata.md).

In dit artikel wordt uitgelegd hoe u van de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van een on-premises Teradata-database. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit geeft.

U kunt gegevens uit een on-premises Teradata-gegevensarchief kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als sink voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. Data factory ondersteunt momenteel alleen gegevens te verplaatsen van een gegevensarchief Teradata naar andere gegevensarchieven, maar niet voor het verplaatsen van gegevens uit andere gegevensarchieven met een Teradata-gegevensarchief.

## <a name="prerequisites"></a>Vereisten
Data factory ondersteunt verbinding te maken met on-premises Teradata bronnen via Data Management Gateway. Zie [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies over het instellen van de gateway.

Gateway is vereist, zelfs als de Teradata wordt gehost in een Azure IaaS-VM. U kunt de gateway installeren op dezelfde IaaS-VM als het gegevensarchief of op een andere virtuele machine, zolang de gateway verbinding met de database maken kan.

> [!NOTE]
> Zie [oplossen van problemen met gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) voor tips over het oplossen van de verbindingsgateway/problemen met betrekking tot.

## <a name="supported-versions-and-installation"></a>Ondersteunde versies en installatie
Voor Data Management Gateway verbinding maken met de Teradata-Database, moet u voor het installeren van de [.NET Data Provider voor Teradata](https://go.microsoft.com/fwlink/?LinkId=278886) versie 14 of hoger op hetzelfde systeem als Data Management Gateway. Teradata versie 12 en hoger wordt ondersteund.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens uit een on-premises Cassandra-gegevensarchief verplaatst met behulp van verschillende hulpprogramma's / API's.

- De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren.
- U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en  **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit.

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren:

1. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory.
2. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking.
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer.

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een on-premises Teradata-gegevensarchief, [JSON-voorbeeld: Gegevens kopiëren van Teradata naar Azure Blob](#json-example-copy-data-from-teradata-to-azure-blob) sectie van dit artikel.

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met een Teradata-gegevensarchief:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor Teradata gekoppelde service.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OnPremisesTeradata** |Ja |
| server |Naam van de Teradata-server. |Ja |
| authenticationType |Het type verificatie gebruikt voor verbinding met de Teradata-database. Mogelijke waarden zijn: Anoniem, basis en Windows. |Ja |
| gebruikersnaam |Geef de gebruikersnaam op als u basisverificatie of Windows-verificatie. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |
| gatewayName |De naam van de gateway die de Data Factory-service gebruiken moet voor verbinding met de on-premises Teradata-database. |Ja |

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van gegevensset (Azure SQL, Azure-blob, Azure-tabel, enz.).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. Er zijn momenteel geen type eigenschappen die worden ondersteund voor de Teradata-gegevensset.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en beleidsregels zijn beschikbaar voor alle soorten activiteiten.

Terwijl de eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit is afhankelijk van elk activiteitstype. Ze verschillen voor de kopieeractiviteit, afhankelijk van de typen van bronnen en sinks.

Wanneer de bron is van het type **RelationalSource** (waaronder Teradata), de volgende eigenschappen zijn beschikbaar in **typeProperties** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: Selecteer * uit MyTable. |Ja |

### <a name="json-example-copy-data-from-teradata-to-azure-blob"></a>JSON-voorbeeld: Gegevens kopiëren van Teradata naar Azure Blob
Het volgende voorbeeld biedt een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Ze laten zien hoe gegevens kopiëren van Teradata naar Azure Blob Storage. Echter gegevens kunnen worden gekopieerd naar een van de vermelde sinks [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

Het voorbeeld heeft de volgende data factory-entiteiten:

1. Een gekoppelde service van het type [OnPremisesTeradata](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [RelationalTable](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. De [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [RelationalSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld worden gegevens gekopieerd van de resultaten van een query in Teradata-database naar een blob eenmaal per uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

Instellen als eerste stap de data management gateway. De instructies vindt u in de [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

**Teradata gekoppelde service:**

```json
{
    "name": "OnPremTeradataLinkedService",
    "properties": {
        "type": "OnPremisesTeradata",
        "typeProperties": {
            "server": "<server>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

**Gekoppelde Azure Blob storage-service:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorageLinkedService",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
    }
}
```

**De invoergegevensset Teradata:**

Het voorbeeld wordt ervan uitgegaan dat u hebt een tabel 'MyTable' gemaakt in Teradata en bevat een kolom met de naam "timestamp" voor time series-gegevens.

Instellen van 'extern': waar Data Factory-service wordt geïnformeerd dat de tabel bevindt zich buiten de data factory en niet door een activiteit in de data factory wordt geproduceerd.

```json
{
    "name": "TeradataDataSet",
    "properties": {
        "published": false,
        "type": "RelationalTable",
        "linkedServiceName": "OnPremTeradataLinkedService",
        "typeProperties": {
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

**Azure Blob-uitvoergegevensset:**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1). Het pad naar de map voor de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

```json
{
    "name": "AzureBlobTeradataDataSet",
    "properties": {
        "published": false,
        "location": {
            "type": "AzureBlobLocation",
            "folderPath": "mycontainer/teradata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
            ],
            "linkedServiceName": "AzureStorageLinkedService"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```
**Een pijplijn met Copy activity in:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur wordt uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **RelationalSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **query** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

```json
{
    "name": "CopyTeradataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "TeradataDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobTeradataDataSet"
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
                "name": "TeradataToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z",
        "isPaused": false
    }
}
```
## <a name="type-mapping-for-teradata"></a>Toewijzing van het type voor Teradata
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, de Copy-activiteit voert een automatische conversie van de typen gegevensbronnen met sink-type met de volgende stappen 2-benadering:

1. Systeemeigen brontypen converteren naar .NET-type
2. .NET-type converteren naar systeemeigen sink-type

Wanneer u gegevens naar Teradata verplaatst, worden de volgende toewijzingen van Teradata-type gebruikt om .NET-type.

| Type van de Teradata-Database | .NET framework-type |
| --- | --- |
| CHAR |String |
| CLOB |String |
| Afbeelding |String |
| VarChar |String |
| VarGraphic |String |
| Blob |Byte[] |
| Byte |Byte[] |
| VarByte |Byte[] |
| BigInt |Int64 |
| ByteInt |Int16 |
| Decimal |Decimal |
| Double |Double |
| Geheel getal |Int32 |
| Aantal |Double |
| SmallInt |Int16 |
| Date |DateTime |
| Time |TimeSpan |
| Tijd met de tijdzone |String |
| Tijdstempel |DateTime |
| Tijdstempel met tijdzone |DateTimeOffset |
| Interval Day |TimeSpan |
| Interval voor dag uur |TimeSpan |
| Interval voor dag minuut |TimeSpan |
| Tweede interval dag |TimeSpan |
| Interval uur |TimeSpan |
| Interval voor uur minuut |TimeSpan |
| Tweede interval uur |TimeSpan |
| Interval minuut |TimeSpan |
| Tweede interval minuut |TimeSpan |
| Interval voor tweede |TimeSpan |
| Interval van jaar |String |
| Interval voor jaar tot maand |String |
| Interval van maand |String |
| Period(date) |String |
| Period(Time) |String |
| Periode (tijd met tijdzone) |String |
| Period(timestamp) |String |
| Periode (Timestamp met tijdzone) |String |
| Xml |String |

## <a name="map-source-to-sink-columns"></a>Kaartbron met sink-kolommen
Zie voor meer informatie over het toewijzen van kolommen in de brongegevensset naar kolommen in de sink-gegevensset, [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Herhaalbare lezen van relationele bronnen
Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, houd er rekening mee om te voorkomen dat ongewenste resultaten herhaalbaarheid. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment wordt opnieuw uitgevoerd in beide gevallen, moet u om ervoor te zorgen dat de dezelfde gegevens worden gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable leesrechten voor relationele bronnen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie [prestaties kopiëren en Afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren.
