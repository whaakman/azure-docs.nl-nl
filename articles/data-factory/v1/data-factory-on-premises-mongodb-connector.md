---
title: Gegevens verplaatsen van MongoDB gebruik Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit de MongoDB-database met behulp van Azure Data Factory.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 10ca7d9a-7715-4446-bf59-2d2876584550
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 20df17ba01cfc18ce751491d154d7401001e706e
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-from-mongodb-using-azure-data-factory"></a>Verplaatsen van gegevens van MongoDB met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-on-premises-mongodb-connector.md)
> * [Versie 2 - Preview](../connector-mongodb.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [MongoDB-connector in V2](../connector-mongodb.md).


In dit artikel wordt uitgelegd hoe de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van een on-premises MongoDB-database gebruiken. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, hetgeen een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit toont.

U kunt gegevens uit een on-premises MongoDB-gegevensopslag kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven als PUT wordt ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. Data factory ondersteunt momenteel alleen zwevend gegevens uit een gegevensopslag MongoDB tot andere gegevensarchieven, maar niet voor het verplaatsen van gegevens uit andere gegevensarchieven met een MongoDB-gegevensarchief. 

## <a name="prerequisites"></a>Vereisten
Voor de Azure Data Factory-service kunnen verbinding maken met uw lokale MongoDB-database, moet u de volgende onderdelen installeren:

- Ondersteunde MongoDB-versies zijn: 2.4, 2.6, 3.0 en 3.2.
- Data Management Gateway op dezelfde computer die als host fungeert voor de database of op een afzonderlijke computer om te voorkomen dat concurrentie voor resources met de database. Data Management Gateway is een software die on-premises gegevensbronnen met cloud-services op een manier veilig en beheerd verbindt. Zie [Data Management Gateway](data-factory-data-management-gateway.md) voor meer informatie over Data Management Gateway. Zie [gegevens verplaatsen van on-premises naar cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies over het instellen van de gateway een gegevens-pijplijn om gegevens te verplaatsen.

    Wanneer u de gateway installeert, installeert deze automatisch een Microsoft MongoDB ODBC-stuurprogramma waarmee verbinding met MongoDB.

    > [!NOTE]
    > U moet de gateway verbinding maken met MongoDB, zelfs als deze wordt gehost in Azure IaaS VM's gebruiken. Als u probeert verbinding maken met een exemplaar van MongoDB gehost in de cloud, kunt u ook het gatewayexemplaar installeren in de IaaS-VM.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens vanuit een on-premises MongoDB-gegevensopslag verplaatst met behulp van verschillende hulpprogramma's voor API's.

De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren: 

1. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory.
2. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een on-premises MongoDB-gegevensopslag [JSON-voorbeeld: gegevens kopiëren van MongoDB naar Azure Blob](#json-example-copy-data-from-mongodb-to-azure-blob) sectie van dit artikel. 

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke MongoDB-bron:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor **OnPremisesMongoDB** gekoppelde service.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OnPremisesMongoDb** |Ja |
| server |IP-adres of de hostnaam de naam van de MongoDB-server. |Ja |
| poort |TCP-poort die de MongoDB-server gebruikt om te luisteren naar verbindingen van clients. |Optionele standaardwaarde: 27017 |
| authenticationType |Basic of anonieme. |Ja |
| gebruikersnaam |Het gebruikersaccount voor toegang tot MongoDB. |Ja (als u basisverificatie wordt gebruikt). |
| wachtwoord |Wachtwoord voor de gebruiker. |Ja (als u basisverificatie wordt gebruikt). |
| authSource |De naam van de MongoDB-database die u wilt gebruiken om te controleren van uw referenties voor verificatie. |Optioneel (als basisverificatie wordt gebruikt). Standaard: maakt gebruik van het beheerdersaccount en de database die is opgegeven met behulp van de eigenschap databaseName. |
| databaseName |De naam van de MongoDB-database die u wilt openen. |Ja |
| gatewayName |De naam van de gateway die toegang heeft tot het gegevensarchief. |Ja |
| encryptedCredential |Referentie versleuteld door de gateway. |Optioneel |

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets van de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle gegevensset typen (Azure SQL, Azure blob-, Azure-tabel, enz.).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **MongoDbCollection** heeft de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| CollectionName |De naam van de verzameling in de MongoDB-database. |Ja |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten van de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen op, zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle typen activiteiten.

Eigenschappen die beschikbaar zijn in de **typeProperties** sectie van de activiteit aan de andere kant variëren met elk activiteitstype. Voor de kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put.

Wanneer de bron is van het type **MongoDbSource** de volgende eigenschappen beschikbaar zijn in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |Gebruik de aangepaste query om gegevens te lezen. |SQL-92 queryreeks. Bijvoorbeeld: Selecteer * from MijnTabel. |Nee (als **collectionName** van **gegevensset** is opgegeven) |



## <a name="json-example-copy-data-from-mongodb-to-azure-blob"></a>JSON-voorbeeld: gegevens kopiëren van MongoDB naar Azure Blob
In dit voorbeeld bevat definities van de voorbeeld-JSON die u een pijplijn maken kunt met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Er wordt weergegeven hoe gegevens kopiëren van een lokale MongoDB naar een Azure Blob Storage. Gegevens kunnen echter worden gekopieerd naar een van de PUT vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

Het voorbeeld heeft de volgende data factory-entiteiten:

1. Een gekoppelde service van het type [OnPremisesMongoDb](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [MongoDbCollection](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [MongoDbSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld kopieert gegevens van de resultaten van een query in de MongoDB-database naar een blob elk uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

Instellen als eerste stap de data management gateway volgens de instructies in de [Data Management Gateway](data-factory-data-management-gateway.md) artikel.

**MongoDB gekoppelde service:**

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties":
    {
        "type": "OnPremisesMongoDb",
        "typeProperties":
        {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< The IP address or host name of the MongoDB server >",  
            "port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>",
            "username": "<username>",
            "password": "<password>",
           "authSource": "< The database that you want to use to check your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<mygateway>"
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

**MongoDB-invoergegevensset:** instellen 'extern': 'true' informeert de Data Factory-service dat de tabel aan de gegevensfactory extern en niet wordt geproduceerd door een activiteit in de gegevensfactory.

```json
{
     "name":  "MongoDbInputDataset",
    "properties": {
        "type": "MongoDbCollection",
        "linkedServiceName": "OnPremisesMongoDbLinkedService",
        "typeProperties": {
            "collectionName": "<Collection name>"    
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

**Azure Blob-uitvoergegevensset:**

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad maakt gebruik van jaar, maand, dag en uur delen van de begintijd.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/frommongodb/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**De kopieeractiviteit in een pijplijn met MongoDB-bron en sink van Blob:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de bovenstaande invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **MongoDbSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **query** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "MongoDbSource",
                        "query": "$$Text.Format('select * from  MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "MongoDbInputDataset"
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
                "name": "MongoDBToAzureBlob"
            }
        ],
        "start": "2016-06-01T18:00:00Z",
        "end": "2016-06-01T19:00:00Z"
    }
}
```


## <a name="schema-by-data-factory"></a>Schema door Data Factory
Azure Data Factory-service afleidt schema uit een verzameling MongoDB met behulp van de meest recente 100 documenten in de verzameling. Als deze 100 documenten niet volledig schema bevatten, kunnen sommige kolommen worden genegeerd tijdens de kopieerbewerking.

## <a name="type-mapping-for-mongodb"></a>Toewijzing van het type voor MongoDB
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieerbewerking wordt automatische typeconversies van brontypen opvangen typen met de volgende stappen 2-benadering uitgevoerd:

1. Systeemeigen brontypen converteren naar .NET-type
2. Converteren van .NET-type naar systeemeigen sink-type

Bij het verplaatsen van gegevens naar MongoDB worden de volgende toewijzingen van MongoDB-types gebruikt voor .NET-typen.

| MongoDB-type | .NET framework-type |
| --- | --- |
| Binair bestand |Byte[] |
| Boole-waarde |Boole-waarde |
| Date |Datum en tijd |
| NumberDouble |Double |
| NumberInt |Int32 |
| NumberLong |Int64 |
| ObjectID |Tekenreeks |
| Tekenreeks |Tekenreeks |
| UUID |GUID |
| Object |Renormalized plat in kolommen met '_' als geneste scheidingsteken |

> [!NOTE]
> Raadpleeg voor meer informatie over ondersteuning voor arrays met virtuele tabellen, [ondersteuning voor complexe typen virtuele tabellen met](#support-for-complex-types-using-virtual-tables) hieronder.

Op dit moment wordt de volgende MongoDB-gegevenstypen worden niet ondersteund: DBPointer, JavaScript, Max per minuut sleutel, reguliere expressie, symbool, Timestamp, Undefined

## <a name="support-for-complex-types-using-virtual-tables"></a>Ondersteuning voor complexe typen met behulp van de virtuele tabellen
Azure Data Factory gebruikt een ingebouwde ODBC-stuurprogramma voor het verbinding maken met en het kopiëren van gegevens uit de MongoDB-database. Voor complexe gegevenstypen zoals matrices of objecten met verschillende typen op de documenten Normaliseert het stuurprogramma gegevens opnieuw in de bijbehorende virtuele tabellen. Als een tabel bevat een dergelijke kolommen, genereert het stuurprogramma in het bijzonder de volgende virtuele tabellen:

* Een **basistabel**, die dezelfde gegevens als de echte tabel, met uitzondering van de kolommen van het complexe type bevat. De basistabel gebruikt dezelfde naam als de echte tabel die wordt vertegenwoordigd.
* Een **virtuele tabel** voor elke kolom complex type zijn dat de geneste gegevens wordt uitgebreid. De virtuele tabellen zijn benoemde met de naam van de bestaande tabel, een scheidingsteken '_' en de naam van de matrix of object.

Virtuele tabellen verwijzen naar de gegevens in de echte tabel, waardoor het stuurprogramma voor toegang tot de gedenormaliseerd gegevens. Zie de sectie Voorbeeld onder meer informatie. U kunt toegang tot de inhoud van MongoDB matrices door het uitvoeren van query's en het toevoegen aan de virtuele tabellen.

U kunt de [Wizard kopiëren](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) intuïtief de lijst met tabellen weergeven in de MongoDB-database met inbegrip van de virtuele tabellen en een voorbeeld van de gegevens in. U kunt ook maakt u een query in de Wizard kopiëren en valideren als u wilt het resultaat te bekijken.

### <a name="example"></a>Voorbeeld
'ExampleTable' hieronder is bijvoorbeeld een MongoDB-tabel met één kolom met een matrix van objecten in elke cel – facturen en één kolom met een matrix van scalaire typen – classificaties.

| _id | Naam van de klant | Facturen | Servicelaag | De classificaties |
| --- | --- | --- | --- | --- |
| 1111 |ABC |[{invoice_id: '123' item: 'toaster', de prijs: '456' korting: "0,2"}, {invoice_id: '124' item: 'ingesteld', prijs: '1235' korting: "0,2"}] |Zilver |[5,6] |
| 2222 |XYZ |[{invoice_id: item '135': 'koelkast', prijs: '12543' korting: "0,0"}] |Goudkleurig |[1,2] |

Het stuurprogramma genereert meerdere virtuele tabellen te deze één tabel vertegenwoordigen. De eerste virtuele tabel is de basistabel met de naam 'ExampleTable', hieronder weergegeven. De basistabel bevat alle gegevens van de oorspronkelijke tabel, maar de gegevens van de matrices is weggelaten en in de virtuele tabellen is uitgevouwen.

| _id | Naam van de klant | Servicelaag |
| --- | --- | --- |
| 1111 |ABC |Zilver |
| 2222 |XYZ |Goudkleurig |

De volgende tabellen tonen de virtuele tabellen waarbij de oorspronkelijke matrices in het voorbeeld. Deze tabellen bevatten het volgende:

* Een verwijzing naar de oorspronkelijke primaire-sleutelkolom overeenkomt met de rij van de oorspronkelijke matrix (via de kolom _id)
* Een indicatie van de positie van de gegevens in de oorspronkelijke matrix
* De uitgebreide gegevens voor elk element in de matrix

Tabel 'ExampleTable_Invoices':

| _id | ExampleTable_Invoices_dim1_idx | invoice_id | item | price | Korting |
| --- | --- | --- | --- | --- | --- |
| 1111 |0 |123 |toaster |456 |0.2 |
| 1111 |1 |124 |oven |1235 |0.2 |
| 2222 |0 |135 |koelkast |12543 |0.0 |

Tabel 'ExampleTable_Ratings':

| _id | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings |
| --- | --- | --- |
| 1111 |0 |5 |
| 1111 |1 |6 |
| 2222 |0 |1 |
| 2222 |1 |2 |

## <a name="map-source-to-sink-columns"></a>Bron van de kaart opvangen kolommen
Zie voor meer informatie over het toewijzen van kolommen in gegevensset naar kolommen in gegevensset sink bron, [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Herhaalbare leesbewerking van relationele bronnen
Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, moet u herhaalbaarheid Houd in gedachten om te voorkomen dat ongewenste resultaten. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook een beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment opnieuw in beide gevallen wordt uitgevoerd, moet u ervoor zorgen dat dezelfde gegevens is gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable van relationele bronnen lezen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en het afstemmen
Zie [uitvoering van de activiteit & afstemmen handleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren.

## <a name="next-steps"></a>Volgende stappen
Zie [gegevens verplaatsen tussen on-premises en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies voor het maken van een pijplijn gegevens die gegevens vanuit een on-premises gegevensopslag verplaatst naar een Azure-gegevensarchief.
