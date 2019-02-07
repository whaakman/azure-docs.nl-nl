---
title: Gegevenssets maken in Azure Data Factory | Microsoft Docs
description: Informatie over het maken van gegevenssets in Azure Data Factory, met voorbeelden met eigenschappen op, zoals de offset en anchorDateTime.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 0614cd24-2ff0-49d3-9301-06052fd4f92a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: af90a946f12e11602d45300a2796787f839dcf02
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55811083"
---
# <a name="datasets-in-azure-data-factory"></a>Gegevenssets in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-create-datasets.md)
> * [Versie 2 (huidige versie)](../concepts-datasets-linked-services.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [gegevenssets in V2](../concepts-datasets-linked-services.md).

Dit artikel wordt beschreven welke gegevenssets bent, hoe ze worden gedefinieerd in JSON-indeling, en hoe ze worden gebruikt Azure Data Factory-pijplijnen. Het biedt details over elke sectie (bijvoorbeeld structuur, beschikbaarheid en beleid) in de gegevensset JSON-definitie. Het artikel bevat ook voorbeelden voor het gebruik van de **offset**, **anchorDateTime**, en **stijl** eigenschappen in de JSON-definitie van een gegevensset.

> [!NOTE]
> Als u niet bekend bent met Data Factory, raadpleegt u [Inleiding tot Azure Data Factory](data-factory-introduction.md) voor een overzicht. Als u praktische ervaring met het maken van data factory's hebt, krijgt u een beter begrip van het lezen van de [data transformation zelfstudie](data-factory-build-your-first-pipeline.md) en de [data movement zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="overview"></a>Overzicht
Een gegevensfactory kan één of meer pijplijnen hebben. Een **pijplijn** is een logische groepering van **activiteiten** die samen een taak uitvoeren. Met activiteiten in een pijplijn definieert u welk acties moeten worden uitgevoerd voor uw gegevens. Bijvoorbeeld, kunt u een kopieeractiviteit om gegevens te kopiëren uit een on-premises SQL Server naar Azure Blob storage. Vervolgens kunt u een Hive-activiteit die een Hive-script uitvoert op een Azure HDInsight-cluster om gegevens te verwerken van Blob-opslag om uitvoergegevens te produceren. U kunt tot slot een tweede kopieeractiviteit gebruiken om het kopiëren van de uitvoergegevens naar Azure SQL Data Warehouse, boven op welke business intelligence (BI) reporting oplossingen zijn gemaakt. Zie voor meer informatie over pijplijnen en activiteiten [pijplijnen en activiteiten in Azure Data Factory](data-factory-create-pipelines.md).

Een activiteit kan nul of meer duren **gegevenssets**, en een of meer uitvoergegevenssets produceren. Een invoergegevensset vertegenwoordigt de invoer voor een activiteit in de pijplijn en een uitvoergegevensset vertegenwoordigt de uitvoer voor de activiteit. Met gegevenssets worden gegevens binnen andere gegevensarchieven geïdentificeerd, waaronder tabellen, bestanden, mappen en documenten. Een Azure Blob-gegevensset benoemt bijvoorbeeld de blobcontainer en -map in Blob-opslag van waaruit de pijplijn de gegevens moet lezen.

Voordat u een gegevensset maakt, maakt u een **gekoppelde service** uw gegevensopslag aan de data factory koppelen. Gekoppelde services zijn te vergelijken met verbindingsreeksen, die de verbindingsinformatie bevatten die Data Factory nodig heeft om verbinding te maken met externe bronnen. Gegevenssets worden gegevens in de gekoppelde gegevensarchieven, zoals SQL-tabellen, bestanden, mappen en documenten identificeren. Bijvoorbeeld, gekoppelde een Azure Storage-service wordt een storage-account aan de data factory. Een Azure Blob-gegevensset geeft de blob-container en de map met de blobs voor invoer om te worden verwerkt.

Hier volgt een voorbeeldscenario. Om gegevens te kopiëren van Blob-opslag met een SQL-database, moet u twee gekoppelde services maken: Azure Storage en Azure SQL Database. Vervolgens maakt u twee gegevenssets: Azure Blob-gegevensset (die verwijst naar de gekoppelde Azure Storage-service) en Azure SQL Table-gegevensset (die verwijst naar de gekoppelde Azure SQL Database-service). De Azure Storage en de gekoppelde Azure SQL Database-services bevatten verbindingsreeksen die Data Factory tijdens runtime gebruikt verbinding maken met uw Azure Storage en Azure SQL Database, respectievelijk. De Azure Blob-gegevensset specificeert de blob-container en de blob-map met de blobs voor invoer in uw Blob storage. De Azure SQL Table-gegevensset bevat de SQL-tabel in uw SQL-database waarnaar de gegevens zijn om te worden gekopieerd.

Het volgende diagram toont de relaties tussen de pijplijn, activiteit, gegevensset en gekoppelde service in Data Factory:

![Relatie tussen pijplijn, activiteit, gegevensset, gekoppelde services](media/data-factory-create-datasets/relationship-between-data-factory-entities.png)

## <a name="dataset-json"></a>JSON van de gegevensset
Een gegevensset in Data Factory wordt in JSON-indeling als volgt gedefinieerd:

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "external": "<boolean flag to indicate external data. only for input datasets>",
        "linkedServiceName": "<Name of the linked service that refers to a data store.>",
        "structure": [
            {
                "name": "<Name of the column>",
                "type": "<Name of the type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        },
        "availability": {
            "frequency": "<Specifies the time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
            "interval": "<Specifies the interval within the defined frequency. For example, frequency set to 'Hour' and interval set to 1 indicates that new data slices should be produced hourly>"
        },
        "policy":
        {
        }
    }
}
```

De volgende tabel beschrijft de eigenschappen in de bovenstaande JSON:

| Eigenschap | Description | Vereist | Standaard |
| --- | --- | --- | --- |
| naam |Naam van de gegevensset. Zie [Azure Data Factory - naamgevingsregels](data-factory-naming-rules.md) voor naamgevingsregels. |Ja |N.v.t. |
| type |Het type van de gegevensset. Geef een van de typen die worden ondersteund door Data Factory (bijvoorbeeld: AzureBlob, AzureSqlTable). <br/><br/>Zie voor meer informatie, [gegevenssettype](#Type). |Ja |N.v.t. |
| structuur |Het schema van de gegevensset.<br/><br/>Zie voor meer informatie, [gegevenssetstructuur](#Structure). |Nee |N.v.t. |
| typeProperties | De type-eigenschappen zijn verschillend voor elk type (bijvoorbeeld: Azure Blob-, Azure SQL-tabel). Zie voor meer informatie over de ondersteunde typen en de bijbehorende eigenschappen, [gegevenssettype](#Type). |Ja |N.v.t. |
| external | Booleaanse vlag om op te geven of een gegevensset expliciet wordt geproduceerd door een data factory-pijplijn of niet. Als de invoergegevensset voor een activiteit wordt niet door de huidige pijplijn geproduceerd, moet u deze markering instellen op true. Deze markering instellen op true voor de invoergegevensset van de eerste activiteit in de pijplijn.  |Nee |false |
| availability | Definieert het segmenteringsmodel (bijvoorbeeld elk uur of dagelijks) of het segmenteringshulplijnen model voor de gegevensset voor productie. Elke eenheid van de gegevens verwerkt en geproduceerd door het uitvoeren van een activiteit wordt een gegevenssegment genoemd. Als de beschikbaarheid van een uitvoergegevensset is ingesteld op dagelijks (frequentie: dag, interval: 1), wordt een segment dagelijks geproduceerd. <br/><br/>Zie voor meer informatie, beschikbaarheid van gegevenssets. <br/><br/>Zie voor meer informatie over de gegevensset voor het verwerkingsvenster, de [planning en uitvoering](data-factory-scheduling-and-execution.md) artikel. |Ja |N.v.t. |
| policy |Definieert de criteria of de voorwaarde die moeten voldoen aan de gegevensset segmenten. <br/><br/>Zie voor meer informatie, de [gegevensset beleid](#Policy) sectie. |Nee |N.v.t. |

## <a name="dataset-example"></a>Voorbeeld van de gegevensset
In het volgende voorbeeld wordt de gegevensset vertegenwoordigt een tabel met de naam **MyTable** in een SQL-database.

```json
{
    "name": "DatasetSample",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties":
        {
            "tableName": "MyTable"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

Houd rekening met de volgende punten:

* **type** is ingesteld op AzureSqlTable.
* **tableName** type eigenschap (specifiek voor AzureSqlTable) is ingesteld op MyTable.
* **linkedServiceName** verwijst naar een gekoppelde service van het type AzureSqlDatabase, die is gedefinieerd in de volgende JSON-codefragment.
* **beschikbaarheid frequentie** is ingesteld op de dag en **interval** is ingesteld op 1. Dit betekent dat het segment gegevensset dagelijks wordt geproduceerd.

**AzureSqlLinkedService** wordt als volgt gedefinieerd:

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

In het voorgaande JSON-fragment:

* **type** is ingesteld op AzureSqlDatabase.
* **connectionString** type-eigenschap geeft u informatie om te verbinden met een SQL-database.

Zoals u ziet, definieert de gekoppelde service verbinding maken met een SQL-database. De gegevensset wordt gedefinieerd welke tabel wordt gebruikt als invoer en uitvoer voor de activiteit in een pijplijn.

> [!IMPORTANT]
> Tenzij een gegevensset wordt geproduceerd door de pijplijn, deze moet worden gemarkeerd als **externe**. Deze instelling geldt doorgaans voor invoer van de eerste activiteit in een pijplijn.

## <a name="Type"></a> Type Brongegevensset
Het type van de gegevensset is afhankelijk van het gegevensarchief dat u gebruikt. Zie de volgende tabel voor een lijst met gegevensarchieven die worden ondersteund door Data Factory. Klik op een gegevensarchief voor informatie over het maken van een gekoppelde service en een gegevensset voor het betreffende gegevensarchief.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Gegevensarchieven met * kunnen zich on-premises of op Azure-infrastructuur als een dienst (IaaS). Deze gegevensarchieven vereisen dat u installeert [Data Management Gateway](data-factory-data-management-gateway.md).

In het voorbeeld in de vorige sectie, het type van de gegevensset is ingesteld op **AzureSqlTable**. Op dezelfde manier het type van de gegevensset voor een Azure Blob-gegevensset is ingesteld op **AzureBlob**, zoals weergegeven in de volgende JSON:

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

## <a name="Structure"></a>Structuur van de gegevensset
De **structuur** sectie is optioneel. Het schema van de gegevensset wordt gedefinieerd door een verzameling van namen en gegevenstypen van kolommen. De structuur sectie kunt u opgeven type-informatie die wordt gebruikt om te converteren en kolommen uit de bron naar de bestemming toewijzen. In het volgende voorbeeld wordt de gegevensset heeft drie kolommen: `slicetimestamp`, `projectname`, en `pageviews`. Ze zijn van het type String, String en Decimal.

```json
structure:
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

Elke kolom in de structuur bevat de volgende eigenschappen:

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| naam |Naam van de kolom. |Ja |
| type |Het gegevenstype van de kolom.  |Nee |
| cultuur |. NET-gebaseerde cultuur moet worden gebruikt wanneer het type een .NET-type is: `Datetime` of `Datetimeoffset`. De standaardwaarde is `en-us`. |Nee |
| Indeling |Tekenreeks die moet worden gebruikt wanneer het type een .NET-type is-indeling: `Datetime` of `Datetimeoffset`. |Nee |

De volgende richtlijnen kunnen u bepalen waar structuur informatie op te nemen, en wat u wilt opnemen de **structuur** sectie.

* **Voor gestructureerde gegevensbronnen**, geef de structuur sectie alleen als u toewijzen bronkolommen wilt sink-kolommen, en hun namen niet hetzelfde zijn. Dit soort gestructureerde gegevens worden opgeslagen gegevens schema en het type informatie samen met de gegevens zelf. Voorbeelden van gestructureerde gegevensbronnen zijn SQL Server, Oracle en Azure-tabel.
  
    Als u informatie over het type is al beschikbaar voor gestructureerde gegevensbronnen, moet u informatie over het type niet opnemen wanneer u de sectie structuur opgeeft.
* **Voor het schema op lezen gegevensbronnen (met name Blob storage)**, u kunt kiezen voor het opslaan van gegevens zonder een schema of het type informatie met de gegevens op te slaan. Voor deze typen gegevensbronnen, structuur opnemen wanneer u toewijzen bronkolommen wilt sink-kolommen. Structuur ook bevatten wanneer de gegevensset een invoer voor een kopieeractiviteit is en gegevenstypen van brongegevensset moeten worden geconverteerd naar native-typen voor de sink.
    
    Data Factory ondersteunt de volgende waarden voor het ontwikkelen van type informatie in de structuur: **Int16, Int32, Int64, één, Double, Decimal, Byte [], Booleaanse waarde, String, Guid, datum/tijd, Datetimeoffset en Timespan**. Deze waarden zijn Common Language Specification (CLS)-compatibel zijn,. Type NET-op basis van waarden.

Data Factory wordt automatisch typeconversies voert bij het verplaatsen van gegevens van een brongegevensarchief naar een sink-gegevensopslag.

## <a name="dataset-availability"></a>Beschikbaarheid van gegevenssets
De **beschikbaarheid** gedeelte in een gegevensset definieert het segmenteringsmodel voor de gegevensset (bijvoorbeeld elk uur, dagelijks of wekelijks). Zie voor meer informatie over activiteitsvensters [planning en uitvoering](data-factory-scheduling-and-execution.md).

De volgende sectie van de beschikbaarheid geeft aan dat de uitvoergegevensset ofwel per uur wordt geproduceerd, of de invoergegevensset beschikbaar per uur is:

```json
"availability":
{
    "frequency": "Hour",
    "interval": 1
}
```

Als de pijplijn de volgende begin- en eindtijd ervan heeft:

```json
    "start": "2016-08-25T00:00:00Z",
    "end": "2016-08-25T05:00:00Z",
```

De uitvoergegevensset wordt geproduceerd per uur in de pijplijn starten en de eindtijd. Daarom zijn er vijf gegevensset segmenten die worden geproduceerd door deze pijplijn, één voor elke activiteitvenster (12: 00 uur - 1 uur, 01: 00 - 2 AM, 2 AM - 3 uur, 3 AM - 4 AM, 4 AM - 5 uur).

De volgende tabel beschrijft de eigenschappen die u in de beschikbaarheidssectie gebruiken kunt:

| Eigenschap | Description | Vereist | Standaard |
| --- | --- | --- | --- |
| frequency |Hiermee geeft u de tijdseenheid voor de gegevensset segment productie.<br/><br/><b>Ondersteunde frequentie</b>: Minuut, uur, dag, Week, maand |Ja |N.v.t. |
| interval |Hiermee geeft u een vermenigvuldiger voor de frequentie.<br/><br/>"X Synchronisatiefrequentie-interval" bepaalt hoe vaak het segment wordt geproduceerd. Bijvoorbeeld, als u de gegevensset om te worden gesegmenteerd op uurbasis, moet u instellen <b>frequentie</b> naar <b>uur</b>, en <b>interval</b> naar <b>1</b>.<br/><br/>Houd er rekening mee dat als u opgeeft **frequentie** als **minuut**, moet u het interval instellen op niet kleiner zijn dan 15. |Ja |N.v.t. |
| stijl |Hiermee geeft u op of het segment aan het begin of einde van het interval moet worden gemaakt.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul>Als **frequentie** is ingesteld op **maand**, en **stijl** is ingesteld op **EndOfInterval**, het segment op de laatste dag van maand wordt geproduceerd. Als **stijl** is ingesteld op **StartOfInterval**, het segment op de eerste dag van maand wordt geproduceerd.<br/><br/>Als **frequentie** is ingesteld op **dag**, en **stijl** is ingesteld op **EndOfInterval**, in het afgelopen uur van de dag het segment wordt geproduceerd.<br/><br/>Als **frequentie** is ingesteld op **uur**, en **stijl** is ingesteld op **EndOfInterval**, het segment aan het einde van het uur wordt geproduceerd. Het segment wordt bijvoorbeeld voor een segment voor de periode van 1-2 uur worden geproduceerd om 2 uur. |Nee |EndOfInterval |
| anchorDateTime |Hiermee definieert u de absolute positie in de tijd die door de scheduler wordt gebruikt voor het berekenen van de grenzen van de gegevensset-segment. <br/><br/>Houd er rekening mee dat als deze propoerty onderdelen van een datum die meer gedetailleerde dan de opgegeven frequentie heeft, de gedetailleerdere onderdelen worden genegeerd. Bijvoorbeeld, als de **interval** is **per uur** (frequentie: uur en interval: 1), en de **anchorDateTime** bevat **minuten en seconden**, en vervolgens de minuten en seconden delen van **anchorDateTime** worden genegeerd. |Nee |01/01/0001 |
| offset |TimeSpan waarmee het begin en einde van alle segmenten van de gegevensset worden verschoven. <br/><br/>Houd er rekening mee dat als beide **anchorDateTime** en **offset** zijn opgegeven, wordt het resultaat is de gecombineerde verschuiving. |Nee |N.v.t. |

### <a name="offset-example"></a>Voorbeeld van de offset
Standaard dagelijks (`"frequency": "Day", "interval": 1`) segmenten beginnen bij 12: 00 uur (middernacht) Coordinated Universal Time (UTC). Als u de begintijd moet 6 AM UTC-tijd in plaats daarvan wilt, stelt u de verschuiving zoals wordt weergegeven in het volgende codefragment:

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a>Voorbeeld van de anchorDateTime
In het volgende voorbeeld wordt de elke 23 uur in de gegevensset wordt geproduceerd. Het eerste segment wordt gestart op het moment dat is opgegeven door **anchorDateTime**, die is ingesteld op `2017-04-19T08:00:00` (UTC).

```json
"availability":
{
    "frequency": "Hour",
    "interval": 23,
    "anchorDateTime":"2017-04-19T08:00:00"
}
```

### <a name="offsetstyle-example"></a>Voorbeeld van de offset/style
De volgende gegevensset wordt maandelijks, en de 3e van elke maand om 8:00 uur wordt geproduceerd (`3.08:00:00`):

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

## <a name="Policy"></a>Beleid voor gegevensset
De **beleid** sectie in de definitie van de criteria of de voorwaarde die moeten voldoen aan de gegevensset segmenten definieert.

### <a name="validation-policies"></a>Validatiebeleid voor
| Beleidsnaam | Description | Toegepast op | Vereist | Standaard |
| --- | --- | --- | --- | --- |
| minimumSizeMB |Valideert dat de gegevens in **Azure Blob-opslag** voldoet aan de minimale grootte (in MB). |Azure Blob Storage |Nee |N.v.t. |
| minimumRows |Valideert dat de gegevens in een **Azure SQL-database** of een **Azure table** het minimum aantal rijen bevat. |<ul><li>Azure SQL Database</li><li>Azure-tabel</li></ul> |Nee |N.v.t. |

#### <a name="examples"></a>Voorbeelden
**minimumSizeMB:**

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

**minimumRows:**

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

### <a name="external-datasets"></a>Externe gegevenssets
Externe gegevenssets zijn die niet worden gegenereerd door een actieve pijplijn in de data factory. Als de gegevensset is gemarkeerd als **externe**, wordt de **ExternalData** beleid van invloed zijn op het gedrag van de beschikbaarheid van gegevenssets segment worden gedefinieerd.

Tenzij een gegevensset wordt geproduceerd door Data Factory, deze moet worden gemarkeerd als **externe**. Deze instelling in het algemeen geldt voor de invoer van de eerste activiteit in een pijplijn, tenzij de activiteit of het koppelen van de pijplijn wordt gebruikt.

| Name | Description | Vereist | Standaardwaarde |
| --- | --- | --- | --- |
| dataDelay |De tijd te stellen de controle van de beschikbaarheid van de externe gegevens voor het opgegeven segment. Bijvoorbeeld, kunt u een per uur controleren met behulp van deze instelling uitstellen.<br/><br/>De instelling is alleen van toepassing op de huidige tijd. Bijvoorbeeld, als het is nu om 13:00 uur en deze waarde 10 minuten is, begint de validatie om 1:10 uur.<br/><br/>Houd er rekening mee dat deze instelling heeft geen invloed op segmenten in het verleden. Segmenten met **eindtijd gegevenssegment** + **dataDelay** < **nu** zonder enige vertraging worden verwerkt.<br/><br/>Keer groter zijn dan 23:59 uur moeten worden opgegeven met behulp van de `day.hours:minutes:seconds` indeling. Bijvoorbeeld, om op te geven 24 uur, gebruik geen 24:00:00 uur. Gebruik in plaats daarvan 1.00:00:00. Als u 24:00:00, wordt dit beschouwd als 24 dagen (24.00:00:00). Geef 1:04:00:00 1 dag en vier uur. |Nee |0 |
| retryInterval |De wachttijd tussen een storing en de volgende poging. Deze instelling geldt voor de huidige tijd. Als de vorige is mislukt probeert, de volgende poging is na de **retryInterval** periode. <br/><br/>Als het nu om 13:00 uur, beginnen we de eerste poging. Als de duur van de eerste van validatiecontrole is 1 minuut en de bewerking is mislukt, de volgende poging is 1:00 + 1 min (duur) + 1 minuut (interval voor opnieuw proberen) = 13:02 uur. <br/><br/>Er is geen vertraging voor segmenten in het verleden. De nieuwe poging gebeurt onmiddellijk. |Nee |00:01:00 uur (1 minuut) |
| retryTimeout |De time-out voor elke nieuwe poging.<br/><br/>Als deze eigenschap is ingesteld op 10 minuten, moet de validatie worden uitgevoerd binnen 10 minuten. Als het duurt langer dan 10 minuten duren om uit te voeren van de validatie, wordt de nieuwe poging verloopt.<br/><br/>Als alle pogingen voor de time-out voor de validatie van het segment is gemarkeerd als **TimedOut**. |Nee |00:10:00 uur (10 minuten) |
| maximumRetry |Het aantal keren om te controleren op de beschikbaarheid van de externe gegevens. De maximaal toegestane waarde is 10. |Nee |3 |


## <a name="create-datasets"></a>Gegevenssets maken
U kunt gegevenssets maken met behulp van een van deze hulpprogramma's of de SDK's:

- De wizard Kopiëren
- Azure Portal
- Visual Studio
- PowerShell
- Azure Resource Manager-sjabloon
- REST-API
- .NET API

Zie de volgende zelfstudies voor stapsgewijze instructies voor het maken van pijplijnen en gegevenssets met behulp van een van deze hulpprogramma's of de SDK's:

- [Een pijplijn met een transformatieactiviteit voor gegevens bouwen](data-factory-build-your-first-pipeline.md)
- [Een pijplijn met een activiteit van de verplaatsing van gegevens maken](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

Nadat u een pijplijn wordt gemaakt en geïmplementeerd, kunt u deze kunt beheren en bewaken van uw pijplijnen met behulp van de Azure portal-blades of de app bewaking en beheer. Zie de volgende onderwerpen voor stapsgewijze instructies:

- [Pijplijnen bewaken en beheren met behulp van Azure portal-blades](data-factory-monitor-manage-pipelines.md)
- [Pijplijnen bewaken en beheren met behulp van de app bewaking en beheer](data-factory-monitor-manage-app.md)

## <a name="scoped-datasets"></a>Scoped gegevenssets
Kunt u gegevenssets die zijn gericht op een pijplijn met behulp van de **gegevenssets** eigenschap. Deze gegevenssets kan alleen worden gebruikt door activiteiten binnen deze pijplijn, niet door de activiteiten in andere pijplijnen. Het volgende voorbeeld definieert een pijplijn met twee gegevenssets (InputDataset rdc en OutputDataset rdc) moet worden gebruikt in de pijplijn.

> [!IMPORTANT]
> Scoped gegevenssets worden alleen ondersteund met eenmalige pijplijnen (waarbij **pipelineMode** is ingesteld op **OneTime**). Zie [Onetime pijplijn](data-factory-create-pipelines.md#onetime-pipeline) voor meer informatie.
>
>

```json
{
    "name": "CopyPipeline-rdc",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset-rdc"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-rdc"
                    }
                ],
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1,
                    "style": "StartOfInterval"
                },
                "name": "CopyActivity-0"
            }
        ],
        "start": "2016-02-28T00:00:00Z",
        "end": "2016-02-28T00:00:00Z",
        "isPaused": false,
        "pipelineMode": "OneTime",
        "expirationTime": "15.00:00:00",
        "datasets": [
            {
                "name": "InputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "InputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/input",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "external": true,
                    "policy": {}
                }
            },
            {
                "name": "OutputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "OutputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/output",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "external": false,
                    "policy": {}
                }
            }
        ]
    }
}
```

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over pijplijnen [pijplijnen maken](data-factory-create-pipelines.md).
- Zie voor meer informatie over hoe pijplijnen worden gepland en uitgevoerd, [planning en uitvoering in Azure Data Factory](data-factory-scheduling-and-execution.md).
