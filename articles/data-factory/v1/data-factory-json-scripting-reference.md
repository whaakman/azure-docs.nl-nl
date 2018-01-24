---
title: Azure Data Factory - JSON-scriptverwerking verwijzing | Microsoft Docs
description: JSON-schema's biedt voor Data Factory-entiteiten.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 9457e90f378cf7b30810ca9cadfcad139e91e2d4
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="azure-data-factory---json-scripting-reference"></a>Azure Data Factory - JSON-scriptverwerking verwijzing
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is.


Dit artikel bevat JSON-schema's en voorbeelden voor het definiëren van Azure Data Factory-entiteiten (pipeline, activiteit, gegevensset en gekoppelde service).  

## <a name="pipeline"></a>Pijplijn 
De structuur op hoog niveau voor de definitie van een pijplijn is als volgt: 

```json
{
  "name": "SamplePipeline",
  "properties": {
    "description": "Describe what pipeline does",
    "activities": [
    ],
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
} 
```

Volgende tabel beschrijft de eigenschappen in de pijplijn-JSON-definitie:

| Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
| naam | Naam van de pijplijn. Geef een naam voor de actie die dat de activiteit of een pipeline is geconfigureerd om te doen<br/><ul><li>Maximum aantal tekens: 260</li><li>Moet beginnen met een letter, cijfer of onderstrepingsteken (_)</li><li>Volgende tekens zijn niet toegestaan: '. ', '+ ','?', '/', '< ',' >', ' * ', "%", "&", ":","\\'</li></ul> |Ja |
| description |Beschrijving van wat de activiteit of pijplijn wordt gebruikt voor | Nee |
| activities | Bevat een lijst van activiteiten. | Ja |
| start |Datum-begintijd voor de pijplijn. Moet in [ISO-indeling](http://en.wikipedia.org/wiki/ISO_8601). Bijvoorbeeld: 2014-10-14T16:32:41. <br/><br/>Het is mogelijk om op te geven van een lokale tijd, bijvoorbeeld een geschatte tijd. Hier volgt een voorbeeld: `2016-02-27T06:00:00**-05:00`, namelijk 6 AM EST.<br/><br/>De begin- en -eigenschappen opgeven samen actieve periode voor de pijplijn. Uitvoer segmenten worden alleen gemaakt met deze actieve periode. |Nee<br/><br/>Als u een waarde op voor de end-eigenschap opgeeft, moet u de waarde voor de eigenschap start opgeven.<br/><br/>De begin- en eindtijden kunnen niet leeg zijn voor het maken van een pijplijn. U moet beide waarden om in te stellen van een actieve periode voor de pijplijn om uit te voeren. Als u geen begin- en eindtijden opgeeft wanneer u een pijplijn maakt, kunt u deze met de cmdlet Set-AzureRmDataFactoryPipelineActivePeriod later instellen. |
| einde |Einddatum en-tijd voor de pijplijn. Als in de ISO-indeling moet worden opgegeven. Bijvoorbeeld: 2014-10-14T17:32:41 <br/><br/>Het is mogelijk om op te geven van een lokale tijd, bijvoorbeeld een geschatte tijd. Hier volgt een voorbeeld: `2016-02-27T06:00:00**-05:00`, namelijk 6 AM EST.<br/><br/>Als u wilt de pijplijn voor onbepaalde tijd uitvoeren, geef 9999-09-09 als de waarde voor de end-eigenschap. |Nee <br/><br/>Als u een waarde voor de start-eigenschap opgeeft, moet u de waarde voor de end-eigenschap opgeven.<br/><br/>Zie de opmerkingen voor de **start** eigenschap. |
| isPaused |Als is ingesteld op waar de pijplijn wordt niet uitgevoerd. Standaardwaarde = false. U kunt deze eigenschap in- of uitschakelen. |Nee |
| pipelineMode |De methode voor de planning wordt uitgevoerd voor de pijplijn. Toegestane waarden zijn: (standaard), gepland eenmalige.<br/><br/>De geplande' geeft aan dat de pijplijn met een opgegeven tijdsinterval volgens de actieve periode (begin- en -tijd) wordt uitgevoerd. 'Eenmalige' geeft aan dat de pijplijn wordt slechts één keer uitgevoerd. Eenmalige pijplijnen eenmaal gemaakt kunnen niet worden gewijzigd/bijgewerkt op dit moment. Zie [Onetime pijplijn](data-factory-create-pipelines.md#onetime-pipeline) voor meer informatie over het instellen van eenmalige. |Nee |
| expirationTime |Tijdsduur na het maken waarvoor de pijplijn is geldig en moet worden ingericht. Als er geen elk actief is, is mislukt, of in behandeling wordt uitgevoerd, de pijplijn wordt automatisch verwijderd wanneer het de verlooptijd is bereikt. |Nee |


## <a name="activity"></a>Activiteit 
De structuur op hoog niveau voor een activiteit in de definitie van een pijplijn (activiteiten element) is als volgt:

```json
{
    "name": "ActivityName",
    "description": "description", 
    "type": "<ActivityType>",
    "inputs":  "[]",
    "outputs":  "[]",
    "linkedServiceName": "MyLinkedService",
    "typeProperties":
    {

    },
    "policy":
    {
    }
    "scheduler":
    {
    }
}
```

De eigenschappen in de JSON-definitie van de activiteit volgende tabel wordt beschreven:

| Label | Beschrijving | Vereist |
| --- | --- | --- |
| naam |De naam van de activiteit. Geef een naam die staat voor de actie die de activiteit is geconfigureerd om te doen<br/><ul><li>Maximum aantal tekens: 260</li><li>Moet beginnen met een letter, cijfer of onderstrepingsteken (_)</li><li>Volgende tekens zijn niet toegestaan: '. ', '+ ','?', '/', '< ',' >', ' * ', "%", "&", ":","\\'</li></ul> |Ja |
| description |Tekst die beschrijft wat de activiteit wordt gebruikt. |Nee |
| type |Geeft het type van de activiteit. Zie de [GEGEVENSARCHIEVEN](#data-stores) en [activiteiten voor GEGEVENSTRANSFORMATIE](#data-transformation-activities) secties voor verschillende soorten activiteiten. |Ja |
| Invoer |Invoertabellen die wordt gebruikt door de activiteit<br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |Er is geen voor HDInsightStreaming en SqlServerStoredProcedure activiteiten <br/> <br/> Ja voor alle andere |
| uitvoer |Uitvoergegevens tabellen die worden gebruikt door de activiteit.<br/><br/>`// one output table`<br/>`"outputs":  [ { "name": “outputtable1” } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": “outputtable1” }, { "name": “outputtable2” }  ],` |Ja |
| linkedServiceName |De naam van de gekoppelde service die door de activiteit wordt gebruikt. <br/><br/>Een activiteit kan vereisen dat u de gekoppelde service opgeeft die is gekoppeld aan de vereiste rekenomgeving. |Ja voor HDInsight-activiteiten, Azure Machine Learning-activiteiten en activiteit opgeslagen Procedure. <br/><br/>Nee voor alle andere |
| typeProperties |Eigenschappen in de sectie typeProperties is afhankelijk van het type van de activiteit. |Nee |
| policy |Beleidsregels die van invloed zijn op het runtimegedrag van de activiteit. Als deze niet is opgegeven, worden de standaardbeleidsregels gebruikt. |Nee |
| Scheduler |'scheduler'-eigenschap wordt gebruikt voor het definiëren van de gewenste planning voor de activiteit. Bijbehorende subeigenschappen zijn hetzelfde als de waarden in de [eigenschap beschikbaarheid in een dataset](data-factory-create-datasets.md#dataset-availability). |Nee |

### <a name="policies"></a>Beleidsregels
Beleid geldt voor de run-time-gedrag van een activiteit specifiek wanneer het segment van een tabel wordt verwerkt. De volgende tabel bevat de details.

| Eigenschap | Toegestane waarden | Standaardwaarde | Beschrijving |
| --- | --- | --- | --- |
| Gelijktijdigheid van taken |Geheel getal <br/><br/>De maximale waarde: 10 |1 |Het aantal gelijktijdige uitvoeringen van de activiteit.<br/><br/>Bepaalt het aantal parallelle activiteit uitvoeringen die kunnen ontstaan op verschillende segmenten. Bijvoorbeeld, als een activiteit moet doorlopen sneller een groot aantal beschikbare gegevens, een grotere waarde voor gelijktijdigheid het verwerken van gegevens. |
| executionPriorityOrder |NewestFirst<br/><br/>OldestFirst |OldestFirst |Bepaalt de volgorde van de gegevenssegmenten dat wordt verwerkt.<br/><br/>Als er 2 segmenten (één gebeurt om 4 uur en 17: 00 uur een andere één voor één) en beide zijn in behandeling kan worden uitgevoerd. Als u de executionPriorityOrder NewestFirst worden ingesteld, wordt het segment om 17.00 eerst verwerkt. Op dezelfde manier als u de executionPriorityORder OldestFIrst worden ingesteld, klikt u vervolgens het segment om 4 uur is verwerkt. |
| retry |Geheel getal<br/><br/>De maximale waarde is 10 |0 |Aantal nieuwe pogingen voordat de verwerking van de gegevens voor het segment is gemarkeerd als mislukt. Uitvoering van de activiteit voor een gegevenssegment wordt herhaald tot maximaal het aantal pogingen opgegeven. De nieuwe poging wordt gedaan zo snel mogelijk na de fout. |
| timeout |TimeSpan |00:00:00 uur |Time-out voor de activiteit. Voorbeeld: 00:10:00 (impliceert time-out 10 minuten)<br/><br/>Als een waarde niet opgegeven is of 0 is, is de time-out van oneindig.<br/><br/>Als de verwerkingstijd van de gegevens op een segment de time-outwaarde overschrijdt, deze wordt geannuleerd en wordt geprobeerd om opnieuw te proberen de verwerking. Het aantal nieuwe pogingen, is afhankelijk van de eigenschap probeer het opnieuw. Wanneer een time-out optreedt, wordt de status ingesteld op een time-out opgetreden bij. |
| Vertraging |TimeSpan |00:00:00 uur |Geef de vertraging optreden voordat de verwerking van het segment wordt gestart.<br/><br/>De uitvoering van de activiteit voor een gegevenssegment wordt gestart nadat de vertraging voorbij de verwachte tijd voor uitvoering is.<br/><br/>Voorbeeld: 00:10:00 (betekent vertraging van 10 minuten) |
| longRetry |Geheel getal<br/><br/>De maximale waarde: 10 |1 |Het aantal pogingen lang voordat de uitvoering van het segment is mislukt.<br/><br/>longRetry pogingen door longRetryInterval gelijkmatig verdeeld. Als u een tijd tussen pogingen opgeven moet, gebruikt u dus longRetry. Als zowel de nieuwe pogingen en de longRetry worden opgegeven, elke poging longRetry bevat nieuwe pogingen en het maximale aantal pogingen is opnieuw * longRetry.<br/><br/>Bijvoorbeeld, als we hebben de volgende instellingen in het activiteitenbeleid voor:<br/>Probeer: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00 uur<br/><br/>Wordt ervan uitgegaan dat er is slechts één segment uit te voeren (status Waiting) en de activiteit is uitgevoerd elke keer mislukt. Er zou worden in eerste instantie 3 opeenvolgende uitvoering pogingen. De status van het segment is na elke poging probeer het opnieuw. Nadat de eerste 3 pogingen hebben bekeken, zou de status van het segment LongRetry zijn.<br/><br/>Na een uur (dat wil zeggen, de longRetryInteval waarde), zou er een andere set 3 opeenvolgende uitvoering pogingen. Hierna is de status van het segment zou niet en geen pogingen meer zou worden uitgevoerd. Daarom is algemene 6 geprobeerd.<br/><br/>Als een uitvoering is voltooid, het segment de status gereed zijn en er worden geen pogingen meer geprobeerd.<br/><br/>longRetry kan worden gebruikt in situaties waarbij afhankelijke gegevens aankomen op niet-deterministische tijdstippen of de hele omgeving flaky onder welke gegevensverwerking plaatsvindt. In dergelijke gevallen tijd nieuwe pogingen achter elkaar niet kunt doen, en dit na een interval van resultaten in de gewenste uitvoer.<br/><br/>Waarschuwing: geen hoge waarden voor de longRetry of longRetryInterval instelt. Hogere waarden dat normaal gesproken andere al problemen. |
| longRetryInterval |TimeSpan |00:00:00 uur |De vertraging tussen pogingen lang |

### <a name="typeproperties-section"></a>typeProperties sectie
De sectie typeProperties verschilt voor elke activiteit. Activiteiten voor gegevenstransformatie hebben de type-eigenschappen. Zie [activiteiten voor GEGEVENSTRANSFORMATIE](#data-transformation-activities) sectie in dit artikel voor JSON-voorbeelden die activiteiten voor gegevenstransformatie in een pijplijn definiëren. 

**Kopieeractiviteit** heeft twee subsecties in de sectie typeProperties: **bron** en **sink**. Zie [GEGEVENSARCHIEVEN](#data-stores) sectie in dit artikel voor JSON-voorbeelden die laten zien hoe u een data store als een bron-en/of sink. 

### <a name="sample-copy-pipeline"></a>Voorbeeld van kopieerpijplijn
De volgende voorbeeldpijplijn bevat een activiteit van het type **Copy** in de sectie **activities**. In dit voorbeeld wordt de [Kopieeractiviteit](data-factory-data-movement-activities.md) kopieert gegevens van een Azure Blob-opslag naar een Azure SQL database. 

```json
{
  "name": "CopyPipeline",
  "properties": {
    "description": "Copy data from a blob to Azure SQL table",
    "activities": [
      {
        "name": "CopyFromBlobToSQL",
        "type": "Copy",
        "inputs": [
          {
            "name": "InputDataset"
          }
        ],
        "outputs": [
          {
            "name": "OutputDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink",
            "writeBatchSize": 10000,
            "writeBatchTimeout": "60:00:00"
          }
        },
        "Policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
    ],
    "start": "2016-07-12T00:00:00",
    "end": "2016-07-13T00:00:00"
  }
} 
```

Houd rekening met de volgende punten:

* In het gedeelte Activiteiten is er slechts één activiteit waarvan **type** is ingesteld op **Copy**.
* De invoer voor de activiteit is ingesteld op **InputDataset** en de uitvoer voor de activiteit is ingesteld op **OutputDataset**.
* In het gedeelte **typeProperties** is **BlobSource** opgegeven als het brontype en **SqlSink** als het sink-type.

Zie [GEGEVENSARCHIEVEN](#data-stores) sectie in dit artikel voor JSON-voorbeelden die laten zien hoe u een data store als een bron-en/of sink.    

Zie voor een volledige procedure voor het maken van deze pipeline [zelfstudie: gegevens kopiëren van Blob-opslag naar SQL-Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

### <a name="sample-transformation-pipeline"></a>Voorbeeld van pijplijn voor transformatie
De volgende voorbeeldpijplijn bevat een activiteit van het type **HDInsightHive** in de sectie **activities**. In dit voorbeeld transformeert de [HDInsight Hive-activiteit](data-factory-hive-activity.md) gegevens uit een Azure-blobopslag door een Hive-scriptbestand uit te voeren op een Azure HDInsight Hadoop-cluster. 

```json
{
    "name": "TransformPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00",
        "isPaused": false
    }
}
```

Houd rekening met de volgende punten: 

* In het gedeelte activities is er slechts één activiteit waarvan **type** is ingesteld op **HDInsightHive**.
* Het Hive-scriptbestand **partitionweblogs.hql** wordt opgeslagen in het Azure-opslagaccount (opgegeven door de scriptLinkedService met de naam **AzureStorageLinkedService**) en in de map **script** van de container **adfgetstarted**.
* De **definieert** sectie wordt gebruikt om op te geven van de runtime-instellingen die worden doorgegeven aan het hive-script als Hive-configuratiewaarden (bijvoorbeeld `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).

Zie [activiteiten voor GEGEVENSTRANSFORMATIE](#data-transformation-activities) sectie in dit artikel voor JSON-voorbeelden die activiteiten voor gegevenstransformatie in een pijplijn definiëren.

Zie voor een volledige procedure voor het maken van deze pipeline [zelfstudie: uw eerste pijplijn om gegevens te verwerken met behulp van Hadoop-cluster](data-factory-build-your-first-pipeline.md). 

## <a name="linked-service"></a>Gekoppelde service
De structuur op hoog niveau voor de definitie van een gekoppelde service is als volgt:

```json
{
    "name": "<name of the linked service>",
    "properties": {
        "type": "<type of the linked service>",
        "typeProperties": {
        }
    }
}
```

De eigenschappen in de JSON-definitie van de activiteit volgende tabel wordt beschreven:

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- | 
| naam | Naam van de gekoppelde service. | Ja | 
| Eigenschappen - type | Type van de gekoppelde service. Bijvoorbeeld: Azure Storage, Azure SQL Database. |
| typeProperties | De sectie typeProperties heeft elementen die zijn verschillend voor elke gegevensopslag of compute-omgeving. Zie [gegevensarchieven](#datastores) sectie voor alle gegevens store gekoppelde services en [omgevingen compute](#compute-environments) voor alle rekencapaciteit gekoppelde services |   

## <a name="dataset"></a>Gegevensset 
Een gegevensset in Azure Data Factory is als volgt gedefinieerd:

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
| naam | Naam van de gegevensset. Zie [Azure Data Factory - naamgevingsregels](data-factory-naming-rules.md) voor naamgevingsregels. |Ja |N.v.t. |
| type | Het type van de gegevensset. Geef een van de typen die worden ondersteund door Azure Data Factory (bijvoorbeeld: AzureBlob, AzureSqlTable). Zie [GEGEVENSARCHIEVEN](#data-stores) sectie voor de gegevensopslag en dataset die worden ondersteund door de Data Factory. | 
| structuur | Schema voor de gegevensset. Het bevat kolommen, de typen, enz. | Nee |N.v.t. |
| typeProperties | Eigenschappen die overeenkomt met het geselecteerde type. Zie [GEGEVENSARCHIEVEN](#data-stores) sectie voor de ondersteunde typen en hun eigenschappen. |Ja |N.v.t. |
| external | Booleaanse vlag om op te geven of een gegevensset expliciet wordt geproduceerd door een data factory-pijplijn of niet. |Nee |onwaar |
| availability | Definieert het venster verwerking of het segmenteringshulplijnen model voor de gegevensset voor productie. Zie voor meer informatie op de gegevensset segmentering model [planning en uitvoering](data-factory-scheduling-and-execution.md) artikel. |Ja |N.v.t. |
| policy |Definieert de criteria of de conditie van de segmenten van de gegevensset moeten voldoen. <br/><br/>Zie voor meer informatie [gegevensset beleid](#Policy) sectie. |Nee |N.v.t. |

Elke kolom in de **structuur** sectie bevat de volgende eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| naam |De naam van de kolom. |Ja |
| type |Het gegevenstype van de kolom.  |Nee |
| Cultuur |.NET-gebaseerde cultuur moet worden gebruikt wanneer het type is opgegeven en .NET-type `Datetime` of `Datetimeoffset`. Standaard is `en-us`. |Nee |
| Indeling |Indeling van tekenreeks moet worden gebruikt wanneer het type is opgegeven en .NET-type `Datetime` of `Datetimeoffset`. |Nee |

In het volgende voorbeeld wordt de gegevensset heeft drie kolommen `slicetimestamp`, `projectname`, en `pageviews` en ze zijn van het type: String, String en Decimal respectievelijk.

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

De volgende tabel beschrijft de eigenschappen kunt u in de **beschikbaarheid** sectie:

| Eigenschap | Beschrijving | Vereist | Standaard |
| --- | --- | --- | --- |
| frequency |Hiermee geeft u tijdseenheid voor de gegevensset segment productie.<br/><br/><b>Ondersteunde frequentie</b>: minuut, uur, dag, Week, maand |Ja |N.v.t. |
| interval |Hiermee geeft u een vermenigvuldiger voor de frequentie<br/><br/>"Frequentie x-interval" bepaalt hoe vaak het segment wordt geproduceerd.<br/><br/>Als u de gegevensset worden gesegmenteerd op uurbasis moet, stelt u <b>frequentie</b> naar <b>uur</b>, en <b>interval</b> naar <b>1</b>.<br/><br/><b>Opmerking</b>: als u frequentie als minuut opgeeft, raden wij aan dat u het interval aan niet lager zijn dan 15 |Ja |N.v.t. |
| stijl |Hiermee geeft u op of het segment aan het begin/einde van het interval moet worden geproduceerd.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Als de frequentie is ingesteld op maand en stijl is ingesteld op EndOfInterval, wordt het segment op de laatste dag van de maand geproduceerd. Als de stijl is ingesteld op StartOfInterval, wordt het segment op de eerste dag van de maand geproduceerd.<br/><br/>Als de frequentie is ingesteld op de dag en stijl is ingesteld op EndOfInterval, wordt het segment wordt geproduceerd in het afgelopen uur van de dag.<br/><br/>Als de frequentie is ingesteld op uur en stijl is ingesteld op EndOfInterval, wordt het segment wordt geproduceerd aan het einde van het uur. Voor een segment 1-2 uur gedurende het segment geproduceerd om 2 uur. |Nee |EndOfInterval |
| anchorDateTime |Hiermee definieert u de absolute positie in de tijd die wordt gebruikt door de planner gegevensset segmentgrenzen berekenen. <br/><br/><b>Opmerking</b>: als de AnchorDateTime bevat de datumonderdelen die meer gedetailleerd dan de frequentie zijn, wordt de gedetailleerdere onderdelen worden genegeerd. <br/><br/>Bijvoorbeeld, als de <b>interval</b> is <b>per uur</b> (frequentie: uur en interval: 1) en de <b>AnchorDateTime</b> bevat <b>minuten en seconden</b> de <b>minuten en seconden</b> delen van de AnchorDateTime worden genegeerd. |Nee |01-01/0001 |
| offset |TimeSpan waarmee het begin en einde van alle segmenten van de gegevensset zijn verplaatst. <br/><br/><b>Opmerking</b>: als zowel anchorDateTime als offset worden opgegeven, het resultaat is een gecombineerde verschuiving. |Nee |N.v.t. |

De volgende sectie van de beschikbaarheid geeft aan dat de uitvoergegevensset die geproduceerd per uur (of) invoer is gegevensset per uur beschikbaar is:

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

De **beleid** sectie in de definitie van gegevensset definieert u de criteria of de conditie van de segmenten van de gegevensset moeten voldoen.

| Beleidsnaam | Beschrijving | Toegepast op | Vereist | Standaard |
| --- | --- | --- | --- | --- |
| minimumSizeMB |Valideert dat de gegevens in een **Azure blob** voldoet aan de minimale grootte (in MB). |Azure Blob |Nee |N.v.t. |
| minimumRows |Valideert dat de gegevens in een **Azure SQL-database** of een **Azure-tabel** het minimum aantal rijen bevat. |<ul><li>Azure SQL Database</li><li>Azure-tabel</li></ul> |Nee |N.v.t. |

**Voorbeeld:**

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

Tenzij een gegevensset wordt geproduceerd door Azure Data Factory, moet dit worden gemarkeerd als **externe**. Deze instelling in het algemeen geldt voor de eerste activiteit in een pijplijn-invoer tenzij activiteit of koppeling van de pijplijn wordt gebruikt.

| Naam | Beschrijving | Vereist | Standaardwaarde |
| --- | --- | --- | --- |
| dataDelay |Tijd voor het uitstellen van de controle van de beschikbaarheid van de externe gegevens voor het opgegeven segment. Als de gegevens elk uur beschikbaar is, kan er bijvoorbeeld het selectievakje voor een overzicht van de externe gegevens beschikbaar is en het bijbehorende segment gereed worden uitgesteld met behulp van dataDelay.<br/><br/>Geldt alleen voor de huidige tijd.  Bijvoorbeeld, als het is nu om 13:00 uur en deze waarde 10 minuten is, begint de validatie om 1:10 uur.<br/><br/>Deze instelling heeft geen invloed op segmenten in het verleden (segmenten met segment eindtijd + dataDelay < nu) zonder enige vertraging worden verwerkt.<br/><br/>Tijd groter is dan 23:59 uur wilt opgeven met de `day.hours:minutes:seconds` indeling. Bijvoorbeeld, als u 24 uur, gebruik geen 24:00:00; Gebruik in plaats daarvan 1.00:00:00. Als u 24:00:00, wordt dit beschouwd als 24 dagen (24.00:00:00). Geef 1:04:00:00 voor 1 dag en 4 uur. |Nee |0 |
| retryInterval |De wachttijd tussen een fout en de volgende opnieuw geprobeerd. Als een try mislukt, is de volgende poging na retryInterval. <br/><br/>Als het nu om 13:00 uur, beginnen we in de eerste poging. Als de duur van de eerste validatiecontrole is 1 minuut en de bewerking is mislukt, de volgende poging is 1:00 + 1 min (duur), 1 min. (interval voor opnieuw proberen) + = 1:02 PM. <br/><br/>Er is geen vertraging segmenten in het verleden. De nieuwe poging gebeurt onmiddellijk. |Nee |00:01:00 (1 minuut) |
| retryTimeout |De time-out voor nieuwe pogingen.<br/><br/>Als deze eigenschap is ingesteld op 10 minuten, moet de validatie binnen 10 minuten worden voltooid. Als het duurt langer dan 10 minuten de validatie uitvoeren, wordt de nieuwe poging time-out.<br/><br/>Als alle pogingen voor de validatie van een time-out optreedt, wordt als een time-out opgetreden bij het segment gemarkeerd. |Nee |00:10:00 (10 minuten) |
| maximumRetry |Aantal keren om te controleren op de beschikbaarheid van de externe gegevens. De toegestane maximumwaarde is 10. |Nee |3 |


## <a name="data-stores"></a>GEGEVENSOPSLAG
De [gekoppelde service](#linked-service) beschrijvingen van de opgegeven sectie voor JSON-elementen die gemeenschappelijk voor alle typen gekoppelde services zijn. Deze sectie bevat informatie over de JSON-elementen die specifiek voor elke gegevensarchief zijn.

De [gegevensset](#dataset) beschrijvingen van de opgegeven sectie voor JSON-elementen die gemeenschappelijk voor alle typen gegevenssets zijn. Deze sectie bevat informatie over de JSON-elementen die specifiek voor elke gegevensarchief zijn.

De [activiteit](#activity) beschrijvingen van de opgegeven sectie voor JSON-elementen die gemeenschappelijk voor alle typen activiteiten zijn. Deze sectie bevat informatie over de JSON-elementen die specifiek voor elke gegevensarchief zijn wanneer deze wordt gebruikt als een bron/sink in een kopieeractiviteit.  

Klik op de koppeling voor het archief dat u geïnteresseerd bent in de JSON-schema's voor de gekoppelde service, gegevensset en de bron/sink voor de kopieeractiviteit zien.

| Category | Gegevensarchief 
|:--- |:--- |
| **Azure** |[Azure Blob Storage](#azure-blob-storage) |
| &nbsp; |[Azure Data Lake Store](#azure-datalake-store) |
| &nbsp; |[Azure Cosmos DB](#azure-cosmos-db) |
| &nbsp; |[Azure SQL Database](#azure-sql-database) |
| &nbsp; |[Azure SQL Data Warehouse](#azure-sql-data-warehouse) |
| &nbsp; |[Azure Search](#azure-search) |
| &nbsp; |[Azure Table storage](#azure-table-storage) |
| **Databases** |[Amazon Redshift](#amazon-redshift) |
| &nbsp; |[IBM DB2](#ibm-db2) |
| &nbsp; |[MySQL](#mysql) |
| &nbsp; |[Oracle](#oracle) |
| &nbsp; |[PostgreSQL](#postgresql) |
| &nbsp; |[SAP Business Warehouse](#sap-business-warehouse) |
| &nbsp; |[SAP HANA](#sap-hana) |
| &nbsp; |[SQL Server](#sql-server) |
| &nbsp; |[Sybase](#sybase) |
| &nbsp; |[Teradata](#teradata) |
| **NoSQL** |[Cassandra](#cassandra) |
| &nbsp; |[MongoDB](#mongodb) |
| **File** |[Amazon S3](#amazon-s3) |
| &nbsp; |[Bestandssysteem](#file-system) |
| &nbsp; |[FTP](#ftp) |
| &nbsp; |[HDFS](#hdfs) |
| &nbsp; |[SFTP](#sftp) |
| **Andere** |[HTTP](#http) |
| &nbsp; |[OData](#odata) |
| &nbsp; |[ODBC](#odbc) |
| &nbsp; |[Salesforce](#salesforce) |
| &nbsp; |[Web-tabel](#web-table) |

## <a name="azure-blob-storage"></a>Azure Blob Storage

### <a name="linked-service"></a>Gekoppelde service
Er zijn twee soorten gekoppelde services: gekoppelde Azure Storage-service en de gekoppelde Azure Storage SAS-service.

#### <a name="azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service
Uw Azure storage-account koppelen aan een gegevensfactory met behulp van de **accountsleutel**, maak een gekoppelde Azure Storage-service. Voor het definiëren van een Azure Storage gekoppelde service, stelt u de **type** van de gekoppelde service naar **AzureStorage**. U kunt vervolgens opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met Azure-opslag voor de eigenschap connectionString. |Ja |

##### <a name="example"></a>Voorbeeld  

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

#### <a name="azure-storage-sas-linked-service"></a>Gekoppelde Azure Storage SAS-Service
De service Azure Storage SAS is gekoppeld, kunt u een Azure Storage-Account koppelen aan een Azure data factory met behulp van een Shared Access Signature (SAS). Het biedt de gegevensfactory beperkt/tijdsgebonden toegang tot alle/specifieke bronnen, (blobcontainer) / in de opslag. Als uw Azure storage-account koppelen aan een gegevensfactory met behulp van Shared Access Signature, maakt u een SAS van Azure Storage gekoppelde service. Voor het definiëren van een SAS van Azure Storage gekoppelde service, stelt u de **type** van de gekoppelde service naar **AzureStorageSas**. U kunt vervolgens opgeven na eigenschappen in de **typeProperties** sectie:   

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| sasUri |Shared Access Signature URI voor de Azure Storage-resources zoals blob-container of tabel opgeven. |Ja |

##### <a name="example"></a>Voorbeeld

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<storageUri>?<sasToken>"   
        }  
    }  
}  
```

Zie voor meer informatie over deze gekoppelde services, [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Instellen als u een Azure Blob-gegevensset definieert, de **type** van de gegevensset **AzureBlob**. Geef vervolgens de volgende specifieke Azure Blob-eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| folderPath |Pad naar de container en map in blob storage. Voorbeeld: myblobcontainer\myblobfolder\ |Ja |
| fileName |De naam van de blob. Bestandsnaam is optioneel en is hoofdlettergevoelig.<br/><br/>Als u een filename opgeeft, wordt de activiteit (inclusief kopiëren) werkt op de specifieke Blob.<br/><br/>FileName is opgegeven, bevat kopiëren alle Blobs in de folderPath voor invoergegevensset.<br/><br/>Wanneer fileName niet voor een uitvoergegevensset opgegeven is, de naam van het gegenereerde bestand zou worden in de volgende indeling: Data. <Guid>.txt (voorbeeld:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nee |
| partitionedBy |partitionedBy is een optionele eigenschap. U kunt deze gebruiken om op te geven van een dynamische folderPath en de bestandsnaam voor de reeksgegevens. FolderPath kan bijvoorbeeld parameters worden gebruikt voor elk uur van gegevens. |Nee |
| Indeling | De volgende indelingstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [parketvloeren indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset. |Nee |
| Compressie | Geef het type en de compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus: **optimale** en **snelst**. Zie voor meer informatie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

#### <a name="example"></a>Voorbeeld

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


Zie voor meer informatie [Azure Blob-connector](data-factory-azure-blob-connector.md#dataset-properties) artikel.

### <a name="blobsource-in-copy-activity"></a>BlobSource in de kopieerbewerking
Als u gegevens uit een Azure Blob Storage kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **BlobSource**, en opgeven na eigenschappen in de ** bron ** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| Recursieve |Hiermee wordt aangegeven of de gegevens recursief is gelezen uit de submappen of alleen uit de opgegeven map. |True (standaardwaarde), False |Nee |

#### <a name="example-blobsource"></a>Voorbeeld: BlobSource **
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
### <a name="blobsink-in-copy-activity"></a>BlobSink in de kopieerbewerking
Als u gegevens naar een Azure Blob Storage kopiëren wilt, stelt u de **type sink** van de kopieeractiviteit naar **BlobSink**, en opgeven na eigenschappen in de **sink** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| copyBehavior |Definieert het gedrag kopiëren wanneer de bron BlobSource of het bestandssysteem. |<b>PreserveHierarchy</b>: behoudt de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand naar de bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><br/><b>FlattenHierarchy</b>: alle bestanden uit de bronmap zijn in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde naam. <br/><br/><b>MergeFiles (standaard):</b> alle bestanden uit de bronmap op één bestand worden samengevoegd. Als de naam van het bestand/de opgegeven Blob is opgegeven, zijn de samengevoegde bestandsnaam de opgegeven naam; anders zou worden automatisch gegenereerde naam. |Nee |

#### <a name="example-blobsink"></a>Voorbeeld: BlobSink

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Zie voor meer informatie [Azure Blob-connector](data-factory-azure-blob-connector.md#copy-activity-properties) artikel. 

## <a name="azure-data-lake-store"></a>Azure Data Lake Store

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Azure Data Lake Store, stelt u het type van de gekoppelde service naar **AzureDataLakeStore**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AzureDataLakeStore** | Ja |
| dataLakeStoreUri | Geef informatie op over de Azure Data Lake Store-account. Het is in de volgende indeling: `https://[accountname].azuredatalakestore.net/webhdfs/v1` of `adl://[accountname].azuredatalakestore.net/`. | Ja |
| subscriptionId | Azure-abonnement-Id waartoe Data Lake Store behoort. | Vereist voor sink |
| resourceGroupName | Azure-resource groepsnaam waartoe Data Lake Store behoort. | Vereist voor sink |
| servicePrincipalId | Geef de toepassing client-ID. | Ja (voor verificatie voor service-principal) |
| servicePrincipalKey | De sleutel van de toepassing opgeven. | Ja (voor verificatie voor service-principal) |
| tenant | De tenant-gegevens (domain name of tenant-ID) opgeven onder uw toepassing zich bevindt. U kunt deze ophalen door de muis in de rechterbovenhoek van de Azure portal. | Ja (voor verificatie voor service-principal) |
| Autorisatie | Klik op **autoriseren** knop in de **Data Factory-Editor** en voer uw referenties op waarmee de automatisch gegenereerde autorisatie-URL worden toegewezen aan deze eigenschap. | Ja (voor verificatie van gebruikersreferenties)|
| sessie-id | OAuth-sessie-id van de OAuth-autorisatie-sessie. Elke sessie-id is uniek en kan slechts eenmaal worden gebruikt. Deze instelling wordt automatisch gegenereerd wanneer u Data Factory-Editor. | Ja (voor verificatie van gebruikersreferenties) |

#### <a name="example-using-service-principal-authentication"></a>Voorbeeld: met behulp van verificatie van de service-principal
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info. Example: microsoft.onmicrosoft.com>"
        }
    }
}
```

#### <a name="example-using-user-credential-authentication"></a>Voorbeeld: met behulp van verificatie van gebruikersreferenties
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

Zie voor meer informatie [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Instellen als u een Azure Data Lake Store-gegevensset definieert, de **type** van de gegevensset **AzureDataLakeStore**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| folderPath |Pad naar de container en map in de Azure Data Lake opslaan. |Ja |
| fileName |De naam van het bestand in de Azure Data Lake store. Bestandsnaam is optioneel en is hoofdlettergevoelig. <br/><br/>Als u een filename opgeeft, wordt de activiteit (inclusief kopiëren) werkt op het specifieke bestand.<br/><br/>FileName is opgegeven, bevat Kopieer alle bestanden in het mappad voor invoergegevensset.<br/><br/>Wanneer fileName niet voor een uitvoergegevensset opgegeven is, de naam van het gegenereerde bestand zou worden in de volgende indeling: Data. <Guid>.txt (voorbeeld:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nee |
| partitionedBy |partitionedBy is een optionele eigenschap. U kunt deze gebruiken om op te geven van een dynamische folderPath en de bestandsnaam voor de reeksgegevens. FolderPath kan bijvoorbeeld parameters worden gebruikt voor elk uur van gegevens. |Nee |
| Indeling | De volgende indelingstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [parketvloeren indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset. |Nee |
| Compressie | Geef het type en de compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus: **optimale** en **snelst**. Zie voor meer informatie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

#### <a name="example"></a>Voorbeeld
```json
{
    "name": "AzureDataLakeStoreInput",
    "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
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

Zie voor meer informatie [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#dataset-properties) artikel. 

### <a name="azure-data-lake-store-source-in-copy-activity"></a>Azure Data Lake Store-bron in de kopieerbewerking
Als u gegevens uit een Azure Data Lake Store kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **AzureDataLakeStoreSource**, en opgeven na eigenschappen in de **bron** sectie:

**AzureDataLakeStoreSource** ondersteunt de volgende eigenschappen **typeProperties** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| Recursieve |Hiermee wordt aangegeven of de gegevens recursief is gelezen uit de submappen of alleen uit de opgegeven map. |True (standaardwaarde), False |Nee |

#### <a name="example-azuredatalakestoresource"></a>Voorbeeld: AzureDataLakeStoreSource

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureDakeLaketoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureDataLakeStoreInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureDataLakeStoreSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Zie voor meer informatie [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#copy-activity-properties) artikel.

### <a name="azure-data-lake-store-sink-in-copy-activity"></a>Azure Data Lake Store Sink in de kopieerbewerking
Als u gegevens naar een Azure Data Lake Store kopiëren wilt, stelt u de **type sink** van de kopieeractiviteit naar **AzureDataLakeStoreSink**, en opgeven na eigenschappen in de **sink** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| copyBehavior |Hiermee geeft u het gedrag van de kopie. |<b>PreserveHierarchy</b>: behoudt de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand naar de bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><br/><b>FlattenHierarchy</b>: alle bestanden uit de bronmap worden gemaakt in het eerste niveau van de doelmap. De doelbestanden worden gemaakt met de automatisch gegenereerde naam.<br/><br/><b>MergeFiles</b>: alle bestanden uit de bronmap op één bestand worden samengevoegd. Als de naam van het bestand/de opgegeven Blob is opgegeven, zijn de samengevoegde bestandsnaam de opgegeven naam; anders zou worden automatisch gegenereerde naam. |Nee |

#### <a name="example-azuredatalakestoresink"></a>Voorbeeld: AzureDataLakeStoreSink
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoDataLake",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureDataLakeStoreOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Zie voor meer informatie [Azure Data Lake Store connector](data-factory-azure-datalake-connector.md#copy-activity-properties) artikel. 

## <a name="azure-cosmos-db"></a>Azure Cosmos DB  

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Cosmos Azure DB, stelt u de **type** van de gekoppelde service naar **DocumentDb**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| **Eigenschap** | **Beschrijving** | **Vereist** |
| --- | --- | --- |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met Azure DB die Cosmos-database. |Ja |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "CosmosDBLinkedService",
    "properties": {
        "type": "DocumentDb",
        "typeProperties": {
            "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
    }
}
```
Zie voor meer informatie [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Instellen als u een gegevensset Azure Cosmos DB definieert, de **type** van de gegevensset **DocumentDbCollection**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| **Eigenschap** | **Beschrijving** | **Vereist** |
| --- | --- | --- |
| CollectionName |De naam van de Azure DB die Cosmos-verzameling. |Ja |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "PersonCosmosDBTable",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "CosmosDBLinkedService",
        "typeProperties": {
            "collectionName": "Person"
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```
Zie voor meer informatie [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#dataset-properties) artikel.

### <a name="azure-cosmos-db-collection-source-in-copy-activity"></a>Azure Cosmos DB verzameling bron in de kopieerbewerking
Als u gegevens uit een Cosmos Azure DB kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **DocumentDbCollectionSource**, en opgeven na eigenschappen in de **bron** sectie:


| **Eigenschap** | **Beschrijving** | **Toegestane waarden** | **Vereist** |
| --- | --- | --- | --- |
| query |Specificeer de query voor het lezen van gegevens. |Queryreeks wordt ondersteund door Azure Cosmos DB. <br/><br/>Voorbeeld:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Nee <br/><br/>Als niet wordt opgegeven, de SQL-instructie die is uitgevoerd:`select <columns defined in structure> from mycollection` |
| nestingSeparator |Speciaal teken om aan te geven dat het document is genest |Willekeurig teken. <br/><br/>Azure Cosmos-database is een NoSQL-opslagplaats voor JSON-documenten, waarbij geneste structuren zijn toegestaan. Azure Data Factory kan gebruiker duiden hiërarchie via nestingSeparator die "." in de bovenstaande voorbeelden. Met het scheidingsteken, met de kopieerbewerking genereert het object "Name" met de drie onderliggende elementen eerst middelste en de laatste volgens 'Name.First', 'Name.Middle' en 'Name.Last' in de tabeldefinitie. |Nee |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "DocDbToBlobPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "DocumentDbCollectionSource",
                    "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                    "nestingSeparator": "."
                },
                "sink": {
                    "type": "BlobSink",
                    "blobWriterAddHeader": true,
                    "writeBatchSize": 1000,
                    "writeBatchTimeout": "00:00:59"
                }
            },
            "inputs": [{
                "name": "PersonCosmosDBTable"
            }],
            "outputs": [{
                "name": "PersonBlobTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromCosmosDbToBlob"
        }],
        "start": "2016-04-01T00:00:00",
        "end": "2016-04-02T00:00:00"
    }
}
```

### <a name="azure-cosmos-db-collection-sink-in-copy-activity"></a>Azure Cosmos DB verzameling Sink in de kopieerbewerking
Als u gegevens naar Azure Cosmos DB kopiëren wilt, stelt u de **type sink** van de kopieeractiviteit naar **DocumentDbCollectionSink**, en opgeven na eigenschappen in de **sink** sectie:

| **Eigenschap** | **Beschrijving** | **Toegestane waarden** | **Vereist** |
| --- | --- | --- | --- |
| nestingSeparator |Er is een speciaal teken in naam van de bronkolom om aan te geven dat geneste document nodig. <br/><br/>Zoals hierboven: `Name.First` in de uitvoer van de tabel de volgende JSON-structuur in het document Cosmos DB produceert:<br/><br/>"Name": {<br/>    ' First ': 'John'<br/>}, |Teken dat wordt gebruikt voor het scheiden van geneste niveaus.<br/><br/>Standaardwaarde is `.` (punt). |Teken dat wordt gebruikt voor het scheiden van geneste niveaus. <br/><br/>Standaardwaarde is `.` (punt). |
| writeBatchSize |Het aantal parallelle aanvragen voor Azure DB die Cosmos-service om documenten te maken.<br/><br/>U kunt de prestaties aanpassen bij het kopiëren van gegevens van Azure DB die Cosmos met behulp van deze eigenschap. U kunt een betere prestaties verwachten wanneer u writeBatchSize verhogen omdat meer parallelle aanvragen bij Azure Cosmos DB worden verzonden. Moet u echter om te voorkomen dat beperking kunnen genereert het foutbericht weergegeven: 'Vragen snelheid is groot'.<br/><br/>Beperking wordt bepaald door een aantal factoren, onder andere de grootte van documenten, aantal termen in documenten, indexeren beleid van de doelverzameling, enzovoort. Voor het kopiëren van, kunt u een betere verzameling (bijvoorbeeld S3) hebben de meeste beschikbare doorvoer (2500 aanvragen eenheden/seconde). |Geheel getal |Nee (standaard: 5) |
| writeBatchTimeout |Wachttijd voor de bewerking te voltooien voordat er een optreedt time-out. |TimeSpan<br/><br/> Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "BlobToDocDbPipeline",
    "properties": {
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "DocumentDbCollectionSink",
                    "nestingSeparator": ".",
                    "writeBatchSize": 2,
                    "writeBatchTimeout": "00:00:00"
                },
                "translator": {
                    "type": "TabularTranslator",
                    "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix"
                }
            },
            "inputs": [{
                "name": "PersonBlobTableIn"
            }],
            "outputs": [{
                "name": "PersonCosmosDbTableOut"
            }],
            "policy": {
                "concurrency": 1
            },
            "name": "CopyFromBlobToCosmosDb"
        }],
        "start": "2016-04-14T00:00:00",
        "end": "2016-04-15T00:00:00"
    }
}
```

Zie voor meer informatie [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#copy-activity-properties) artikel.

## <a name="azure-sql-database"></a>Azure SQL Database

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Azure SQL Database, stelt u de **type** van de gekoppelde service naar **AzureSqlDatabase**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met de Azure SQL Database-exemplaar voor de eigenschap connectionString. |Ja |

#### <a name="example"></a>Voorbeeld
```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

Zie voor meer informatie [Azure SQL-connector](data-factory-azure-sql-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Instellen als u een Azure SQL Database-gegevensset definieert, de **type** van de gegevensset **AzureSqlTable**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel of weergave in de Azure SQL Database-instantie waarnaar de gekoppelde service verwijst. |Ja |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "AzureSqlInput",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
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
Zie voor meer informatie [Azure SQL-connector](data-factory-azure-sql-connector.md#dataset-properties) artikel. 

### <a name="sql-source-in-copy-activity"></a>SQL-bron in de kopieerbewerking
Als u gegevens uit een Azure SQL Database kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **SqlSource**, en opgeven na eigenschappen in de **bron** sectie:


| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| sqlReaderQuery |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Voorbeeld: `select * from MyTable`. |Nee |
| sqlReaderStoredProcedureName |Naam van de opgeslagen procedure die gegevens uit de brontabel leest. |Naam van de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. |Nee |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
Zie voor meer informatie [Azure SQL-connector](data-factory-azure-sql-connector.md#copy-activity-properties) artikel. 

### <a name="sql-sink-in-copy-activity"></a>SQL-Sink in de kopieerbewerking
Als u gegevens naar Azure SQL Database kopiëren wilt, stelt u de **type sink** van de kopieeractiviteit naar **SqlSink**, en opgeven na eigenschappen in de **sink** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| writeBatchTimeout |Wachttijd voor de batch-insert-bewerking te voltooien voordat er een optreedt time-out. |TimeSpan<br/><br/> Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |
| writeBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt. |Geheel getal (aantal rijen) |Nee (standaard: 10000) |
| sqlWriterCleanupScript |Geef een query voor de Kopieeractiviteit uitvoeren zodat de gegevens van een bepaald segment wordt opgeschoond. |Een query-instructie. |Nee |
| sliceIdentifierColumnName |Geef de naam van een kolom voor de Kopieeractiviteit te vullen met de segment-ID automatisch gegenereerd en die wordt gebruikt voor het opschonen van de gegevens van een bepaald segment wanneer opnieuw uitgevoerd. |De naam van de kolom van een kolom met gegevenstype binary(32). |Nee |
| sqlWriterStoredProcedureName |Naam van de opgeslagen procedure die upserts (updates/INSERT) gegevens in de doeltabel. |Naam van de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. |Nee |
| sqlWriterTableType |Geef een naam van het type tabel moet worden gebruikt in de opgeslagen procedure. Kopieeractiviteit maakt u de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Code van de opgeslagen procedure kan de gegevens wordt gekopieerd met de bestaande gegevens vervolgens samenvoegen. |Een typenaam van de tabel. |Nee |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Zie voor meer informatie [Azure SQL-connector](data-factory-azure-sql-connector.md#copy-activity-properties) artikel. 

## <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Azure SQL Data Warehouse, stelt u de **type** van de gekoppelde service naar **AzureSqlDW**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met de Azure SQL Data Warehouse-exemplaar voor de eigenschap connectionString. |Ja |



#### <a name="example"></a>Voorbeeld

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

Zie voor meer informatie [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Instellen als u een Azure SQL Data Warehouse-gegevensset definieert, de **type** van de gegevensset **AzureSqlDWTable**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |Naam van de tabel of weergave in de Azure SQL Data Warehouse-database waarnaar de gekoppelde service verwijst. |Ja |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "AzureSqlDWInput",
    "properties": {
    "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
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

Zie voor meer informatie [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) artikel. 

### <a name="sql-dw-source-in-copy-activity"></a>SQL DW-bron in de kopieerbewerking
Als u gegevens uit Azure SQL Data Warehouse kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **SqlDWSource**, en opgeven na eigenschappen in de **bron** sectie:


| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| sqlReaderQuery |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `select * from MyTable`. |Nee |
| sqlReaderStoredProcedureName |Naam van de opgeslagen procedure die gegevens uit de brontabel leest. |Naam van de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. |Nee |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLDWtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSqlDWInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlDWSource",
                    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Zie voor meer informatie [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) artikel. 

### <a name="sql-dw-sink-in-copy-activity"></a>SQL DW Sink in de kopieerbewerking
Als u gegevens naar Azure SQL Data Warehouse kopiëren wilt, stelt u de **type sink** van de kopieeractiviteit naar **SqlDWSink**, en opgeven na eigenschappen in de **sink** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| sqlWriterCleanupScript |Geef een query voor de Kopieeractiviteit uitvoeren zodat de gegevens van een bepaald segment wordt opgeschoond. |Een query-instructie. |Nee |
| allowPolyBase |Geeft aan of PolyBase (indien van toepassing) gebruiken in plaats van BULKINSERT mechanisme. <br/><br/> **Met PolyBase is de aanbevolen manier om gegevens te laden in SQL Data Warehouse.** |True <br/>False (standaard) |Nee |
| polyBaseSettings |Een groep met eigenschappen die kunnen worden opgegeven wanneer de **allowPolybase** eigenschap is ingesteld op **true**. |&nbsp; |Nee |
| rejectValue |Hiermee geeft u het nummer of het percentage van de rijen die kunnen worden afgewezen voordat de query is mislukt. <br/><br/>Meer informatie over opties voor het weigeren van de PolyBase in de **argumenten** sectie van [maken EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) onderwerp. |0 (standaard), 1, 2... |Nee |
| rejectType |Hiermee geeft u op of de optie rejectValue is opgegeven als een letterlijke waarde of een percentage. |In waarde (standaard), Percentage |Nee |
| rejectSampleValue |Bepaalt het aantal rijen om op te halen voordat de PolyBase berekent het percentage van de geweigerde rijen opnieuw. |1, 2... |Ja, als **rejectType** is **percentage** |
| useTypeDefault |Geeft aan hoe de ontbrekende waarden in tekstbestanden met scheidingstekens verwerken wanneer PolyBase gegevens uit het tekstbestand ophaalt.<br/><br/>Meer informatie over deze eigenschap in de sectie argumenten [maken EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx). |True, False (standaard) |Nee |
| writeBatchSize |Gegevens invoegen in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt |Geheel getal (aantal rijen) |Nee (standaard: 10000) |
| writeBatchTimeout |Wachttijd voor de batch-insert-bewerking te voltooien voordat er een optreedt time-out. |TimeSpan<br/><br/> Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQLDW",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureSqlDWOutput"
            }],
            "typeProperties": {
                "source": {
                "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlDWSink",
                    "allowPolyBase": true
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Zie voor meer informatie [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) artikel. 

## <a name="azure-search"></a>Azure Search

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Azure Search, stelt u de **type** van de gekoppelde service naar **AzureSearch**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| url | De URL voor de Azure Search-service. | Ja |
| sleutel | Administrator-code voor de Azure Search-service. | Ja |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

Zie voor meer informatie [Azure Search-connector](data-factory-azure-search-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Instellen als u een Azure Search-gegevensset definieert, de **type** van de gegevensset **AzureSearchIndex**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| type | De eigenschap type moet worden ingesteld op **AzureSearchIndex**.| Ja |
| NaamCommunity | De naam van de Azure Search-index. Data Factory maakt niet de index. De index moet bestaan in Azure Search. | Ja |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties": {
            "indexName": "products"
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
    }
}
```

Zie voor meer informatie [Azure Search-connector](data-factory-azure-search-connector.md#dataset-properties) artikel.

### <a name="azure-search-index-sink-in-copy-activity"></a>Sink voor Azure Search-Index in de kopieerbewerking
Als u gegevens naar een Azure Search-index kopiëren wilt, stelt u de **type sink** van de kopieeractiviteit naar **AzureSearchIndexSink**, en opgeven na eigenschappen in de **sink** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------------- | -------- |
| WriteBehavior | Geeft aan of samenvoegen of wanneer een document al in de index bestaat te vervangen. | samenvoegen (standaard)<br/>Uploaden| Nee |
| writeBatchSize | Gegevens geüpload naar de Azure Search-index wanneer de buffergrootte writeBatchSize bereikt. | 1-1000. Standaardwaarde is 1000. | Nee |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoAzureSearchIndex",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureSearchIndexDataset"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "AzureSearchIndexSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Zie voor meer informatie [Azure Search-connector](data-factory-azure-search-connector.md#copy-activity-properties) artikel.

## <a name="azure-table-storage"></a>Azure-tabelopslag

### <a name="linked-service"></a>Gekoppelde service
Er zijn twee soorten gekoppelde services: gekoppelde Azure Storage-service en de gekoppelde Azure Storage SAS-service.

#### <a name="azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service
Uw Azure storage-account koppelen aan een gegevensfactory met behulp van de **accountsleutel**, maak een gekoppelde Azure Storage-service. Voor het definiëren van een Azure Storage gekoppelde service, stelt u de **type** van de gekoppelde service naar **AzureStorage**. U kunt vervolgens opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op: **AzureStorage** |Ja |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met Azure-opslag voor de eigenschap connectionString. |Ja |

**Voorbeeld:**  

```json
{  
    "name": "StorageLinkedService",  
    "properties": {  
        "type": "AzureStorage",  
        "typeProperties": {  
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
        }  
    }  
}  
```

#### <a name="azure-storage-sas-linked-service"></a>Gekoppelde Azure Storage SAS-Service
De service Azure Storage SAS is gekoppeld, kunt u een Azure Storage-Account koppelen aan een Azure data factory met behulp van een Shared Access Signature (SAS). Het biedt de gegevensfactory beperkt/tijdsgebonden toegang tot alle/specifieke bronnen, (blobcontainer) / in de opslag. Als uw Azure storage-account koppelen aan een gegevensfactory met behulp van Shared Access Signature, maakt u een SAS van Azure Storage gekoppelde service. Voor het definiëren van een SAS van Azure Storage gekoppelde service, stelt u de **type** van de gekoppelde service naar **AzureStorageSas**. U kunt vervolgens opgeven na eigenschappen in de **typeProperties** sectie:   

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op: **AzureStorageSas** |Ja |
| sasUri |Shared Access Signature URI voor de Azure Storage-resources zoals blob-container of tabel opgeven. |Ja |

**Voorbeeld:**

```json
{  
    "name": "StorageSasLinkedService",  
    "properties": {  
        "type": "AzureStorageSas",  
        "typeProperties": {  
            "sasUri": "<storageUri>?<sasToken>"   
        }  
    }  
}  
```

Zie voor meer informatie over deze gekoppelde services, [Azure Table Storage connector](data-factory-azure-table-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Instellen als u een gegevensset Azure Table definieert, de **type** van de gegevensset **AzureTable**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in de Azure-tabel Database-instantie waarnaar de gekoppelde service verwijst. |Ja. Wanneer een tabelnaam is opgegeven zonder een azureTableSourceQuery, worden alle records uit de tabel worden gekopieerd naar de bestemming. Als een azureTableSourceQuery ook is opgegeven, worden de records uit de tabel die voldoet aan de query gekopieerd naar de bestemming. |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "AzureTableInput",
    "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
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

Zie voor meer informatie over deze gekoppelde services, [Azure Table Storage connector](data-factory-azure-table-connector.md#dataset-properties) artikel. 

### <a name="azure-table-source-in-copy-activity"></a>Bron van de Azure-tabel in de kopieerbewerking
Als u gegevens uit Azure Table Storage kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **AzureTableSource**, en opgeven na eigenschappen in de **bron** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| azureTableSourceQuery |Gebruik de aangepaste query om gegevens te lezen. |Azure-tabel queryreeks. Zie de voorbeelden in de volgende sectie. |Nee. Wanneer een tabelnaam is opgegeven zonder een azureTableSourceQuery, worden alle records uit de tabel worden gekopieerd naar de bestemming. Als een azureTableSourceQuery ook is opgegeven, worden de records uit de tabel die voldoet aan de query gekopieerd naar de bestemming. |
| azureTableSourceIgnoreTableNotFound |Geef aan of de uitzondering van de tabel slikken bestaat niet. |DE WAARDE TRUE<br/>DE WAARDE FALSE |Nee |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureTabletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "AzureTableSource",
                    "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Zie voor meer informatie over deze gekoppelde services, [Azure Table Storage connector](data-factory-azure-table-connector.md#copy-activity-properties) artikel. 

### <a name="azure-table-sink-in-copy-activity"></a>Azure-tabel Sink in de kopieerbewerking
Als u gegevens naar Azure Table Storage kopiëren wilt, stelt u de **type sink** van de kopieeractiviteit naar **AzureTableSink**, en opgeven na eigenschappen in de **sink** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| azureTableDefaultPartitionKeyValue |Standaardwaarde voor de partitiesleutel die kan worden gebruikt door de sink. |Een string-waarde. |Nee |
| azureTablePartitionKeyName |Geef de naam van de kolom waarvan de waarden worden gebruikt als partitiesleutels. Als niet wordt opgegeven, wordt AzureTableDefaultPartitionKeyValue gebruikt als de partitiesleutel. |De naam van een kolom. |Nee |
| azureTableRowKeyName |Geef de naam van de kolom waarvan de kolomwaarden worden gebruikt als de rijsleutel. Als niet wordt opgegeven, gebruikt u een GUID voor elke rij. |De naam van een kolom. |Nee |
| azureTableInsertType |De modus voor gegevens in Azure-tabel invoegen.<br/><br/>Deze eigenschap bepaalt of bestaande rijen in de uitvoertabel met partitie-als rijsleutels die overeenkomt met de waarden vervangen of samenvoegen van hebben. <br/><br/>Zie voor meer informatie over de werking van deze instellingen (merge en vervangen), [invoegen of Merge entiteit](https://msdn.microsoft.com/library/azure/hh452241.aspx) en [invoegen of vervangen entiteit](https://msdn.microsoft.com/library/azure/hh452242.aspx) onderwerpen. <br/><br> Deze instelling is van toepassing op het rijniveau van niet tabelniveau, en geen van beide optie worden de rijen in de uitvoertabel die niet bestaan in de invoer verwijderd. |samenvoegen (standaard)<br/>vervangen |Nee |
| writeBatchSize |Voegt de gegevens in de Azure-tabel wanneer de writeBatchSize of writeBatchTimeout is bereikt. |Geheel getal (aantal rijen) |Nee (standaard: 10000) |
| writeBatchTimeout |Voegt de gegevens in de Azure-tabel wanneer de writeBatchSize of writeBatchTimeout is bereikt. |TimeSpan<br/><br/>Voorbeeld: "00: 20:00 ' (20 minuten) |Nee (voor opslag client standaardtime-out standaardwaarde is 90 sec) |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureTableOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "AzureTableSink",
                    "writeBatchSize": 100,
                    "writeBatchTimeout": "01:00:00"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
Zie voor meer informatie over deze gekoppelde services, [Azure Table Storage connector](data-factory-azure-table-connector.md#copy-activity-properties) artikel. 

## <a name="amazon-redshift"></a>Amazon RedShift

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Redshift Amazon, stelt u de **type** van de gekoppelde service naar **AmazonRedshift**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| server |IP-adres of de hostnaam de naam van de server Amazon Redshift. |Ja |
| poort |Het nummer van de TCP-poort die de Amazon Redshift-server gebruikt om te luisteren naar verbindingen van clients. |Nee, de standaardwaarde: 5439 |
| database |De naam van de Amazon Redshift-database. |Ja |
| gebruikersnaam |Naam van de gebruiker die toegang tot de database heeft. |Ja |
| wachtwoord |Wachtwoord voor het gebruikersaccount. |Ja |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties": {
        "type": "AmazonRedshift",
        "typeProperties": {
            "server": "<Amazon Redshift host name or IP address>",
            "port": 5439,
            "database": "<database name>",
            "username": "user",
            "password": "password"
        }
    }
}
```

Zie voor meer informatie [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Instellen als u een gegevensset Amazon Redshift definieert, de **type** van de gegevensset **RelationalTable**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in de Amazon Redshift database waarnaar de gekoppelde service verwijst. |Nee (als **query** van **RelationalSource** is opgegeven) |


#### <a name="example"></a>Voorbeeld

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
Zie voor meer informatie [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#dataset-properties) artikel.

### <a name="relational-source-in-copy-activity"></a>Relationele gegevensbron in de kopieerbewerking 
Als u gegevens vanaf Amazon Redshift kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en opgeven na eigenschappen in de **bron** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `select * from MyTable`. |Nee (als **tableName** van **gegevensset** is opgegeven) |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
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
            "inputs": [{
                "name": "AmazonRedshiftInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonRedshiftToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
Zie voor meer informatie [Amazon Redshift connector](#data-factory-amazon-redshift-connector.md#copy-activity-properties) artikel.

## <a name="ibm-db2"></a>IBM DB2

### <a name="linked-service"></a>Gekoppelde service
Voor het definiëren van een IBM DB2 gekoppelde service, stelt u de **type** van de gekoppelde service naar **OnPremisesDB2**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| server |De naam van de DB2-server. |Ja |
| database |Naam van de DB2-database. |Ja |
| Schema |De naam van het schema in de database. Naam van het schema is hoofdlettergevoelig. |Nee |
| authenticationType |Het soort verificatie die verbinding maken met de DB2-database wordt gebruikt. Mogelijke waarden zijn: anoniem, basis en Windows. |Ja |
| gebruikersnaam |Geef de gebruikersnaam als u basisverificatie of Windows-verificatie gebruikt. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |
| gatewayName |Naam van de gateway die voor de Data Factory-service gebruiken moet voor verbinding met de lokale DB2-database. |Ja |

#### <a name="example"></a>Voorbeeld
```json
{
    "name": "OnPremDb2LinkedService",
    "properties": {
        "type": "OnPremisesDb2",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
Zie voor meer informatie [IBM DB2-connector](#data-factory-onprem-db2-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Instellen als u een gegevensset met de DB2 definieert, de **type** van de gegevensset **RelationalTable**, en de volgende eigenschappen opgeven in de **typeProperties** sectie:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in de DB2-Database-instantie waarnaar de gekoppelde service verwijst. De tabelnaam is hoofdlettergevoelig. |Nee (als **query** van **RelationalSource** is opgegeven) 

#### <a name="example"></a>Voorbeeld
```json
{
    "name": "Db2DataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremDb2LinkedService",
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

Zie voor meer informatie [IBM DB2-connector](#data-factory-onprem-db2-connector.md#dataset-properties) artikel.

### <a name="relational-source-in-copy-activity"></a>Relationele gegevensbron in de kopieerbewerking
Als u gegevens uit IBM DB2 kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en opgeven na eigenschappen in de **bron** sectie:


| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `"query": "select * from "MySchema"."MyTable""`. |Nee (als **tableName** van **gegevensset** is opgegeven) |

#### <a name="example"></a>Voorbeeld
```json
{
    "name": "CopyDb2ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"Orders\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "Db2DataSet"
            }],
            "outputs": [{
                "name": "AzureBlobDb2DataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "Db2ToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```
Zie voor meer informatie [IBM DB2-connector](#data-factory-onprem-db2-connector.md#copy-activity-properties) artikel.

## <a name="mysql"></a>MySQL

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een MySQL, stelt u de **type** van de gekoppelde service naar **OnPremisesMySql**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| server |Naam van de MySQL-server. |Ja |
| database |Naam van de MySQL-database. |Ja |
| Schema |De naam van het schema in de database. |Nee |
| authenticationType |Het soort verificatie die verbinding maken met de MySQL-database wordt gebruikt. Mogelijke waarden zijn: `Basic`. |Ja |
| gebruikersnaam |Geef de naam van de gebruiker verbinding maken met de MySQL-database. |Ja |
| wachtwoord |Geef het wachtwoord voor het gebruikersaccount dat u hebt opgegeven. |Ja |
| gatewayName |Naam van de gateway die voor de Data Factory-service gebruiken moet voor verbinding met de lokale MySQL-database. |Ja |

#### <a name="example"></a>Voorbeeld

```json
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

Zie voor meer informatie [MySQL connector](data-factory-onprem-mysql-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Instellen als u een MySQL-gegevensset definieert, de **type** van de gegevensset **RelationalTable**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in de MySQL-Database-instantie waarnaar de gekoppelde service verwijst. |Nee (als **query** van **RelationalSource** is opgegeven) |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "MySqlDataSet",
    "properties": {
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
Zie voor meer informatie [MySQL connector](data-factory-onprem-mysql-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationele gegevensbron in de kopieerbewerking
Als u gegevens uit een MySQL-database kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en opgeven na eigenschappen in de **bron** sectie:


| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `select * from MyTable`. |Nee (als **tableName** van **gegevensset** is opgegeven) |


#### <a name="example"></a>Voorbeeld
```json
{
    "name": "CopyMySqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
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
            "inputs": [{
                "name": "MySqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobMySqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MySqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Zie voor meer informatie [MySQL connector](data-factory-onprem-mysql-connector.md#copy-activity-properties) artikel. 

## <a name="oracle"></a>Oracle 

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Oracle, stelt u de **type** van de gekoppelde service naar **OnPremisesOracle**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| driverType | Geef op welke stuurprogramma moet worden gebruikt om gegevens te kopiëren van/naar Oracle-Database. Toegestane waarden zijn **Microsoft** of **ODP** (standaard). Zie [ondersteunde versie en installatieopties](#supported-versions-and-installation) gedeelte stuurprogrammagegevens. | Nee |
| connectionString | Geef informatie op die nodig zijn voor het verbinding maken met de Oracle-Database-exemplaar voor de eigenschap connectionString. | Ja |
| gatewayName | Naam van de gateway die die wordt gebruikt voor verbinding met de lokale Oracle-server |Ja |

#### <a name="example"></a>Voorbeeld
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Zie voor meer informatie [Oracle connector](data-factory-onprem-oracle-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Om te definiëren van een gegevensset Oracle, stel de **type** van de gegevensset **OracleTable**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in de Oracle-Database waarnaar de gekoppelde service verwijst. |Nee (als **oracleReaderQuery** van **OracleSource** is opgegeven) |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2016-02-27T12:00:00",
            "frequency": "Hour"
        },
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
Zie voor meer informatie [Oracle connector](data-factory-onprem-oracle-connector.md#dataset-properties) artikel.

### <a name="oracle-source-in-copy-activity"></a>Oracle-bron in de kopieerbewerking
Als u gegevens uit een Oracle-database kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **OracleSource**, en opgeven na eigenschappen in de **bron** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| oracleReaderQuery |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `select * from MyTable` <br/><br/>Als niet wordt opgegeven, de SQL-instructie die is uitgevoerd:`select * from MyTable` |Nee (als **tableName** van **gegevensset** is opgegeven) |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " OracleInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "OracleSource",
                    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
            "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Zie voor meer informatie [Oracle connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) artikel.

### <a name="oracle-sink-in-copy-activity"></a>Oracle-Sink in de kopieerbewerking
Als u gegevens AM Oracle-database kopiëren wilt, stelt u de **type sink** van de kopieeractiviteit naar **OracleSink**, en opgeven na eigenschappen in de **sink** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| writeBatchTimeout |Wachttijd voor de batch-insert-bewerking te voltooien voordat er een optreedt time-out. |TimeSpan<br/><br/> Voorbeeld: 00:30:00 (30 minuten). |Nee |
| writeBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt. |Geheel getal (aantal rijen) |Nee (standaard: 100) |
| sqlWriterCleanupScript |Geef een query voor de Kopieeractiviteit uitvoeren zodat de gegevens van een bepaald segment wordt opgeschoond. |Een query-instructie. |Nee |
| sliceIdentifierColumnName |Geef de naam van de kolom voor de Kopieeractiviteit te vullen met de segment-ID automatisch gegenereerd en die wordt gebruikt voor het opschonen van de gegevens van een bepaald segment wanneer opnieuw uitgevoerd. |De naam van de kolom van een kolom met gegevenstype binary(32). |Nee |

#### <a name="example"></a>Voorbeeld
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-05T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "OracleOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource"
                },
                "sink": {
                    "type": "OracleSink"
                }
            },
            "scheduler": {
                "frequency": "Day",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
Zie voor meer informatie [Oracle connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) artikel.

## <a name="postgresql"></a>PostgreSQL

### <a name="linked-service"></a>Gekoppelde service
Voor het definiëren van een PostgreSQL gekoppelde service, stelt u de **type** van de gekoppelde service naar **OnPremisesPostgreSql**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| server |De naam van de PostgreSQL-server. |Ja |
| database |De naam van de PostgreSQL-database. |Ja |
| Schema |De naam van het schema in de database. Naam van het schema is hoofdlettergevoelig. |Nee |
| authenticationType |Het soort verificatie die wordt gebruikt voor verbinding met de PostgreSQL-database. Mogelijke waarden zijn: anoniem, basis en Windows. |Ja |
| gebruikersnaam |Geef de gebruikersnaam als u basisverificatie of Windows-verificatie gebruikt. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |
| gatewayName |Naam van de gateway die voor de Data Factory-service gebruiken moet voor verbinding met de lokale PostgreSQL-database. |Ja |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
Zie voor meer informatie [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Om te definiëren een PostgreSQL-gegevensset, stel de **type** van de gegevensset **RelationalTable**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in de PostgreSQL-Database-instantie waarnaar de gekoppelde service verwijst. De tabelnaam is hoofdlettergevoelig. |Nee (als **query** van **RelationalSource** is opgegeven) |

#### <a name="example"></a>Voorbeeld
```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
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
Zie voor meer informatie [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#dataset-properties) artikel.

### <a name="relational-source-in-copy-activity"></a>Relationele gegevensbron in de kopieerbewerking
Als u gegevens uit een PostgreSQL-database kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en opgeven na eigenschappen in de **bron** sectie:


| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: "query": "Selecteer * uit \"MySchema\".\" MyTable\"'. |Nee (als **tableName** van **gegevensset** is opgegeven) |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from \"public\".\"usstates\""
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "PostgreSqlDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobPostgreSqlDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "PostgreSqlToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Zie voor meer informatie [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#copy-activity-properties) artikel.

## <a name="sap-business-warehouse"></a>SAP Business Warehouse


### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een SAP Business Warehouse (BW), stelt u de **type** van de gekoppelde service naar **SapBw**, en opgeven na eigenschappen in de **typeProperties** sectie:  

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
server | Naam van de server waarop het exemplaar SAP BW zich bevindt. | tekenreeks | Ja
systemNumber | Systeemnummer van het SAP BW-systeem. | Twee cijfers decimaal getal weergegeven als een tekenreeks. | Ja
clientId | Client-ID van de client in het systeem SAP-W. | Drie cijfers decimaal getal weergegeven als een tekenreeks. | Ja
gebruikersnaam | Naam van de gebruiker die toegang tot de SAP-server heeft | tekenreeks | Ja
wachtwoord | Wachtwoord voor de gebruiker. | tekenreeks | Ja
gatewayName | Naam van de gateway die voor de Data Factory-service gebruiken moet voor verbinding met de lokale SAP BW-exemplaar. | tekenreeks | Ja
encryptedCredential | De versleutelde referentie-tekenreeks. | tekenreeks | Nee

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SapBwLinkedService",
    "properties": {
        "type": "SapBw",
        "typeProperties": {
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

Zie voor meer informatie [SAP Business Warehouse-connector](data-factory-sap-business-warehouse-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Instellen als u een gegevensset SAP BW definieert, de **type** van de gegevensset **RelationalTable**. Er zijn geen type-specifieke eigenschappen voor de gegevensset SAP BW van het type ondersteund **RelationalTable**.  

#### <a name="example"></a>Voorbeeld

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
Zie voor meer informatie [SAP Business Warehouse-connector](data-factory-sap-business-warehouse-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationele gegevensbron in de kopieerbewerking
Als u gegevens uit een SAP Business Warehouse kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en opgeven na eigenschappen in de **bron** sectie:


| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query | Hiermee geeft u de MDX-query om te lezen van gegevens uit de SAP BW-exemplaar. | MDX-query. | Ja |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
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
            "inputs": [{
                "name": "SapBwDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapBwToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

Zie voor meer informatie [SAP Business Warehouse-connector](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) artikel. 

## <a name="sap-hana"></a>SAP HANA

### <a name="linked-service"></a>Gekoppelde service
Voor het definiëren van een SAP HANA gekoppelde service, stelt u de **type** van de gekoppelde service naar **SapHana**, en opgeven na eigenschappen in de **typeProperties** sectie:  

Eigenschap | Beschrijving | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
server | Naam van de server waarop het exemplaar SAP HANA zich bevindt. Als de server een aangepaste poort gebruikt is, geeft u `server:port`. | tekenreeks | Ja
authenticationType | Het type verificatie. | De tekenreeks. 'Basic' of 'Windows' | Ja 
gebruikersnaam | Naam van de gebruiker die toegang tot de SAP-server heeft | tekenreeks | Ja
wachtwoord | Wachtwoord voor de gebruiker. | tekenreeks | Ja
gatewayName | Naam van de gateway die voor de Data Factory-service gebruiken moet voor verbinding met de lokale SAP HANA-exemplaar. | tekenreeks | Ja
encryptedCredential | De versleutelde referentie-tekenreeks. | tekenreeks | Nee

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SapHanaLinkedService",
    "properties": {
        "type": "SapHana",
        "typeProperties": {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}

```
Zie voor meer informatie [SAP HANA-connector](data-factory-sap-hana-connector.md#linked-service-properties) artikel.
 
### <a name="dataset"></a>Gegevensset
Om te definiëren een SAP HANA-gegevensset, stel de **type** van de gegevensset **RelationalTable**. Er zijn geen type-specifieke eigenschappen voor de gegevensset SAP HANA van het type ondersteund **RelationalTable**. 

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SapHanaDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapHanaLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
Zie voor meer informatie [SAP HANA-connector](data-factory-sap-hana-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationele gegevensbron in de kopieerbewerking
Als u gegevens uit een SAP HANA-gegevensarchief kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en opgeven na eigenschappen in de **bron** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query | Hiermee geeft u de SQL-query voor het lezen van gegevens uit de SAP HANA-exemplaar. | SQL-query. | Ja |


#### <a name="example"></a>Voorbeeld


```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "<SQL Query for HANA>"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "SapHanaDataset"
            }],
            "outputs": [{
                "name": "AzureBlobDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SapHanaToBlob"
        }],
        "start": "2017-03-01T18:00:00",
        "end": "2017-03-01T19:00:00"
    }
}
```

Zie voor meer informatie [SAP HANA-connector](data-factory-sap-hana-connector.md#copy-activity-properties) artikel.


## <a name="sql-server"></a>SQL Server

### <a name="linked-service"></a>Gekoppelde service
Maken van een gekoppelde service van het type **OnPremisesSqlServer** een on-premises SQL Server database koppelen aan een gegevensfactory. De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor lokale SQL Server gekoppelde service.

De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor de service SQL Server gekoppeld.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OnPremisesSqlServer**. |Ja |
| connectionString |ConnectionString informatie die nodig zijn voor de verbinding met de lokale SQL Server-database met behulp van de SQL-verificatie of de Windows-verificatie opgeven. |Ja |
| gatewayName |Naam van de gateway die voor de Data Factory-service gebruiken moet voor verbinding met het lokale SQL Server-database. |Ja |
| gebruikersnaam |Geef de gebruikersnaam als u Windows-verificatie gebruikt. Voorbeeld: **domainname\\gebruikersnaam**. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |

U kunt versleutelen referenties met behulp van de **nieuw AzureRmDataFactoryEncryptValue** cmdlet en in de verbindingsreeks gebruiken, zoals wordt weergegeven in het volgende voorbeeld (**EncryptedCredential** eigenschap):  

```json
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a>Voorbeeld: JSON voor het gebruik van SQL-verificatie

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a>Voorbeeld: JSON voor het gebruik van Windows-verificatie

Als de gebruikersnaam en wachtwoord zijn opgegeven, worden deze door gateway imiteren van de opgegeven gebruikersaccount verbinding maken met de lokale SQL Server-database gebruikt. Anders gateway maakt verbinding met de SQL-Server rechtstreeks met de beveiligingscontext van Gateway (het starten van de account).

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Zie voor meer informatie [SQL Server-connector](data-factory-sqlserver-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Instellen als u een SQL Server-gegevensset definieert, de **type** van de gegevensset **SqlServerTable**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel of weergave in de SQL Server-Database-instantie waarnaar de gekoppelde service verwijst. |Ja |

#### <a name="example"></a>Voorbeeld
```json
{
    "name": "SqlServerInput",
    "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlServerLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
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

Zie voor meer informatie [SQL Server-connector](data-factory-sqlserver-connector.md#dataset-properties) artikel. 

### <a name="sql-source-in-copy-activity"></a>SQL-bron in de kopieerbewerking
Als u gegevens uit een SQL Server-database kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **SqlSource**, en opgeven na eigenschappen in de **bron** sectie:


| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| sqlReaderQuery |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `select * from MyTable`. Kan naar meerdere tabellen uit de database waarnaar wordt verwezen door de invoergegevensset verwijzen. Als niet wordt opgegeven, de SQL-instructie die is uitgevoerd: Selecteer een van de MyTable. |Nee |
| sqlReaderStoredProcedureName |Naam van de opgeslagen procedure die gegevens uit de brontabel leest. |Naam van de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. |Nee |

Als de **sqlReaderQuery** is opgegeven voor de SqlSource met de kopieerbewerking wordt deze query uitgevoerd op basis van de bron van de SQL Server-Database om de gegevens te verkrijgen.

U kunt ook een opgeslagen procedure opgeven door te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters nodig heeft).

Als u sqlReaderQuery of sqlReaderStoredProcedureName niet opgeeft, worden de kolommen die zijn gedefinieerd in de sectie structuur gebruikt voor het bouwen van een select-query uitvoeren op de SQL Server-Database. Als de definitie van de gegevensset heeft niet de structuur, worden alle kolommen uit de tabel geselecteerd.

> [!NOTE]
> Als u werkt met **sqlReaderStoredProcedureName**, moet u toch een waarde opgeven voor de **tableName** eigenschap in de JSON van de gegevensset. Er zijn geen controles uitgevoerd voor deze tabel al.


#### <a name="example"></a>Voorbeeld
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "SqlServertoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": " SqlServerInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

In dit voorbeeld **sqlReaderQuery** is opgegeven voor de SqlSource. De Kopieeractiviteit wordt deze query uitgevoerd op basis van de bron van de SQL Server-Database om de gegevens te verkrijgen. U kunt ook een opgeslagen procedure opgeven door te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters nodig heeft). De sqlReaderQuery kunt verwijzen naar meerdere tabellen in de database waarnaar wordt verwezen door de invoergegevensset. Het is niet beperkt tot alleen de tabel als de dataset tableName typeProperty instellen.

Als u geen sqlReaderQuery of sqlReaderStoredProcedureName opgeeft, worden de kolommen die zijn gedefinieerd in de sectie structuur gebruikt voor het bouwen van een select-query uitvoeren op de SQL Server-Database. Als de definitie van de gegevensset heeft niet de structuur, worden alle kolommen uit de tabel geselecteerd.

Zie voor meer informatie [SQL Server-connector](data-factory-sqlserver-connector.md#copy-activity-properties) artikel. 

### <a name="sql-sink-in-copy-activity"></a>SQL-Sink in de kopieerbewerking
Als u gegevens naar een SQL Server-database kopiëren wilt, stelt u de **type sink** van de kopieeractiviteit naar **SqlSink**, en opgeven na eigenschappen in de **sink** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| writeBatchTimeout |Wachttijd voor de batch-insert-bewerking te voltooien voordat er een optreedt time-out. |TimeSpan<br/><br/> Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |
| writeBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt. |Geheel getal (aantal rijen) |Nee (standaard: 10000) |
| sqlWriterCleanupScript |Query voor de Kopieeractiviteit uitvoeren zodat de gegevens van een bepaald segment wordt opgeschoond opgeven. Zie voor meer informatie [herhaalbaarheid](#repeatability-during-copy) sectie. |Een query-instructie. |Nee |
| sliceIdentifierColumnName |Geef de naam van de kolom voor de Kopieeractiviteit te vullen met de segment-ID automatisch gegenereerd en die wordt gebruikt voor het opschonen van de gegevens van een bepaald segment wanneer opnieuw uitgevoerd. Zie voor meer informatie [herhaalbaarheid](#repeatability-during-copy) sectie. |De naam van de kolom van een kolom met gegevenstype binary(32). |Nee |
| sqlWriterStoredProcedureName |Naam van de opgeslagen procedure die upserts (updates/INSERT) gegevens in de doeltabel. |Naam van de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de parameters van opgeslagen procedure. |Nee |
| sqlWriterTableType |Type tabelnaam moet worden gebruikt in de opgeslagen procedure opgeven. Kopieeractiviteit maakt u de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Code van de opgeslagen procedure kan de gegevens wordt gekopieerd met de bestaande gegevens vervolgens samenvoegen. |Een typenaam van de tabel. |Nee |

#### <a name="example"></a>Voorbeeld
De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor gebruik van deze invoer- en uitvoergegevenssets en elk uur is gepland. In de pijplijn-JSON-definitie de **bron** type is ingesteld op **BlobSource** en **sink** type is ingesteld op **SqlSink**.

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": " SqlServerOutput "
            }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "blobColumnSeparators": ","
                },
                "sink": {
                    "type": "SqlSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Zie voor meer informatie [SQL Server-connector](data-factory-sqlserver-connector.md#copy-activity-properties) artikel. 

## <a name="sybase"></a>Sybase

### <a name="linked-service"></a>Gekoppelde service
Voor het definiëren van een Sybase gekoppelde service, stelt u de **type** van de gekoppelde service naar **OnPremisesSybase**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| server |De naam van de Sybase-server. |Ja |
| database |Naam van de Sybase-database. |Ja |
| Schema |De naam van het schema in de database. |Nee |
| authenticationType |Het soort verificatie die verbinding maken met de Sybase-database wordt gebruikt. Mogelijke waarden zijn: anoniem, basis en Windows. |Ja |
| gebruikersnaam |Geef de gebruikersnaam als u basisverificatie of Windows-verificatie gebruikt. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |
| gatewayName |Naam van de gateway die voor de Data Factory-service gebruiken moet voor verbinding met de lokale Sybase-database. |Ja |

#### <a name="example"></a>Voorbeeld
```json
{
    "name": "OnPremSybaseLinkedService",
    "properties": {
        "type": "OnPremisesSybase",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```

Zie voor meer informatie [Sybase connector](data-factory-onprem-sybase-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Instellen als u een gegevensset met de Sybase definieert, de **type** van de gegevensset **RelationalTable**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in de Sybase-Database-instantie waarnaar de gekoppelde service verwijst. |Nee (als **query** van **RelationalSource** is opgegeven) |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SybaseDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremSybaseLinkedService",
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

Zie voor meer informatie [Sybase connector](data-factory-onprem-sybase-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationele gegevensbron in de kopieerbewerking
Als u gegevens uit een Sybase-database kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en opgeven na eigenschappen in de **bron** sectie:


| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `select * from MyTable`. |Nee (als **tableName** van **gegevensset** is opgegeven) |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "CopySybaseToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "select * from DBA.Orders"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "inputs": [{
                "name": "SybaseDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobSybaseDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "SybaseToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Zie voor meer informatie [Sybase connector](data-factory-onprem-sybase-connector.md#copy-activity-properties) artikel.

## <a name="teradata"></a>Teradata

### <a name="linked-service"></a>Gekoppelde service
Voor het definiëren van een Teradata gekoppelde service, stelt u de **type** van de gekoppelde service naar **OnPremisesTeradata**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| server |De naam van de Teradata-server. |Ja |
| authenticationType |Het soort verificatie die verbinding maken met de Teradata-database wordt gebruikt. Mogelijke waarden zijn: anoniem, basis en Windows. |Ja |
| gebruikersnaam |Geef de gebruikersnaam als u basisverificatie of Windows-verificatie gebruikt. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |
| gatewayName |Naam van de gateway die voor de Data Factory-service gebruiken moet voor verbinding met de lokale Teradata-database. |Ja |

#### <a name="example"></a>Voorbeeld
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

Zie voor meer informatie [Teradata connector](data-factory-onprem-teradata-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Om te definiëren een Teradata-Blob-gegevensset, stel de **type** van de gegevensset **RelationalTable**. Er zijn momenteel geen type-eigenschappen voor de gegevensset Teradata ondersteund. 

#### <a name="example"></a>Voorbeeld
```json
{
    "name": "TeradataDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremTeradataLinkedService",
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

Zie voor meer informatie [Teradata connector](data-factory-onprem-teradata-connector.md#dataset-properties) artikel.

### <a name="relational-source-in-copy-activity"></a>Relationele gegevensbron in de kopieerbewerking
Als u gegevens uit een Teradata-database kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en opgeven na eigenschappen in de **bron** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `select * from MyTable`. |Ja |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "CopyTeradataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
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
            "inputs": [{
                "name": "TeradataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobTeradataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "TeradataToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "isPaused": false
    }
}
```

Zie voor meer informatie [Teradata connector](data-factory-onprem-teradata-connector.md#copy-activity-properties) artikel.

## <a name="cassandra"></a>Cassandra


### <a name="linked-service"></a>Gekoppelde service
Instellen voor het definiëren van een service gekoppelde Cassandra de **type** van de gekoppelde service naar **OnPremisesCassandra**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| host |Een of meer IP-adressen of hostnamen van Cassandra servers.<br/><br/>Geef een door komma's gescheiden lijst met IP-adressen of hostnamen gelijktijdig verbinding maken met alle servers. |Ja |
| poort |De TCP-poort die de Cassandra-server gebruikt om te luisteren naar verbindingen van clients. |Nee, de standaardwaarde: 9042 |
| authenticationType |Basic- of anonieme |Ja |
| gebruikersnaam |Geef de gebruikersnaam voor het gebruikersaccount. |Ja, als authenticationType is ingesteld op Basic. |
| wachtwoord |Wachtwoord voor het gebruikersaccount opgeven. |Ja, als authenticationType is ingesteld op Basic. |
| gatewayName |De naam van de gateway die wordt gebruikt voor verbinding met de lokale Cassandra-database. |Ja |
| encryptedCredential |De referentie is versleuteld met de gateway. |Nee |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "CassandraLinkedService",
    "properties": {
        "type": "OnPremisesCassandra",
        "typeProperties": {
            "authenticationType": "Basic",
            "host": "<cassandra server name or IP address>",
            "port": 9042,
            "username": "user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Zie voor meer informatie [Cassandra connector](data-factory-onprem-cassandra-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Instellen als u een gegevensset Cassandra definieert, de **type** van de gegevensset **CassandraTable**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| met keyspace |Naam van de keyspace of het schema in Cassandra-database. |Ja (als **query** voor **CassandraSource** is niet gedefinieerd). |
| tableName |Naam van de tabel in Cassandra-database. |Ja (als **query** voor **CassandraSource** is niet gedefinieerd). |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "CassandraInput",
    "properties": {
        "linkedServiceName": "CassandraLinkedService",
        "type": "CassandraTable",
        "typeProperties": {
            "tableName": "mytable",
            "keySpace": "<key space>"
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

Zie voor meer informatie [Cassandra connector](data-factory-onprem-cassandra-connector.md#dataset-properties) artikel. 

### <a name="cassandra-source-in-copy-activity"></a>Cassandra-bron in de kopieerbewerking
Als u gegevens uit Cassandra kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **CassandraSource**, en opgeven na eigenschappen in de **bron** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |Gebruik de aangepaste query om gegevens te lezen. |SQL-92 query of CQL query. Zie [CQL verwijzing](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Wanneer u SQL-query gebruikt, geef **keyspace name.table naam** om weer te geven van de tabel die u wilt zoeken. |Nee (als tableName en keyspace op gegevensset zijn gedefinieerd). |
| consistencyLevel |Het consistentieniveau van de geeft het aantal replica's moeten reageren op een leesaanvraag voor het retourneren van gegevens naar de clienttoepassing. Cassandra controleert het opgegeven aantal replica's voor gegevens om te voldoen aan de leesaanvraag. |EEN, TWEE, DRIE, QUORUM, ALL, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE. Zie [gegevensconsistentie configureren](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) voor meer informatie. |Nee. Standaardwaarde is een. |

#### <a name="example"></a>Voorbeeld
  
```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "CassandraToAzureBlob",
            "description": "Copy from Cassandra to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "CassandraInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "CassandraSource",
                    "query": "select id, firstname, lastname from mykeyspace.mytable"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Zie voor meer informatie [Cassandra connector](data-factory-onprem-cassandra-connector.md#copy-activity-properties) artikel.

## <a name="mongodb"></a>MongoDB

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een MongoDB, stelt u de **type** van de gekoppelde service naar **OnPremisesMongoDB**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| server |IP-adres of de hostnaam de naam van de MongoDB-server. |Ja |
| poort |TCP-poort die de MongoDB-server gebruikt om te luisteren naar verbindingen van clients. |Optionele standaardwaarde: 27017 |
| authenticationType |Basic of anonieme. |Ja |
| gebruikersnaam |Het gebruikersaccount voor toegang tot MongoDB. |Ja (als u basisverificatie wordt gebruikt). |
| wachtwoord |Wachtwoord voor de gebruiker. |Ja (als u basisverificatie wordt gebruikt). |
| authSource |De naam van de MongoDB-database die u wilt gebruiken om te controleren van uw referenties voor verificatie. |Optioneel (als basisverificatie wordt gebruikt). Standaard: maakt gebruik van het beheerdersaccount en de database die is opgegeven met behulp van de eigenschap databaseName. |
| DatabaseName |De naam van de MongoDB-database die u wilt openen. |Ja |
| gatewayName |De naam van de gateway die toegang heeft tot het gegevensarchief. |Ja |
| encryptedCredential |Referentie versleuteld door de gateway. |Optioneel |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "OnPremisesMongoDbLinkedService",
    "properties": {
        "type": "OnPremisesMongoDb",
        "typeProperties": {
            "authenticationType": "<Basic or Anonymous>",
            "server": "< The IP address or host name of the MongoDB server >",
            "port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>",
            "username": "<username>",
            "password": "<password>",
            "authSource": "< The database that you want to use to check your credentials for authentication. >",
            "databaseName": "<database name>",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Zie voor meer informatie [MongoDB-connector artikel](data-factory-on-premises-mongodb-connector.md#linked-service-properties)

### <a name="dataset"></a>Gegevensset
Om te definiëren van een gegevensset MongoDB, stel de **type** van de gegevensset **MongoDbCollection**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| CollectionName |De naam van de verzameling in de MongoDB-database. |Ja |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "MongoDbInputDataset",
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

Zie voor meer informatie [MongoDB-connector artikel](data-factory-on-premises-mongodb-connector.md#dataset-properties)

#### <a name="mongodb-source-in-copy-activity"></a>MongoDB-bron in de kopieerbewerking
Als u gegevens uit MongoDB kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **MongoDbSource**, en opgeven na eigenschappen in de **bron** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |Gebruik de aangepaste query om gegevens te lezen. |SQL-92 queryreeks. Bijvoorbeeld: `select * from MyTable`. |Nee (als **collectionName** van **gegevensset** is opgegeven) |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "CopyMongoDBToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "MongoDbSource",
                    "query": "select * from MyTable"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "MongoDbInputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "MongoDBToAzureBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Zie voor meer informatie [MongoDB-connector artikel](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)

## <a name="amazon-s3"></a>Amazon S3


### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Amazon S3, stelt u de **type** van de gekoppelde service naar **AwsAccessKey**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| accessKeyID |ID van de geheime toegangssleutel. |tekenreeks |Ja |
| secretAccessKey |De geheime toegangssleutel zelf. |Versleutelde geheime tekenreeks |Ja |

#### <a name="example"></a>Voorbeeld
```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

Zie voor meer informatie [Amazon S3 connector artikel](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties).

### <a name="dataset"></a>Gegevensset
Om te definiëren van een gegevensset Amazon S3, stel de **type** van de gegevensset **AmazonS3**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| bucketName |De naam van de S3-bucket. |Tekenreeks |Ja |
| sleutel |De sleutel van het object S3. |Tekenreeks |Nee |
| voorvoegsel |Voorvoegsel voor de sleutel S3-object. Objecten waarvan sleutels met dit voorvoegsel beginnen worden geselecteerd. Geldt alleen als sleutel is leeg. |Tekenreeks |Nee |
| versie |De versie van S3-object als S3 versiebeheer is ingeschakeld. |Tekenreeks |Nee |
| Indeling | De volgende indelingstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [parketvloeren indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset. |Nee | |
| Compressie | Geef het type en de compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. De ondersteunde niveaus: **optimale** en **snelst**. Zie voor meer informatie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee | |


> [!NOTE]
> bucketName + sleutel geeft de locatie van het object S3 waarbij bucket de hoofdcontainer voor S3-objecten en -sleutel is het volledige pad naar S3-object.

#### <a name="example-sample-dataset-with-prefix"></a>Voorbeeld: Voorbeeldgegevensset met voorvoegsel

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "<S3 bucket name>",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
#### <a name="example-sample-data-set-with-version"></a>Voorbeeld: Voorbeeldgegevensset (met versie)

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "<S3 bucket name>",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

#### <a name="example-dynamic-paths-for-s3"></a>Voorbeeld: Dynamische paden voor S3
In het voorbeeld gebruiken we vaste waarden voor sleutel en bucketName eigenschappen in de Amazon S3-gegevensset.

```json
"key": "testFolder/test.orc",
"bucketName": "<S3 bucket name>",
```

U kunt Data Factory-sleutel en bucketName dynamisch tijdens runtime berekenen met behulp van systeemvariabelen zoals SliceStart hebben.

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

U kunt hetzelfde zijn voor de eigenschap van het voorvoegsel van een gegevensset Amazon S3 doen. Zie [Data Factory-functies en systeemvariabelen](data-factory-functions-variables.md) voor een lijst van ondersteunde functies en variabelen.

Zie voor meer informatie [Amazon S3 connector artikel](data-factory-amazon-simple-storage-service-connector.md#dataset-properties).

### <a name="file-system-source-in-copy-activity"></a>Systeem bronnen in de kopieerbewerking
Als u gegevens vanaf Amazon S3 kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **FileSystemSource**, en opgeven na eigenschappen in de **bron** sectie:


| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| Recursieve |Hiermee geeft u op of voor recursief S3 lijst objecten in de map. |waar/onwaar |Nee |


#### <a name="example"></a>Voorbeeld


```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource",
                    "recursive": true
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "AmazonS3InputDataset"
            }],
            "outputs": [{
                "name": "AzureBlobOutputDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "AmazonS3ToBlob"
        }],
        "start": "2016-08-08T18:00:00",
        "end": "2016-08-08T19:00:00"
    }
}
```

Zie voor meer informatie [Amazon S3 connector artikel](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties).

## <a name="file-system"></a>Bestandssysteem


### <a name="linked-service"></a>Gekoppelde service
U kunt een on-premises bestandssysteem koppelen aan een Azure-gegevensfactory met de **On-Premises bestandsserver** gekoppelde service. De volgende tabel bevat beschrijvingen van JSON-elementen die specifiek voor de bestandsserver On-Premises gekoppelde service zijn.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |Zorg ervoor dat de type-eigenschap is ingesteld op **OnPremisesFileServer**. |Ja |
| host |Hiermee geeft u het pad naar de hoofdmap van de map die u wilt kopiëren. Gebruik het escape-teken ' \ ' voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppelde service en gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden. |Ja |
| gebruikers-id |Geef de ID van de gebruiker die toegang tot de server heeft. |Nee (als u ervoor encryptedCredential kiest) |
| wachtwoord |Geef het wachtwoord voor de gebruiker (gebruikersnaam). |Nee (als u encryptedCredential kiezen |
| encryptedCredential |Geef de versleutelde referenties die u ophalen kunt door de cmdlet New-AzureRmDataFactoryEncryptValue. |Nee (als u ervoor kiest om op te geven van de gebruikersnaam en wachtwoord in tekst zonder opmaak) |
| gatewayName |Hiermee geeft u de naam van de gateway die voor Data Factory verbinding maken met de lokale bestandsserver wordt gebruikt. |Ja |

#### <a name="sample-folder-path-definitions"></a>Voorbeeld map pad definities 
| Scenario | In de definitie van de gekoppelde service host | folderPath in de definitie van gegevensset |
| --- | --- | --- |
| Lokale map op de Data Management Gateway-apparaat: <br/><br/>Voorbeelden: D:\\ \* of D:\folder\subfolder\\* |D:\\ \\ (voor Data Management Gateway 2.0 en hoger) <br/><br/> localhost (voor oudere versies dan Data Management Gateway 2.0) |. \\ \\ of de map\\\\submap (voor Data Management Gateway 2.0 en hoger) <br/><br/>D:\\ \\ of D:\\\\map\\\\submap (voor gatewayversie lager dan 2.0) |
| Externe gedeelde map: <br/><br/>Voorbeelden: \\ \\MijnServer\\delen\\ \* of \\ \\MijnServer\\delen\\map\\submap\\* |\\\\\\\\MijnServer\\\\delen |. \\ \\ of de map\\\\submap |


#### <a name="example-using-username-and-password-in-plain-text"></a>Voorbeeld: Met behulp van gebruikersnaam en wachtwoord in tekst zonder opmaak

```json
{
    "Name": "OnPremisesFileServerLinkedService",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "\\\\Contosogame-Asia",
            "userid": "Admin",
            "password": "123456",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-encryptedcredential"></a>Voorbeeld: Encryptedcredential gebruiken

```json
{
    "Name": " OnPremisesFileServerLinkedService ",
    "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
            "host": "D:\\",
            "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Zie voor meer informatie [bestandssysteem connector artikel](data-factory-onprem-file-system-connector.md#linked-service-properties).

### <a name="dataset"></a>Gegevensset
Instellen als u een gegevensset bestandssysteem definieert, de **type** van de gegevensset **FileShare**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| folderPath |Hiermee geeft u het subpad naar de map. Gebruik het escape-teken ' \' voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppelde service en gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** map hebben paden op basis van het segment beginnen of eindigen-datums en tijden. |Ja |
| fileName |Geef de naam van het bestand in de **folderPath** als u wilt dat de tabel om te verwijzen naar een bepaald bestand in de map. Als u geen waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Wanneer fileName niet voor een uitvoergegevensset opgegeven is, is de naam van het gegenereerde bestand in de volgende indeling: <br/><br/>`Data.<Guid>.txt`(Voorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) |Nee |
| fileFilter |Hiermee geeft u een filter moet worden gebruikt om een subset van de bestanden in het mappad in plaats van alle bestanden te selecteren. <br/><br/>Toegestane waarden zijn: `*` (meerdere tekens) en `?` (willekeurig teken).<br/><br/>Voorbeeld 1: 'fileFilter":" * .log '<br/>Voorbeeld 2: 'fileFilter': 2016 - 1-?. txt'<br/><br/>Houd er rekening mee dat fileFilter is van toepassing op een bestandsshare-invoergegevensset. |Nee |
| partitionedBy |U kunt partitionedBy gebruiken om op te geven van een dynamische folderPath/bestandsnaam voor timeseries gegevens. Een voorbeeld is folderPath geparametriseerde voor elk uur van gegevens. |Nee |
| Indeling | De volgende indelingstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [parketvloeren indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset. |Nee |
| Compressie | Geef het type en de compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**; en ondersteunde niveaus: **optimale** en **snelst**. Zie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

> [!NOTE]
> U kunt geen bestandsnaam en fileFilter gelijktijdig gebruiken.

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "OnpremisesFileSystemInput",
    "properties": {
        "type": " FileShare",
        "linkedServiceName": " OnPremisesFileServerLinkedService ",
        "typeProperties": {
            "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": [{
                "name": "Year",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                        "format": "yyyy"
                }
            }, {
                "name": "Month",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "MM"
                }
            }, {
                "name": "Day",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "dd"
                }
            }, {
                "name": "Hour",
                "value": {
                    "type": "DateTime",
                    "date": "SliceStart",
                    "format": "HH"
                }
            }]
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
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

Zie voor meer informatie [bestandssysteem connector artikel](data-factory-onprem-file-system-connector.md#dataset-properties).

### <a name="file-system-source-in-copy-activity"></a>Systeem bronnen in de kopieerbewerking
Als u gegevens uit bestandssysteem kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **FileSystemSource**, en opgeven na eigenschappen in de **bron** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| Recursieve |Hiermee wordt aangegeven of de gegevens recursief is gelezen uit de submappen of alleen uit de opgegeven map. |True, False (standaard) |Nee |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T19:00:00",
        "description": "Pipeline for copy activity",
        "activities": [{
            "name": "OnpremisesFileSystemtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "OnpremisesFileSystemInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
            "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```
Zie voor meer informatie [bestandssysteem connector artikel](data-factory-onprem-file-system-connector.md#copy-activity-properties).

### <a name="file-system-sink-in-copy-activity"></a>Bestandssysteem Sink in de kopieerbewerking
Als u gegevens naar File System kopiëren wilt, stelt u de **type sink** van de kopieeractiviteit naar **FileSystemSink**, en opgeven na eigenschappen in de **sink** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| copyBehavior |Definieert het gedrag kopiëren wanneer de bron BlobSource of het bestandssysteem. |**PreserveHierarchy:** behoudt de bestandshiërarchie in de doelmap. Dat wil zeggen, is het relatieve pad van het bronbestand voor de bronmap hetzelfde als het relatieve pad van het doel-bestand naar de doelmap.<br/><br/>**FlattenHierarchy:** alle bestanden uit de bronmap worden gemaakt in het eerste niveau van de doelmap. De doelbestanden worden gemaakt met een automatisch gegenereerde naam.<br/><br/>**MergeFiles:** alle bestanden uit de bronmap op één bestand worden samengevoegd. Als de naam/blob-naam van het bestand is opgegeven, is de samengevoegde bestandsnaam de opgegeven naam. Anders is het een automatisch gegenereerde naam. |Nee |
Automatische-

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2015-06-01T18:00:00",
        "end": "2015-06-01T20:00:00",
        "description": "pipeline for copy activity",
        "activities": [{
            "name": "AzureSQLtoOnPremisesFile",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [{
                "name": "AzureSQLInput"
            }],
            "outputs": [{
                "name": "OnpremisesFileSystemOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "SqlSource",
                    "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
                },
                "sink": {
                    "type": "FileSystemSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 3,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Zie voor meer informatie [bestandssysteem connector artikel](data-factory-onprem-file-system-connector.md#copy-activity-properties).

## <a name="ftp"></a>FTP

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een FTP-, stelt u de **type** van de gekoppelde service naar **FtpServer**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist | Standaard |
| --- | --- | --- | --- |
| host |Naam of IP-adres van de FTP-Server |Ja |&nbsp; |
| authenticationType |Verificatietype opgeven |Ja |Basic, anonieme |
| gebruikersnaam |Gebruiker die toegang tot de FTP-server heeft |Nee |&nbsp; |
| wachtwoord |Wachtwoord voor de gebruiker (username) |Nee |&nbsp; |
| encryptedCredential |Versleutelde referentie voor toegang tot de FTP-server |Nee |&nbsp; |
| gatewayName |Naam van de Data Management Gateway-gateway verbinding maken met een on-premises FTP-server |Nee |&nbsp; |
| poort |Poort waarop de FTP-server luistert |Nee |21 |
| enableSsl |Geef op of gebruik FTP via SSL/TLS-kanaal |Nee |waar |
| enableServerCertificateValidation |Geef op of validatie van het servercertificaat SSL inschakelen wanneer FTP via SSL/TLS-kanaal |Nee |waar |

#### <a name="example-using-anonymous-authentication"></a>Voorbeeld: Anonieme verificatie gebruikt

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
            "typeProperties": {
            "authenticationType": "Anonymous",
            "host": "myftpserver.com"
        }
    }
}
```

#### <a name="example-using-username-and-password-in-plain-text-for-basic-authentication"></a>Voorbeeld: Met behulp van gebruikersnaam en wachtwoord in tekst zonder opmaak voor basisverificatie

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
    }
}
```

#### <a name="example-using-port-enablessl-enableservercertificatevalidation"></a>Voorbeeld: Met behulp van poort, enableSsl, enableServerCertificateValidation

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",    
            "username": "Admin",
            "password": "123456",
            "port": "21",
            "enableSsl": true,
            "enableServerCertificateValidation": true
        }
    }
}
```

#### <a name="example-using-encryptedcredential-for-authentication-and-gateway"></a>Voorbeeld: EncryptedCredential gebruiken voor verificatie en gateway

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",
            "authenticationType": "Basic",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "gatewayName": "<onpremgateway>"
        }
      }
}
```

Zie voor meer informatie [FTP-connector](data-factory-ftp-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Instellen als u een FTP-gegevensset definieert, de **type** van de gegevensset **FileShare**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| folderPath |Subpad naar de map. Gebruik van escape-teken ' \ ' voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppelde service en gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** map hebben paden op basis van het segment beginnen of eindigen-datums en tijden. |Ja 
| fileName |Geef de naam van het bestand in de **folderPath** als u wilt dat de tabel om te verwijzen naar een bepaald bestand in de map. Als u geen waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Wanneer fileName niet voor een uitvoergegevensset opgegeven is, de naam van het gegenereerde bestand zou worden in de volgende indeling: <br/><br/>Gegevens. <Guid>.txt (voorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nee |
| fileFilter |Hiermee geeft u een filter moet worden gebruikt om een subset van de bestanden in het mappad in plaats van alle bestanden te selecteren.<br/><br/>Toegestane waarden zijn: `*` (meerdere tekens) en `?` (willekeurig teken).<br/><br/>Voorbeelden 1:`"fileFilter": "*.log"`<br/>Voorbeeld 2:`"fileFilter": 2016-1-?.txt"`<br/><br/> fileFilter geldt voor een invoergegevensset bestandsshare. Deze eigenschap wordt niet ondersteund met HDFS. |Nee |
| partitionedBy |partitionedBy kan worden gebruikt om op te geven van een dynamische folderPath, filename voor tijd reeksgegevens. Bijvoorbeeld, folderPath geparametriseerde voor elk uur van gegevens. |Nee |
| Indeling | De volgende indelingstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [parketvloeren indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset. |Nee |
| Compressie | Geef het type en de compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**; en ondersteunde niveaus: **optimale** en **snelst**. Zie voor meer informatie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |
| useBinaryTransfer |Geef binaire overdrachtsmodus of gebruiken. De waarde True voor binaire modus en ONWAAR ASCII. Standaardwaarde: True. Deze eigenschap kan alleen worden gebruikt wanneer de bijbehorende gekoppelde-servicetype is van het type: FtpServer. |Nee |

> [!NOTE]
> bestandsnaam en fileFilter worden niet gelijktijdig gebruikt.

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "FTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
            "folderPath": "<path to shared folder>",
            "fileName": "test.csv",
            "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Zie voor meer informatie [FTP-connector](data-factory-ftp-connector.md#dataset-properties) artikel.

### <a name="file-system-source-in-copy-activity"></a>Systeem bronnen in de kopieerbewerking
Als u gegevens uit een FTP-server kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **FileSystemSource**, en opgeven na eigenschappen in de **bron** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| Recursieve |Hiermee wordt aangegeven of de gegevens recursief is gelezen uit de submappen of alleen uit de opgegeven map. |True, False (standaard) |Nee |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "FTPToBlobCopy",
            "inputs": [{
                "name": "FtpFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-08-24T18:00:00",
        "end": "2016-08-24T19:00:00"
    }
}
```

Zie voor meer informatie [FTP-connector](data-factory-ftp-connector.md#copy-activity-properties) artikel.


## <a name="hdfs"></a>HDFS

### <a name="linked-service"></a>Gekoppelde service
Voor het definiëren van een HDFS gekoppelde service, stelt u de **type** van de gekoppelde service naar **Hdfs**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **Hdfs** |Ja |
| URL |URL naar de HDFS |Ja |
| authenticationType |Anoniem, of Windows. <br><br> Gebruik **Kerberos-verificatie** voor HDFS-connector raadpleegt u [in deze sectie](#use-kerberos-authentication-for-hdfs-connector) voor het instellen van uw on-premises omgeving dienovereenkomstig. |Ja |
| userName |Gebruikersnaam voor Windows-verificatie. |Ja (voor Windows-verificatie) |
| wachtwoord |Wachtwoord voor Windows-verificatie. |Ja (voor Windows-verificatie) |
| gatewayName |Naam van de gateway die voor de Data Factory-service gebruiken moet voor verbinding met het HDFS. |Ja |
| encryptedCredential |[Nieuwe AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) uitvoer van de referentie toegang. |Nee |

#### <a name="example-using-anonymous-authentication"></a>Voorbeeld: Anonieme verificatie gebruikt

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "userName": "hadoop",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-windows-authentication"></a>Voorbeeld: Met behulp van Windows-verificatie

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "authenticationType": "Windows",
            "userName": "Administrator",
            "password": "password",
            "url": "http://<machine>:50070/webhdfs/v1/",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Zie voor meer informatie [HDFS connector](#data-factory-hdfs-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Instellen als u een HDFS-gegevensset definieert, de **type** van de gegevensset **FileShare**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| folderPath |Pad naar de map. Voorbeeld:`myfolder`<br/><br/>Gebruik van escape-teken ' \ ' voor speciale tekens in de tekenreeks. Bijvoorbeeld: map opgeven voor folder\subfolder,\\\\submap en geef voor d:\samplefolder, d:\\\\Voorbeeldmap.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** map hebben paden op basis van het segment beginnen of eindigen-datums en tijden. |Ja |
| fileName |Geef de naam van het bestand in de **folderPath** als u wilt dat de tabel om te verwijzen naar een bepaald bestand in de map. Als u geen waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Wanneer fileName niet voor een uitvoergegevensset opgegeven is, de naam van het gegenereerde bestand zou worden in de volgende indeling: <br/><br/>Gegevens. <Guid>.txt (voorbeeld:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nee |
| partitionedBy |partitionedBy kan worden gebruikt om op te geven van een dynamische folderPath, filename voor tijd reeksgegevens. Voorbeeld: folderPath geparametriseerde voor elk uur van gegevens. |Nee |
| Indeling | De volgende indelingstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [parketvloeren indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset. |Nee |
| Compressie | Geef het type en de compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus: **optimale** en **snelst**. Zie voor meer informatie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

> [!NOTE]
> bestandsnaam en fileFilter worden niet gelijktijdig gebruikt.

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "InputDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "HDFSLinkedService",
        "typeProperties": {
            "folderPath": "DataTransfer/UnitTest/"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Zie voor meer informatie [HDFS connector](#data-factory-hdfs-connector.md#dataset-properties) artikel. 

### <a name="file-system-source-in-copy-activity"></a>Systeem bronnen in de kopieerbewerking
Als u gegevens uit HDFS kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **FileSystemSource**, en opgeven na eigenschappen in de **bron** sectie:

**FileSystemSource** ondersteunt de volgende eigenschappen:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| Recursieve |Hiermee wordt aangegeven of de gegevens recursief is gelezen uit de submappen of alleen uit de opgegeven map. |True, False (standaard) |Nee |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "HdfsToBlobCopy",
            "inputs": [{
                "name": "InputDataset"
            }],
            "outputs": [{
                "name": "OutputDataset"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
```

Zie voor meer informatie [HDFS connector](#data-factory-hdfs-connector.md#copy-activity-properties) artikel.

## <a name="sftp"></a>SFTP


### <a name="linked-service"></a>Gekoppelde service
Voor het definiëren van een SFTP gekoppelde service, stelt u de **type** van de gekoppelde service naar **Sftp**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- | --- |
| host | Naam of IP-adres van de SFTP-server. |Ja |
| poort |De poort waarop de SFTP-server luistert. De standaardwaarde is: 21 |Nee |
| authenticationType |Geef het verificatietype. Toegestane waarden: **Basic**, **SshPublicKey**. <br><br> Raadpleeg [Using basisverificatie](#using-basic-authentication) en [met behulp van SSH openbare-sleutelauthenticatie](#using-ssh-public-key-authentication) respectievelijk secties over meer eigenschappen en voorbeelden van JSON. |Ja |
| skipHostKeyValidation | Geef op of validatie host-sleutel overslaan. | Nee. De standaardwaarde: false |
| hostKeyFingerprint | Geef de vingerafdruk van de hostsleutel. | Ja als de `skipHostKeyValidation` is ingesteld op false.  |
| gatewayName |Naam van de Data Management Gateway verbinding maken met een on-premises SFTP-server. | Ja als u kopiëren van gegevens uit een on-premises SFTP-server. |
| encryptedCredential | Versleutelde referentie voor de toegang tot de SFTP-server. Automatisch wordt gegenereerd wanneer u basisverificatie (gebruikersnaam en wachtwoord) of SshPublicKey verificatie (gebruikersnaam + pad naar de persoonlijke sleutel of inhoud) in de wizard kopiëren of de ClickOnce-pop-dialoogvenster opgeven. | Nee. Alleen van toepassing wanneer het kopiëren van gegevens uit een on-premises SFTP-server. |

#### <a name="example-using-basic-authentication"></a>Voorbeeld: Met behulp van basisverificatie

Met basisverificatie, stelt `authenticationType` als `Basic`, en geef de volgende eigenschappen naast algemene die zijn geïntroduceerd in de laatste sectie van de SFTP-connector:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- | --- |
| gebruikersnaam | De gebruiker die toegang tot de SFTP-server heeft. |Ja |
| wachtwoord | Wachtwoord voor de gebruiker (gebruikersnaam). | Ja |

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<SFTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "password": "xxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-basic-authentication-with-encrypted-credential"></a>Voorbeeld: Basisverificatie met versleutelde referentie **

```json
{
    "name": "SftpLinkedService",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "Basic",
            "username": "xxx",
            "encryptedCredential": "xxxxxxxxxxxxxxxxx",
            "skipHostKeyValidation": false,
            "hostKeyFingerPrint": "ssh-rsa 2048 xx:00:00:00:xx:00:x0:0x:0x:0x:0x:00:00:x0:x0:00",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="using-ssh-public-key-authentication"></a>Met behulp van de openbare-sleutelauthenticatie SSH: **

Met basisverificatie, stelt `authenticationType` als `SshPublicKey`, en geef de volgende eigenschappen naast algemene die zijn geïntroduceerd in de laatste sectie van de SFTP-connector:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- | --- |
| gebruikersnaam |Gebruiker die toegang tot de SFTP-server heeft |Ja |
| privateKeyPath | Geef het absolute pad naar het persoonlijke sleutelbestand dat gateway toegankelijk. | Geef ofwel de `privateKeyPath` of `privateKeyContent`. <br><br> Alleen van toepassing wanneer het kopiëren van gegevens uit een on-premises SFTP-server. |
| privateKeyContent | Een geserialiseerde tekenreeks van de inhoud van de persoonlijke sleutel. De Wizard kopiëren kan lezen van bestand met de persoonlijke sleutel en pak de inhoud van de persoonlijke sleutel automatisch. Als u van andere hulpprogramma/SDK gebruikmaakt, in plaats daarvan de eigenschap privateKeyPath gebruiken. | Geef ofwel de `privateKeyPath` of `privateKeyContent`. |
| passPhrase | Geef de pass woordgroep en het wachtwoord voor het ontsleutelen van de persoonlijke sleutel als het sleutelbestand is beveiligd met een wachtwoordzin. | Ja als u een bestand met de persoonlijke sleutel wordt beveiligd door een wachtwoordzin. |

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyPath",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "<FTP server name or IP address>",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyPath": "D:\\privatekey_openssh",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true,
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a>Voorbeeld: SshPublicKey verificatie met persoonlijke sleutel inhoud **

```json
{
    "name": "SftpLinkedServiceWithPrivateKeyContent",
    "properties": {
        "type": "Sftp",
        "typeProperties": {
            "host": "mysftpserver.westus.cloudapp.azure.com",
            "port": 22,
            "authenticationType": "SshPublicKey",
            "username": "xxx",
            "privateKeyContent": "<base64 string of the private key content>",
            "passPhrase": "xxx",
            "skipHostKeyValidation": true
        }
    }
}
```

Zie voor meer informatie [SFTP connector](data-factory-sftp-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Instellen als u een gegevensset SFTP definieert, de **type** van de gegevensset **FileShare**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| folderPath |Subpad naar de map. Gebruik van escape-teken ' \ ' voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppelde service en gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** map hebben paden op basis van het segment beginnen of eindigen-datums en tijden. |Ja |
| fileName |Geef de naam van het bestand in de **folderPath** als u wilt dat de tabel om te verwijzen naar een bepaald bestand in de map. Als u geen waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Wanneer fileName niet voor een uitvoergegevensset opgegeven is, de naam van het gegenereerde bestand zou worden in de volgende indeling: <br/><br/>Data.<Guid>.txt (Example: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nee |
| fileFilter |Hiermee geeft u een filter moet worden gebruikt om een subset van de bestanden in het mappad in plaats van alle bestanden te selecteren.<br/><br/>Toegestane waarden zijn: `*` (meerdere tekens) en `?` (willekeurig teken).<br/><br/>Voorbeelden 1:`"fileFilter": "*.log"`<br/>Voorbeeld 2:`"fileFilter": 2016-1-?.txt"`<br/><br/> fileFilter geldt voor een invoergegevensset bestandsshare. Deze eigenschap wordt niet ondersteund met HDFS. |Nee |
| partitionedBy |partitionedBy kan worden gebruikt om op te geven van een dynamische folderPath, filename voor tijd reeksgegevens. Bijvoorbeeld, folderPath geparametriseerde voor elk uur van gegevens. |Nee |
| Indeling | De volgende indelingstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [parketvloeren indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **kopiëren van bestanden als-is** overslaan tussen bestandsgebaseerde winkels (binaire kopiëren), de sectie indeling in de definities van beide invoer en uitvoer gegevensset. |Nee |
| Compressie | Geef het type en de compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus: **optimale** en **snelst**. Zie voor meer informatie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |
| useBinaryTransfer |Geef binaire overdrachtsmodus of gebruiken. De waarde True voor binaire modus en ONWAAR ASCII. Standaardwaarde: True. Deze eigenschap kan alleen worden gebruikt wanneer de bijbehorende gekoppelde-servicetype is van het type: FtpServer. |Nee |

> [!NOTE]
> bestandsnaam en fileFilter worden niet gelijktijdig gebruikt.

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SFTPFileInput",
    "properties": {
        "type": "FileShare",
        "linkedServiceName": "SftpLinkedService",
        "typeProperties": {
            "folderPath": "<path to shared folder>",
            "fileName": "test.csv"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Zie voor meer informatie [SFTP connector](data-factory-sftp-connector.md#dataset-properties) artikel. 

### <a name="file-system-source-in-copy-activity"></a>Systeem bronnen in de kopieerbewerking
Als u gegevens uit een bron SFTP kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **FileSystemSource**, en opgeven na eigenschappen in de **bron** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| Recursieve |Hiermee wordt aangegeven of de gegevens recursief is gelezen uit de submappen of alleen uit de opgegeven map. |True, False (standaard) |Nee |



#### <a name="example"></a>Voorbeeld

```json
{
    "name": "pipeline",
    "properties": {
        "activities": [{
            "name": "SFTPToBlobCopy",
            "inputs": [{
                "name": "SFTPFileInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "FileSystemSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "00:05:00"
            }
        }],
        "start": "2017-02-20T18:00:00",
        "end": "2017-02-20T19:00:00"
    }
}
```

Zie voor meer informatie [SFTP connector](data-factory-sftp-connector.md#copy-activity-properties) artikel.


## <a name="http"></a>HTTP

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een HTTP, stelt u de **type** van de gekoppelde service naar **Http**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| url | Basis-URL voor de webserver | Ja |
| authenticationType | Hiermee geeft u het verificatietype. Toegestane waarden zijn: **anoniem**, **Basic**, **Digest**, **Windows**, **ClientCertificate**. <br><br> Raadpleeg de secties onder deze tabel over meer eigenschappen en voorbeelden voor deze verificatietypen JSON respectievelijk. | Ja |
| enableServerCertificateValidation | Geef op of validatie van het servercertificaat SSL inschakelen als bron is een HTTPS-webserver | Nee, de standaardwaarde is true |
| gatewayName | Naam van de Data Management Gateway verbinding maken met een lokale HTTP-bron. | Ja als u gegevens kopiëren van een lokale HTTP-bron. |
| encryptedCredential | Versleutelde referentie voor toegang tot het HTTP-eindpunt. Automatisch wordt gegenereerd wanneer u de verificatie-informatie in de wizard kopiëren of de ClickOnce-pop-dialoogvenster configureert. | Nee. Alleen van toepassing wanneer het kopiëren van gegevens uit een lokale HTTP-server. |

#### <a name="example-using-basic-digest-or-windows-authentication"></a>Voorbeeld: Met behulp van basisverificatie, verificatiesamenvatting of Windows-verificatie
Stel `authenticationType` als `Basic`, `Digest`, of `Windows`, en geef de volgende eigenschappen naast de HTTP-connector algemene resources die hierboven zijn geïntroduceerd:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| gebruikersnaam | Gebruikersnaam voor toegang tot het HTTP-eindpunt. | Ja |
| wachtwoord | Wachtwoord voor de gebruiker (gebruikersnaam). | Ja |

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "basic",
            "url": "https://en.wikipedia.org/wiki/",
            "userName": "user name",
            "password": "password"
        }
    }
}
```

#### <a name="example-using-clientcertificate-authentication"></a>Voorbeeld: ClientCertificate verificatie gebruiken

Met basisverificatie, stelt `authenticationType` als `ClientCertificate`, en geef de volgende eigenschappen naast de HTTP-connector algemene resources die hierboven zijn geïntroduceerd:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| embeddedCertData | De Base64-gecodeerde inhoud van de binaire gegevens van het bestand (Personal Information Exchange (PFX). | Geef ofwel de `embeddedCertData` of `certThumbprint`. |
| certThumbprint | De vingerafdruk van het certificaat dat is geïnstalleerd op de gatewaycomputer certificaatarchief. Alleen van toepassing wanneer het kopiëren van gegevens van een lokale HTTP-bron. | Geef ofwel de `embeddedCertData` of `certThumbprint`. |
| wachtwoord | Het wachtwoord is gekoppeld aan het certificaat. | Nee |

Als u `certThumbprint` voor verificatie en het certificaat in het persoonlijke archief van de lokale computer is geïnstalleerd, moet u de machtiging lezen voor de gateway-service:

1. Start Microsoft Management Console (MMC). Voeg de **certificaten** -module die gericht is op de **lokale Computer**.
2. Vouw **certificaten**, **persoonlijke**, en klik op **certificaten**.
3. Met de rechtermuisknop op het certificaat uit het persoonlijke archief en selecteer **alle taken**->**persoonlijke sleutels beheren...**
3. Op de **beveiliging** tabblad, het toevoegen van het gebruikersaccount waaronder Data Management Gateway Host Service wordt uitgevoerd met leestoegang tot het certificaat.  

**Voorbeeld: met behulp van clientcertificaat:** dit de gekoppelde service uw gegevensfactory op een lokale HTTP-webserver. Dit maakt gebruik van een clientcertificaat dat is geïnstalleerd op de machine met Data Management Gateway is geïnstalleerd.

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "certThumbprint": "thumbprint of certificate",
            "gatewayName": "gateway name"
        }
    }
}
```

#### <a name="example-using-client-certificate-in-a-file"></a>Voorbeeld: clientcertificaat gebruiken in een bestand
Deze gekoppelde service wordt uw gegevensfactory op een lokale HTTP-webserver. Een certificaatbestand van de client op de machine wordt met Data Management Gateway is geïnstalleerd.

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "Http",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "https://en.wikipedia.org/wiki/",
            "embeddedCertData": "base64 encoded cert data",
            "password": "password of cert"
        }
    }
}
```

Zie voor meer informatie [HTTP connector](data-factory-http-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Om te definiëren van een HTTP-gegevensset, stel de **type** van de gegevensset **Http**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| relativeUrl | Een relatieve URL naar de resource die de gegevens bevat. Als het pad niet wordt opgegeven, worden alleen de URL die is opgegeven in de definitie van de gekoppelde service wordt gebruikt. <br><br> Kan de dynamische-URL, kunt u [Data Factory-functies en systeemvariabelen](data-factory-functions-variables.md), bijvoorbeeld: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`. | Nee |
| requestMethod | HTTP-methode. Toegestane waarden zijn **ophalen** of **POST**. | Nee. Standaard is `GET`. |
| additionalHeaders | Aanvullende HTTP-aanvraagheaders. | Nee |
| requestBody | Instantie voor HTTP-aanvraag. | Nee |
| Indeling | Als u gewoon wilt **gegevens ophalen van HTTP-eindpunt als-is** overslaan zonder deze parseren, deze instellingen. <br><br> Als u de inhoud van het HTTP-antwoord geparseerd tijdens het kopiëren wilt, de volgende indelingstypen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Zie voor meer informatie [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [parketvloeren indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. |Nee |
| Compressie | Geef het type en de compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus: **optimale** en **snelst**. Zie voor meer informatie [bestands- en compressie-notaties in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

#### <a name="example-using-the-get-default-method"></a>Voorbeeld: met behulp van de methode GET (standaard)

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "XXX/test.xml",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

#### <a name="example-using-the-post-method"></a>Voorbeeld: via de POST-methode

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "Http",
        "linkedServiceName": "HttpLinkedService",
        "typeProperties": {
            "relativeUrl": "/XXX/test.xml",
            "requestMethod": "Post",
            "requestBody": "body for POST HTTP request"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```
Zie voor meer informatie [HTTP connector](data-factory-http-connector.md#dataset-properties) artikel.

### <a name="http-source-in-copy-activity"></a>HTTP-bron in de kopieerbewerking
Als u gegevens van een HTTP-bron kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **HttpSource**, en opgeven na eigenschappen in de **bron** sectie:

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| httpRequestTimeout | De time-out (TimeSpan) voor de HTTP-aanvragen reageert. De time-out is reageert niet op de time-out antwoordgegevens lezen. | Nee. Standaardwaarde: 00:01:40 |


#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "HttpSourceToAzureBlob",
            "description": "Copy from an HTTP source to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "HttpSourceDataInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "HttpSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Zie voor meer informatie [HTTP connector](data-factory-http-connector.md#copy-activity-properties) artikel.

## <a name="odata"></a>OData

### <a name="linked-service"></a>Gekoppelde service
Voor het definiëren van een OData gekoppelde service, stelt u de **type** van de gekoppelde service naar **OData**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| url |URL van de OData-service. |Ja |
| authenticationType |Het soort verificatie gebruikt voor verbinding met de OData-bron. <br/><br/> Mogelijke waarden zijn voor OData-cloud, anoniem, basis en OAuth (Opmerking momenteel alleen ondersteuning voor Azure Data Factory Azure Active Directory op basis van OAuth). <br/><br/> Mogelijke waarden zijn voor lokale OData, anoniem, basis en Windows. |Ja |
| gebruikersnaam |Geef de gebruikersnaam als u basisverificatie gebruikt. |Ja (alleen als u basisverificatie gebruikt) |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Ja (alleen als u basisverificatie gebruikt) |
| authorizedCredential |Als u van OAuth gebruikmaakt, klikt u op **autoriseren** in de Data Factory-Wizard kopiëren of Editor en voer uw referenties en vervolgens de waarde van deze eigenschap wordt niet automatisch wordt gegenereerd. |Ja (alleen als u OAuth-verificatie gebruikt) |
| gatewayName |Naam van de gateway die voor de Data Factory-service verbinding maken met het lokale OData-service wordt gebruikt. Geef alleen als u gegevens uit een on-premises OData-bron kopiëren wilt. |Nee |

#### <a name="example---using-basic-authentication"></a>Voorbeeld: met behulp van basisverificatie
```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Basic",
            "username": "username",
            "password": "password"
        }
    }
}
```

#### <a name="example---using-anonymous-authentication"></a>Voorbeeld: anonieme verificatie

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

#### <a name="example---using-windows-authentication-accessing-on-premises-odata-source"></a>Voorbeeld: met behulp van Windows-verificatie toegang tot lokale OData-bron

```json
{
    "name": "inputLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of on-premises OData source, for example, Dynamics CRM>",
            "authenticationType": "Windows",
            "username": "domain\\user",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example---using-oauth-authentication-accessing-cloud-odata-source"></a>Voorbeeld: met behulp van OAuth-verificatie toegang tot de cloud OData-bron
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of cloud OData source, for example, https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc>",
            "authenticationType": "OAuth",
            "authorizedCredential": "<auto generated by clicking the Authorize button on UI>"
        }
    }
}
```

Zie voor meer informatie [OData-connector](data-factory-odata-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Om te definiëren van een OData-gegevensset, stel de **type** van de gegevensset **ODataResource**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| pad |Pad naar de OData-bron |Nee |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "ODataDataset",
    "properties": {
        "type": "ODataResource",
        "typeProperties": {
            "path": "Products"
        },
        "linkedServiceName": "ODataLinkedService",
        "structure": [],
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
        }
    }
}
```

Zie voor meer informatie [OData-connector](data-factory-odata-connector.md#dataset-properties) artikel.

### <a name="relational-source-in-copy-activity"></a>Relationele gegevensbron in de kopieerbewerking
Als u gegevens uit een OData-bron kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en opgeven na eigenschappen in de **bron** sectie:

| Eigenschap | Beschrijving | Voorbeeld | Vereist |
| --- | --- | --- | --- |
| query |Gebruik de aangepaste query om gegevens te lezen. |"?$select=Name, Description&$top=5" |Nee |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "CopyODataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "?$select=Name, Description&$top=5"
                },
                "sink": {
                    "type": "BlobSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:00"
                }
            },
            "inputs": [{
                "name": "ODataDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobODataDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "ODataToBlob"
        }],
        "start": "2017-02-01T18:00:00",
        "end": "2017-02-03T19:00:00"
    }
}
```

Zie voor meer informatie [OData-connector](data-factory-odata-connector.md#copy-activity-properties) artikel.


## <a name="odbc"></a>ODBC


### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een ODBC, stelt u de **type** van de gekoppelde service naar **OnPremisesOdbc**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| connectionString |Het gedeelte niet access referentie van de verbindingsreeks en een optionele referentie versleuteld. Zie de voorbeelden in de volgende secties. |Ja |
| referentie |Het gedeelte van de referentie toegang van de verbindingsreeks die is opgegeven in de indeling van de eigenschapswaarde specifieke stuurprogramma's. Voorbeeld: "Uid =<user ID>; Pwd =<password>; RefreshToken =<secret refresh token>; '. |Nee |
| authenticationType |Het soort verificatie gebruikt voor verbinding met het beheerprogramma voor ODBC-gegevensarchief. Mogelijke waarden zijn: anonieme en Basic. |Ja |
| gebruikersnaam |Geef de gebruikersnaam als u basisverificatie gebruikt. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |
| gatewayName |Naam van de gateway die voor de Data Factory-service gebruiken moet voor verbinding met het beheerprogramma voor ODBC-gegevensarchief. |Ja |

#### <a name="example---using-basic-authentication"></a>Voorbeeld: met behulp van basisverificatie

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=Server.database.windows.net; Database=TestDatabase;",
            "userName": "username",
            "password": "password",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```
#### <a name="example---using-basic-authentication-with-encrypted-credentials"></a>Voorbeeld: met behulp van basisverificatie met versleutelde referenties
U kunt de referenties met versleutelen de [nieuw AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) cmdlet (versie 1.0 van Azure PowerShell) of [nieuw AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0,9 of een eerdere versie van Azure PowerShell).  

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Basic",
            "connectionString": "Driver={SQL Server};Server=myserver.database.windows.net; Database=TestDatabase;;EncryptedCredential=eyJDb25uZWN0...........................",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

#### <a name="example-using-anonymous-authentication"></a>Voorbeeld: Anonieme verificatie gebruikt

```json
{
    "name": "ODBCLinkedService",
    "properties": {
        "type": "OnPremisesOdbc",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "connectionString": "Driver={SQL Server};Server={servername}.database.windows.net; Database=TestDatabase;",
            "credential": "UID={uid};PWD={pwd}",
            "gatewayName": "<onpremgateway>"
        }
    }
}
```

Zie voor meer informatie [ODBC connector](data-factory-odbc-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Om te definiëren van een ODBC-gegevensset, stel de **type** van de gegevensset **RelationalTable**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in het beheerprogramma voor ODBC-gegevensarchief. |Ja |


#### <a name="example"></a>Voorbeeld

```json
{
    "name": "ODBCDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "ODBCLinkedService",
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

Zie voor meer informatie [ODBC connector](data-factory-odbc-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationele gegevensbron in de kopieerbewerking
Als u gegevens uit een ODBC data store kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en opgeven na eigenschappen in de **bron** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |Gebruik de aangepaste query om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `select * from MyTable`. |Ja |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "CopyODBCToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [{
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
            "inputs": [{
                "name": "OdbcDataSet"
            }],
            "outputs": [{
                "name": "AzureBlobOdbcDataSet"
            }],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "OdbcToBlob"
        }],
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00"
    }
}
``` 

Zie voor meer informatie [ODBC connector](data-factory-odbc-connector.md#copy-activity-properties) artikel.

## <a name="salesforce"></a>SalesForce


### <a name="linked-service"></a>Gekoppelde service
Voor het definiëren van een Salesforce gekoppelde service, stelt u de **type** van de gekoppelde service naar **Salesforce**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| environmentUrl | Geef de URL van de Salesforce-exemplaar. <br><br> -Standaard is 'https://login.salesforce.com'. <br> -Geef om gegevens te kopiëren vanuit sandbox, 'https://test.salesforce.com'. <br> -Om gegevens te kopiëren van een aangepast domein, opgeven, bijvoorbeeld 'https://[domain].my.salesforce.com'. |Nee |
| gebruikersnaam |Geef een naam op voor het gebruikersaccount. |Ja |
| wachtwoord |Geef een wachtwoord voor het gebruikersaccount. |Ja |
| securityToken |Geef een beveiligingstoken voor de gebruikersaccount. Zie [security token ophalen](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) voor instructies over het opnieuw instellen of ophalen van een beveiligingstoken. Zie voor informatie over het algemeen over beveiligingstokens, [beveiligings- en de API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Ja |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```

Zie voor meer informatie [Salesforce-connector](data-factory-salesforce-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Om te definiëren een Salesforce-gegevensset, stel de **type** van de gegevensset **RelationalTable**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in Salesforce. |Nee (als een **query** van **RelationalSource** is opgegeven) |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"
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

Zie voor meer informatie [Salesforce-connector](data-factory-salesforce-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationele gegevensbron in de kopieerbewerking
Als u gegevens uit Salesforce kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en opgeven na eigenschappen in de **bron** sectie:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |Gebruik de aangepaste query om gegevens te lezen. |Een SQL-92-query of [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query. Bijvoorbeeld `select * from MyTable__c`. |Nee (als de **tableName** van de **gegevensset** is opgegeven) |

#### <a name="example"></a>Voorbeeld  



```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "SalesforceInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

> [!IMPORTANT]
> Het gedeelte '__c' van de API-naam is vereist voor elk object dat aangepast.

Zie voor meer informatie [Salesforce-connector](data-factory-salesforce-connector.md#copy-activity-properties) artikel. 

## <a name="web-data"></a>Web-gegevens 

### <a name="linked-service"></a>Gekoppelde service
Voor het definiëren van een gekoppelde service, stelt u de **type** van de gekoppelde service naar **Web**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| URL |URL van de webbron |Ja |
| authenticationType |Anonieme. |Ja |
 

#### <a name="example"></a>Voorbeeld


```json
{
    "name": "web",
    "properties": {
        "type": "Web",
        "typeProperties": {
            "authenticationType": "Anonymous",
            "url": "https://en.wikipedia.org/wiki/"
        }
    }
}
```

Zie voor meer informatie [Web tabel connector](data-factory-web-table-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Instellen als u een Web-gegevensset definieert, de **type** van de gegevensset **WebTable**, en de volgende eigenschappen opgeven in de **typeProperties** sectie: 

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type |Het type van de gegevensset. moet worden ingesteld op **WebTable** |Ja |
| pad |Een relatieve URL naar de resource die de tabel bevat. |Nee. Als het pad niet wordt opgegeven, worden alleen de URL die is opgegeven in de definitie van de gekoppelde service wordt gebruikt. |
| index |De index van de tabel in de resource. Zie [Get-index van een tabel in een HTML-pagina](#get-index-of-a-table-in-an-html-page) sectie voor stappen voor het ophalen van de index van een tabel in een HTML-pagina. |Ja |

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "WebTableInput",
    "properties": {
        "type": "WebTable",
        "linkedServiceName": "WebLinkedService",
        "typeProperties": {
            "index": 1,
            "path": "AFI's_100_Years...100_Movies"
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

Zie voor meer informatie [Web tabel connector](data-factory-web-table-connector.md#dataset-properties) artikel. 

### <a name="web-source-in-copy-activity"></a>Webbron in kopieerbewerking
Als u gegevens uit een tabel web kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **WebSource**. Wanneer de bron is in de kopieerbewerking is momenteel van het type **WebSource**, zonder aanvullende eigenschappen worden ondersteund.

#### <a name="example"></a>Voorbeeld

```json
{
    "name": "SamplePipeline",
    "properties": {
        "start": "2016-06-01T18:00:00",
        "end": "2016-06-01T19:00:00",
        "description": "pipeline with copy activity",
        "activities": [{
            "name": "WebTableToAzureBlob",
            "description": "Copy from a Web table to an Azure blob",
            "type": "Copy",
            "inputs": [{
                "name": "WebTableInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
            "typeProperties": {
                "source": {
                    "type": "WebSource"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }]
    }
}
```

Zie voor meer informatie [Web tabel connector](data-factory-web-table-connector.md#copy-activity-properties) artikel. 

## <a name="compute-environments"></a>COMPUTE-OMGEVINGEN
De volgende tabel bevat de compute-omgevingen wordt ondersteund door de Data Factory en de activiteiten voor gegevenstransformatie die kunnen worden uitgevoerd op deze. Klik op de koppeling voor de berekening die u geïnteresseerd in de JSON-schema's voor de gekoppelde service bent te koppelen aan een gegevensfactory zien. 

| Compute-omgeving | Activiteiten |
| --- | --- |
| [HDInsight-cluster op aanvraag](#on-demand-azure-hdinsight-cluster) of [uw eigen HDInsight-cluster](#existing-azure-hdinsight-cluster) |[Aangepaste activiteit .NET](#net-custom-activity), [Hive-activiteit](#hdinsight-hive-activity), [activiteit varkens] (#hdinsight-pig-activiteit, [MapReduce-activiteit](#hdinsight-mapreduce-activity), [Hadoop-streaming activiteit](#hdinsight-streaming-activityd), [Spark activiteit](#hdinsight-spark-activity) |
| [Azure Batch](#azure-batch) |[.NET aangepaste activiteit](#net-custom-activity) |
| [Azure Machine Learning](#azure-machine-learning) | [Machine Learning-Batchuitvoeringsactiviteit](#machine-learning-batch-execution-activity), [Machine Learning eigen Update Resourceactiviteit](#machine-learning-update-resource-activity) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics) |[Data Lake Analytics U-SQL](#data-lake-analytics-u-sql-activity) |
| [Azure SQL Database](#azure-sql-database-1), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-1), [SQL Server](#sql-server-1) |[Opgeslagen procedure](#stored-procedure-activity) |

## <a name="on-demand-azure-hdinsight-cluster"></a>Azure HDInsight-cluster op aanvraag
De Azure Data Factory-service kan automatisch maken van een Windows, Linux-gebaseerde bellen op HDInsight-cluster om gegevens te verwerken. Het cluster wordt gemaakt in dezelfde regio bevinden als het storage-account (de eigenschap linkedServiceName in de JSON) die zijn gekoppeld aan het cluster. U kunt de volgende activiteiten voor gegevenstransformatie uitvoeren op deze gekoppelde service: [aangepaste activiteit .NET](#net-custom-activity), [Hive-activiteit](#hdinsight-hive-activity), [activiteit varkens] (#hdinsight-pig-activiteit, [MapReduce-activiteit](#hdinsight-mapreduce-activity), [Hadoop-streaming activiteit](#hdinsight-streaming-activityd), [activiteit Spark](#hdinsight-spark-activity). 

### <a name="linked-service"></a>Gekoppelde service 
De volgende tabel bevat beschrijvingen van de eigenschappen die in de Azure-JSON-definitie van een gekoppelde HDInsight-service op aanvraag.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op **HDInsightOnDemand**. |Ja |
| De clustergrootte |Het aantal worker/gegevens knooppunten in het cluster. Het HDInsight-cluster wordt gemaakt met 2 hoofdknooppunten samen met het aantal worker-knooppunten die u voor deze eigenschap opgeeft. De knooppunten zijn met een grootte van Standard_D3 met 4 kernen, zodat een cluster met 4 worker-knooppunten 24 kernen duurt (4\*4 = 16 cores voor worker-knooppunten plus 2\*4 = 8 cores voor hoofdknooppunten). Zie [maken Linux gebaseerde Hadoop-clusters in HDInsight](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) voor meer informatie over de Standard_D3-laag. |Ja |
| TimeToLive |De toegestane niet-actieve tijd voor het HDInsight-cluster op aanvraag. Hiermee geeft u op hoelang het HDInsight-cluster op aanvraag na voltooiing van een activiteit die wordt uitgevoerd als er geen actieve taken in het cluster actief blijft.<br/><br/>Bijvoorbeeld, als het uitvoeren van een activiteit 6 minuten duurt en timetolive is ingesteld op 5 minuten, blijft het cluster actief gedurende vijf minuten nadat de 6 minuten van de verwerking van de activiteit is uitgevoerd. Als een andere activiteit die wordt uitgevoerd met het venster 6 minuten wordt uitgevoerd, wordt het verwerkt door hetzelfde cluster.<br/><br/>Maken van een HDInsight-cluster op aanvraag is een dure bewerking (kan even duren) in dat geval gebruik deze instelling als die nodig zijn voor de prestaties van een gegevensfactory verbeteren door een HDInsight-cluster op aanvraag opnieuw te gebruiken.<br/><br/>Als u timetolive-waarde op 0 instelt, wordt het cluster verwijderd als de activiteit worden uitgevoerd verwerkt. Anderzijds, als u een hoge waarde instelt, kan het cluster blijven inactief onnodig wat resulteert in hoge kosten. Het is daarom belangrijk dat u de juiste waarde op basis van uw behoeften.<br/><br/>Meerdere pijplijnen kunnen hetzelfde exemplaar van het HDInsight-cluster op aanvraag delen als de waarde van de eigenschap timetolive juist is ingesteld |Ja |
| versie |Versie van het HDInsight-cluster. Zie voor meer informatie [HDInsight-versies ondersteund in Azure Data Factory](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory). |Nee |
| linkedServiceName |Gekoppelde Azure Storage-service moet worden gebruikt door het cluster op aanvraag voor het opslaan en verwerken van gegevens. <p>Op dit moment kunt maken u een HDInsight-cluster op aanvraag die gebruikmaakt van een Azure Data Lake Store als de opslag niet. Als u wilt voor het opslaan van de resultaatgegevens van HDInsight worden verwerkt in een Azure Data Lake Store, gebruikt u een Kopieeractiviteit in de gegevens uit de Azure Blob Storage kopiëren naar Azure Data Lake Store.</p>  | Ja |
| additionalLinkedServiceNames |Hiermee geeft u extra opslagaccounts voor het HDInsight gekoppelde service, zodat de Data Factory-service namens jou registreren kunt. |Nee |
| osType |Type besturingssysteem. Toegestane waarden zijn: (standaard) voor Windows en Linux |Nee |
| hcatalogLinkedServiceName |De naam van de Azure SQL gekoppelde service die verwijzen naar de HCatalog-database. Het HDInsight-cluster op aanvraag wordt gemaakt met behulp van de Azure SQL database als de metastore. |Nee |

### <a name="json-example"></a>JSON-voorbeeld
De volgende JSON definieert een service op aanvraag een gekoppelde HDInsight op basis van Linux. De Data Factory-service maakt automatisch een **op basis van Linux** HDInsight-cluster bij het verwerken van een gegevenssegment. 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

Zie voor meer informatie [gekoppelde services berekenen](data-factory-compute-linked-services.md) artikel. 

## <a name="existing-azure-hdinsight-cluster"></a>Bestaande Azure HDInsight-cluster
U kunt een gekoppelde HDInsight-service voor het registreren van uw eigen HDInsight-cluster met Data Factory maken. U kunt de volgende activiteiten voor gegevenstransformatie uitvoeren op deze gekoppelde service: [aangepaste activiteit .NET](#net-custom-activity), [Hive-activiteit](#hdinsight-hive-activity), [activiteit varkens] (#hdinsight-pig-activiteit, [MapReduce-activiteit](#hdinsight-mapreduce-activity), [Hadoop-streaming activiteit](#hdinsight-streaming-activityd), [activiteit Spark](#hdinsight-spark-activity). 

### <a name="linked-service"></a>Gekoppelde service
De volgende tabel bevat beschrijvingen van de eigenschappen die in de Azure-JSON-definitie van een gekoppelde HDInsight-service.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op **HDInsight**. |Ja |
| clusterUri |De URI van het HDInsight-cluster. |Ja |
| gebruikersnaam |Geef de naam van de gebruiker moet worden gebruikt voor het verbinding maken met een bestaand HDInsight-cluster. |Ja |
| wachtwoord |Wachtwoord voor het gebruikersaccount opgeven. |Ja |
| linkedServiceName | Naam van de gekoppelde Azure Storage-service die naar de Azure blob storage gebruikt door het HDInsight-cluster verwijst. <p>Op dit moment kunt opgeven u niet dat een Azure Data Lake Store gekoppelde service voor deze eigenschap. U kunt toegang tot gegevens in Azure Data Lake Store van Hive/Pig-scripts als het HDInsight-cluster toegang tot de Data Lake Store heeft. </p>  |Ja |

Zie voor versies van HDInsight-clusters ondersteund [ondersteunde versies van HDInsight](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory). 

#### <a name="json-example"></a>JSON-voorbeeld

```json
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
            "userName": "admin",
            "password": "<password>",
            "linkedServiceName": "MyHDInsightStoragelinkedService"
        }
    }
}
```

## <a name="azure-batch"></a>Azure Batch
U kunt een gekoppelde Azure-Batch-service voor het registreren van een Batch-pool van virtuele machines (VM's) maken met een data factory. U kunt aangepaste .NET-activiteiten met behulp van Azure Batch of Azure HDInsight kunt uitvoeren. U kunt uitvoeren een [aangepaste activiteit .NET](#net-custom-activity) op deze gekoppelde service. 

### <a name="linked-service"></a>Gekoppelde service
De volgende tabel bevat beschrijvingen van de eigenschappen die in de Azure-JSON-definitie van een gekoppelde Azure-Batch-service.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op **AzureBatch**. |Ja |
| accountName |Naam van de Azure Batch-account. |Ja |
| accessKey |De toegangssleutel voor de Azure Batch-account. |Ja |
| poolName |Naam van de groep van virtuele machines. |Ja |
| linkedServiceName |Naam van de Azure Storage gekoppelde service die is gekoppeld aan deze gekoppelde Azure-Batch-service. Deze gekoppelde service wordt gebruikt voor tijdelijke bestanden die zijn vereist voor het uitvoeren van de activiteit en opslaan van logboekbestanden voor het uitvoeren van de activiteit. |Ja |


#### <a name="json-example"></a>JSON-voorbeeld

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
        "type": "AzureBatch",
        "typeProperties": {
            "accountName": "<Azure Batch account name>",
            "accessKey": "<Azure Batch account key>",
            "poolName": "<Azure Batch pool name>",
            "linkedServiceName": "<Specify associated storage linked service reference here>"
        }
    }
}
```

## <a name="azure-machine-learning"></a>Azure Machine Learning
U maakt een Azure Machine Learning gekoppelde service voor het registreren van een Machine Learning score-eindpunt met een gegevensfactory. Twee activiteiten voor gegevenstransformatie die kunnen worden uitgevoerd op deze gekoppelde service: [Machine Learning-Batchuitvoeringsactiviteit](#machine-learning-batch-execution-activity), [Resourceactiviteit voor Machine Learning Update](#machine-learning-update-resource-activity). 

### <a name="linked-service"></a>Gekoppelde service
De volgende tabel bevat beschrijvingen van de eigenschappen die in de Azure-JSON-definitie van een Azure Machine Learning gekoppelde service.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| Type |De eigenschap type moet worden ingesteld op: **AzureML**. |Ja |
| mlEndpoint |De URL voor batchscores. |Ja |
| apiKey |Het gepubliceerde werkruimtemodel API. |Ja |

#### <a name="json-example"></a>JSON-voorbeeld

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://[batch scoring endpoint]/jobs",
            "apiKey": "<apikey>"
        }
    }
}
```

## <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics
U maakt een **Azure Data Lake Analytics** gekoppelde service om te koppelen van een Azure Data Lake Analytics compute een Azure data factory-service voordat u de [Data Lake Analytics U-SQL-activiteit](data-factory-usql-activity.md) in een pijplijn.

### <a name="linked-service"></a>Gekoppelde service

De volgende tabel bevat beschrijvingen van de eigenschappen die in de JSON-definitie van een service van Azure Data Lake Analytics is gekoppeld. 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| Type |De eigenschap type moet worden ingesteld op: **AzureDataLakeAnalytics**. |Ja |
| accountName |Azure Data Lake Analytics-accountnaam. |Ja |
| dataLakeAnalyticsUri |Azure Data Lake Analytics-URI. |Nee |
| Autorisatie |Autorisatiecode wordt automatisch opgehaald nadat te klikken op **autoriseren** knop in de Data Factory-Editor en het uitvoeren van de OAuth-aanmelding. |Ja |
| subscriptionId |Azure-abonnement-id |Nee (als niet wordt opgegeven, abonnement van de gegevensfactory wordt gebruikt). |
| resourceGroupName |Naam van een Azure-resourcegroep |Nee (als niet wordt opgegeven, brongroep van de gegevensfactory wordt gebruikt). |
| sessie-id |sessie-id van de OAuth-autorisatie-sessie. Elke sessie-id is uniek en kan slechts eenmaal worden gebruikt. Wanneer u de Data Factory-Editor gebruikt, wordt deze ID is automatisch gegenereerd. |Ja |


#### <a name="json-example"></a>JSON-voorbeeld
Het volgende voorbeeld bevat JSON-definitie voor een service van Azure Data Lake Analytics is gekoppeld.

```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "<account name>",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>",
            "subscriptionId": "<subscription id>",
            "resourceGroupName": "<resource group name>"
        }
    }
}
```

## <a name="azure-sql-database"></a>Azure SQL Database
Een Azure SQL gekoppelde service maakt en gebruikt u deze met de [activiteit opgeslagen Procedure](#stored-procedure-activity) aan te roepen, een opgeslagen procedure uit een Data Factory-pijplijn. 

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Azure SQL Database, stelt u de **type** van de gekoppelde service naar **AzureSqlDatabase**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met de Azure SQL Database-exemplaar voor de eigenschap connectionString. |Ja |

#### <a name="json-example"></a>JSON-voorbeeld

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

Zie [Azure SQL-Connector](data-factory-azure-sql-connector.md#linked-service-properties) voor meer informatie over deze gekoppelde service.

## <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse
Een Azure SQL Data Warehouse gekoppelde service maakt en gebruikt u deze met de [activiteit opgeslagen Procedure](data-factory-stored-proc-activity.md) aan te roepen, een opgeslagen procedure uit een Data Factory-pijplijn. 

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Azure SQL Data Warehouse, stelt u de **type** van de gekoppelde service naar **AzureSqlDW**, en opgeven na eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met de Azure SQL Data Warehouse-exemplaar voor de eigenschap connectionString. |Ja |

#### <a name="json-example"></a>JSON-voorbeeld

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
    }
}
```

Zie voor meer informatie [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) artikel. 

## <a name="sql-server"></a>SQL Server 
U maakt een gekoppelde SQL-Server-service en deze gebruikt met de [activiteit opgeslagen Procedure](data-factory-stored-proc-activity.md) aan te roepen, een opgeslagen procedure uit een Data Factory-pijplijn. 

### <a name="linked-service"></a>Gekoppelde service
Maken van een gekoppelde service van het type **OnPremisesSqlServer** een on-premises SQL Server database koppelen aan een gegevensfactory. De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor lokale SQL Server gekoppelde service.

De volgende tabel bevat een beschrijving voor JSON-elementen die specifiek zijn voor de service SQL Server gekoppeld.

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OnPremisesSqlServer**. |Ja |
| connectionString |ConnectionString informatie die nodig zijn voor de verbinding met de lokale SQL Server-database met behulp van de SQL-verificatie of de Windows-verificatie opgeven. |Ja |
| gatewayName |Naam van de gateway die voor de Data Factory-service gebruiken moet voor verbinding met het lokale SQL Server-database. |Ja |
| gebruikersnaam |Geef de gebruikersnaam als u Windows-verificatie gebruikt. Voorbeeld: **domainname\\gebruikersnaam**. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |

U kunt versleutelen referenties met behulp van de **nieuw AzureRmDataFactoryEncryptValue** cmdlet en in de verbindingsreeks gebruiken, zoals wordt weergegeven in het volgende voorbeeld (**EncryptedCredential** eigenschap):  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```


#### <a name="example-json-for-using-sql-authentication"></a>Voorbeeld: JSON voor het gebruik van SQL-verificatie

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
#### <a name="example-json-for-using-windows-authentication"></a>Voorbeeld: JSON voor het gebruik van Windows-verificatie

Als de gebruikersnaam en wachtwoord zijn opgegeven, worden deze door gateway imiteren van de opgegeven gebruikersaccount verbinding maken met de lokale SQL Server-database gebruikt. Anders gateway maakt verbinding met de SQL-Server rechtstreeks met de beveiligingscontext van Gateway (het starten van de account).

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

Zie voor meer informatie [SQL Server-connector](data-factory-sqlserver-connector.md#linked-service-properties) artikel.

## <a name="data-transformation-activities"></a>ACTIVITEITEN VOOR GEGEVENSTRANSFORMATIE

Activiteit | Beschrijving
-------- | -----------
[HDInsight Hive-activiteit](#hdinsight-hive-activity) | De HDInsight Hive-activiteit in een Data Factory-pijplijn voert Hive-query's op uw eigen of op aanvraag Windows, Linux-gebaseerde HDInsight-cluster. 
[HDInsight Pig-activiteit](#hdinsight-pig-activity) | De HDInsight Pig-activiteit in een Data Factory-pijplijn voert Pig-query's in uw eigen of op aanvraag Windows, Linux-gebaseerde HDInsight-cluster.
[HDInsight MapReduce-activiteit](#hdinsight-mapreduce-activity) | De HDInsight MapReduce-activiteit in een Data Factory-pijplijn voert MapReduce-programma's op uw eigen of op aanvraag Windows, Linux-gebaseerde HDInsight-cluster.
[HDInsight Streaming-activiteit](#hdinsight-streaming-activity) | Streamingactiviteit in een Data Factory-pijplijn HDInsight Hadoop-Streaming-programma's op uw eigen of Windows, Linux-gebaseerde HDInsight-cluster op aanvraag worden uitgevoerd.
[HDInsight Spark-activiteit](#hdinsight-spark-activity) | De HDInsight Spark-activiteit in een Data Factory-pijplijn Spark-programma's op uw eigen HDInsight-cluster wordt uitgevoerd. 
[Machine Learning-batchuitvoeringsactiviteit](#machine-learning-batch-execution-activity) | Azure Data Factory kunt u gemakkelijk maken pijplijnen die gebruikmaken van een gepubliceerde Azure Machine Learning-webservice voor predictive analytics. U kunt een Machine Learning-webservice om te maken van voorspellingen op de gegevens in een batch met de Batchuitvoeringsactiviteit in een Azure Data Factory-pijplijn en aanroepen. 
[Machine Learning-activiteit resources bijwerken](#machine-learning-update-resource-activity) | Na verloop van tijd moeten de voorspellende modellen in de Machine Learning-score experimenten worden retrained met nieuwe invoergegevenssets. Nadat u klaar bent met de retraining, wilt u de score webservice bijwerken met de retrained Machine Learning-model. De activiteit van de Resource bijwerken kunt u de webservice bijwerken met het nieuwe getrainde model.
[Opgeslagen procedureactiviteit](#stored-procedure-activity) | U kunt de activiteit opgeslagen Procedure in een Data Factory-pijplijn gebruiken om aan te roepen, een opgeslagen procedure in een van de volgende gegevens worden opgeslagen: Azure SQL Database, Azure SQL Data Warehouse, SQL Server-Database in uw onderneming of een Azure VM. 
[Data Lake Analytics U-SQL-activiteit](#data-lake-analytics-u-sql-activity) | Data Lake Analytics U-SQL-activiteit wordt een U-SQL-script uitgevoerd op een Azure Data Lake Analytics-cluster.  
[.NET aangepaste activiteit](#net-custom-activity) | Als u nodig hebt om gegevens op een manier die niet wordt ondersteund door Data Factory te transformeren, kunt u een aangepaste activiteit maken met uw eigen logica gegevensverwerking en gebruikt u de activiteit in de pijplijn. U kunt de aangepaste .NET-activiteit moet worden uitgevoerd met behulp van een Azure Batch-service of een Azure HDInsight-cluster configureren. 

     
## <a name="hdinsight-hive-activity"></a>HDInsight Hive-activiteit
U kunt de volgende eigenschappen opgeven in een Hive-activiteit JSON-definitie. De eigenschap type voor de activiteit moet zijn: **HDInsightHive**. U moet eerst een gekoppelde HDInsight-service maken en geef de naam van het als een waarde voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** wanneer u het type activiteit ingesteld op HDInsightHive sectie:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| Script |Geef de inline Hive-script |Nee |
| scriptpad |Het Hive-script opslaat in Azure blob storage en geef het pad naar het bestand. Gebruik de eigenschap 'script' of 'scriptPath'. Beide kunnen niet samen worden gebruikt. De bestandsnaam is hoofdlettergevoelig. |Nee |
| Hiermee worden gedefinieerd |Geef parameters op als sleutel-waardeparen voor verwijzende binnen het Hive-script met behulp van 'hiveconf' |Nee |

Deze eigenschappen zijn specifiek voor de Hive-activiteit. Andere eigenschappen (buiten de sectie typeProperties) worden ondersteund voor alle activiteiten.   

### <a name="json-example"></a>JSON-voorbeeld
De volgende JSON definieert een HDInsight Hive-activiteit in een pijplijn.  

```json
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
    "inputs": [
      {
        "name": "input tables"
      }
    ],
    "outputs": [
      {
        "name": "output tables"
      }
    ],
    "linkedServiceName": "MyHDInsightLinkedService",
    "typeProperties": {
      "script": "Hive script",
      "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
      "defines": {
        "param1": "param1Value"
      }
    },
   "scheduler": {
      "frequency": "Day",
      "interval": 1
    }
}
```

Zie voor meer informatie [Hive-activiteit](data-factory-hive-activity.md) artikel. 

## <a name="hdinsight-pig-activity"></a>HDInsight Pig-activiteit
U kunt de volgende eigenschappen opgeven in een Pig activiteit JSON-definitie. De eigenschap type voor de activiteit moet zijn: **HDInsightPig**. U moet eerst een gekoppelde HDInsight-service maken en geef de naam van het als een waarde voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** wanneer u het type activiteit ingesteld op HDInsightPig sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| Script |Geef de inline Pig-script |Nee |
| scriptpad |De Pig-script opslaat in Azure blob storage en geef het pad naar het bestand. Gebruik de eigenschap 'script' of 'scriptPath'. Beide kunnen niet samen worden gebruikt. De bestandsnaam is hoofdlettergevoelig. |Nee |
| Hiermee worden gedefinieerd |Geef parameters op als sleutel-waardeparen voor verwijzende binnen de Pig-script |Nee |

Deze eigenschappen zijn specifiek voor de Pig-activiteit. Andere eigenschappen (buiten de sectie typeProperties) worden ondersteund voor alle activiteiten.   

### <a name="json-example"></a>JSON-voorbeeld

```json
{
    "name": "HiveActivitySamplePipeline",
      "properties": {
    "activities": [
        {
            "name": "Pig Activity",
            "description": "description",
            "type": "HDInsightPig",
            "inputs": [
                  {
                    "name": "input tables"
                  }
            ],
            "outputs": [
                  {
                    "name": "output tables"
                  }
            ],
            "linkedServiceName": "MyHDInsightLinkedService",
            "typeProperties": {
                  "script": "Pig script",
                  "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                  "defines": {
                    "param1": "param1Value"
                  }
            },
               "scheduler": {
                  "frequency": "Day",
                  "interval": 1
            }
          }
    ]
  }
}
```

Zie voor meer informatie [Pig activiteit](#data-factory-pig-activity.md) artikel. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce-activiteit
U kunt de volgende eigenschappen opgeven in een MapReduce-activiteit JSON-definitie. De eigenschap type voor de activiteit moet zijn: **HDInsightMapReduce**. U moet eerst een gekoppelde HDInsight-service maken en geef de naam van het als een waarde voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** wanneer u het type activiteit ingesteld op HDInsightMapReduce sectie: 

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| jarLinkedService | Naam van de gekoppelde service voor de Azure-opslag die het JAR-bestand bevat. | Ja |
| jarFilePath | Pad naar het JAR-bestand in Azure Storage. | Ja | 
| className | Naam van de belangrijkste klasse in het JAR-bestand. | Ja | 
| Argumenten | Een lijst met door komma's gescheiden argumenten voor het programma MapReduce. Tijdens runtime, ziet u enkele extra argumenten (bijvoorbeeld: mapreduce.job.tags) van het MapReduce-framework. Overweeg het gebruik van zowel de optie als de waarde als argumenten zoals weergegeven in het volgende voorbeeld om te onderscheiden van de argumenten met de argumenten MapReduce, (- s,--invoer,--uitvoer enz., zijn opties onmiddellijk wordt gevolgd door hun waarden) | Nee | 

### <a name="json-example"></a>JSON-voorbeeld

```json
{
    "name": "MahoutMapReduceSamplePipeline",
    "properties": {
        "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calculates an Item Similarity Matrix to determine the similarity between two items",
        "activities": [
            {
                "type": "HDInsightMapReduce",
                "typeProperties": {
                    "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                    "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                    "jarLinkedService": "StorageLinkedService",
                    "arguments": ["-s", "SIMILARITY_LOGLIKELIHOOD", "--input", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input", "--output", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/", "--maxSimilaritiesPerItem", "500", "--tempDir", "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"]
                },
                "inputs": [
                    {
                        "name": "MahoutInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "MahoutOutput"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "MahoutActivity",
                "description": "Custom Map Reduce to generate Mahout result",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-01-03T00:00:00",
        "end": "2017-01-04T00:00:00"
    }
}
```

Zie voor meer informatie [MapReduce-activiteit](data-factory-map-reduce.md) artikel. 

## <a name="hdinsight-streaming-activity"></a>HDInsight-streamingactiviteit
U kunt de volgende eigenschappen opgeven in de definitie van een activiteit-JSON voor Hadoop-Streaming. De eigenschap type voor de activiteit moet zijn: **HDInsightStreaming**. U moet eerst een gekoppelde HDInsight-service maken en geef de naam van het als een waarde voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** wanneer u het type activiteit ingesteld op HDInsightStreaming sectie: 

| Eigenschap | Beschrijving | 
| --- | --- |
| toewijzen | Naam van de uitvoerbare toewijzen. In het voorbeeld is cat.exe de uitvoerbare toewijzen.| 
| reducer | Naam van het uitvoerbare bestand reducer. In het voorbeeld is wc.exe de uitvoerbare reducer. | 
| Invoer | Invoerbestand (inclusief locatie) voor de toewijzing. In het voorbeeld: ' wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt ': adfsample is de blob-container, gegevens-voorbeeld/Gutenberg is de map en davinci.txt is de blob. |
| output | Uitvoerbestand (inclusief locatie) voor de reducer. De uitvoer van de Hadoop-Streaming-taak wordt geschreven naar de locatie die is opgegeven voor deze eigenschap. |
| filePaths | Paden voor de toewijzing en reducer uitvoerbare bestanden. In het voorbeeld: 'adfsample/example/apps/wc.exe' adfsample is de blob-container, bijvoorbeeld/apps is de map en wc.exe is het uitvoerbare bestand. | 
| fileLinkedService | Gekoppelde Azure Storage-service die de Azure-opslag met de bestanden die zijn opgegeven in de sectie filePaths vertegenwoordigt. | 
| Argumenten | Een lijst met door komma's gescheiden argumenten voor het programma MapReduce. Tijdens runtime, ziet u enkele extra argumenten (bijvoorbeeld: mapreduce.job.tags) van het MapReduce-framework. Overweeg het gebruik van zowel de optie als de waarde als argumenten zoals weergegeven in het volgende voorbeeld om te onderscheiden van de argumenten met de argumenten MapReduce, (- s,--invoer,--uitvoer enz., zijn opties onmiddellijk wordt gevolgd door hun waarden) | 
| getDebugInfo | Een optioneel element. Wanneer deze is ingesteld op mislukt, worden de logboeken gedownload alleen bij fouten. Wanneer deze is ingesteld op alle, worden altijd logboeken ongeacht de uitvoeringsstatus gedownload. | 

> [!NOTE]
> U moet een uitvoergegevensset opgeven voor het Hadoop-Streamingactiviteit voor de **levert** eigenschap. Deze gegevensset mag alleen een dummy gegevensset die is vereist voor het station van de planning van de pijplijn (elk uur, dagelijks, enz.). Als de activiteit geen invoer nemen, kunt u overslaan geven een invoergegevensset voor de activiteit voor het **invoer** eigenschap.  

## <a name="json-example"></a>JSON-voorbeeld

```json
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": ["<nameofthecluster>/example/apps/wc.exe","<nameofthecluster>/example/apps/cat.exe"],
                    "fileLinkedService": "StorageLinkedService",
                    "getDebugInfo": "Failure"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-04T00:00:00",
        "end": "2014-01-05T00:00:00"
    }
}
```

Zie voor meer informatie [Hadoop-Streamingactiviteit](data-factory-hadoop-streaming-activity.md) artikel. 

## <a name="hdinsight-spark-activity"></a>HDInsight Spark-activiteit
U kunt de volgende eigenschappen opgeven in een Spark activiteit JSON-definitie. De eigenschap type voor de activiteit moet zijn: **HDInsightSpark**. U moet eerst een gekoppelde HDInsight-service maken en geef de naam van het als een waarde voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** wanneer u het type activiteit ingesteld op HDInsightSpark sectie: 

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| rootPath | De Azure Blob-container en de map waarin het Spark-bestand. De bestandsnaam is hoofdlettergevoelig. | Ja |
| entryFilePath | Relatief pad naar de hoofdmap van het Spark/codepakket. | Ja |
| className | Belangrijkste Java/Spark-klasse van de toepassing | Nee | 
| Argumenten | Een lijst met opdrachtregelargumenten aan het programma Spark. | Nee | 
| proxyUser | De account van de gebruiker te imiteren voor het uitvoeren van het Spark-programma | Nee | 
| sparkConfig | Configuratie-eigenschappen van Spark. | Nee | 
| getDebugInfo | Geeft aan wanneer de Spark-logboekbestanden worden gekopieerd naar de Azure-opslag door HDInsight-cluster gebruikt (of) opgegeven door sparkJobLinkedService. Toegestane waarden: None, altijd of fout. Standaardwaarde: geen. | Nee | 
| sparkJobLinkedService | Azure Storage gekoppelde service die de Spark taakbestand, afhankelijkheden en Logboeken bevat.  Als u een waarde op voor deze eigenschap niet opgeeft, wordt de opslag die is gekoppeld aan de HDInsight-cluster gebruikt. | Nee |

### <a name="json-example"></a>JSON-voorbeeld

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-05T00:00:00",
        "end": "2017-02-06T00:00:00"
    }
}
```
Houd rekening met de volgende punten: 

- De **type** eigenschap is ingesteld op **HDInsightSpark**.
- De **rootPath** is ingesteld op **adfspark\\pyFiles** waarbij adfspark Azure Blob-container en pyFiles fijn map in de container. In dit voorbeeld is de Azure Blob-opslag die is gekoppeld aan het Spark-cluster. U kunt het bestand uploaden naar een andere Azure-opslag. Als u doet dit, maakt u een gekoppelde Azure Storage-service als u wilt dat storage-account koppelen aan de gegevensfactory. Geef de naam van de gekoppelde service als een waarde voor de **sparkJobLinkedService** eigenschap. Zie [Spark activiteitseigenschappen](#spark-activity-properties) voor meer informatie over deze eigenschap en andere eigenschappen die ondersteund worden door de activiteit Spark.
- De **entryFilePath** is ingesteld op de **test.py**, namelijk het python-bestand. 
- De **getDebugInfo** eigenschap is ingesteld op **altijd**, wat betekent dat de logboekbestanden altijd zijn gegenereerd (slagen of mislukken).  

    > [!IMPORTANT]
    > U wordt aangeraden dat u niet deze eigenschap op altijd in een productieomgeving instelt tenzij u een probleem wilt oplossen. 
- De **levert** sectie heeft één uitvoergegevensset. U kunt een uitvoergegevensset moet opgeven, zelfs als het programma spark geen uitvoer produceert. De uitvoergegevensset stations de planning voor de pijplijn (elk uur, dagelijks, enz.).

Zie voor meer informatie over de activiteit [Spark activiteit](data-factory-spark.md) artikel.  

## <a name="machine-learning-batch-execution-activity"></a>Machine Learning-batchuitvoeringsactiviteit
U kunt de volgende eigenschappen opgeven in een Azure ML-Batch uitvoering activiteit JSON-definitie. De eigenschap type voor de activiteit moet zijn: **AzureMLBatchExecution**. U moet maken van een Azure Machine Learning gekoppelde service eerst en geef de naam van het als een waarde voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** wanneer u het type activiteit ingesteld op AzureMLBatchExecution sectie:

Eigenschap | Beschrijving | Vereist 
-------- | ----------- | --------
webServiceInput | De gegevensset worden doorgegeven als invoer voor de webservice Azure ML. Deze gegevensset moet ook zijn opgenomen in de invoer voor de activiteit. |WebServiceInput of webServiceInputs gebruiken. | 
webServiceInputs | Gegevenssets worden doorgegeven als invoer voor de webservice Azure ML opgeven. Als de web-service meerdere invoer heeft, gebruikt u de eigenschap webServiceInputs in plaats van de eigenschap webServiceInput. Gegevenssets waarnaar wordt verwezen door de **webServiceInputs** moet ook zijn opgenomen in de activiteit **invoer**. | WebServiceInput of webServiceInputs gebruiken. | 
webServiceOutputs | De gegevenssets die zijn toegewezen als uitvoer voor de webservice Azure ML. De webservice retourneert uitvoergegevens in deze dataset. | Ja | 
globalParameters | Geef waarden voor parameters van de webservice in deze sectie. | Nee | 

### <a name="json-example"></a>JSON-voorbeeld
In dit voorbeeld wordt de activiteit heeft de gegevensset **MLSqlInput** als invoer en **MLSqlOutput** als uitvoer. De **MLSqlInput** wordt doorgegeven als invoer voor de webservice door met de **webServiceInput** JSON-eigenschap. De **MLSqlOutput** wordt doorgegeven als uitvoer met de webservice door met de **webServiceOutputs** JSON-eigenschap. 

```json
{
   "name": "MLWithSqlReaderSqlWriter",
   "properties": {
      "description": "Azure ML model with sql azure reader/writer",
      "activities": [{
         "name": "MLSqlReaderSqlWriterActivity",
         "type": "AzureMLBatchExecution",
         "description": "test",
         "inputs": [ { "name": "MLSqlInput" }],
         "outputs": [ { "name": "MLSqlOutput" } ],
         "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
         "typeProperties":
         {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
               "output1": "MLSqlOutput"
            },
            "globalParameters": {
               "Database server name": "<myserver>.database.windows.net",
               "Database name": "<database>",
               "Server user account name": "<user name>",
               "Server user account password": "<password>"
            }              
         },
         "policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 1,
            "timeout": "02:00:00"
         }
      }],
      "start": "2016-02-13T00:00:00",
       "end": "2016-02-14T00:00:00"
   }
}
```

In het JSON-voorbeeld gebruikt de geïmplementeerde Azure Machine Learning-webservice een lezer en een schrijfmodule tot lezen/schrijven van gegevens van/naar een Azure SQL Database. Deze webservice beschrijft de volgende vier parameters: servernaam, databasenaam, servergebruikersnaam en wachtwoord voor gebruikersaccount Server-Database.

> [!NOTE]
> Alleen invoer en uitvoer van de activiteit AzureMLBatchExecution kunnen als parameters worden doorgegeven aan de webservice. In de bovenstaande JSON-codefragment is MLSqlInput bijvoorbeeld invoer voor de activiteit AzureMLBatchExecution, die wordt doorgegeven als invoer voor de webservice via webServiceInput-parameter.

## <a name="machine-learning-update-resource-activity"></a>Machine Learning-activiteit resources bijwerken
U kunt de volgende eigenschappen opgeven in een Azure ML Update Resource activiteit JSON-definitie. De eigenschap type voor de activiteit moet zijn: **AzureMLUpdateResource**. U moet maken van een Azure Machine Learning gekoppelde service eerst en geef de naam van het als een waarde voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** sectie bij het instellen van het type activiteit voor AzureMLUpdateResource:

Eigenschap | Beschrijving | Vereist 
-------- | ----------- | --------
trainedModelName | De naam van het retrained model. | Ja |  
trainedModelDatasetName | De gegevensset die verwijst naar het iLearner-bestand dat is geretourneerd door de retraining bewerking. | Ja | 

### <a name="json-example"></a>JSON-voorbeeld
De pijplijn heeft twee activiteiten: **AzureMLBatchExecution** en **AzureMLUpdateResource**. De activiteit van de Azure ML-Batchuitvoering duurt de trainingsgegevens als invoer en een iLearner-bestand als uitvoer produceert. De activiteit roept de training webservice (trainingsexperiment weergegeven als een webservice) met de invoer trainingsgegevens en ontvangt van het ilearner-bestand van de webservice. De placeholderBlob is een dummy output dataset is vereist door de Azure Data Factory-service voor het uitvoeren van de pijplijn.


```json
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "trained model",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [{ "name": "trainedModelBlob" }],
                "outputs": [{ "name": "placeholderBlob" }],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00",
        "end": "2016-02-14T00:00:00"
    }
}
```

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL-activiteit
U kunt de volgende eigenschappen opgeven in een U-SQL-activiteit JSON-definitie. De eigenschap type voor de activiteit moet zijn: **DataLakeAnalyticsU SQL**. Moet u een Azure Data Lake Analytics gekoppelde service maken en geef de naam van het als een waarde voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** sectie bij het instellen van het type activiteit naar DataLakeAnalyticsU SQL: 

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| scriptPath |Pad naar de map waarin de U-SQL-script. Naam van het bestand is hoofdlettergevoelig. |Nee (als u een script gebruiken) |
| scriptLinkedService |Gekoppelde service die is gekoppeld aan de opslag die het script aan de gegevensfactory bevat |Nee (als u een script gebruiken) |
| Script |Geef in plaats van het opgeven van scriptPath en scriptLinkedService inline-script. Bijvoorbeeld: 'script': 'Test DATABASE maken'. |Nee (als u scriptPath en scriptLinkedService gebruiken) |
| degreeOfParallelism |Het maximum aantal knooppunten dat tegelijk worden gebruikt voor het uitvoeren van de taak. |Nee |
| prioriteit |Hiermee wordt bepaald welke taken uit in de wachtrij moeten eerst worden geselecteerd. Hoe lager het getal, hoe hoger de prioriteit. |Nee |
| parameters |Parameters voor de U-SQL-script |Nee |

### <a name="json-example"></a>JSON-voorbeeld

```json
{
    "name": "ComputeEventsByRegionPipeline",
    "properties": {
        "description": "This pipeline computes events for en-gb locale and date less than Feb 19, 2012.",
        "activities": 
        [
            {
                "type": "DataLakeAnalyticsU-SQL",
                "typeProperties": {
                    "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                    "scriptLinkedService": "StorageLinkedService",
                    "degreeOfParallelism": 3,
                    "priority": 100,
                    "parameters": {
                        "in": "/datalake/input/SearchLog.tsv",
                        "out": "/datalake/output/Result.tsv"
                    }
                },
                "inputs": [
                    {
                        "name": "DataLakeTable"
                    }
                ],
                "outputs": 
                [
                    {
                        "name": "EventsByRegionTable"
                    }
                ],
                "policy": {
                    "timeout": "06:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "EventsByRegion",
                "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
            }
        ],
        "start": "2015-08-08T00:00:00",
        "end": "2015-08-08T01:00:00",
        "isPaused": false
    }
}
```

Zie voor meer informatie [Data Lake Analytics U-SQL-activiteit](data-factory-usql-activity.md). 

## <a name="stored-procedure-activity"></a>Opgeslagen procedureactiviteit
U kunt de volgende eigenschappen opgeven in een opgeslagen Procedure activiteit JSON-definitie. De eigenschap type voor de activiteit moet zijn: **SqlServerStoredProcedure**. U moet een van de volgende gekoppelde services maken en de naam van de gekoppelde service opgeven als een waarde op voor de **linkedServiceName** eigenschap:

- SQL Server 
- Azure SQL Database
- Azure SQL Data Warehouse

De volgende eigenschappen worden ondersteund in de **typeProperties** wanneer u het type activiteit ingesteld op SqlServerStoredProcedure sectie:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| storedProcedureName |Geef de naam van de opgeslagen procedure in de Azure SQL database of Azure SQL Data Warehouse die wordt vertegenwoordigd door de gekoppelde service die gebruikmaakt van de uitvoertabel. |Ja |
| storedProcedureParameters |Geef waarden op voor parameters van opgeslagen procedure. Als u null zijn voor een parameter doorgeven moet, gebruikt u de syntaxis: "param1": null (alle kleine letter). Zie het volgende voorbeeld voor meer informatie over het gebruik van deze eigenschap. |Nee |

Als u een invoergegevensset opgeeft, moet dit beschikbaar is (in de status 'Gereed') voor de activiteit opgeslagen procedure moet worden uitgevoerd. Invoergegevensset kan niet worden gebruikt in de opgeslagen procedure als een parameter. Het wordt alleen gebruikt om te controleren van de afhankelijkheid voordat u de activiteit opgeslagen procedure start. U moet een uitvoergegevensset voor een activiteit opgeslagen procedure opgeven. 

Uitvoergegevensset geeft de **planning** voor de activiteit opgeslagen procedure (elk uur, wekelijks, maandelijks, enzovoort). De uitvoergegevensset moet gebruiken een **gekoppelde service** die verwijst naar een Azure SQL Database of een Azure SQL Data Warehouse of een SQL Server-Database die u wilt de opgeslagen procedure om uit te voeren. De uitvoergegevensset kan fungeren als een manier om door te geven het resultaat van de opgeslagen procedure voor verdere verwerking door een andere activiteit ([activiteiten chaining](data-factory-scheduling-and-execution.md##multiple-activities-in-a-pipeline)) in de pijplijn. Echter, Data Factory automatisch schrijft niet de uitvoer van een opgeslagen procedure bij deze dataset. De opgeslagen procedure die naar een SQL-tabel die de uitvoergegevensset die naar verwijst schrijft is. In sommige gevallen kan de uitvoergegevensset mag een **dummy gegevensset**, die wordt alleen gebruikt om de planning voor het uitvoeren van de activiteit opgeslagen procedure opgeven.  

### <a name="json-example"></a>JSON-voorbeeld

```json
{
    "name": "SprocActivitySamplePipeline",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                    }
                },
                "outputs": [{ "name": "sprocsampleout" }],
                "name": "SprocActivitySample"
            }
        ],
         "start": "2016-08-02T00:00:00",
         "end": "2016-08-02T05:00:00",
        "isPaused": false
    }
}
```

Zie voor meer informatie [activiteit opgeslagen Procedure](data-factory-stored-proc-activity.md) artikel. 

## <a name="net-custom-activity"></a>Aangepaste .NET-activiteit
U kunt de volgende eigenschappen opgeven in een aangepaste activiteit .NET JSON-definitie. De eigenschap type voor de activiteit moet zijn: **DotNetActivity**. U moet een gekoppelde HDInsight-service maken of een Azure Batch gekoppelde service en de naam van de gekoppelde service opgeven als een waarde op voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** wanneer u het type activiteit ingesteld op DotNetActivity sectie:
 
| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| AssemblyName | De naam van de assembly. In het voorbeeld is: **MyDotnetActivity.dll**. | Ja |
| EntryPoint |De naam van de klasse die de IDotNetActivity-interface implementeert. In het voorbeeld is: **MyDotNetActivityNS.MyDotNetActivity** waarbij MyDotNetActivityNS is de naamruimte en MyDotNetActivity is de klasse.  | Ja | 
| PackageLinkedService | De naam van de gekoppelde Azure Storage-service die verwijst naar de blobopslag dat het zip-bestand voor aangepaste activiteit bevat. In het voorbeeld is: **AzureStorageLinkedService**.| Ja |
| PackageFile | Naam van het zip-bestand. In het voorbeeld is: **customactivitycontainer/MyDotNetActivity.zip**. | Ja |
| extendedProperties | Uitgebreide eigenschappen die u kunt definiëren en doorgeven aan de .NET-code. In dit voorbeeld wordt de **SliceStart** variabele is ingesteld op een waarde op basis van de SliceStart system-variabele. | Nee | 

### <a name="json-example"></a>JSON-voorbeeld

```json
{
  "name": "ADFTutorialPipelineCustom",
  "properties": {
    "description": "Use custom activity",
    "activities": [
      {
        "Name": "MyDotNetActivity",
        "Type": "DotNetActivity",
        "Inputs": [
          {
            "Name": "InputDataset"
          }
        ],
        "Outputs": [
          {
            "Name": "OutputDataset"
          }
        ],
        "LinkedServiceName": "AzureBatchLinkedService",
        "typeProperties": {
          "AssemblyName": "MyDotNetActivity.dll",
          "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
          "PackageLinkedService": "AzureStorageLinkedService",
          "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
          "extendedProperties": {
            "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
          }
        },
        "Policy": {
          "Concurrency": 2,
          "ExecutionPriorityOrder": "OldestFirst",
          "Retry": 3,
          "Timeout": "00:30:00",
          "Delay": "00:00:00"
        }
      }
    ],
    "start": "2016-11-16T00:00:00",
    "end": "2016-11-16T05:00:00",
    "isPaused": false
  }
}
```

Zie voor gedetailleerde informatie [gebruik van aangepaste activiteiten in de Data Factory](data-factory-use-custom-activities.md) artikel. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende zelfstudies: 

- [Zelfstudie: een pijplijn maken met een kopieeractiviteit](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Zelfstudie: een pijplijn maken met een hive-activiteit](data-factory-build-your-first-pipeline-using-editor.md)
