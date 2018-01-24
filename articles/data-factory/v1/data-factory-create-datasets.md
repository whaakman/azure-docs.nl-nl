---
title: Gegevenssets maken in Azure Data Factory | Microsoft Docs
description: Informatie over het maken van gegevenssets in Azure Data Factory met voorbeelden die gebruikmaken van eigenschappen zoals de offset en anchorDateTime.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 0614cd24-2ff0-49d3-9301-06052fd4f92a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 1733e953d9dd65a3d2b801e6c5ba5cfbb5f82920
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="datasets-in-azure-data-factory"></a>Gegevenssets in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-create-datasets.md)
> * [Versie 2 - Preview](../concepts-datasets-linked-services.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [gegevenssets in V2](../concepts-datasets-linked-services.md).

In dit artikel wordt beschreven welke gegevenssets zijn, hoe ze worden gedefinieerd in JSON-indeling, en hoe ze worden gebruikt Azure Data Factory-pijplijnen. Het biedt details over elke sectie (bijvoorbeeld, structuur, beschikbaarheid en beleid) in de JSON-definitie van de gegevensset. Het artikel bevat ook voorbeelden voor het gebruik van de **offset**, **anchorDateTime**, en **stijl** eigenschappen in de JSON-definitie van een dataset.

> [!NOTE]
> Als u niet bekend met Data Factory bent, Zie [Inleiding tot Azure Data Factory](data-factory-introduction.md) voor een overzicht. Als u praktische ervaring met het maken van gegevensfactory niet hebt, kunt u toegang een beter inzicht in de vorm van de [data transformation zelfstudie](data-factory-build-your-first-pipeline.md) en de [data movement zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="overview"></a>Overzicht
Een gegevensfactory kan één of meer pijplijnen hebben. Een **pijplijn** is een logische groepering van **activiteiten** die samen een taak uitvoeren. Met activiteiten in een pijplijn definieert u welk acties moeten worden uitgevoerd voor uw gegevens. Bijvoorbeeld, kunt u een kopieeractiviteit om gegevens te kopiëren van een lokale SQL Server naar Azure Blob-opslag. Vervolgens kunt u een Hive-activiteit die wordt uitgevoerd van een Hive-script op een Azure HDInsight-cluster om gegevens te verwerken van Blob-opslag voor het produceren van uitvoergegevens. Ten slotte kunt u een tweede kopieeractiviteit de uitvoergegevens kopiëren naar Azure SQL Data Warehouse boven op welke business intelligence (BI) reporting oplossingen zijn gebouwd. Zie voor meer informatie over de pijplijnen en activiteiten [pijplijnen en activiteiten in Azure Data Factory](data-factory-create-pipelines.md).

Een activiteit kan duren voordat de invoer van nul of meer **gegevenssets**, en een of meer uitvoergegevenssets te produceren. Een invoergegevensset staat voor de invoer voor een activiteit in de pijplijn en een uitvoergegevensset voor de uitvoer voor de activiteit. Met gegevenssets worden gegevens binnen andere gegevensarchieven geïdentificeerd, waaronder tabellen, bestanden, mappen en documenten. Een Azure Blob-gegevensset geeft bijvoorbeeld de blob-container en map in Blob storage van waaruit de pijplijn de gegevens moet lezen. 

Voordat u een gegevensset maakt, maakt u een **gekoppelde service** uw data store koppelen aan de gegevensfactory. Gekoppelde services zijn te vergelijken met verbindingsreeksen, die de verbindingsinformatie bevatten die Data Factory nodig heeft om verbinding te maken met externe bronnen. Gegevenssets identificeren gegevens binnen de gekoppelde gegevens worden opgeslagen, zoals SQL-tabellen, bestanden, mappen en documenten. Bijvoorbeeld, gekoppelde een Azure Storage service een opslagaccount aan de gegevensfactory. Een Azure Blob-gegevensset vertegenwoordigt de blob-container en de map waarin de invoer blobs moeten worden verwerkt. 

Hier volgt een voorbeeldscenario. Om gegevens te kopiëren van Blob-opslag met een SQL-database, hebt u twee gekoppelde services: Azure Storage en Azure SQL Database. Vervolgens maakt u twee gegevenssets: Azure Blob-gegevensset (die verwijst naar de gekoppelde Azure Storage-service) en Azure SQL-tabel dataset (die verwijst naar de service Azure SQL Database gekoppeld). De Azure Storage en Azure SQL Database, gekoppelde services bevatten verbindingstekenreeksen die Data Factory tijdens runtime verbinding maakt met uw Azure Storage en Azure SQL Database, respectievelijk. De Azure Blob-gegevensset bevat de blob-container en de blob-map waarin de invoer blobs in de Blob-opslag. De gegevensset Azure SQL-tabel bevat de SQL-tabel in uw SQL-database die de gegevens wordt worden gekopieerd.

Het volgende diagram toont de relaties tussen pipeline, de activiteit, de gegevensset en de gekoppelde service in de Data Factory: 

![Relatie tussen een pijplijn, activiteit, gegevensset, gekoppelde services](media/data-factory-create-datasets/relationship-between-data-factory-entities.png)

## <a name="dataset-json"></a>Dataset JSON
Een gegevensset in Gegevensfactory is gedefinieerd in JSON-indeling als volgt:

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "external": <boolean flag to indicate external data. only for input datasets>,
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

| Eigenschap | Beschrijving | Vereist | Standaard |
| --- | --- | --- | --- |
| naam |Naam van de gegevensset. Zie [Azure Data Factory - naamgevingsregels](data-factory-naming-rules.md) voor naamgevingsregels. |Ja |N.v.t. |
| type |Het type van de gegevensset. Geef een van de typen die door Data Factory worden ondersteund (bijvoorbeeld: AzureBlob, AzureSqlTable). <br/><br/>Zie voor meer informatie [gegevensettype](#Type). |Ja |N.v.t. |
| structuur |Schema voor de gegevensset.<br/><br/>Zie voor meer informatie [gegevenssetstructuur](#Structure). |Nee |N.v.t. |
| typeProperties | De type-eigenschappen zijn verschillend voor elk type (bijvoorbeeld: Azure Blob, Azure SQL-tabel). Zie voor meer informatie over de ondersteunde typen en hun eigenschappen [gegevensettype](#Type). |Ja |N.v.t. |
| external | Booleaanse vlag om op te geven of een gegevensset expliciet wordt geproduceerd door een data factory-pijplijn of niet. Als de invoergegevensset voor een activiteit niet door de huidige pipeline gemaakt wordt, moet u deze vlag ingesteld op true. Deze vlag ingesteld op true voor de invoer gegevensset van de eerste activiteit in de pijplijn.  |Nee |onwaar |
| availability | Definieert het venster verwerking (bijvoorbeeld elk uur of dagelijks) of het segmenteringshulplijnen model voor de gegevensset voor productie. Elke eenheid gegevens verbruikt en die wordt geproduceerd door het uitvoeren van een activiteit wordt een gegevenssegment genoemd. Als de beschikbaarheid van een uitvoergegevensset is ingesteld op dagelijks (frequentie - dag, interval - 1), wordt dagelijks een segment geproduceerd. <br/><br/>Zie voor meer informatie [gegevensset beschikbaarheid](#Availability). <br/><br/>Zie voor informatie over de gegevensset segmentering model, de [planning en uitvoering](data-factory-scheduling-and-execution.md) artikel. |Ja |N.v.t. |
| policy |Definieert de criteria of de conditie van de segmenten van de gegevensset moeten voldoen. <br/><br/>Zie voor meer informatie de [gegevensset beleid](#Policy) sectie. |Nee |N.v.t. |

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

* **type** is ingesteld op azuresqltable zijn.
* **tableName** type-eigenschap (die specifiek is voor AzureSqlTable type) is ingesteld op MyTable.
* **linkedServiceName** verwijst naar een gekoppelde service van het type AzureSqlDatabase die is gedefinieerd in het volgende JSON-codefragment. 
* **beschikbaarheid frequentie** is ingesteld op dag, en **interval** is ingesteld op 1. Dit betekent dat het segment gegevensset dagelijks wordt geproduceerd.  

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
* **connectionString** type-eigenschap geeft u informatie voor verbinding met een SQL-database.  

Zoals u ziet, definieert de gekoppelde service verbinding maken met een SQL-database. De gegevensset wordt gedefinieerd welke tabel wordt gebruikt als invoer en uitvoer voor de activiteit in een pijplijn.   

> [!IMPORTANT]
> Tenzij een gegevensset wordt geproduceerd door de pijplijn, moet dit worden gemarkeerd als **externe**. Deze instelling is algemeen van toepassing op invoer van de eerste activiteit in een pijplijn.   


## <a name="Type"></a>Gegevensettype
Het type van de gegevensset is afhankelijk van het gegevensarchief die u gebruikt. Zie de volgende tabel voor een lijst met opgeslagen gegevens die door Data Factory worden ondersteund. Klik op een gegevensarchief voor meer informatie over het maken van een gekoppelde service en een gegevensset voor die gegevensopslag.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Gegevens worden opgeslagen met * on-premises kan worden of op Azure-infrastructuur als een service (IaaS). Deze gegevensarchieven moeten u voor het installeren van [Data Management Gateway](data-factory-data-management-gateway.md).

In het voorbeeld in de vorige sectie, het type van de gegevensset is ingesteld op **AzureSqlTable**. Op dezelfde manier, het type van de gegevensset voor een Azure Blob-gegevensset is ingesteld op **AzureBlob**, zoals wordt weergegeven in de volgende JSON:

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
De **structuur** sectie is optioneel. Hiermee definieert u het schema van de gegevensset door dat een verzameling van namen en gegevenstypen van de kolommen bevat. De structuur sectie kunt u type informatie dat wordt gebruikt voor het converteren van de typen en het toewijzen van kolommen van de bron naar de bestemming. In het volgende voorbeeld wordt de gegevensset heeft drie kolommen: `slicetimestamp`, `projectname`, en `pageviews`. Zijn van het type String, String en Decimal, respectievelijk.

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

Elke kolom in de structuur bevat de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| naam |De naam van de kolom. |Ja |
| type |Het gegevenstype van de kolom.  |Nee |
| Cultuur |. NET-gebaseerde cultuur moet worden gebruikt wanneer het type een .NET-type is: `Datetime` of `Datetimeoffset`. De standaardwaarde is `en-us`. |Nee |
| Indeling |Indeling van tekenreeks moet worden gebruikt wanneer het type een .NET-type is: `Datetime` of `Datetimeoffset`. |Nee |

De volgende richtlijnen te bepalen wanneer gegevens van de structuur en wat u wilt opnemen de **structuur** sectie.

* **Voor gestructureerde gegevensbronnen**, de sectie structuur alleen opgeven als u toewijzen bronkolommen wilt voor het opvangen van kolommen en hun namen niet overeen komen. Dit soort gegevensbron gestructureerde slaat schema en het type informatie samen met de gegevens zelf. Voorbeelden van gestructureerde gegevensbronnen zijn SQL Server, Oracle en Azure-tabel. 
  
    Als het type informatie is al beschikbaar voor gestructureerde gegevensbronnen, moet u type-informatie niet opnemen wanneer u de sectie structuur opneemt.
* **Voor schema op lezen gegevensbronnen (specifiek Blob-opslag)**, kunt u gegevens opslaan zonder een schema of het type informatie met de gegevens op te slaan. Voor deze typen gegevensbronnen, structuur opnemen wanneer u toewijzen bronkolommen wilt voor het opvangen van kolommen. Structuur ook bevatten wanneer de dataset de invoer voor een kopieeractiviteit is en gegevenstypen van de gegevensset bron moeten worden geconverteerd naar native-typen voor de sink. 
    
    Data Factory ondersteunt de volgende waarden voor het type informatie in de structuur: **Int16, Int32, Int64, Single, Double, Decimal, Byte [], Booleaanse waarde, String, Guid, Datetime, Datetimeoffset en Timespan**. Deze waarden zijn Common Language Specification (CLS)-compatibel zijn,. NET-type op basis van waarden.

Data Factory voert automatisch typeconversies bij het verplaatsen van gegevens uit een gegevensopslag bron naar een gegevensarchief sink. 
  

## <a name="dataset-availability"></a>Beschikbaarheid van gegevensset
De **beschikbaarheid** sectie in een gegevensset definieert het venster verwerking (bijvoorbeeld elk uur, dagelijks of wekelijks) voor de gegevensset. Zie voor meer informatie over windows activiteit [planning en uitvoering](data-factory-scheduling-and-execution.md).

De volgende sectie van de beschikbaarheid geeft aan dat de uitvoergegevensset ofwel per uur wordt geproduceerd, of de invoergegevensset beschikbaar per uur:

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

Als de pijplijn de volgende begin- en eindtijden heeft:  

```json
    "start": "2016-08-25T00:00:00Z",
    "end": "2016-08-25T05:00:00Z",
```

De uitvoergegevensset wordt geproduceerd per uur in de pijplijn start- en eindtijden. Er zijn daarom vijf gegevensset gegevenssegmenten geproduceerd door deze pipeline, één voor elke activiteitvenster (12: 00 A.M. - 1 uur, 01: 00 - 2 uur, 2 uur - 3 uur, 3 uur - 4 AM, 4 uur - 5 uur). 

De volgende tabel beschrijft de eigenschappen die u in de beschikbaarheidssectie gebruiken kunt:

| Eigenschap | Beschrijving | Vereist | Standaard |
| --- | --- | --- | --- |
| frequency |Hiermee geeft u tijdseenheid voor de gegevensset segment productie.<br/><br/><b>Ondersteunde frequentie</b>: minuut, uur, dag, Week, maand |Ja |N.v.t. |
| interval |Hiermee geeft u een vermenigvuldiger voor de frequentie.<br/><br/>"Frequentie x-interval" bepaalt hoe vaak het segment wordt geproduceerd. Bijvoorbeeld, als u de gegevensset worden gesegmenteerd op uurbasis moet, u ingesteld <b>frequentie</b> naar <b>uur</b>, en <b>interval</b> naar <b>1</b>.<br/><br/>Houd er rekening mee dat als u opgeeft **frequentie** als **minuut**, moet u het interval instellen op niet kleiner zijn dan 15. |Ja |N.v.t. |
| stijl |Hiermee geeft u op of het segment aan het begin of einde van het interval moet worden geproduceerd.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul>Als **frequentie** is ingesteld op **maand**, en **stijl** is ingesteld op **EndOfInterval**, het segment op de laatste dag van de maand wordt geproduceerd. Als **stijl** is ingesteld op **StartOfInterval**, het segment op de eerste dag van de maand wordt geproduceerd.<br/><br/>Als **frequentie** is ingesteld op **dag**, en **stijl** is ingesteld op **EndOfInterval**, in het afgelopen uur van de dag het segment wordt geproduceerd.<br/><br/>Als **frequentie** is ingesteld op **uur**, en **stijl** is ingesteld op **EndOfInterval**, het segment wordt geproduceerd aan het einde van het uur. Voor een segment voor de periode van 1-2 uur, het segment geproduceerd om 2 uur. |Nee |EndOfInterval |
| anchorDateTime |Hiermee definieert u de absolute positie in de tijd die wordt gebruikt door de planner gegevensset segmentgrenzen berekenen. <br/><br/>Houd er rekening mee dat als deze propoerty de onderdelen van de datum die meer gedetailleerd dan de opgegeven frequentie heeft, de gedetailleerdere onderdelen worden genegeerd. Bijvoorbeeld, als de **interval** is **per uur** (frequentie: uur en interval: 1), en de **anchorDateTime** bevat **minuten en seconden**, en vervolgens de minuten en seconden delen van **anchorDateTime** worden genegeerd. |Nee |01/01/0001 |
| offset |TimeSpan waarmee het begin en einde van alle segmenten van de gegevensset zijn verplaatst. <br/><br/>Houd er rekening mee dat als beide **anchorDateTime** en **offset** zijn opgegeven, wordt het resultaat is een gecombineerde verschuiving. |Nee |N.v.t. |

### <a name="offset-example"></a>offset voorbeeld
Standaard dagelijks (`"frequency": "Day", "interval": 1`) segmenten beginnen bij 12: 00 A.M. (middernacht) Coordinated Universal Time (UTC). Als u de begintijd moet 6 uur UTC-tijd in plaats daarvan wilt, stelt u de verschuiving zoals weergegeven in het volgende fragment: 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a>Voorbeeld van anchorDateTime
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
De volgende gegevensset wordt maandelijks, en op de 3rd van elke maand om 8:00 uur wordt geproduceerd (`3.08:00:00`):

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

## <a name="Policy"></a>DataSet beleid
De **beleid** sectie in de definitie van de gegevensset definieert u de criteria of de conditie van de segmenten van de gegevensset moeten voldoen.

### <a name="validation-policies"></a>Van validatiebeleid
| Beleidsnaam | Beschrijving | Toegepast op | Vereist | Standaard |
| --- | --- | --- | --- | --- |
| minimumSizeMB |Valideert dat de gegevens in **Azure Blob storage** voldoet aan de minimale grootte (in MB). |Azure Blob Storage |Nee |N.v.t. |
| minimumRows |Valideert dat de gegevens in een **Azure SQL-database** of een **Azure-tabel** het minimum aantal rijen bevat. |<ul><li>Azure SQL-database</li><li>Azure-tabel</li></ul> |Nee |N.v.t. |

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
Externe gegevenssets zijn degene die niet worden geproduceerd door een actieve pijplijn in de gegevensfactory. Als de dataset is gemarkeerd als **externe**, wordt de **ExternalData** beleid invloed op de werking van de beschikbaarheid van gegevensset segment worden gedefinieerd.

Tenzij een gegevensset wordt geproduceerd door Data Factory, moet dit worden gemarkeerd als **externe**. Deze instelling in het algemeen geldt voor de eerste activiteit in een pijplijn-invoer tenzij activiteit of koppeling van de pijplijn wordt gebruikt.

| Naam | Beschrijving | Vereist | Standaardwaarde |
| --- | --- | --- | --- |
| dataDelay |De tijd om uit te stellen de controle van de beschikbaarheid van de externe gegevens voor het opgegeven segment. Bijvoorbeeld, kunt u een controle per uur uitstellen met deze instelling.<br/><br/>De instelling is alleen van toepassing op de huidige tijd.  Bijvoorbeeld, als het is nu om 13:00 uur en deze waarde 10 minuten is, begint de validatie om 1:10 uur.<br/><br/>Houd er rekening mee dat deze instelling heeft geen invloed op segmenten in het verleden. Segmenten met **segment eindtijd** + **dataDelay** < **nu** zonder enige vertraging worden verwerkt.<br/><br/>Tijden groter is dan 23:59 uur moeten worden opgegeven met de `day.hours:minutes:seconds` indeling. Bijvoorbeeld, als u 24 uur, gebruik geen 24:00:00. Gebruik in plaats daarvan 1.00:00:00. Als u 24:00:00, wordt dit beschouwd als 24 dagen (24.00:00:00). Geef 1:04:00:00 voor 1 dag en 4 uur. |Nee |0 |
| retryInterval |De wachttijd tussen een fout en het opnieuw probeert. Deze instelling geldt voor de huidige tijd. Als de vorige is mislukt probeert, de volgende poging is na de **retryInterval** periode. <br/><br/>Als het nu om 13:00 uur, beginnen we in de eerste poging. Als de duur van de eerste validatiecontrole is 1 minuut en de bewerking is mislukt, de volgende poging is 1:00 + 1 min (duur), 1 min. (interval voor opnieuw proberen) + = 1:02 PM. <br/><br/>Er is geen vertraging segmenten in het verleden. De nieuwe poging gebeurt onmiddellijk. |Nee |00:01:00 (1 minuut) |
| retryTimeout |De time-out voor nieuwe pogingen.<br/><br/>Als deze eigenschap is ingesteld op 10 minuten, moet de validatie binnen 10 minuten worden voltooid. Als het duurt langer dan 10 minuten de validatie uitvoeren, wordt de nieuwe poging time-out.<br/><br/>Als alle pogingen voor de time-out voor de validatie van het segment is gemarkeerd als **time-out**. |Nee |00:10:00 (10 minuten) |
| maximumRetry |Het aantal keren om te controleren op de beschikbaarheid van de externe gegevens. De maximaal toegestane waarde is 10. |Nee |3 |


## <a name="create-datasets"></a>Gegevenssets maken
U kunt de gegevenssets maken met behulp van een van deze hulpprogramma's of de SDK's: 

- De wizard Kopiëren 
- Azure Portal
- Visual Studio
- PowerShell
- Azure Resource Manager-sjabloon
- REST-API
- .NET API

Zie de volgende zelfstudies voor stapsgewijze instructies voor het maken van de pijplijnen en gegevenssets met behulp van een van deze hulpprogramma's of de SDK's:
 
- [Een pijplijn met een transformatieactiviteit voor gegevens bouwen](data-factory-build-your-first-pipeline.md)
- [Maken van een pijplijn met een activiteit van de verplaatsing van gegevens](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

Nadat u een pijplijn gemaakt en geïmplementeerd, kunt u deze kunt beheren en uw pijplijnen bewaken met behulp van de Azure portal-blades of de app voor bewaking en beheer. Zie de volgende onderwerpen voor stapsgewijze instructies: 

- [Bewaken en beheren van pijplijnen via Azure portal-blades](data-factory-monitor-manage-pipelines.md)
- [Bewaken en beheren van pijplijnen met behulp van de app voor bewaking en beheer](data-factory-monitor-manage-app.md)


## <a name="scoped-datasets"></a>Bereik gegevenssets
Kunt u gegevenssets die zijn gericht op een pijplijn met behulp van de **gegevenssets** eigenschap. Deze gegevenssets kan alleen worden gebruikt door activiteiten binnen deze pipeline, niet door de activiteiten in andere pijplijnen. Het volgende voorbeeld definieert een pijplijn met twee gegevenssets (InputDataset rdc en OutputDataset rdc) dat in de pijplijn wordt gebruikt.  

> [!IMPORTANT]
> Bereik gegevenssets worden alleen ondersteund voor eenmalige pijplijnen (waarbij **pipelineMode** is ingesteld op **OneTime**). Zie [Onetime pijplijn](data-factory-create-pipelines.md#onetime-pipeline) voor meer informatie.
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
