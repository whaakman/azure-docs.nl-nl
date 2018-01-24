---
title: Verplaatsen van gegevens uit een SAP Business Warehouse met behulp van Azure Data Factory | Microsoft Docs
description: Meer informatie over het verplaatsen van gegevens uit een SAP Business Warehouse met behulp van Azure Data Factory.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: b81dc9f13533eaeec56625ede0e4c534b83e7cf7
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="move-data-from-sap-business-warehouse-using-azure-data-factory"></a>Verplaatsen van gegevens uit SAP Business Warehouse met behulp van Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-sap-business-warehouse-connector.md)
> * [Versie 2 - Preview](../connector-sap-business-warehouse.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [SAP Business Warehouse-connector in V2](../connector-sap-business-warehouse.md).


In dit artikel wordt uitgelegd hoe de Kopieeractiviteit in Azure Data Factory om gegevens te verplaatsen van een lokale SAP Business Warehouse (BW) gebruiken. Dit is gebaseerd op de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel, hetgeen een algemeen overzicht van de verplaatsing van gegevens met de kopieeractiviteit toont.

U kunt gegevens uit een on-premises SAP Business Warehouse-gegevensopslag kopiëren naar een ondersteunde sink-gegevensarchief. Zie voor een lijst met gegevensarchieven als PUT wordt ondersteund door de kopieeractiviteit, de [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabel. Data factory ondersteunt momenteel alleen zwevend gegevens uit een SAP Business Warehouse tot andere gegevensarchieven, maar niet voor het verplaatsen van gegevens uit andere gegevensarchieven naar een SAP Business Warehouse. 

## <a name="supported-versions-and-installation"></a>Ondersteunde versies en installatie
Deze connector ondersteunt SAP Business Warehouse versie 7.x. Het ondersteunt kopiëren van gegevens van InfoCubes en QueryCubes (inclusief BEx query's) met behulp van de MDX-query's.

Als u de verbinding met het exemplaar SAP BW installeert de volgende onderdelen:
- **Data Management Gateway**: Data Factory-service ondersteunt het verbinden met on-premises gegevens winkels (inclusief SAP Business Warehouse) met een onderdeel genaamd Data Management Gateway. Zie voor meer informatie over Data Management Gateway en stapsgewijze instructies voor het instellen van de gateway, [Moving gegevens tussen on-premises gegevens opslaan in de cloud gegevensarchief](data-factory-move-data-between-onprem-and-cloud.md) artikel. Gateway is vereist, zelfs als de SAP Business Warehouse wordt gehost in een Azure IaaS virtuele machine (VM). U kunt de gateway installeren op dezelfde virtuele machine als gegevensopslag of op een andere virtuele machine, zolang de gateway verbinding met de database maken kan.
- **SAP NetWeaver bibliotheek** op de gateway-apparaat. U kunt de bibliotheek SAP Netweaver ophalen uit de SAP-beheerder of rechtstreeks uit de [SAP Software Download Center](https://support.sap.com/swdc). Zoeken naar de **SAP-notitie #1025361** ophalen van de downloadlocatie voor de meest recente versie. Zorg ervoor dat de architectuur voor de bibliotheek SAP NetWeaver (32-bits of 64-bits) overeenkomt met de gateway-installatie. Installeer vervolgens alle bestanden die zijn opgenomen in de SDK SAP NetWeaver RFC volgens de SAP-notitie. De bibliotheek SAP NetWeaver is ook opgenomen in de clienthulpprogramma's voor SAP-installatie.

> [!TIP]
> De DLL's van de NetWeaver RFC-SDK hebt uitgepakt in de map system32 plaatsen.

## <a name="getting-started"></a>Aan de slag
U kunt een pijplijn maken met een kopieeractiviteit waarmee gegevens vanuit een on-premises Cassandra-gegevensopslag verplaatst met behulp van verschillende hulpprogramma's voor API's. 

- De eenvoudigste manier om een pijplijn maken is met de **Wizard kopiëren**. Zie [zelfstudie: een pijplijn maken met de Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md) voor een snel overzicht over het maken van een pijplijn met de wizard kopiëren. 
- U kunt ook de volgende hulpprogramma's gebruiken voor het maken van een pijplijn: **Azure-portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sjabloon**, **.NET API**, en **REST-API**. Zie [kopie activiteit zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) voor stapsgewijze instructies voor een pijplijn maken met een kopieeractiviteit. 

Of u de hulpprogramma's of API's gebruiken, moet u de volgende stappen voor het maken van een pijplijn die de gegevens vanuit een brongegevensarchief naar een gegevensarchief sink verplaatst uitvoeren:

1. Maak **gekoppelde services** slaat om invoer- en gegevens te koppelen aan uw gegevensfactory.
2. Maak **gegevenssets** vertegenwoordigt de invoer- en -gegevens voor de kopieerbewerking. 
3. Maak een **pijplijn** met een kopieeractiviteit waarmee een gegevensset als invoer en een gegevensset als uitvoer. 

Wanneer u de wizard gebruikt, worden de JSON-definities voor deze Data Factory-entiteiten (gekoppelde services, gegevenssets en pijplijn) automatisch voor u gemaakt. Wanneer u extra/API's (met uitzondering van de .NET API) gebruikt, kunt u deze Data Factory-entiteiten definiëren met behulp van de JSON-indeling.  Zie voor een voorbeeld met JSON-definities voor Data Factory-entiteiten die worden gebruikt om gegevens te kopiëren van een lokale SAP Business Warehouse [JSON-voorbeeld: gegevens kopiëren van een SAP Business Warehouse naar Azure Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) sectie van dit artikel. 

De volgende secties bevatten informatie over de JSON-eigenschappen die worden gebruikt voor het definiëren van specifieke Data Factory-entiteiten met een SAP BW-gegevensopslag:

## <a name="linked-service-properties"></a>Eigenschappen van de gekoppelde service
De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor SAP Business Warehouse (BW) gekoppelde service.

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
server | Naam van de server waarop het exemplaar SAP BW zich bevindt. | tekenreeks | Ja
systemNumber | Systeemnummer van het SAP BW-systeem. | Twee cijfers decimaal getal weergegeven als een tekenreeks. | Ja
clientId | Client-ID van de client in het systeem SAP-W. | Drie cijfers decimaal getal weergegeven als een tekenreeks. | Ja
gebruikersnaam | Naam van de gebruiker die toegang tot de SAP-server heeft | tekenreeks | Ja
wachtwoord | Wachtwoord voor de gebruiker. | tekenreeks | Ja
gatewayName | Naam van de gateway die voor de Data Factory-service gebruiken moet voor verbinding met de lokale SAP BW-exemplaar. | tekenreeks | Ja
encryptedCredential | De versleutelde referentie-tekenreeks. | tekenreeks | Nee

## <a name="dataset-properties"></a>Eigenschappen van gegevensset
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets van de [gegevenssets maken](data-factory-create-datasets.md) artikel. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON zijn identiek voor alle gegevensset typen (Azure SQL, Azure blob-, Azure-tabel, enz.).

De **typeProperties** sectie verschilt voor elk type gegevensset en bevat informatie over de locatie van de gegevens in het gegevensarchief. Er zijn geen type-specifieke eigenschappen voor de gegevensset SAP BW van het type ondersteund **RelationalTable**. 


## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit
Zie voor een volledige lijst van de secties en de eigenschappen die beschikbaar zijn voor het definiëren van activiteiten van de [pijplijnen maken](data-factory-create-pipelines.md) artikel. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen, zijn de beleidsregels zijn beschikbaar voor alle typen activiteiten.

Dat eigenschappen beschikbaar zijn in de **typeProperties** sectie van de activiteit variëren met elk activiteitstype. Voor de kopieeractiviteit variëren ze, afhankelijk van de typen van bronnen en Put.

Wanneer u de gegevensbron in de kopieerbewerking is van het type **RelationalSource** (waaronder SAP BW), de volgende eigenschappen beschikbaar zijn in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query | Hiermee geeft u de MDX-query om te lezen van gegevens uit de SAP BW-exemplaar. | MDX query. | Ja |


## <a name="json-example-copy-data-from-sap-business-warehouse-to-azure-blob"></a>JSON-voorbeeld: gegevens kopiëren van een SAP Business Warehouse naar Azure Blob
Het volgende voorbeeld bevat definities van de voorbeeld-JSON die u een pijplijn maken kunt met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Dit voorbeeld laat zien hoe gegevens kopiëren van een lokale SAP Business Warehouse naar een Azure Blob Storage. Echter, de gegevens kunnen worden gekopieerd **rechtstreeks** aan een van de PUT vermeld [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) met behulp van de Kopieeractiviteit in Azure Data Factory.  

> [!IMPORTANT]
> Dit voorbeeld bevat JSON-fragmenten. Dit omvat geen stapsgewijze instructies voor het maken van de gegevensfactory. Zie [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel voor stapsgewijze instructies.

Het voorbeeld heeft de volgende data factory-entiteiten:

1. Een gekoppelde service van het type [SapBw](#linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Invoer [gegevensset](data-factory-create-datasets.md) van het type [RelationalTable](#dataset-properties).
4. Uitvoer [gegevensset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. Een [pijplijn](data-factory-create-pipelines.md) met de Kopieeractiviteit die gebruikmaakt van [RelationalSource](#copy-activity-properties) en [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

Het voorbeeld worden gegevens gekopieerd van een SAP Business Warehouse-exemplaar met een Azure-blob per uur. De JSON-eigenschappen die in deze voorbeelden worden beschreven in de secties na de voorbeelden.

Het instellen van de data management gateway als eerste stap. De instructies vindt u in de [verplaatsen van gegevens tussen lokale locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) artikel.

### <a name="sap-business-warehouse-linked-service"></a>SAP Business Warehouse gekoppelde service
Deze service koppelingen uw SAP BW-exemplaar gekoppeld aan de gegevensfactory. De eigenschap type is ingesteld op **SapBw**. De sectie typeProperties biedt verbindingsgegevens voor het SAP BW-exemplaar. 

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
Deze service koppelingen uw Azure Storage-account gekoppeld aan de gegevensfactory. De eigenschap type is ingesteld op **AzureStorage**. De sectie typeProperties biedt verbindingsgegevens voor de Azure Storage-account.

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

### <a name="sap-bw-input-dataset"></a>Invoergegevensset voor SAP BW
Deze gegevensset definieert de SAP Business Warehouse-gegevensset. Instellen van het type van de Data Factory-gegevensset **RelationalTable**. Op dit moment kunt opgeeft u geen type-specifieke eigenschappen voor een gegevensset SAP BW. De query in de definitie van de Kopieeractiviteit geeft aan welke gegevens moeten worden gelezen uit de SAP BW-exemplaar. 

De Data Factory-service als externe eigenschap instelt op true worden informeert dat de tabel aan de gegevensfactory extern en niet wordt geproduceerd door een activiteit in de gegevensfactory.

Frequentie en het interval eigenschappen definieert de planning. In dit geval worden de gegevens gelezen uit de SAP BW-exemplaar per uur. 

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
Deze gegevensset definieert de uitvoer-Azure Blob-gegevensset. De eigenschap type is ingesteld op AzureBlob. De sectie typeProperties biedt waar de gegevens die zijn gekopieerd uit de SAP BW-exemplaar wordt opgeslagen. De gegevens worden geschreven naar een nieuwe blob elk uur (frequentie: uur, interval: 1). Het pad naar de blob wordt dynamisch geëvalueerd op basis van de begintijd van het segment dat wordt verwerkt. Het mappad maakt gebruik van jaar, maand, dag en uur delen van de begintijd.

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


### <a name="pipeline-with-copy-activity"></a>Pijplijn met de kopieeractiviteit
De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van de invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **RelationalSource** (voor SAP BW-bron) en **sink** type is ingesteld op **BlobSink**. De query is opgegeven voor de **query** eigenschap selecteert u de gegevens in het afgelopen uur te kopiëren.

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
Zoals vermeld in de [activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) artikel kopieerbewerking wordt automatische typeconversies van brontypen opvangen typen met de volgende benadering voor in twee stappen uitgevoerd:

1. Systeemeigen brontypen converteren naar .NET-type
2. Converteren van .NET-type naar systeemeigen sink-type

Bij het verplaatsen van gegevens uit SAP BW, worden de volgende toewijzingen uit SAP BW typen gebruikt voor .NET-typen.

Gegevenstype in de woordenlijst ABAP | .NET-gegevenstype
-------------------------------- | --------------
ACCP |  Int
CHAR | Tekenreeks
CLNT | Tekenreeks
CURR | Decimale
CUKY | Tekenreeks
DEC | Decimale
FLTP | Double
INT1 | Byte
INT2 | Int16
INT4 | Int
LANG | Tekenreeks
LCHR | Tekenreeks
LRAW | Byte[]
PREC | Int16
QUAN | Decimale
RAW | Byte[]
RAWSTRING | Byte[]
TEKENREEKS | Tekenreeks
EENHEID | Tekenreeks
DATS | Tekenreeks
NUMC | Tekenreeks
TIMS | Tekenreeks

> [!NOTE]
> Zie het toewijzen van kolommen uit de bron-gegevensset naar kolommen uit de dataset sink [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).


## <a name="map-source-to-sink-columns"></a>Bron van de kaart opvangen kolommen
Zie voor meer informatie over het toewijzen van kolommen in gegevensset naar kolommen in gegevensset sink bron, [toewijzing gegevensset kolommen in Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Herhaalbare leesbewerking van relationele bronnen
Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, moet u herhaalbaarheid Houd in gedachten om te voorkomen dat ongewenste resultaten. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook een beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment opnieuw in beide gevallen wordt uitgevoerd, moet u ervoor zorgen dat dezelfde gegevens is gelezen ongeacht hoe vaak een segment wordt uitgevoerd. Zie [Repeatable van relationele bronnen lezen](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Prestaties en het afstemmen
Zie [uitvoering van de activiteit & afstemmen handleiding](data-factory-copy-activity-performance.md) voor meer informatie over de belangrijkste factoren die prestaties impact van gegevensverplaatsing (Kopieeractiviteit) in Azure Data Factory en verschillende manieren om te optimaliseren.
