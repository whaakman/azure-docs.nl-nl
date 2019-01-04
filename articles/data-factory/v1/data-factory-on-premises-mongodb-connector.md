---
title: Gegevens verplaatsen van MongoDB met behulp van Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit een MongoDB-database met behulp van Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 10ca7d9a-7715-4446-bf59-2d2876584550
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 4059d8d2f6020a23e3593bb906c2e3fc64a4779e
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54025586"
---
# <a name="move-data-from-mongodb-using-azure-data-factory"></a>Gegevens verplaatsen van MongoDB met Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-on-premises-mongodb-connector.md)
> * [Versie 2 (huidige versie)](../connector-mongodb.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [MongoDB-connector in V2](../connector-mongodb.md).


In dit artikel wordt uitgelegd hoe u van de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van een on-premises MongoDB-database. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit geeft.

U kunt gegevens uit een on-premises MongoDB-gegevensarchief kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als sink voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. Data factory ondersteunt momenteel alleen gegevens te verplaatsen van een MongoDB-gegevensarchief naar andere gegevensarchieven, maar niet voor het verplaatsen van gegevens uit andere gegevensarchieven met een MongoDB-gegevensarchief. 

## <a name="prerequisites"></a>Vereisten
Voor de Azure Data Factory-service te kunnen verbinding maken met uw on-premises MongoDB-database, moet u de volgende onderdelen installeren:

- Ondersteunde MongoDB-versies zijn:  2.4, 2.6, 3.0, 3.2, 3.4 en 3.6.
- Data Management Gateway op dezelfde computer die als host fungeert voor de database of op een afzonderlijke computer om te voorkomen en dingen om de resources met de database. Data Management Gateway is software waarmee on-premises gegevensbronnen met cloud-services in een veilige, beheerde manier verbinding maakt. Zie [Data Management Gateway](data-factory-data-management-gateway.md) artikel voor meer informatie over Data Management Gateway. Zie [gegevens verplaatsen van on-premises naar de cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies over het instellen van de gateway van een pijplijn om gegevens te verplaatsen.

    Wanneer u de gateway installeert, installeert deze automatisch een MongoDB-ODBC-stuurprogramma dat is gebruikt om te verbinden met MongoDB.

    > [!NOTE]
    > U moet de gateway gebruiken om te verbinden met MongoDB, zelfs als deze wordt gehost in Azure IaaS VM's. Als u probeert verbinding maken met een exemplaar van MongoDB gehost in de cloud, kunt u ook de gateway-exemplaar installeren in de IaaS-VM.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens uit een on-premises MongoDB-gegevensarchief verplaatst met behulp van verschillende hulpprogramma's / API's.

De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren.

U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en  **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren: 

1. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory.
2. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een on-premises MongoDB-gegevensarchief, [JSON-voorbeeld: Gegevens kopiëren van MongoDB naar Azure Blob](#json-example-copy-data-from-mongodb-to-azure-blob) sectie van dit artikel. 

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten die specifiek voor MongoDB-bron:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor **OnPremisesMongoDB** gekoppelde service.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OnPremisesMongoDb** |Ja |
| server |IP-adres of de hostnaam de naam van de MongoDB-server. |Ja |
| poort |TCP-poort die de MongoDB-server wordt gebruikt om te luisteren naar clientverbindingen. |Optionele standaardwaarde: 27017 |
| authenticationType |Basic, of anonieme. |Ja |
| gebruikersnaam |Gebruikersaccount voor toegang tot MongoDB. |Ja (als basisverificatie wordt gebruikt). |
| wachtwoord |Het wachtwoord voor de gebruiker. |Ja (als basisverificatie wordt gebruikt). |
| authSource |De naam van de MongoDB-database die u wilt gebruiken om te controleren of uw referenties voor verificatie. |Optioneel (als basisverificatie wordt gebruikt). Standaard: maakt gebruik van het beheerdersaccount en de database die is opgegeven met behulp van de eigenschap databaseName. |
| databaseName |De naam van de MongoDB-database die u wilt openen. |Ja |
| gatewayName |Naam van de gateway die toegang heeft tot het gegevensarchief. |Ja |
| encryptedCredential |De referentie is versleuteld met de gateway. |Optioneel |

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van gegevensset (Azure SQL, Azure-blob, Azure-tabel, enz.).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. De typeProperties sectie voor de gegevensset van het type **MongoDbCollection** heeft de volgende eigenschappen:

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| collectionName |De naam van de verzameling in de MongoDB-database. |Ja |

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en -beleid zijn beschikbaar voor alle soorten activiteiten.

Eigenschappen die beschikbaar zijn in de **typeProperties** sectie van de activiteit aan de andere kant variëren met elk activiteitstype. Ze verschillen voor de kopieeractiviteit, afhankelijk van de typen van bronnen en sinks.

Wanneer de bron is van het type **MongoDbSource** de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |SQL-92 query-tekenreeks. Bijvoorbeeld: Selecteer * uit MyTable. |Nee (als **collectionName** van **gegevensset** is opgegeven) |



## <a name="json-example-copy-data-from-mongodb-to-azure-blob"></a>JSON-voorbeeld: Gegevens kopiëren van MongoDB naar Azure Blob
In dit voorbeeld biedt een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Het laat zien hoe u gegevens kopiëren van een on-premises MongoDB naar een Azure Blob-opslag. Echter gegevens kunnen worden gekopieerd naar een van de vermelde sinks [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.

Het voorbeeld heeft de volgende data factory-entiteiten:

1. Een gekoppelde service van het type [OnPremisesMongoDb](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [MongoDbCollection](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [MongoDbSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld worden gegevens gekopieerd van de resultaten van een query in de MongoDB-database naar een blob eenmaal per uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

Instellen als eerste stap de data management gateway aan de hand van de instructies in de [Data Management Gateway](data-factory-data-management-gateway.md) artikel.

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

**De invoergegevensset MongoDB:** Instellen van "extern": "true" informeert de Data Factory-service dat de tabel bevindt zich buiten de data factory en niet door een activiteit in de data factory wordt geproduceerd.

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

Gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1). Het pad naar de map voor de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

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

**De kopieeractiviteit in een pijplijn met de MongoDB-bron- en Blob:**

De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de bovenstaande invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **MongoDbSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de **query** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

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
Azure Data Factory-service afleidt schema uit een MongoDB-verzameling met behulp van de meest recente 100 documenten in de verzameling. Als deze 100 documenten niet volledige schema bevatten, kunnen sommige kolommen worden genegeerd tijdens de kopieerbewerking.

## <a name="type-mapping-for-mongodb"></a>Toewijzing van het type voor MongoDB
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieeractiviteit voert automatische conversie van de typen gegevensbronnen met sink-type met de volgende stappen 2-benadering:

1. Systeemeigen brontypen converteren naar .NET-type
2. .NET-type converteren naar systeemeigen sink-type

Bij het verplaatsen van gegevens met MongoDB worden de volgende toewijzingen van MongoDB typen gebruikt om .NET-typen.

| MongoDB-type | .NET framework-type |
| --- | --- |
| Binair bestand |Byte[] |
| Booleaans |Booleaans |
| Date |DateTime |
| NumberDouble |Double-waarde |
| NumberInt |Int32 |
| NumberLong |Int64 |
| ObjectID |Reeks |
| Reeks |Reeks |
| UUID |GUID |
| Object |Renormalized afvlakken in kolommen met '_' als geneste scheidingsteken |

> [!NOTE]
> Raadpleeg voor meer informatie over ondersteuning voor arrays met behulp van virtuele tabellen, [ondersteuning voor complexe typen met behulp van virtuele tabellen](#support-for-complex-types-using-virtual-tables) onderstaande sectie.

De volgende typen van de MongoDB-gegevens worden op dit moment niet ondersteund: DBPointer, JavaScript, Max/Min-sleutel, reguliere expressie, symbool, Timestamp, niet gedefinieerd

## <a name="support-for-complex-types-using-virtual-tables"></a>Ondersteuning voor complexe typen virtuele tabellen gebruiken
Azure Data Factory maakt gebruik van een ingebouwde ODBC-stuurprogramma's verbinding maken met en gegevens kopiëren van de MongoDB-database. Voor complexe gegevenstypen zoals matrices of objecten met verschillende typen in de documenten Normaliseert het stuurprogramma opnieuw gegevens in de bijbehorende virtuele tabellen. Met name als een tabel bevat een dergelijke kolommen, genereert het stuurprogramma in de volgende virtuele tabellen:

* Een **basistabel**, die dezelfde gegevens als de echte tabel, met uitzondering van het complexe typekolommen bevat. De basistabel gebruikt dezelfde naam als de echte tabel die wordt vertegenwoordigd.
* Een **virtuele tabel** voor elke typekolom complexe waarmee de geneste gegevens wordt uitgebreid. De virtuele tabellen zijn met de naam met de naam van de echte tabel, een scheidingsteken '_' en de naam van de matrix of één object.

Virtuele tabellen verwijzen naar de gegevens in de echte tabel, waardoor het stuurprogramma voor toegang tot de Gedenormaliseerde gegevens. Zie de sectie voorbeeld hieronder meer informatie. U kunt toegang tot de inhoud van de MongoDB-matrices door het uitvoeren van query's en deze koppelen aan de virtuele tabellen.

U kunt de [Kopieerwizard](data-factory-data-movement-activities.md#create-a-pipeline-with-copy-activity) intuïtief weergeven van de lijst met tabellen in MongoDB-database met inbegrip van de virtuele tabellen en voorbeeld van de gegevens in. U kunt ook een query in de Wizard kopiëren maken en valideren als het resultaat wilt weergeven.

### <a name="example"></a>Voorbeeld
Bijvoorbeeld, is "ExampleTable" hieronder een MongoDB-tabel met één kolom met een matrix met objecten in elke cel – facturen en één kolom met een matrix met scalaire typen – classificaties.

| i_d | Naam van de klant | Facturen | Servicelaag | Beoordelingen |
| --- | --- | --- | --- | --- |
| 1111 |ABC |[{invoice_id: "123", artikel: "toaster", prijs: "456", korting: "0.2"}, {invoice_id: "124", item: "oven", prijs: korting "1235",: "0.2"}] |Zilver |[5,6] |
| 2222 |XYZ |[{invoice_id: "135", item: "koelkast", price: "12543", korting: "0.0"}] |Goudkleurig |[1,2] |

Het stuurprogramma genereert meerdere virtuele tabellen om weer te geven deze enkele tabel. De eerste virtuele tabel is de basistabel met de naam 'ExampleTable', hieronder weergegeven. De basistabel bevat alle gegevens van de oorspronkelijke tabel, maar de gegevens van de matrices is weggelaten en in de virtuele tabellen is uitgevouwen.

| i_d | Naam van de klant | Servicelaag |
| --- | --- | --- |
| 1111 |ABC |Zilver |
| 2222 |XYZ |Goudkleurig |

De volgende tabellen ziet u de virtuele tabellen waarbij de oorspronkelijke matrices in het voorbeeld. Deze tabellen bevatten de volgende:

* Een verwijzing naar de oorspronkelijke primaire-sleutelkolom overeenkomt met de rij van de oorspronkelijke matrix (via de kolom i_d)
* Een indicatie van de positie van de gegevens binnen de oorspronkelijke matrix
* De uitgebreide gegevens voor elk element in de matrix

Tabel 'ExampleTable_Invoices':

| i_d | ExampleTable_Invoices_dim1_idx | invoice_id | Item | price | Korting |
| --- | --- | --- | --- | --- | --- |
| 1111 |0 |123 |toaster |456 |0.2 |
| 1111 |1 |124 |oven |1235 |0.2 |
| 2222 |0 |135 |koelkast |12543 |0,0 |

Tabel 'ExampleTable_Ratings':

| i_d | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings |
| --- | --- | --- |
| 1111 |0 |5 |
| 1111 |1 |6 |
| 2222 |0 |1 |
| 2222 |1 |2 |

## <a name="map-source-to-sink-columns"></a>Kaartbron met sink-kolommen
Zie voor meer informatie over het toewijzen van kolommen in de brongegevensset naar kolommen in de sink-gegevensset, [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Herhaalbare lezen van relationele bronnen
Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, houd er rekening mee om te voorkomen dat ongewenste resultaten herhaalbaarheid. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment wordt opnieuw uitgevoerd in beide gevallen, moet u om ervoor te zorgen dat de dezelfde gegevens worden gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable leesrechten voor relationele bronnen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie [prestaties kopiëren en Afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren.

## <a name="next-steps"></a>Volgende stappen
Zie [gegevens verplaatsen tussen on-premises en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies voor het maken van een pijplijn die gegevens verplaatst van een on-premises gegevensarchief naar een Azure-gegevensopslag.
