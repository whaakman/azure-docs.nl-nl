---
title: Gegevens verplaatsen van SAP Business Warehouse met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit SAP Business Warehouse met behulp van Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 9e972ee64d60f0fc9703e766c3ab45c3057c32a2
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54019874"
---
# <a name="move-data-from-sap-business-warehouse-using-azure-data-factory"></a>Gegevens verplaatsen van SAP Business Warehouse met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-sap-business-warehouse-connector.md)
> * [Versie 2 (huidige versie)](../connector-sap-business-warehouse.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [SAP Business Warehouse-connector in V2](../connector-sap-business-warehouse.md).


In dit artikel wordt uitgelegd hoe u van de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van een on-premises SAP Business Warehouse (BW). Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit geeft.

U kunt gegevens uit een on-premises SAP Business Warehouse-gegevensarchief kopiëren naar een ondersteunde sink-gegevensopslag. Zie voor een lijst met gegevensarchieven die worden ondersteund als sink voor de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. Data factory ondersteunt momenteel alleen gegevens te verplaatsen van een SAP Business Warehouse naar andere gegevensarchieven, maar niet voor het verplaatsen van gegevens uit andere gegevensarchieven naar een SAP Business Warehouse. 

## <a name="supported-versions-and-installation"></a>Ondersteunde versies en installatie
Deze connector biedt ondersteuning voor SAP Business Warehouse-versie 7.x. Het ondersteunt kopiëren van gegevens vanuit InfoCubes en QueryCubes (inclusief BEx-query's) met behulp van MDX-query's.

Om in te schakelen van de verbinding met de SAP BW-exemplaar, de volgende onderdelen te installeren:
- **Data Management Gateway**: Data Factory-service ondersteunt het verbinden met on-premises gegevensarchieven (met inbegrip van SAP Business Warehouse) met behulp van een onderdeel Data Management Gateway genoemd. Zie voor meer informatie over Data Management Gateway en stapsgewijze instructies voor het instellen van de gateway, [gegevens verplaatsen tussen on-premises gegevens opslaan in de cloud gegevensarchief](data-factory-move-data-between-onprem-and-cloud.md) artikel. Gateway is vereist, zelfs als de SAP Business Warehouse wordt gehost in een Azure IaaS-machine (VM). U kunt de gateway installeren op dezelfde virtuele machine als het gegevensarchief of een andere virtuele machine, zolang de gateway verbinding met de database maken kan.
- **SAP NetWeaver-bibliotheek** op de gatewaycomputer. U kunt de SAP Netweaver-bibliotheek ophalen van uw SAP-beheerder, of rechtstreeks vanuit de [SAP Software Download Center](https://support.sap.com/swdc). Zoek de **SAP Opmerking #1025361** om op te halen van de downloadlocatie voor de meest recente versie. Zorg ervoor dat de architectuur voor de SAP NetWeaver-bibliotheek (32-bits of 64-bits) overeenkomt met de gatewayinstallatie. Installeer vervolgens alle bestanden die zijn opgenomen in de SAP NetWeaver RFC-SDK op basis van de SAP-notitie. De SAP NetWeaver-bibliotheek is ook opgenomen in de installatie van de SAP-clienthulpprogramma's.

> [!TIP]
> Plaats de dll-bestanden in de map system32 van de NetWeaver RFC-SDK hebt uitgepakt.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit die gegevens uit een on-premises Cassandra-gegevensarchief verplaatst met behulp van verschillende hulpprogramma's / API's. 

- De eenvoudigste manier om een pijplijn te maken is met de **Kopieerwizard**. Zie [zelfstudie: Een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht van het maken van een pijplijn met behulp van de wizard kopiëren. 
- U kunt ook de volgende hulpprogramma's gebruiken om een pijplijn te maken: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en  **REST-API**. Zie [zelfstudie Kopieeractiviteit](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor het maken van een pijplijn met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruikt, kunt u de volgende stappen uit voor het maken van een pijplijn die gegevens van een brongegevensarchief naar een sink-gegevensopslag verplaatst uitvoeren:

1. Maak **gekoppelde services** opgeslagen om invoer- en gegevens te koppelen aan uw data factory.
2. Maak **gegevenssets** te vertegenwoordigen invoer- en uitvoergegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en de pijplijn) automatisch voor u gemaakt. Wanneer u hulpprogramma's / API's (met uitzondering van de .NET API), kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren uit een on-premises SAP Business Warehouse, [JSON-voorbeeld: Gegevens kopiëren van SAP Business Warehouse naar Azure Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) sectie van dit artikel. 

De volgende secties bevatten meer informatie over JSON-eigenschappen die worden gebruikt voor het definiëren van Data Factory-entiteiten specifieke met een SAP BW-gegevensarchief:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor SAP Business Warehouse (BW) gekoppelde service.

Eigenschap | Description | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
server | Naam van de server waarop de SAP BW-instantie zich bevindt. | string | Ja
systemNumber | Het systeemnummer van de SAP BW-systeem. | Decimaal getal van twee cijfers weergegeven als een tekenreeks. | Ja
ClientId | Client-ID van de client in het W SAP-systeem. | Decimaal getal van drie cijfers wordt weergegeven als een tekenreeks. | Ja
gebruikersnaam | Naam van de gebruiker die toegang tot de SAP-server heeft | string | Ja
wachtwoord | Het wachtwoord voor de gebruiker. | string | Ja
gatewayName | De naam van de gateway die de Data Factory-service gebruiken moet voor verbinding met de on-premises SAP BW-exemplaar. | string | Ja
encryptedCredential | De versleutelde referentie-tekenreeks. | string | Nee

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets, de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties, zoals de structuur, beschikbaarheid en het beleid van een gegevensset JSON zijn vergelijkbaar voor alle typen van gegevensset (Azure SQL, Azure-blob, Azure-tabel, enz.).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. Er zijn geen specifieke eigenschappen die worden ondersteund voor de SAP BW-gegevensset van het type **RelationalTable**. 


## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten, de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen, zijn de beleidsregels zijn beschikbaar voor alle soorten activiteiten.

Dat eigenschappen die beschikbaar zijn in de **typeProperties** sectie van de activiteit variëren met elk activiteitstype. Ze verschillen voor de kopieeractiviteit, afhankelijk van de typen van bronnen en sinks.

Wanneer u de gegevensbron in de kopieeractiviteit is van het type **RelationalSource** (waaronder SAP BW), de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query | Hiermee geeft u de MDX-query voor het lezen van gegevens van de SAP BW-exemplaar. | MDX-query. | Ja |


## <a name="json-example-copy-data-from-sap-business-warehouse-to-azure-blob"></a>JSON-voorbeeld: Gegevens kopiëren van SAP Business Warehouse naar Azure Blob
Het volgende voorbeeld biedt een voorbeeld van JSON-definities die u gebruiken kunt voor het maken van een pijplijn met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Dit voorbeeld laat zien hoe u gegevens kopiëren van een on-premises SAP Business Warehouse naar een Azure Blob-opslag. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** aan een van de vermelde sinks [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.  

> [!IMPORTANT]
> In dit voorbeeld bevat JSON-fragmenten. Deze omvatten geen stapsgewijze instructies voor het maken van de data factory. Zie [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies.

Het voorbeeld heeft de volgende data factory-entiteiten:

1. Een gekoppelde service van het type [SapBw](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [RelationalTable](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [RelationalSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld worden gegevens gekopieerd van een SAP Business Warehouse-exemplaar naar een Azure-blob per uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

Instellen als eerste stap de data management gateway. De instructies vindt u in de [om gegevens te verplaatsen tussen on-premises locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

### <a name="sap-business-warehouse-linked-service"></a>SAP Business Warehouse gekoppelde service
Deze gekoppelde service koppelt uw SAP BW-exemplaar aan de data factory. De eigenschap type is ingesteld op **SapBw**. De sectie typeProperties biedt verbindingsgegevens voor de SAP BW-instantie. 

```json
{
    "name": "SapBwLinkedService",
    "properties":
    {
        "type": "SapBw",
        "typeProperties":
        {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service
Deze gekoppelde service koppelt uw Azure Storage-account aan de data factory. De eigenschap type is ingesteld op **AzureStorage**. De sectie typeProperties biedt verbindingsgegevens voor de Azure Storage-account.

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

### <a name="sap-bw-input-dataset"></a>SAP BW-invoergegevensset
Deze gegevensset bepaalt de SAP Business Warehouse-gegevensset. Instellen van het type van de Data Factory-gegevensset in **RelationalTable**. U opgeeft op dit moment niet elk type-specifieke eigenschappen voor een SAP BW-gegevensset. De query in het definitie van de Kopieeractiviteit geeft aan welke gegevens worden gelezen uit de SAP BW-instantie. 

Externe eigenschap instelt op true, wordt de Data Factory-service geïnformeerd dat de tabel bevindt zich buiten de data factory en niet door een activiteit in de data factory wordt geproduceerd.

Eigenschappen van de frequentie en interval definieert de planning. In dit geval de gegevens gelezen uit de SAP BW-instantie per uur. 

```json
{
    "name": "SapBwDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapBwLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```



### <a name="azure-blob-output-dataset"></a>Azure Blob-uitvoergegevensset
Deze gegevensset bepaalt de uitvoer-Azure Blob-gegevensset. De eigenschap type is ingesteld op AzureBlob. De sectie typeProperties biedt waar de gegevens die zijn gekopieerd uit de SAP BW-instantie is opgeslagen. De gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur en interval: 1). Het pad naar de map voor de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sapbw/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="pipeline-with-copy-activity"></a>Pijplijn met kopieeractiviteit
De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van de invoer- en uitvoergegevenssets en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **RelationalSource** (voor SAP BW-bron) en **sink** type is ingesteld op **BlobSink**. De query die is opgegeven voor de **query** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "<MDX query for SAP BW>"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "SapBwDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDataSet"
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
                "name": "SapBwToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```



### <a name="type-mapping-for-sap-bw"></a>Toewijzing van het type voor SAP BW
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieeractiviteit wordt uitgevoerd automatisch typeconversies van typen gegevensbronnen met sink-type met de volgende methode voor verificatie in twee stappen:

1. Systeemeigen brontypen converteren naar .NET-type
2. .NET-type converteren naar systeemeigen sink-type

Bij het verplaatsen van gegevens van SAP BW, worden de volgende toewijzingen van SAP BW-typen gebruikt om .NET-typen.

Het gegevenstype in de ABAP-woordenlijst | .NET-gegevenstype
-------------------------------- | --------------
ACCP |  Int
CHAR | Reeks
CLNT | Reeks
VAL | Decimaal
CUKY | Reeks
DECEMBER | Decimaal
FLTP | Double-waarde
INT1 | Byte
INT2 | Int16
INT4 | Int
LANG | Reeks
LCHR | Reeks
LRAW | Byte[]
PREC | Int16
QUAN | Decimaal
ONBEWERKTE | Byte[]
RAWSTRING | Byte[]
TEKENREEKS | Reeks
EENHEID | Reeks
DATS | Reeks
NUMC | Reeks
TIM | Reeks

> [!NOTE]
> Zie het toewijzen van kolommen in de brongegevensset op kolommen uit de sink-gegevensset [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).


## <a name="map-source-to-sink-columns"></a>Kaartbron met sink-kolommen
Zie voor meer informatie over het toewijzen van kolommen in de brongegevensset naar kolommen in de sink-gegevensset, [toewijzing van kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Herhaalbare lezen van relationele bronnen
Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, houd er rekening mee om te voorkomen dat ongewenste resultaten herhaalbaarheid. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment wordt opnieuw uitgevoerd in beide gevallen, moet u om ervoor te zorgen dat de dezelfde gegevens worden gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable leesrechten voor relationele bronnen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Prestaties en afstemmen
Zie [prestaties kopiëren en Afstemmingshandleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die invloed prestaties van de verplaatsing van gegevens (Kopieeractiviteit) in Azure Data Factory en de verschillende manieren om te optimaliseren.
