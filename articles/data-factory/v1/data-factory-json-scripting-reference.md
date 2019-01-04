---
title: Azure Data Factory - JSON-scriptverwerking voor referentie | Microsoft Docs
description: JSON-schema biedt voor Data Factory-entiteiten.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: c61612bad181eb600f449fea7eb22ca2abc17a12
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54020452"
---
# <a name="azure-data-factory---json-scripting-reference"></a>Azure Data Factory - JSON-scriptverwerking van verwijzing
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory.


In dit artikel bevat JSON-schema's en voorbeelden voor het definiëren van Azure Data Factory-entiteiten (pipeline, activiteit, gegevensset en gekoppelde service).  

## <a name="pipeline"></a>Pijplijn 
De structuur van de op hoog niveau voor de pijplijndefinitie van een is als volgt: 

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

| Eigenschap | Description | Vereist
-------- | ----------- | --------
| naam | Naam van de pijplijn. Geef een naam op die staat voor de actie die de activiteit of de pijplijn is geconfigureerd om te doen<br/><ul><li>Maximum aantal tekens: 260</li><li>Moet beginnen met een letter, cijfer of onderstrepingsteken (_)</li><li>De volgende tekens zijn niet toegestaan: '. ', '+ ','?', '/', '< ',' >', ' * ', '%', '&', ': ','\\"</li></ul> |Ja |
| description |Beschrijving van het doel waarvoor de activiteit of een pijplijn wordt gebruikt | Nee |
| activities | Bevat een lijst van activiteiten. | Ja |
| start |Startdatum en-tijd voor de pijplijn. Moet zich in [ISO-indeling](http://en.wikipedia.org/wiki/ISO_8601). Bijvoorbeeld: 2014-10-14T16:32:41. <br/><br/>Het is mogelijk om op te geven een plaatselijke tijd, bijvoorbeeld een geschatte tijd. Hier volgt een voorbeeld: `2016-02-27T06:00:00**-05:00`, die is 6 AM EST.<br/><br/>De begin- en -eigenschappen opgeven samen actieve periode voor de pijplijn. Uitvoersegmenten worden alleen gemaakt met deze actieve periode. |Nee<br/><br/>Als u een waarde voor de end-eigenschap opgeeft, moet u de waarde voor de eigenschap QueryDefinition opgeven.<br/><br/>De begin- en eindtijden kunnen beide niet leeg zijn om een pijplijn te maken. U moet beide waarden om in te stellen van een actieve periode voor de pijplijn om uit te voeren. Als u geen begin- en eindtijden opgeeft wanneer u een pijplijn maakt, kunt u instellen met behulp van de cmdlet Set-AzureRmDataFactoryPipelineActivePeriod later opnieuw. |
| einde |Einddatum en-tijd voor de pijplijn. Als u opgeeft moet zich in de ISO-indeling. Bijvoorbeeld: 2014-10-14T17:32:41 <br/><br/>Het is mogelijk om op te geven een plaatselijke tijd, bijvoorbeeld een geschatte tijd. Hier volgt een voorbeeld: `2016-02-27T06:00:00**-05:00`, die is 6 AM EST.<br/><br/>Als u wilt de pijplijn voor onbepaalde tijd worden uitgevoerd, geef 9999-09-09 als de waarde voor de end-eigenschap. |Nee <br/><br/>Als u een waarde voor de eigenschap QueryDefinition opgeeft, moet u de waarde voor de end-eigenschap opgeven.<br/><br/>Zie de opmerkingen voor de **start** eigenschap. |
| isPaused |Als is ingesteld op waar de pijplijn wordt niet uitgevoerd. Standaardwaarde = false. Deze eigenschap kunt u in- of uitschakelen. |Nee |
| pipelineMode |De methode voor de planning wordt uitgevoerd voor de pijplijn. Toegestane waarden zijn: (standaard), gepland eenmalig.<br/><br/>'Gepland' geeft aan dat de pijplijn met een opgegeven tijdsinterval op basis van de actieve periode (begin- en -tijd) wordt uitgevoerd. 'Eenmalige' geeft aan dat de pijplijn slechts één keer wordt uitgevoerd. Eenmalige pijplijnen eenmaal gemaakt kunnen niet worden gewijzigd/bijgewerkt op dit moment. Zie [Onetime pijplijn](data-factory-create-pipelines.md#onetime-pipeline) voor meer informatie over eenmalige instelling. |Nee |
| expirationTime |Duur van de tijd na het maken van waarvoor de pijplijn is geldig en ingericht moet blijven. Als er geen een actieve, is mislukt, of in behandeling wordt uitgevoerd, de pijplijn wordt automatisch verwijderd wanneer het de vervaltijd is bereikt. |Nee |


## <a name="activity"></a>Activiteit 
De structuur van de op hoog niveau voor een activiteit in de definitie van een pijplijn (activiteiten element) is als volgt:

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

Na de tabel door de eigenschappen in de JSON-definitie van de activiteit te beschrijven:

| Label | Description | Vereist |
| --- | --- | --- |
| naam |De naam van de activiteit. Geef een naam op die staat voor de actie die de activiteit is geconfigureerd<br/><ul><li>Maximum aantal tekens: 260</li><li>Moet beginnen met een letter, cijfer of onderstrepingsteken (_)</li><li>De volgende tekens zijn niet toegestaan: '. ', '+ ','?', '/', '< ',' >', ' * ', '%', '&', ': ','\\"</li></ul> |Ja |
| description |Beschrijving van het doel waarvoor de activiteit wordt gebruikt. |Nee |
| type |Hiermee geeft u het type van de activiteit. Zie de [GEGEVENSARCHIEVEN](#data-stores) en [activiteiten voor GEGEVENSTRANSFORMATIE](#data-transformation-activities) secties voor verschillende soorten activiteiten. |Ja |
| invoer |Invoer tabellen die worden gebruikt door de activiteit<br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |Nee voor HDInsightStreaming en SqlServerStoredProcedure activiteiten <br/> <br/> Ja voor alle andere |
| uitvoer |Uitvoer tabellen die worden gebruikt door de activiteit.<br/><br/>`// one output table`<br/>`"outputs":  [ { "name": “outputtable1” } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": “outputtable1” }, { "name": “outputtable2” }  ],` |Ja |
| linkedServiceName |De naam van de gekoppelde service die door de activiteit wordt gebruikt. <br/><br/>Een activiteit kan vereisen dat u de gekoppelde service opgeeft die is gekoppeld aan de vereiste rekenomgeving. |Ja voor HDInsight-activiteiten, Azure Machine Learning-activiteiten en activiteit opgeslagen Procedure. <br/><br/>Nee voor alle andere |
| typeProperties |Eigenschappen in de sectie typeProperties zijn afhankelijk van type van de activiteit. |Nee |
| policy |Beleidsregels die van invloed zijn op het runtimegedrag van de activiteit. Als deze niet is opgegeven, worden de standaardbeleidsregels gebruikt. |Nee |
| Scheduler |''-plannereigenschap wordt gebruikt voor het definiëren van de gewenste planning voor de activiteit. De subeigenschappen zijn hetzelfde als blobs in de [beschikbaarheid-eigenschap in een gegevensset](data-factory-create-datasets.md#dataset-availability). |Nee |

### <a name="policies"></a>Beleidsregels
Beleid van invloed op het runtimegedrag van een activiteit, specifiek wanneer het segment de status van een tabel wordt verwerkt. De volgende tabel bevat de details.

| Eigenschap | Toegestane waarden | Standaardwaarde | Description |
| --- | --- | --- | --- |
| gelijktijdigheid van taken |Geheel getal <br/><br/>De maximale waarde: 10 |1 |Het aantal gelijktijdige uitvoeringen van de activiteit.<br/><br/>Bepaalt het aantal parallelle activiteit uitvoeringen die kunnen ontstaan op verschillende segmenten. Bijvoorbeeld, als een activiteit moet doorlopen versnelt een groot aantal beschikbare gegevens, met een grotere waarde voor gelijktijdigheid de verwerking van gegevens. |
| executionPriorityOrder |NewestFirst<br/><br/>OldestFirst |OldestFirst |Bepaalt de volgorde van de gegevenssegmenten dat wordt verwerkt.<br/><br/>Als er 2 segmenten (één gebeurt: 00 uur en een andere naam om 17: 00) en beide uitvoering in behandeling zijn. Als u de executionPriorityOrder NewestFirst worden ingesteld, wordt het segment om 17: 00 eerst verwerkt. Op dezelfde manier als u de executionPriorityORder OldestFIrst worden ingesteld, klikt u vervolgens het segment: 00 uur is verwerkt. |
| retry |Geheel getal<br/><br/>De maximale waarde is 10 |0 |Het aantal nieuwe pogingen voordat de verwerking van gegevens voor het segment is gemarkeerd als fout. De uitvoeringsomgeving van de activiteit van een gegevenssegment is opnieuw uitgevoerd tot het opgegeven aantal nieuwe pogingen. De nieuwe poging wordt gedaan zo snel mogelijk na de fout. |
| timeout |TimeSpan |00:00:00 |Een time-out opgetreden voor de activiteit. Voorbeeld: 00:10:00 (impliceert time-out 10 minuten)<br/><br/>Als een waarde niet opgegeven is of is ingesteld op 0, wordt de time-out is oneindig.<br/><br/>Als de verwerkingstijd van de gegevens op een segment de time-outwaarde overschrijdt, wordt geannuleerd en het systeem probeert om opnieuw te proberen de verwerking. Het aantal nieuwe pogingen, is afhankelijk van de eigenschap opnieuw proberen. Wanneer een time-out optreedt, wordt de status is ingesteld op time-out. |
| vertraging |TimeSpan |00:00:00 |Geef de vertraging voor gegevensverwerking van het segment wordt gestart.<br/><br/>De uitvoering van activiteit van een gegevenssegment wordt gestart nadat de vertraging de verwachte tijd voor uitvoering in het verleden is.<br/><br/>Voorbeeld: 00:10:00 (betekent vertraging van 10 minuten) |
| longRetry |Geheel getal<br/><br/>De maximale waarde: 10 |1 |Het aantal pogingen lang voordat de segment-uitvoering is mislukt.<br/><br/>longRetry pogingen zijn door longRetryInterval verdeeld. Dus als u nodig hebt om op te geven van een tijd tussen nieuwe pogingen, longRetry gebruiken. Als zowel nieuwe pogingen als longRetry zijn opgegeven, elke poging longRetry bevat nieuwe pogingen en het maximale aantal pogingen wordt opnieuw geprobeerd * longRetry.<br/><br/>Bijvoorbeeld, als we de volgende instellingen hebben in de beleidsregels voor activiteiten:<br/>Opnieuw uitvoeren: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00 uur<br/><br/>Wordt ervan uitgegaan dat er wordt slechts één segment om uit te voeren (status is in afwachting van) en de activiteit is uitgevoerd elke keer mislukt. Er zou worden in eerste instantie 3 opeenvolgende uitvoeringspogingen. Na elke poging zou de segmentstatus opnieuw proberen. Nadat de eerste 3 pogingen staan, kan het de segmentstatus LongRetry zou zijn.<br/><br/>Na een uur (dat wil zeggen, de longRetryInteval waarde) wordt een andere set 3 opeenvolgende uitvoeringspogingen. Hierna zou het de segmentstatus mislukt en geen pogingen meer zou worden uitgevoerd. Daarom algemene 6 pogingen zijn gedaan.<br/><br/>Als een uitvoering is geslaagd, het de segmentstatus gereed zijn en geen pogingen meer worden geprobeerd.<br/><br/>longRetry kan worden gebruikt in situaties waarin afhankelijke gegevens binnenkomen op niet-deterministisch tijdstippen of de totale omgeving is flaky onder welke gegevensverwerking plaatsvindt. In dergelijke gevallen tijd nieuwe pogingen na elkaar niet kunt doen, en dit na een interval van resultaten in de gewenste uitvoer.<br/><br/>Waarschuwing: hoge waarde voor longRetry of longRetryInterval niet instelt. Normaal gesproken dat andere systematische problemen hoofdgeheugen worden bij hogere waarden. |
| longRetryInterval |TimeSpan |00:00:00 |De vertraging tussen pogingen lang opnieuw proberen |

### <a name="typeproperties-section"></a>sectie typeProperties
De sectie typeProperties verschilt voor elke activiteit. Activiteiten voor gegevenstransformatie hebben alleen de type-eigenschappen. Zie [activiteiten voor GEGEVENSTRANSFORMATIE](#data-transformation-activities) sectie in dit artikel voor JSON-voorbeelden die activiteiten voor gegevenstransformatie in een pijplijn definieert. 

**Kopieeractiviteit** heeft twee subsecties in de sectie typeProperties: **bron** en **sink**. Zie [GEGEVENSARCHIEVEN](#data-stores) sectie in dit artikel voor JSON-voorbeelden die laten Zie hoe u een data store-als een bron en/of de sink. 

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

Zie [GEGEVENSARCHIEVEN](#data-stores) sectie in dit artikel voor JSON-voorbeelden die laten Zie hoe u een data store-als een bron en/of de sink.    

Zie voor een volledige procedure voor het maken van deze pijplijn, [zelfstudie: Gegevens kopiëren van Blob Storage naar SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

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

Zie [activiteiten voor GEGEVENSTRANSFORMATIE](#data-transformation-activities) sectie in dit artikel voor JSON-voorbeelden die activiteiten voor gegevenstransformatie in een pijplijn definieert.

Zie voor een volledige procedure voor het maken van deze pijplijn, [zelfstudie: Uw eerste pijplijn om gegevens te verwerken met behulp van Hadoop-cluster maken](data-factory-build-your-first-pipeline.md). 

## <a name="linked-service"></a>Gekoppelde service
De structuur van de op hoog niveau voor de definitie van een gekoppelde service is als volgt:

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

Na de tabel door de eigenschappen in de JSON-definitie van de activiteit te beschrijven:

| Eigenschap | Description | Vereist |
| -------- | ----------- | -------- | 
| naam | De naam van de gekoppelde service. | Ja | 
| Eigenschappen - type | Het type van de gekoppelde service. Bijvoorbeeld: Azure Storage, Azure SQL Database. |
| typeProperties | De sectie typeProperties heeft-elementen die zijn verschillend voor elk gegevensarchief of compute-omgeving. Zie [gegevensarchieven](#datastores) sectie voor alle gegevens store gekoppelde services en [omgevingen compute](#compute-environments) voor alle compute gekoppelde services |   

## <a name="dataset"></a>Gegevensset 
Een gegevensset in Azure Data Factory wordt als volgt gedefinieerd:

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

| Eigenschap | Description | Vereist | Standaard |
| --- | --- | --- | --- |
| naam | Naam van de gegevensset. Zie [Azure Data Factory - naamgevingsregels](data-factory-naming-rules.md) voor naamgevingsregels. |Ja |N.v.t. |
| type | Het type van de gegevensset. Geef een van de typen die worden ondersteund door Azure Data Factory (bijvoorbeeld: AzureBlob, AzureSqlTable). Zie [GEGEVENSARCHIEVEN](#data-stores) sectie voor alle gegevensarchieven en de gegevensset die worden ondersteund door Data Factory. | 
| structuur | Het schema van de gegevensset. Het bevat kolommen, de typen, enzovoort. | Nee |N.v.t. |
| typeProperties | Eigenschappen die overeenkomen met het geselecteerde type. Zie [GEGEVENSARCHIEVEN](#data-stores) sectie voor de ondersteunde typen en de bijbehorende eigenschappen. |Ja |N.v.t. |
| external | Booleaanse vlag om op te geven of een gegevensset expliciet wordt geproduceerd door een data factory-pijplijn of niet. |Nee |false |
| availability | Definieert het segmenteringsmodel of het segmenteringshulplijnen model voor de gegevensset voor productie. Zie voor meer informatie over de gegevensset voor het verwerkingsvenster, [planning en uitvoering](data-factory-scheduling-and-execution.md) artikel. |Ja |N.v.t. |
| policy |Definieert de criteria of de voorwaarde die moeten voldoen aan de gegevensset segmenten. <br/><br/>Zie voor meer informatie, [gegevensset beleid](#Policy) sectie. |Nee |N.v.t. |

Elke kolom in de **structuur** sectie bevat de volgende eigenschappen:

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| naam |Naam van de kolom. |Ja |
| type |Het gegevenstype van de kolom.  |Nee |
| cultuur |.NET gebaseerde cultuur moet worden gebruikt wanneer het type is opgegeven en .NET-type `Datetime` of `Datetimeoffset`. De standaardwaarde is `en-us`. |Nee |
| Indeling |Tekenreeks die moet worden gebruikt bij het type is opgegeven en .NET-type-indeling `Datetime` of `Datetimeoffset`. |Nee |

In het volgende voorbeeld wordt de gegevensset heeft drie kolommen `slicetimestamp`, `projectname`, en `pageviews` en ze zijn van het type: String, String en Decimal respectievelijk.

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

De volgende tabel beschrijft de eigenschappen die u kunt gebruiken in de **beschikbaarheid** sectie:

| Eigenschap | Description | Vereist | Standaard |
| --- | --- | --- | --- |
| frequency |Hiermee geeft u de tijdseenheid voor de gegevensset segment productie.<br/><br/><b>Ondersteunde frequentie</b>: Minuut, uur, dag, Week, maand |Ja |N.v.t. |
| interval |Hiermee geeft u een vermenigvuldiger voor de frequentie<br/><br/>"X Synchronisatiefrequentie-interval" bepaalt hoe vaak het segment wordt geproduceerd.<br/><br/>Als u de gegevensset om te worden gesegmenteerd op uurbasis, stelt u <b>frequentie</b> naar <b>uur</b>, en <b>interval</b> naar <b>1</b>.<br/><br/><b>Opmerking</b>: Als u de frequentie als minuut opgeeft, wordt aangeraden dat u het interval ingesteld op niet kleiner zijn dan 15 |Ja |N.v.t. |
| stijl |Hiermee geeft u op of het segment aan het begin/einde van het interval moet worden gemaakt.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Als Frequency wordt ingesteld op maand en stijl is ingesteld op EndOfInterval, wordt het segment op de laatste dag van maand geproduceerd. Als de stijl is ingesteld op StartOfInterval, wordt het segment op de eerste dag van maand geproduceerd.<br/><br/>Als Frequency wordt ingesteld op de dag en stijl is ingesteld op EndOfInterval, kan het segment in het afgelopen uur van de dag wordt geproduceerd.<br/><br/>Als Frequency wordt ingesteld op uur en stijl is ingesteld op EndOfInterval, wordt het segment wordt geproduceerd aan het einde van het uur. Het segment wordt bijvoorbeeld voor een segment voor 1-2 uur periode worden geproduceerd om 2 uur. |Nee |EndOfInterval |
| anchorDateTime |Hiermee definieert u de absolute positie in de tijd die door de scheduler wordt gebruikt voor het berekenen van de grenzen van de gegevensset-segment. <br/><br/><b>Opmerking</b>: De gedetailleerdere onderdelen worden genegeerd als de AnchorDateTime heeft de datumonderdelen die gedetailleerder zijn dan de frequentie zijn. <br/><br/>Bijvoorbeeld, als de <b>interval</b> is <b>per uur</b> (frequentie: uur en interval: (1) en de <b>AnchorDateTime</b> bevat <b>minuten en seconden</b> dan zal de <b>minuten en seconden</b> onderdelen van de AnchorDateTime worden genegeerd. |Nee |01/01/0001 |
| offset |TimeSpan waarmee het begin en einde van alle segmenten van de gegevensset worden verschoven. <br/><br/><b>Opmerking</b>: Als zowel anchorDateTime en offset zijn opgegeven, is het resultaat van de gecombineerde verschuiving. |Nee |N.v.t. |

De volgende sectie van de beschikbaarheid geeft aan dat de uitvoergegevensset geproduceerd per uur (of) invoer is gegevensset beschikbaar is per uur:

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

De **beleid** sectie in het definitie van de gegevensset worden gedefinieerd de criteria of de voorwaarde die moeten voldoen aan de gegevensset segmenten.

| Policy Name | Description | Toegepast op | Vereist | Standaard |
| --- | --- | --- | --- | --- |
| minimumSizeMB |Valideert dat de gegevens in een **Azure blob** voldoet aan de minimale grootte (in MB). |Azure Blob |Nee |N.v.t. |
| minimumRows |Valideert dat de gegevens in een **Azure SQL-database** of een **Azure table** het minimum aantal rijen bevat. |<ul><li>Azure SQL Database</li><li>Azure Table</li></ul> |Nee |N.v.t. |

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

Tenzij een gegevensset wordt geproduceerd door Azure Data Factory, deze moet worden gemarkeerd als **externe**. Deze instelling in het algemeen geldt voor de invoer van de eerste activiteit in een pijplijn, tenzij de activiteit of het koppelen van de pijplijn wordt gebruikt.

| Name | Description | Vereist | Standaardwaarde |
| --- | --- | --- | --- |
| dataDelay |De tijd te stellen de controle van de beschikbaarheid van de externe gegevens voor het opgegeven segment. Bijvoorbeeld, als de gegevens per uur beschikbaar is, kan de controle om te zien van de externe gegevens zijn beschikbaar en het bijbehorende segment is gereed kan worden uitgesteld met behulp van dataDelay.<br/><br/>Geldt alleen voor de huidige tijd.  Bijvoorbeeld, als het is nu om 13:00 uur en deze waarde 10 minuten is, begint de validatie om 1:10 uur.<br/><br/>Deze instelling heeft geen invloed op segmenten in het verleden (segmenten met de eindtijd van segment + dataDelay < nu) worden verwerkt zonder enige vertraging.<br/><br/>Dat groter is dan 23:59 uur wilt opgeven met de `day.hours:minutes:seconds` indeling. Bijvoorbeeld, om op te geven 24 uur, gebruik geen 24:00:00; Gebruik in plaats daarvan 1.00:00:00. Als u 24:00:00, wordt dit beschouwd als 24 dagen (24.00:00:00). Geef 1:04:00:00 1 dag en vier uur. |Nee |0 |
| retryInterval |De wachttijd tussen een storing en de volgende nieuwe poging. Als een try mislukt, is de volgende poging na retryInterval. <br/><br/>Als het nu om 13:00 uur, beginnen we de eerste poging. Als de duur van de eerste van validatiecontrole is 1 minuut en de bewerking is mislukt, de volgende poging is 1:00 + 1 min (duur) + 1 minuut (interval voor opnieuw proberen) = 13:02 uur. <br/><br/>Er is geen vertraging voor segmenten in het verleden. De nieuwe poging gebeurt onmiddellijk. |Nee |00:01:00 uur (1 minuut) |
| retryTimeout |De time-out voor elke nieuwe poging.<br/><br/>Als deze eigenschap is ingesteld op 10 minuten, moet de validatie binnen 10 minuten worden voltooid. Als het duurt langer dan 10 minuten duren om uit te voeren van de validatie, wordt de nieuwe poging verloopt.<br/><br/>Als alle pogingen voor de validatie van een time-out optreedt, wordt het segment is gemarkeerd als time-out. |Nee |00:10:00 uur (10 minuten) |
| maximumRetry |Aantal keren dat om te controleren op de beschikbaarheid van de externe gegevens. De toegestane maximale waarde is 10. |Nee |3 |


## <a name="data-stores"></a>GEGEVENSARCHIEVEN
De [gekoppelde service](#linked-service) sectie opgegeven beschrijvingen van JSON-elementen die gemeenschappelijk voor alle typen van gekoppelde services zijn. In deze sectie bevat informatie over JSON-elementen die specifiek voor elke gegevensopslag zijn.

De [gegevensset](#dataset) sectie opgegeven beschrijvingen van JSON-elementen die gemeenschappelijk voor alle typen gegevenssets zijn. In deze sectie bevat informatie over JSON-elementen die specifiek voor elke gegevensopslag zijn.

De [activiteit](#activity) sectie opgegeven beschrijvingen van JSON-elementen die gemeenschappelijk voor alle soorten activiteiten zijn. In deze sectie bevat informatie over JSON-elementen die specifiek voor elk gegevensarchief zijn wanneer deze wordt gebruikt als een bron/sink in een kopieeractiviteit.  

Klik op de koppeling voor het archief dat u geïnteresseerd bent in de JSON-schema voor de gekoppelde service, gegevensset en het bron/sink voor de kopieerbewerking te zien.

| Categorie | Gegevensarchief 
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
| &nbsp; |[Webtabel](#web-table) |

## <a name="azure-blob-storage"></a>Azure Blob Storage

### <a name="linked-service"></a>Gekoppelde service
Er zijn twee soorten gekoppelde services: Gekoppelde Azure Storage-service en Azure Storage SAS gekoppelde service.

#### <a name="azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service
Uw Azure storage-account koppelen aan een data factory met behulp van de **accountsleutel**, een gekoppelde Azure Storage-service maken. Voor het definiëren van een Azure Storage gekoppelde service, stelt u de **type** van de gekoppelde service om **AzureStorage**. Vervolgens kunt u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met Azure storage voor de connectionString-eigenschap. |Ja |

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
De SAS van Azure Storage gekoppelde service kunt u een Azure Storage-Account koppelen aan een Azure data factory met behulp van een Shared Access Signature (SAS). Het biedt de data factory met beperkte/tijdelijke toegang tot alle of naar een specifiek resources (blob/container) in de opslag. Gekoppelde service voor uw Azure storage-account koppelen aan een gegevensfactory met behulp van de handtekening voor gedeelde toegang, het maken van een Azure Storage-SAS. Voor het definiëren van een Azure Storage-SAS gekoppelde service, stelt u de **type** van de gekoppelde service om **AzureStorageSas**. Vervolgens kunt u de volgende eigenschappen in de **typeProperties** sectie:   

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| sasUri |Shared Access Signature URI voor de Azure Storage-resources, zoals blob, container of tabel opgeven. |Ja |

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

Zie voor meer informatie over deze gekoppelde services, [Azure Blob Storage-connector](data-factory-azure-blob-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een Azure Blob-gegevensset, stel de **type** van de gegevensset in **AzureBlob**. Geef vervolgens de volgende specifieke Azure Blob-eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| folderPath |Pad naar de container en map in de blob-opslag. Voorbeeld: myblobcontainer\myblobfolder\ |Ja |
| fileName |Naam van de blob. Bestandsnaam is optioneel en is hoofdlettergevoelig.<br/><br/>Als u een filename opgeeft, wordt de activiteit (inclusief kopie) werkt op de specifieke Blob.<br/><br/>Als geen bestandsnaam is opgegeven, bevat kopiëren alle Blobs in de folderPath voor invoergegevensset.<br/><br/>Als geen bestandsnaam is opgegeven voor een uitvoergegevensset, de naam van het gegenereerde bestand zou worden in de volgende notatie: De gegevens. <Guid>.txt (voorbeeld:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nee |
| partitionedBy |partitionedBy is een optionele eigenschap. U kunt deze gebruiken om op te geven van een dynamische folderPath en de bestandsnaam voor time series-gegevens. Bijvoorbeeld, folderPath kan worden als parameters gebruikt voor elk uur gegevens. |Nee |
| Indeling | De volgende bestandsindelingen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc-indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [Parquet-indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer. |Nee |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus zijn: **Optimale** en **snelste**. Zie voor meer informatie, [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

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


Zie voor meer informatie, [Azure Blob-connectoren](data-factory-azure-blob-connector.md#dataset-properties) artikel.

### <a name="blobsource-in-copy-activity"></a>BlobSource in de Kopieeractiviteit
Als u gegevens van een Azure Blob-opslag kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **BlobSource**, en geeft u de volgende eigenschappen in de **bron** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| recursieve |Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. |True (standaardwaarde), False |Nee |

#### <a name="example-blobsource"></a>Voorbeeld: **BlobSource**
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
### <a name="blobsink-in-copy-activity"></a>BlobSink in de Kopieeractiviteit
Als u gegevens naar een Azure Blob-opslag kopieert, stelt u de **sink-type** van de kopieeractiviteit naar **BlobSink**, en geeft u de volgende eigenschappen in de **sink** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| copyBehavior |Definieert het gedrag kopiëren wanneer de bron BlobSource of bestandssysteem is. |<b>PreserveHierarchy</b>: behoudt de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand voor bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><br/><b>FlattenHierarchy</b>: alle bestanden uit de bronmap van het zich in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde naam. <br/><br/><b>MergeFiles (standaard):</b> alle bestanden uit de bronmap naar één bestand worden samengevoegd. Als de naam van de bestands-/ Blob is opgegeven, is de naam van het samengevoegde de opgegeven naam. anders zou worden automatisch gegenereerde naam. |Nee |

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

Zie voor meer informatie, [Azure Blob-connectoren](data-factory-azure-blob-connector.md#copy-activity-properties) artikel. 

## <a name="azure-data-lake-store"></a>Azure Data Lake Store

### <a name="linked-service"></a>Gekoppelde service
Voor het definiëren van een Azure Data Lake Store gekoppelde service, stelt u het type van de gekoppelde service om **AzureDataLakeStore**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op: **AzureDataLakeStore** | Ja |
| dataLakeStoreUri | Geef informatie op over de Azure Data Lake Store-account. Het is in de volgende indeling: `https://[accountname].azuredatalakestore.net/webhdfs/v1` of `adl://[accountname].azuredatalakestore.net/`. | Ja |
| subscriptionId | Azure-abonnement-Id die Data Lake Store behoort. | Vereist voor sink |
| resourceGroupName | Naam van Azure-resource die Data Lake Store behoort. | Vereist voor sink |
| servicePrincipalId | Opgeven van de toepassing client-ID. | Ja (voor service-principal verificatie) |
| servicePrincipalKey | Geef de sleutel van de toepassing. | Ja (voor service-principal verificatie) |
| tenant | De tenantgegevens (domain name of tenant-ID) opgeven in uw toepassing zich bevindt. U kunt het ophalen van de muis in de rechterbovenhoek van de Azure-portal. | Ja (voor service-principal verificatie) |
| Autorisatie | Klik op **autoriseren** knop in de **Data Factory-Editor** en voer uw referenties op waarmee de automatisch gegenereerde autorisatie-URL worden toegewezen aan deze eigenschap. | Ja (voor verificatie van referentie)|
| sessie-id | OAuth-sessie-id van de OAuth-autorisatie-sessie. Elke sessie-id is uniek en kan slechts eenmaal worden gebruikt. Deze instelling wordt automatisch gegenereerd wanneer u Data Factory-Editor gebruiken. | Ja (voor verificatie van referentie) |

#### <a name="example-using-service-principal-authentication"></a>Voorbeeld: met behulp van service-principal verificatie
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

Zie voor meer informatie, [Azure Data Lake Store-connector](data-factory-azure-datalake-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een Azure Data Lake Store-gegevensset, stel de **type** van de gegevensset in **AzureDataLakeStore**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| folderPath |Pad naar de container en map in de Azure Data Lake opslaan. |Ja |
| fileName |De naam van het bestand in de Azure Data Lake store. Bestandsnaam is optioneel en is hoofdlettergevoelig. <br/><br/>Als u een filename opgeeft, wordt de activiteit (inclusief kopie) werkt op het specifieke bestand.<br/><br/>Als geen bestandsnaam is opgegeven, bevat kopiëren alle bestanden in het mappad voor invoergegevensset.<br/><br/>Als geen bestandsnaam is opgegeven voor een uitvoergegevensset, de naam van het gegenereerde bestand zou worden in de volgende notatie: De gegevens. <Guid>.txt (voorbeeld:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nee |
| partitionedBy |partitionedBy is een optionele eigenschap. U kunt deze gebruiken om op te geven van een dynamische folderPath en de bestandsnaam voor time series-gegevens. Bijvoorbeeld, folderPath kan worden als parameters gebruikt voor elk uur gegevens. |Nee |
| Indeling | De volgende bestandsindelingen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc-indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [Parquet-indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer. |Nee |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus zijn: **Optimale** en **snelste**. Zie voor meer informatie, [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

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

Zie voor meer informatie, [Azure Data Lake Store-connector](data-factory-azure-datalake-connector.md#dataset-properties) artikel. 

### <a name="azure-data-lake-store-source-in-copy-activity"></a>Azure Data Lake Store-bron in de Kopieeractiviteit
Als u gegevens van een Azure Data Lake Store kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **AzureDataLakeStoreSource**, en geeft u de volgende eigenschappen in de **bron**sectie:

**AzureDataLakeStoreSource** ondersteunt de volgende eigenschappen **typeProperties** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| recursieve |Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. |True (standaardwaarde), False |Nee |

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

Zie voor meer informatie, [Azure Data Lake Store-connector](data-factory-azure-datalake-connector.md#copy-activity-properties) artikel.

### <a name="azure-data-lake-store-sink-in-copy-activity"></a>Azure Data Lake Store-Sink in de Kopieeractiviteit
Als u gegevens naar een Azure Data Lake Store kopieert, stelt u de **sink-type** van de kopieeractiviteit naar **AzureDataLakeStoreSink**, en geeft u de volgende eigenschappen in de **sink** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| copyBehavior |Hiermee geeft u het gedrag van de kopie. |<b>PreserveHierarchy</b>: behoudt de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand voor bronmap is identiek aan het relatieve pad van doelbestand naar doelmap.<br/><br/><b>FlattenHierarchy</b>: alle bestanden uit de bronmap zijn gemaakt in het eerste niveau van de doelmap. De doelbestanden worden gemaakt met automatisch gegenereerde naam.<br/><br/><b>MergeFiles</b>: alle bestanden uit de bronmap naar één bestand worden samengevoegd. Als de naam van de bestands-/ Blob is opgegeven, is de naam van het samengevoegde de opgegeven naam. anders zou worden automatisch gegenereerde naam. |Nee |

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

Zie voor meer informatie, [Azure Data Lake Store-connector](data-factory-azure-datalake-connector.md#copy-activity-properties) artikel. 

## <a name="azure-cosmos-db"></a>Azure Cosmos DB  

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Azure Cosmos DB, stelt u de **type** van de gekoppelde service om **DocumentDb**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| **Eigenschap** | **Beschrijving** | **Vereist** |
| --- | --- | --- |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met Azure Cosmos DB-database. |Ja |

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
Zie voor meer informatie, [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een Azure Cosmos DB-gegevensset, stel de **type** van de gegevensset in **DocumentDbCollection**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| **Eigenschap** | **Beschrijving** | **Vereist** |
| --- | --- | --- |
| collectionName |Naam van de Azure Cosmos DB-verzameling. |Ja |

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
Zie voor meer informatie, [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#dataset-properties) artikel.

### <a name="azure-cosmos-db-collection-source-in-copy-activity"></a>Bron voor Azure Cosmos DB-verzameling in de Kopieeractiviteit
Als u gegevens van een Azure Cosmos DB kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **DocumentDbCollectionSource**, en geeft u de volgende eigenschappen in de **bron**sectie:


| **Eigenschap** | **Beschrijving** | **Toegestane waarden** | **Vereist** |
| --- | --- | --- | --- |
| query |Geef de query voor het lezen van gegevens. |De queryreeks wordt ondersteund door Azure Cosmos DB. <br/><br/>Voorbeeld: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Nee <br/><br/>Indien niet opgegeven, de SQL-instructie die wordt uitgevoerd: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Speciaal teken om aan te geven dat het document is genest |Willekeurig teken. <br/><br/>Azure Cosmos DB is een NoSQL-archief voor JSON-documenten, waarin geneste structuren zijn toegestaan. Azure Data Factory kan de gebruiker om aan te duiden hiërarchie via nestingSeparator, namelijk '. ' in de bovenstaande voorbeelden. Bij het scheidingsteken, de kopieeractiviteit genereren het object "Naam" met de drie onderliggende elementen eerst, midden- en achternaam, op basis van "Name.First", "Name.Middle" en "Name.Last" in de tabeldefinitie. |Nee |

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

### <a name="azure-cosmos-db-collection-sink-in-copy-activity"></a>Azure Cosmos DB-verzameling Sink in de Kopieeractiviteit
Als u gegevens naar de Azure Cosmos DB kopieert, stelt u de **sink-type** van de kopieeractiviteit naar **DocumentDbCollectionSink**, en geeft u de volgende eigenschappen in de **sink** sectie :

| **Eigenschap** | **Beschrijving** | **Toegestane waarden** | **Vereist** |
| --- | --- | --- | --- |
| nestingSeparator |Er is een speciaal teken in naam van de bronkolom om aan te geven dat geneste document nodig. <br/><br/>Zoals hierboven: `Name.First` in de uitvoer van de tabel de volgende JSON-structuur in de Cosmos DB-document maakt:<br/><br/>"Naam": {<br/>    'First': "John"<br/>}, |Teken dat wordt gebruikt voor het scheiden van geneste niveaus.<br/><br/>Standaardwaarde is `.` (punt). |Teken dat wordt gebruikt voor het scheiden van geneste niveaus. <br/><br/>Standaardwaarde is `.` (punt). |
| WriteBatchSize |Het aantal parallelle aanvragen voor Azure Cosmos DB-service om documenten te maken.<br/><br/>U kunt de prestaties afstemmen bij het kopiëren van gegevens naar/van Azure Cosmos DB met behulp van deze eigenschap. U kunt een betere prestaties verwachten wanneer u writeBatchSize verhogen omdat meer parallelle aanvragen met Azure Cosmos DB worden verzonden. Echter, moet u voorkomen dat beperking, kunnen het foutbericht genereren: 'Aanvraagsnelheid is hoog'.<br/><br/>Beperking wordt bepaald door een aantal factoren, onder andere de grootte van de documenten, het aantal voorwaarden in documenten, het indexeringsbeleid van verzameling, enzovoort. Voor kopieerbewerkingen, kunt u een betere verzameling (bijvoorbeeld S3) gebruiken om de meeste doorvoer beschikbaar (2.500 request units per seconde). |Geheel getal |Nee (standaard: 5) |
| writeBatchTimeout |Wachttijd voor de bewerking is voltooid voordat er een optreedt time-out. |TimeSpan<br/><br/> Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |

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

Zie voor meer informatie, [Azure Cosmos DB connector](data-factory-azure-documentdb-connector.md#copy-activity-properties) artikel.

## <a name="azure-sql-database"></a>Azure SQL Database

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Azure SQL Database, stelt u de **type** van de gekoppelde service om **AzureSqlDatabase**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met de Azure SQL Database-exemplaar voor de connectionString-eigenschap. |Ja |

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

Zie voor meer informatie, [Azure SQL-connector](data-factory-azure-sql-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een Azure SQL Database-gegevensset, stel de **type** van de gegevensset in **AzureSqlTable**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
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
Zie voor meer informatie, [Azure SQL-connector](data-factory-azure-sql-connector.md#dataset-properties) artikel. 

### <a name="sql-source-in-copy-activity"></a>De SQL-bron in de Kopieeractiviteit
Als u gegevens van een Azure SQL Database kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **SqlSource**, en geeft u de volgende eigenschappen in de **bron** sectie:


| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| sqlReaderQuery |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Voorbeeld: `select * from MyTable`. |Nee |
| sqlReaderStoredProcedureName |De naam van de opgeslagen procedure die gegevens uit de brontabel leest. |De naam van de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee |

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
Zie voor meer informatie, [Azure SQL-connector](data-factory-azure-sql-connector.md#copy-activity-properties) artikel. 

### <a name="sql-sink-in-copy-activity"></a>SQL-Sink in de Kopieeractiviteit
Als u gegevens naar Azure SQL Database kopieert, stelt u de **sink-type** van de kopieeractiviteit naar **SqlSink**, en geeft u de volgende eigenschappen in de **sink** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| writeBatchTimeout |Wachttijd voor de batch insert bewerking is voltooid voordat er een optreedt time-out. |TimeSpan<br/><br/> Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |
| WriteBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt. |Geheel getal (aantal rijen) |Nee (standaard: 10000) |
| sqlWriterCleanupScript |Geef een query voor de Kopieeractiviteit om uit te voeren waarbij de gegevens van een bepaald segment wordt opgeschoond. |Een query-instructie. |Nee |
| sliceIdentifierColumnName |Geef de naam van een kolom voor de Kopieeractiviteit in te vullen met automatisch gegenereerde segment-id, die wordt gebruikt voor het opschonen van gegevens van een bepaald segment wanneer opnieuw uitgevoerd. |De naam van de kolom van een kolom met het gegevenstype van binary(32). |Nee |
| sqlWriterStoredProcedureName |Naam van de opgeslagen procedure die gegevens van de upsert-bewerking (updates/ingevoegd) in de doeltabel. |De naam van de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee |
| sqlWriterTableType |Geef een naam van het type tabel moet worden gebruikt in de opgeslagen procedure. Kopieeractiviteit maakt de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Opgeslagen procedurecode kunt vervolgens de gegevens worden gekopieerd met bestaande gegevens samenvoegen. |Een typenaam van de tabel. |Nee |

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

Zie voor meer informatie, [Azure SQL-connector](data-factory-azure-sql-connector.md#copy-activity-properties) artikel. 

## <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Azure SQL Data Warehouse, stelt u de **type** van de gekoppelde service om **AzureSqlDW**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met de Azure SQL Data Warehouse-exemplaar voor de connectionString-eigenschap. |Ja |



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

Zie voor meer informatie, [Azure SQL Data Warehouse-connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een Azure SQL Data Warehouse-gegevensset, stel de **type** van de gegevensset in **AzureSqlDWTable**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel of weergave in de Azure SQL Data Warehouse-database waarnaar de gekoppelde service verwijst. |Ja |

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

Zie voor meer informatie, [Azure SQL Data Warehouse-connector](data-factory-azure-sql-data-warehouse-connector.md#dataset-properties) artikel. 

### <a name="sql-dw-source-in-copy-activity"></a>SQL DW-bron in de Kopieeractiviteit
Als u gegevens van Azure SQL Data Warehouse kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **SqlDWSource**, en geeft u de volgende eigenschappen in de **bron** sectie:


| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| sqlReaderQuery |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `select * from MyTable`. |Nee |
| sqlReaderStoredProcedureName |De naam van de opgeslagen procedure die gegevens uit de brontabel leest. |De naam van de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee |

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

Zie voor meer informatie, [Azure SQL Data Warehouse-connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) artikel. 

### <a name="sql-dw-sink-in-copy-activity"></a>SQL DW-Sink in de Kopieeractiviteit
Als u gegevens naar Azure SQL Data Warehouse kopieert, stelt u de **sink-type** van de kopieeractiviteit naar **SqlDWSink**, en geeft u de volgende eigenschappen in de **sink** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| sqlWriterCleanupScript |Geef een query voor de Kopieeractiviteit om uit te voeren waarbij de gegevens van een bepaald segment wordt opgeschoond. |Een query-instructie. |Nee |
| allowPolyBase |Hiermee wordt aangegeven of het gebruik van PolyBase (indien van toepassing) in plaats van BULKINSERT mechanisme. <br/><br/> **Met PolyBase is de aanbevolen manier om gegevens te laden in SQL Data Warehouse.** |True <br/>False (standaard) |Nee |
| polyBaseSettings |Een groep met eigenschappen die kunnen worden opgegeven wanneer de **allowPolybase** eigenschap is ingesteld op **waar**. |&nbsp; |Nee |
| rejectValue |Hiermee geeft u het getal of het percentage van de rijen die kunnen worden afgewezen voordat de query is mislukt. <br/><br/>Meer informatie over van de PolyBase-weigeringsopties in de **argumenten** sectie van [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) onderwerp. |0 (standaardinstelling), 1, 2... |Nee |
| rejectType |Hiermee geeft u op of de optie rejectValue is opgegeven als een letterlijke waarde of een percentage. |Waarde (standaard), Percentage |Nee |
| rejectSampleValue |Bepaalt het aantal rijen om op te halen voordat de PolyBase berekent het percentage van geweigerde rijen opnieuw. |1, 2, … |Ja, als **rejectType** is **percentage** |
| useTypeDefault |Hiermee geeft u ontbrekende waarden in de tekstbestanden verwerken als PolyBase worden gegevens opgehaald uit het tekstbestand.<br/><br/>Meer informatie over deze eigenschap in de sectie argumenten [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx). |True, False (standaard) |Nee |
| WriteBatchSize |Gegevens invoegen in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt |Geheel getal (aantal rijen) |Nee (standaard: 10000) |
| writeBatchTimeout |Wachttijd voor de batch insert bewerking is voltooid voordat er een optreedt time-out. |TimeSpan<br/><br/> Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |

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

Zie voor meer informatie, [Azure SQL Data Warehouse-connector](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties) artikel. 

## <a name="azure-search"></a>Azure Search

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Azure Search, stelt u de **type** van de gekoppelde service om **AzureSearch**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
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

Zie voor meer informatie, [Azure Search connector](data-factory-azure-search-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een Azure Search-gegevensset, stel de **type** van de gegevensset in **AzureSearchIndex**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
| -------- | ----------- | -------- |
| type | De eigenschap type moet worden ingesteld op **AzureSearchIndex**.| Ja |
| indexName | De naam van de Azure Search-index. Data Factory maakt niet de index. De index moet bestaan in Azure Search. | Ja |

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

Zie voor meer informatie, [Azure Search connector](data-factory-azure-search-connector.md#dataset-properties) artikel.

### <a name="azure-search-index-sink-in-copy-activity"></a>Azure Search-Index Sink in de Kopieeractiviteit
Als u gegevens naar een Azure Search-index kopieert, stelt u de **sink-type** van de kopieeractiviteit naar **AzureSearchIndexSink**, en geeft u de volgende eigenschappen in de **sink** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| -------- | ----------- | -------------- | -------- |
| WriteBehavior | Hiermee geeft u op of u wilt samenvoegen of vervangen wanneer een document al in de index bestaat. | samenvoegen (standaard)<br/>Uploaden| Nee |
| WriteBatchSize | Wanneer de buffergrootte writeBatchSize bereikt, uploadt u gegevens in de Azure Search-index. | 1-1000. Standaardwaarde is 1000. | Nee |

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

Zie voor meer informatie, [Azure Search connector](data-factory-azure-search-connector.md#copy-activity-properties) artikel.

## <a name="azure-table-storage"></a>Azure-tabelopslag

### <a name="linked-service"></a>Gekoppelde service
Er zijn twee soorten gekoppelde services: Gekoppelde Azure Storage-service en Azure Storage SAS gekoppelde service.

#### <a name="azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service
Uw Azure storage-account koppelen aan een data factory met behulp van de **accountsleutel**, een gekoppelde Azure Storage-service maken. Voor het definiëren van een Azure Storage gekoppelde service, stelt u de **type** van de gekoppelde service om **AzureStorage**. Vervolgens kunt u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op: **AzureStorage** |Ja |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met Azure storage voor de connectionString-eigenschap. |Ja |

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
De SAS van Azure Storage gekoppelde service kunt u een Azure Storage-Account koppelen aan een Azure data factory met behulp van een Shared Access Signature (SAS). Het biedt de data factory met beperkte/tijdelijke toegang tot alle of naar een specifiek resources (blob/container) in de opslag. Gekoppelde service voor uw Azure storage-account koppelen aan een gegevensfactory met behulp van de handtekening voor gedeelde toegang, het maken van een Azure Storage-SAS. Voor het definiëren van een Azure Storage-SAS gekoppelde service, stelt u de **type** van de gekoppelde service om **AzureStorageSas**. Vervolgens kunt u de volgende eigenschappen in de **typeProperties** sectie:   

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type |De eigenschap type moet worden ingesteld op: **AzureStorageSas** |Ja |
| sasUri |Shared Access Signature URI voor de Azure Storage-resources, zoals blob, container of tabel opgeven. |Ja |

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
Voor het definiëren van een Azure Table-gegevensset, stel de **type** van de gegevensset in **AzureTable**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in de Azure Table-Database-instantie waarnaar de gekoppelde service verwijst. |Ja. Wanneer een tabelnaam wordt opgegeven zonder een azureTableSourceQuery, worden alle records uit de tabel worden gekopieerd naar de bestemming. Als een azureTableSourceQuery ook is opgegeven, worden records uit de tabel die voldoet aan de query worden gekopieerd naar de bestemming. |

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

### <a name="azure-table-source-in-copy-activity"></a>Bron van de Azure-tabel in de Kopieeractiviteit
Als u gegevens van Azure Table Storage kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **AzureTableSource**, en geeft u de volgende eigenschappen in de **bron** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| azureTableSourceQuery |De aangepaste query gebruiken om gegevens te lezen. |Azure-tabel query-tekenreeks. Zie de voorbeelden in de volgende sectie. |Nee. Wanneer een tabelnaam wordt opgegeven zonder een azureTableSourceQuery, worden alle records uit de tabel worden gekopieerd naar de bestemming. Als een azureTableSourceQuery ook is opgegeven, worden records uit de tabel die voldoet aan de query worden gekopieerd naar de bestemming. |
| azureTableSourceIgnoreTableNotFound |Aangeven of de uitzondering van de tabel slikken niet bestaat. |DE WAARDE TRUE<br/>DE WAARDE FALSE |Nee |

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

### <a name="azure-table-sink-in-copy-activity"></a>Azure Table-Sink in de Kopieeractiviteit
Als u gegevens naar Azure Table Storage kopieert, stelt u de **sink-type** van de kopieeractiviteit naar **AzureTableSink**, en geeft u de volgende eigenschappen in de **sink** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| azureTableDefaultPartitionKeyValue |Standaard partitiesleutelwaarde die kan worden gebruikt door de sink. |Een string-waarde. |Nee |
| azureTablePartitionKeyName |Geef de naam van de kolom waarvan de waarden worden gebruikt als partitiesleutels. Als niet is opgegeven, wordt AzureTableDefaultPartitionKeyValue gebruikt als de partitiesleutel. |De naam van een kolom. |Nee |
| azureTableRowKeyName |Geef de naam van de kolom waarvan de kolomwaarden worden gebruikt als de rijsleutel. Als niet is opgegeven, gebruikt u een GUID voor elke rij. |De naam van een kolom. |Nee |
| azureTableInsertType |De modus invoegen van gegevens in Azure-tabel.<br/><br/>Deze eigenschap bepaalt of bestaande rijen in de uitvoertabel met de bijbehorende partitie-en recordsleutels hebben hun waarden vervangen of samenvoegen. <br/><br/>Zie voor meer informatie over de werking van deze instellingen (samenvoegen en vervangen), [invoegen of samenvoegen entiteit](https://msdn.microsoft.com/library/azure/hh452241.aspx) en [invoegen of vervangen entiteit](https://msdn.microsoft.com/library/azure/hh452242.aspx) onderwerpen. <br/><br> Deze instelling is van toepassing op het rijniveau van de, niet in de tabelniveau en geen van beide optie verwijdert rijen in de uitvoertabel die niet zijn opgenomen in de invoer. |samenvoegen (standaard)<br/>vervangen |Nee |
| WriteBatchSize |Voegt de gegevens in de Azure-tabel wanneer de writeBatchSize of writeBatchTimeout is bereikt. |Geheel getal (aantal rijen) |Nee (standaard: 10000) |
| writeBatchTimeout |Voegt de gegevens in de Azure-tabel wanneer de writeBatchSize of writeBatchTimeout is bereikt |TimeSpan<br/><br/>Voorbeeld: "00: 20:00" (20 minuten) |Nee (standaard ingesteld op de standaardtime-out opslag client waarde 90 sec.) |

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
Gekoppelde service voor het definiëren van een Amazon Redshift, stelt u de **type** van de gekoppelde service om **AmazonRedshift**, en geeft u de volgende eigenschappen in de **typeProperties** sectie :  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| server |IP-adres of de hostnaam de naam van de Amazon Redshift-server. |Ja |
| poort |Het nummer van de TCP-poort die gebruikmaakt van de Amazon Redshift-server om te luisteren naar clientverbindingen. |Nee, standaardwaarde: 5439 |
| database |De naam van de Amazon Redshift-database. |Ja |
| gebruikersnaam |De naam van de gebruiker die toegang tot de database heeft. |Ja |
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

Zie voor meer informatie, [Amazon Redshift-connector](#data-factory-amazon-redshift-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een Amazon Redshift-gegevensset, stel de **type** van de gegevensset in **RelationalTable**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| tableName |De naam van de tabel in de Amazon Redshift-database waarnaar de gekoppelde service verwijst. |Nee (als **query** van **RelationalSource** is opgegeven) |


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
Zie voor meer informatie, [Amazon Redshift-connector](#data-factory-amazon-redshift-connector.md#dataset-properties) artikel.

### <a name="relational-source-in-copy-activity"></a>Relationele bron in de Kopieeractiviteit 
Als u gegevens van Amazon Redshift kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en geeft u de volgende eigenschappen in de **bron** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `select * from MyTable`. |Nee (als **tableName** van **gegevensset** is opgegeven) |

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
Zie voor meer informatie, [Amazon Redshift-connector](#data-factory-amazon-redshift-connector.md#copy-activity-properties) artikel.

## <a name="ibm-db2"></a>IBM DB2

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een IBM DB2, stelt u de **type** van de gekoppelde service om **OnPremisesDB2**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| server |Naam van de DB2-server. |Ja |
| database |De naam van de DB2-database. |Ja |
| schema |De naam van het schema in de database. Naam van het schema is hoofdlettergevoelig. |Nee |
| authenticationType |Het type verificatie gebruikt voor verbinding met de DB2-database. Mogelijke waarden zijn: Anoniem, basis en Windows. |Ja |
| gebruikersnaam |Geef de gebruikersnaam op als u basisverificatie of Windows-verificatie. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |
| gatewayName |De naam van de gateway die de Data Factory-service wordt gebruikt om verbinding met de on-premises DB2-database te maken. |Ja |

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
Zie voor meer informatie, [IBM DB2-connector](#data-factory-onprem-db2-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een DB2-gegevensset, stel de **type** van de gegevensset in **RelationalTable**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:

| Eigenschap | Description | Vereist |
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

Zie voor meer informatie, [IBM DB2-connector](#data-factory-onprem-db2-connector.md#dataset-properties) artikel.

### <a name="relational-source-in-copy-activity"></a>Relationele bron in de Kopieeractiviteit
Als u gegevens uit IBM DB2 kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en geeft u de volgende eigenschappen in de **bron** sectie:


| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `"query": "select * from "MySchema"."MyTable""`. |Nee (als **tableName** van **gegevensset** is opgegeven) |

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
Zie voor meer informatie, [IBM DB2-connector](#data-factory-onprem-db2-connector.md#copy-activity-properties) artikel.

## <a name="mysql"></a>MySQL

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een MySQL, stelt u de **type** van de gekoppelde service om **OnPremisesMySql**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| server |De naam van de MySQL-server. |Ja |
| database |De naam van de MySQL-database. |Ja |
| schema |De naam van het schema in de database. |Nee |
| authenticationType |Het type verificatie gebruikt voor verbinding met de MySQL-database. Mogelijke waarden zijn: `Basic`. |Ja |
| gebruikersnaam |Geef de naam van de gebruiker verbinding maken met de MySQL-database. |Ja |
| wachtwoord |Geef het wachtwoord voor het gebruikersaccount dat u hebt opgegeven. |Ja |
| gatewayName |De naam van de gateway die de Data Factory-service wordt gebruikt om verbinding met de on-premises MySQL-database te maken. |Ja |

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

Zie voor meer informatie, [MySQL-connector](data-factory-onprem-mysql-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een MySQL-gegevensset, stel de **type** van de gegevensset in **RelationalTable**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
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
Zie voor meer informatie, [MySQL-connector](data-factory-onprem-mysql-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationele bron in de Kopieeractiviteit
Als u gegevens van een MySQL-database kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en geeft u de volgende eigenschappen in de **bron** sectie:


| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `select * from MyTable`. |Nee (als **tableName** van **gegevensset** is opgegeven) |


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

Zie voor meer informatie, [MySQL-connector](data-factory-onprem-mysql-connector.md#copy-activity-properties) artikel. 

## <a name="oracle"></a>Oracle 

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Oracle, stelt u de **type** van de gekoppelde service om **OnPremisesOracle**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| driverType | Geef op welke stuurprogramma om te gebruiken om gegevens te kopiëren van/naar Oracle-Database. Toegestane waarden zijn **Microsoft** of **ODP** (standaard). Zie [ondersteunde versie en installatie](#supported-versions-and-installation) sectie op stuurprogrammagegevens. | Nee |
| connectionString | Geef informatie op die nodig zijn voor het verbinding maken met de Oracle-Database-exemplaar voor de connectionString-eigenschap. | Ja |
| gatewayName | Naam van de gateway die wordt gebruikt voor verbinding met de on-premises Oracle-server |Ja |

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

Zie voor meer informatie, [Oracle-connector](data-factory-onprem-oracle-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een Oracle-gegevensset, stelt de **type** van de gegevensset in **OracleTable**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
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
Zie voor meer informatie, [Oracle-connector](data-factory-onprem-oracle-connector.md#dataset-properties) artikel.

### <a name="oracle-source-in-copy-activity"></a>Oracle-bron in de Kopieeractiviteit
Als u gegevens uit een Oracle-database kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **OracleSource**, en geeft u de volgende eigenschappen in de **bron** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| oracleReaderQuery |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `select * from MyTable` <br/><br/>Indien niet opgegeven, de SQL-instructie die wordt uitgevoerd: `select * from MyTable` |Nee (als **tableName** van **gegevensset** is opgegeven) |

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

Zie voor meer informatie, [Oracle-connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) artikel.

### <a name="oracle-sink-in-copy-activity"></a>Oracle-Sink in de Kopieeractiviteit
Als u gegevens naar am Oracle-database kopieert, stelt u de **sink-type** van de kopieeractiviteit naar **OracleSink**, en geeft u de volgende eigenschappen in de **sink** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| writeBatchTimeout |Wachttijd voor de batch insert bewerking is voltooid voordat er een optreedt time-out. |TimeSpan<br/><br/> Voorbeeld: 00:30:00 (30 minuten). |Nee |
| WriteBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt. |Geheel getal (aantal rijen) |Nee (standaard: 100) |
| sqlWriterCleanupScript |Geef een query voor de Kopieeractiviteit om uit te voeren waarbij de gegevens van een bepaald segment wordt opgeschoond. |Een query-instructie. |Nee |
| sliceIdentifierColumnName |Geef de naam van de kolom voor de Kopieeractiviteit in te vullen met automatisch gegenereerde segment-id, die wordt gebruikt voor het opschonen van gegevens van een bepaald segment wanneer opnieuw uitgevoerd. |De naam van de kolom van een kolom met het gegevenstype van binary(32). |Nee |

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
Zie voor meer informatie, [Oracle-connector](data-factory-onprem-oracle-connector.md#copy-activity-properties) artikel.

## <a name="postgresql"></a>PostgreSQL

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een PostgreSQL, stelt u de **type** van de gekoppelde service om **OnPremisesPostgreSql**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| server |De naam van de PostgreSQL-server. |Ja |
| database |De naam van de PostgreSQL-database. |Ja |
| schema |De naam van het schema in de database. Naam van het schema is hoofdlettergevoelig. |Nee |
| authenticationType |Het type verificatie gebruikt voor verbinding met de PostgreSQL-database. Mogelijke waarden zijn: Anoniem, basis en Windows. |Ja |
| gebruikersnaam |Geef de gebruikersnaam op als u basisverificatie of Windows-verificatie. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |
| gatewayName |De naam van de gateway die de Data Factory-service gebruiken moet voor verbinding met de on-premises PostgreSQL-database. |Ja |

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
Zie voor meer informatie, [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een PostgreSQL-gegevensset, stel de **type** van de gegevensset in **RelationalTable**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
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
Zie voor meer informatie, [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#dataset-properties) artikel.

### <a name="relational-source-in-copy-activity"></a>Relationele bron in de Kopieeractiviteit
Als u gegevens uit een PostgreSQL-database kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en geeft u de volgende eigenschappen in de **bron** sectie:


| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: 'query': ' Selecteer * uit \"MySchema\".\" MyTable\"'. |Nee (als **tableName** van **gegevensset** is opgegeven) |

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

Zie voor meer informatie, [PostgreSQL connector](data-factory-onprem-postgresql-connector.md#copy-activity-properties) artikel.

## <a name="sap-business-warehouse"></a>SAP Business Warehouse


### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een SAP Business Warehouse (BW), stelt u de **type** van de gekoppelde service om **SapBw**, en geeft u de volgende eigenschappen in de **typeProperties** sectie :  

Eigenschap | Description | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
server | Naam van de server waarop de SAP BW-instantie zich bevindt. | string | Ja
systemNumber | Het systeemnummer van de SAP BW-systeem. | Decimaal getal van twee cijfers weergegeven als een tekenreeks. | Ja
ClientId | Client-ID van de client in het W SAP-systeem. | Decimaal getal van drie cijfers wordt weergegeven als een tekenreeks. | Ja
gebruikersnaam | Naam van de gebruiker die toegang tot de SAP-server heeft | string | Ja
wachtwoord | Het wachtwoord voor de gebruiker. | string | Ja
gatewayName | De naam van de gateway die de Data Factory-service gebruiken moet voor verbinding met de on-premises SAP BW-exemplaar. | string | Ja
encryptedCredential | De versleutelde referentie-tekenreeks. | string | Nee

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

Zie voor meer informatie, [connector SAP Business Warehouse](data-factory-sap-business-warehouse-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een gegevensset SAP BW, stel de **type** van de gegevensset in **RelationalTable**. Er zijn geen specifieke eigenschappen die worden ondersteund voor de SAP BW-gegevensset van het type **RelationalTable**.  

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
Zie voor meer informatie, [connector SAP Business Warehouse](data-factory-sap-business-warehouse-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationele bron in de Kopieeractiviteit
Als u gegevens van SAP Business Warehouse kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en geeft u de volgende eigenschappen in de **bron** sectie:


| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query | Hiermee geeft u de MDX-query voor het lezen van gegevens van de SAP BW-exemplaar. | MDX-query. | Ja |

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

Zie voor meer informatie, [connector SAP Business Warehouse](data-factory-sap-business-warehouse-connector.md#copy-activity-properties) artikel. 

## <a name="sap-hana"></a>SAP HANA

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een SAP HANA, stelt u de **type** van de gekoppelde service om **SapHana**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

Eigenschap | Description | Toegestane waarden | Vereist
-------- | ----------- | -------------- | --------
server | Naam van de server waarop de SAP HANA-instantie zich bevindt. Als uw server een aangepaste poort gebruikt is, geeft u `server:port`. | string | Ja
authenticationType | Het type verificatie. | tekenreeks. 'Basic' of 'Windows' | Ja 
gebruikersnaam | Naam van de gebruiker die toegang tot de SAP-server heeft | string | Ja
wachtwoord | Het wachtwoord voor de gebruiker. | string | Ja
gatewayName | De naam van de gateway die de Data Factory-service gebruiken moet voor verbinding met de on-premises SAP HANA-exemplaar. | string | Ja
encryptedCredential | De versleutelde referentie-tekenreeks. | string | Nee

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
Zie voor meer informatie, [SAP HANA-connector](data-factory-sap-hana-connector.md#linked-service-properties) artikel.
 
### <a name="dataset"></a>Gegevensset
Voor het definiëren van een SAP HANA-gegevensset, stel de **type** van de gegevensset in **RelationalTable**. Er zijn geen specifieke eigenschappen die worden ondersteund voor de SAP HANA-gegevensset van het type **RelationalTable**. 

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
Zie voor meer informatie, [SAP HANA-connector](data-factory-sap-hana-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationele bron in de Kopieeractiviteit
Als u gegevens uit een SAP HANA-gegevensarchief kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en geeft u de volgende eigenschappen in de **bron** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query | Hiermee geeft u de SQL-query voor het lezen van gegevens van de SAP HANA-instantie. | SQL-query. | Ja |


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

Zie voor meer informatie, [SAP HANA-connector](data-factory-sap-hana-connector.md#copy-activity-properties) artikel.


## <a name="sql-server"></a>SQL Server

### <a name="linked-service"></a>Gekoppelde service
U maakt een gekoppelde service van het type **OnPremisesSqlServer** een on-premises SQL Server-database koppelen aan een data factory. De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor on-premises gekoppelde SQL Server-service.

De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor de gekoppelde SQL Server-service.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OnPremisesSqlServer**. |Ja |
| connectionString |Geef connectionString informatie die nodig zijn voor het verbinding maken met de on-premises SQL Server-database met behulp van SQL-verificatie of Windows-verificatie. |Ja |
| gatewayName |De naam van de gateway die de Data Factory-service gebruiken moet voor verbinding met de on-premises SQL Server-database. |Ja |
| gebruikersnaam |Geef de gebruikersnaam op als u van Windows-verificatie gebruikmaakt. Voorbeeld: **domainname\\gebruikersnaam**. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |

U kunt versleutelen referenties met behulp van de **New-AzureRmDataFactoryEncryptValue** cmdlet te gebruiken in de verbindingsreeks, zoals wordt weergegeven in het volgende voorbeeld (**EncryptedCredential** eigenschap):  

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

Als gebruikersnaam en wachtwoord zijn opgegeven, worden deze door gateway te imiteren van het opgegeven gebruikersaccount verbinding maken met de on-premises SQL Server-database gebruikt. Anders gateway maakt verbinding met de SQL-Server rechtstreeks met de beveiligingscontext van de Gateway (de opstartaccount).

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

Zie voor meer informatie, [SQL Server-connector](data-factory-sqlserver-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een SQL Server-gegevensset, stel de **type** van de gegevensset in **SqlServerTable**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
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

Zie voor meer informatie, [SQL Server-connector](data-factory-sqlserver-connector.md#dataset-properties) artikel. 

### <a name="sql-source-in-copy-activity"></a>De SQL-bron in de Kopieeractiviteit
Als u gegevens van een SQL Server-database kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **SqlSource**, en geeft u de volgende eigenschappen in de **bron** sectie:


| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| sqlReaderQuery |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `select * from MyTable`. Kan verwijzen naar meerdere tabellen uit de database waarnaar wordt verwezen door de invoergegevensset. Indien niet opgegeven, de SQL-instructie die wordt uitgevoerd: Selecteer een MyTable. |Nee |
| sqlReaderStoredProcedureName |De naam van de opgeslagen procedure die gegevens uit de brontabel leest. |De naam van de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee |

Als de **sqlReaderQuery** is opgegeven voor de SqlSource de Kopieeractiviteit deze query wordt uitgevoerd op basis van de bron SQL Server-Database de gegevens op te halen.

U kunt ook een opgeslagen procedure opgeven door op te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters zijn vereist).

Als u sqlReaderQuery of sqlReaderStoredProcedureName niet opgeeft, worden de kolommen die zijn gedefinieerd in de structuur gebruikt voor het bouwen van een select-query op de SQL Server-Database uit te voeren. Als de definitie van de gegevensset niet de structuur heeft, worden alle kolommen uit de tabel geselecteerd.

> [!NOTE]
> Bij het gebruik **sqlReaderStoredProcedureName**, u moet nog steeds een waarde opgeven voor de **tableName** eigenschap in de gegevensset JSON. Er zijn geen verdere controles uitgevoerd op deze tabel al.


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

In dit voorbeeld **sqlReaderQuery** is opgegeven voor de SqlSource. De Kopieeractiviteit wordt deze query uitgevoerd op basis van de bron SQL Server-Database de gegevens op te halen. U kunt ook een opgeslagen procedure opgeven door op te geven de **sqlReaderStoredProcedureName** en **storedProcedureParameters** (als de opgeslagen procedure parameters zijn vereist). De sqlReaderQuery kunt verwijzen naar meerdere tabellen in de database waarnaar wordt verwezen door de invoergegevensset. Het is niet beperkt tot alleen de tabel als van de gegevensset tableName typeProperty instellen.

Als u geen sqlReaderQuery of sqlReaderStoredProcedureName opgeeft, worden de kolommen die zijn gedefinieerd in de structuur worden gebruikt voor het bouwen van een select-query op de SQL Server-Database uit te voeren. Als de definitie van de gegevensset niet de structuur heeft, worden alle kolommen uit de tabel geselecteerd.

Zie voor meer informatie, [SQL Server-connector](data-factory-sqlserver-connector.md#copy-activity-properties) artikel. 

### <a name="sql-sink-in-copy-activity"></a>SQL-Sink in de Kopieeractiviteit
Als u gegevens naar een SQL Server-database kopieert, stelt u de **sink-type** van de kopieeractiviteit naar **SqlSink**, en geeft u de volgende eigenschappen in de **sink** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| writeBatchTimeout |Wachttijd voor de batch insert bewerking is voltooid voordat er een optreedt time-out. |TimeSpan<br/><br/> Voorbeeld: "00: 30:00 ' (30 minuten). |Nee |
| WriteBatchSize |Voegt de gegevens in de SQL-tabel wanneer de buffergrootte writeBatchSize bereikt. |Geheel getal (aantal rijen) |Nee (standaard: 10000) |
| sqlWriterCleanupScript |Query voor de Kopieeractiviteit om uit te voeren waarbij de gegevens van een bepaald segment wordt opgeschoond opgeven. Zie voor meer informatie, [herhaalbaarheid](#repeatability-during-copy) sectie. |Een query-instructie. |Nee |
| sliceIdentifierColumnName |Geef de naam van de kolom voor de Kopieeractiviteit in te vullen met automatisch gegenereerde segment-id, die wordt gebruikt voor het opschonen van gegevens van een bepaald segment wanneer opnieuw uitgevoerd. Zie voor meer informatie, [herhaalbaarheid](#repeatability-during-copy) sectie. |De naam van de kolom van een kolom met het gegevenstype van binary(32). |Nee |
| sqlWriterStoredProcedureName |Naam van de opgeslagen procedure die gegevens van de upsert-bewerking (updates/ingevoegd) in de doeltabel. |De naam van de opgeslagen procedure. |Nee |
| storedProcedureParameters |Parameters voor de opgeslagen procedure. |Naam/waarde-paren. Namen en hoofdlettergebruik van parameters moeten overeenkomen met de naam en het hoofdlettergebruik van de opgeslagen-procedureparameters. |Nee |
| sqlWriterTableType |Geef de typenaam tabel moet worden gebruikt in de opgeslagen procedure. Kopieeractiviteit maakt de gegevens worden verplaatst beschikbaar zijn in een tijdelijke tabel met dit tabeltype. Opgeslagen procedurecode kunt vervolgens de gegevens worden gekopieerd met bestaande gegevens samenvoegen. |Een typenaam van de tabel. |Nee |

#### <a name="example"></a>Voorbeeld
De pijplijn bevat een Kopieeractiviteit die is geconfigureerd voor het gebruik van deze gegevenssets voor invoer en uitvoer en is gepland voor elk uur uitgevoerd. In de pijplijn-JSON-definitie heeft de **bron** type is ingesteld op **BlobSource** en **sink** type is ingesteld op **SqlSink**.

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

Zie voor meer informatie, [SQL Server-connector](data-factory-sqlserver-connector.md#copy-activity-properties) artikel. 

## <a name="sybase"></a>Sybase

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Sybase, stelt u de **type** van de gekoppelde service om **OnPremisesSybase**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| server |Naam van de Sybase-server. |Ja |
| database |De naam van de Sybase-database. |Ja |
| schema |De naam van het schema in de database. |Nee |
| authenticationType |Het type verificatie gebruikt voor verbinding met de Sybase-database. Mogelijke waarden zijn: Anoniem, basis en Windows. |Ja |
| gebruikersnaam |Geef de gebruikersnaam op als u basisverificatie of Windows-verificatie. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |
| gatewayName |De naam van de gateway die de Data Factory-service wordt gebruikt om verbinding met de on-premises Sybase-database te maken. |Ja |

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

Zie voor meer informatie, [Sybase-connector](data-factory-onprem-sybase-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een Sybase-gegevensset, stel de **type** van de gegevensset in **RelationalTable**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
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

Zie voor meer informatie, [Sybase-connector](data-factory-onprem-sybase-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationele bron in de Kopieeractiviteit
Als u gegevens uit een Sybase-database kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en geeft u de volgende eigenschappen in de **bron** sectie :


| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `select * from MyTable`. |Nee (als **tableName** van **gegevensset** is opgegeven) |

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

Zie voor meer informatie, [Sybase-connector](data-factory-onprem-sybase-connector.md#copy-activity-properties) artikel.

## <a name="teradata"></a>Teradata

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Teradata, stelt u de **type** van de gekoppelde service om **OnPremisesTeradata**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| server |Naam van de Teradata-server. |Ja |
| authenticationType |Het type verificatie gebruikt voor verbinding met de Teradata-database. Mogelijke waarden zijn: Anoniem, basis en Windows. |Ja |
| gebruikersnaam |Geef de gebruikersnaam op als u basisverificatie of Windows-verificatie. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |
| gatewayName |De naam van de gateway die de Data Factory-service gebruiken moet voor verbinding met de on-premises Teradata-database. |Ja |

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

Zie voor meer informatie, [Teradata connector](data-factory-onprem-teradata-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een Teradata-Blob-gegevensset, stel de **type** van de gegevensset in **RelationalTable**. Er zijn momenteel geen type eigenschappen die worden ondersteund voor de Teradata-gegevensset. 

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

Zie voor meer informatie, [Teradata connector](data-factory-onprem-teradata-connector.md#dataset-properties) artikel.

### <a name="relational-source-in-copy-activity"></a>Relationele bron in de Kopieeractiviteit
Als u gegevens van een Teradata-database kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en geeft u de volgende eigenschappen in de **bron** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `select * from MyTable`. |Ja |

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

Zie voor meer informatie, [Teradata connector](data-factory-onprem-teradata-connector.md#copy-activity-properties) artikel.

## <a name="cassandra"></a>Cassandra


### <a name="linked-service"></a>Gekoppelde service
Instellen voor het definiëren van een Cassandra-gekoppelde service, de **type** van de gekoppelde service om **OnPremisesCassandra**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| host |Een of meer IP-adressen of hostnamen van de Cassandra-servers.<br/><br/>Geef een door komma's gescheiden lijst met IP-adressen of hostnamen gelijktijdig verbinding maken met alle servers. |Ja |
| poort |De TCP-poort die gebruikmaakt van de Cassandra-server om te luisteren naar clientverbindingen. |Nee, standaardwaarde: 9042 |
| authenticationType |Basis- of anonieme |Ja |
| gebruikersnaam |Geef de gebruikersnaam op voor het gebruikersaccount. |Ja, als authenticationType is ingesteld op het Basislidmaatschap. |
| wachtwoord |Wachtwoord voor het gebruikersaccount opgeven. |Ja, als authenticationType is ingesteld op het Basislidmaatschap. |
| gatewayName |De naam van de gateway die wordt gebruikt om te verbinden met de on-premises Cassandra-database. |Ja |
| encryptedCredential |Referentie versleuteld door de gateway. |Nee |

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

Zie voor meer informatie, [Cassandra connector](data-factory-onprem-cassandra-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een Cassandra-gegevensset, stel de **type** van de gegevensset in **CassandraTable**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| keyspace |De naam van de keyspace of het schema in Cassandra-database. |Ja (als **query** voor **CassandraSource** is niet gedefinieerd). |
| tableName |De naam van de tabel in de Cassandra-database. |Ja (als **query** voor **CassandraSource** is niet gedefinieerd). |

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

Zie voor meer informatie, [Cassandra connector](data-factory-onprem-cassandra-connector.md#dataset-properties) artikel. 

### <a name="cassandra-source-in-copy-activity"></a>Cassandra-bron in de Kopieeractiviteit
Als u gegevens van Cassandra kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **CassandraSource**, en geeft u de volgende eigenschappen in de **bron** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |SQL-92 query of CQL-query. Zie [CQL-verwijzing](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Wanneer u SQL-query gebruikt, geef **keyspace name.table naam** om weer te geven van de tabel die u wilt zoeken. |Nee (als de tabelnaam en keyspace op de gegevensset zijn gedefinieerd). |
| consistencyLevel |Het consistentieniveau Hiermee geeft u het aantal replica's moeten reageren op een leesaanvraag alvorens gegevens naar de clienttoepassing. Cassandra wordt het opgegeven aantal replica's voor gegevens om te voldoen aan de leesaanvraag gecontroleerd. |EEN, TWEE, DRIE, QUORUM, ALLE, LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE. Zie [gegevensconsistentie configureren](https://docs.datastax.com/en/cassandra/2.1/cassandra/dml/dml_config_consistency_c.html) voor meer informatie. |Nee. Standaardwaarde is een. |

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

Zie voor meer informatie, [Cassandra connector](data-factory-onprem-cassandra-connector.md#copy-activity-properties) artikel.

## <a name="mongodb"></a>MongoDB

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een mongodb-database, stelt u de **type** van de gekoppelde service om **OnPremisesMongoDB**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| server |IP-adres of de hostnaam de naam van de MongoDB-server. |Ja |
| poort |TCP-poort die de MongoDB-server wordt gebruikt om te luisteren naar clientverbindingen. |Optionele standaardwaarde: 27017 |
| authenticationType |Basic, of anonieme. |Ja |
| gebruikersnaam |Gebruikersaccount voor toegang tot MongoDB. |Ja (als basisverificatie wordt gebruikt). |
| wachtwoord |Het wachtwoord voor de gebruiker. |Ja (als basisverificatie wordt gebruikt). |
| authSource |De naam van de MongoDB-database die u wilt gebruiken om te controleren of uw referenties voor verificatie. |Optioneel (als basisverificatie wordt gebruikt). Standaard: maakt gebruik van het beheerdersaccount en de database die is opgegeven met behulp van de eigenschap databaseName. |
| databaseName |De naam van de MongoDB-database die u wilt openen. |Ja |
| gatewayName |Naam van de gateway die toegang heeft tot het gegevensarchief. |Ja |
| encryptedCredential |De referentie is versleuteld met de gateway. |Optioneel |

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

Zie voor meer informatie, [MongoDB-connector artikel](data-factory-on-premises-mongodb-connector.md#linked-service-properties)

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een MongoDB-gegevensset, stel de **type** van de gegevensset in **MongoDbCollection**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| collectionName |De naam van de verzameling in de MongoDB-database. |Ja |

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

Zie voor meer informatie, [MongoDB-connector artikel](data-factory-on-premises-mongodb-connector.md#dataset-properties)

#### <a name="mongodb-source-in-copy-activity"></a>MongoDB-bron in de Kopieeractiviteit
Als u gegevens van MongoDB kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **MongoDbSource**, en geeft u de volgende eigenschappen in de **bron** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |SQL-92 query-tekenreeks. Bijvoorbeeld: `select * from MyTable`. |Nee (als **collectionName** van **gegevensset** is opgegeven) |

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

Zie voor meer informatie, [MongoDB-connector artikel](data-factory-on-premises-mongodb-connector.md#copy-activity-properties)

## <a name="amazon-s3"></a>Amazon S3


### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Amazon S3, stelt u de **type** van de gekoppelde service om **AwsAccessKey**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| accessKeyID |ID van de geheime toegangssleutel. |string |Ja |
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

Zie voor meer informatie, [Amazon S3-connector artikel](data-factory-amazon-simple-storage-service-connector.md#linked-service-properties).

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een Amazon S3-gegevensset, stel de **type** van de gegevensset in **Amazon S3**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| bucketName |De naam van de S3-bucket. |Reeks |Ja |
| sleutel |De sleutel van de S3-object. |Reeks |Nee |
| voorvoegsel |Voorvoegsel voor de sleutel S3-object. Objecten waarvan sleutels met dit voorvoegsel beginnen worden geselecteerd. Geldt alleen wanneer de sleutel is leeg. |Reeks |Nee |
| versie |De versie van de S3-object als S3 versiebeheer is ingeschakeld. |Reeks |Nee |
| Indeling | De volgende bestandsindelingen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc-indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [Parquet-indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer. |Nee | |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. De ondersteunde niveaus zijn: **Optimale** en **snelste**. Zie voor meer informatie, [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee | |


> [!NOTE]
> Hiermee geeft u de locatie van de S3-object waarbij bucket de hoofdcontainer voor S3-objecten en -sleutel is het volledige pad naar een S3-object bucketName + te drukken.

#### <a name="example-sample-dataset-with-prefix"></a>Voorbeeld: Voorbeeldgegevensset met het voorvoegsel

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
#### <a name="example-sample-data-set-with-version"></a>Voorbeeld: Verzameling voorbeeldgegevens (met versie)

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
In het voorbeeld gebruiken we vaste waarden voor de sleutel en bucketName eigenschappen in de Amazon S3-gegevensset.

```json
"key": "testFolder/test.orc",
"bucketName": "<S3 bucket name>",
```

U kunt Data Factory de sleutel en bucketName in runtime dynamisch berekenen met behulp van de systeemvariabelen zoals slicestart-waarde hebben.

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

U kunt Doe hetzelfde voor de eigenschap van het voorvoegsel van een Amazon S3-gegevensset. Zie [Data Factory-functies en systeemvariabelen](data-factory-functions-variables.md) voor een lijst van ondersteunde functies en variabelen.

Zie voor meer informatie, [Amazon S3-connector artikel](data-factory-amazon-simple-storage-service-connector.md#dataset-properties).

### <a name="file-system-source-in-copy-activity"></a>Bestand System-bron in de Kopieeractiviteit
Als u gegevens vanaf Amazon S3 kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **FileSystemSource**, en geeft u de volgende eigenschappen in de **bron** sectie:


| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| recursieve |Hiermee geeft u op of S3 recursief moeten lijst objecten in de map. |waar/onwaar |Nee |


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

Zie voor meer informatie, [Amazon S3-connector artikel](data-factory-amazon-simple-storage-service-connector.md#copy-activity-properties).

## <a name="file-system"></a>Bestandssysteem


### <a name="linked-service"></a>Gekoppelde service
U kunt een on-premises bestandssysteem koppelen aan een Azure-gegevensfactory met de **On-Premises bestandsserver** gekoppelde service. De volgende tabel bevat beschrijvingen van JSON-elementen die specifiek voor de bestandsserver van On-Premises gekoppelde service zijn.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |Zorg ervoor dat de eigenschap type wordt ingesteld op **OnPremisesFileServer**. |Ja |
| host |Hiermee geeft u het pad naar de hoofdmap van de map die u wilt kopiëren. Gebruik het escape-teken ' \ ' voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppelde service en de gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden. |Ja |
| gebruikers-id |Geef de ID van de gebruiker die toegang tot de server heeft. |Nee (als u ervoor encryptedCredential kiest) |
| wachtwoord |Geef het wachtwoord voor de gebruiker (gebruikersnaam). |Nee (als u ervoor encryptedCredential kiest |
| encryptedCredential |Geef de versleutelde referenties die u krijgen kunt door de cmdlet New-AzureRmDataFactoryEncryptValue uit te voeren. |Nee (als u ervoor kiest om op te geven van gebruikers-id en wachtwoord in tekst zonder opmaak) |
| gatewayName |Hiermee geeft u de naam van de gateway die Data Factory moet worden gebruikt verbinding maken met de on-premises bestandsserver. |Ja |

#### <a name="sample-folder-path-definitions"></a>Voorbeeld van map pad definities 
| Scenario | Hosten in de definitie van de gekoppelde service | Mappad in de definitie van de gegevensset |
| --- | --- | --- |
| Lokale map op de machine Data Management Gateway: <br/><br/>Voorbeelden: D:\\ \* of D:\folder\subfolder\\* |D:\\ \\ (voor Data Management Gateway 2.0 en hoger) <br/><br/> localhost (voor oudere versies dan Data Management Gateway 2.0) |. \\ \\ of de map\\\\submap (voor Data Management Gateway 2.0 en hoger) <br/><br/>D:\\ \\ of D:\\\\map\\\\submap (voor de gatewayversie lager dan 2.0) |
| Externe gedeelde map: <br/><br/>Voorbeelden: \\ \\MijnServer\\delen\\ \* of \\ \\MijnServer\\delen\\map\\submap\\* |\\\\\\\\myserver\\\\share |. \\ \\ of de map\\\\submap |


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

#### <a name="example-using-encryptedcredential"></a>Voorbeeld: Met behulp van encryptedcredential

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

Zie voor meer informatie, [File System-connector artikel](data-factory-onprem-file-system-connector.md#linked-service-properties).

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een gegevensset File System, stel de **type** van de gegevensset in **bestandsshare**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| folderPath |Hiermee geeft u het subpad naar de map. Gebruik het escape-teken ' \' voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppelde service en de gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** naar de map paden op basis van het segment de status begin/einde en tijden. |Ja |
| fileName |Geef de naam van het bestand in de **folderPath** als u wilt dat de tabel om te verwijzen naar een specifiek bestand in de map. Als u een waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Als geen bestandsnaam is opgegeven voor een uitvoergegevensset, wordt de naam van het gegenereerde bestand is in de volgende indeling: <br/><br/>`Data.<Guid>.txt` (Voorbeeld: Data.0a405f8a-93ff-4C6F-b3be-f69616f1df7a.txt) |Nee |
| fileFilter |Geef een filter op dat moet worden gebruikt voor het selecteren van een subset van de bestanden in het mappad in plaats van alle bestanden. <br/><br/>Toegestane waarden zijn: `*` (meerdere tekens) en `?` (Eén teken).<br/><br/>Voorbeeld 1: "fileFilter": "* .log"<br/>Voorbeeld 2: "fileFilter': -1 - 2016? txt"<br/><br/>Houd er rekening mee dat fileFilter is van toepassing voor een invoergegevensset van de bestandsshare. |Nee |
| partitionedBy |U kunt partitionedBy gebruiken om op te geven van een dynamische folderPath/bestandsnaam voor time series-gegevens. Een voorbeeld is folderPath geparametriseerde voor elk uur gegevens. |Nee |
| Indeling | De volgende bestandsindelingen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc-indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [Parquet-indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer. |Nee |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**; en ondersteunde niveaus zijn: **Optimale** en **snelste**. Zie [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

> [!NOTE]
> U kunt geen bestandsnaam en fileFilter tegelijkertijd gebruiken.

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

Zie voor meer informatie, [File System-connector artikel](data-factory-onprem-file-system-connector.md#dataset-properties).

### <a name="file-system-source-in-copy-activity"></a>Bestand System-bron in de Kopieeractiviteit
Als u gegevens van het bestandssysteem kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **FileSystemSource**, en geeft u de volgende eigenschappen in de **bron** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| recursieve |Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. |True, False (standaard) |Nee |

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
Zie voor meer informatie, [File System-connector artikel](data-factory-onprem-file-system-connector.md#copy-activity-properties).

### <a name="file-system-sink-in-copy-activity"></a>Sink-bestandssysteem in de Kopieeractiviteit
Als u gegevens naar File System kopieert, stelt u de **sink-type** van de kopieeractiviteit naar **FileSystemSink**, en geeft u de volgende eigenschappen in de **sink** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| copyBehavior |Definieert het gedrag kopiëren wanneer de bron BlobSource of bestandssysteem is. |**PreserveHierarchy:** Hiermee behoudt u de bestandshiërarchie in de doelmap. Dat wil zeggen, is het relatieve pad van het bronbestand voor de bronmap hetzelfde als het relatieve pad van de doel-bestand naar de doelmap.<br/><br/>**FlattenHierarchy:** Alle bestanden uit de bronmap worden gemaakt in het eerste niveau van de doelmap. De doelbestanden worden gemaakt met een automatisch gegenereerde naam.<br/><br/>**MergeFiles:** Hiermee worden alle bestanden uit de bronmap naar één bestand samengevoegd. Als de naam/blob-naam van het bestand is opgegeven, is de naam van het samengevoegde de opgegeven naam. Anders is de bestandsnaam van een automatisch gegenereerde. |Nee |
automatisch:

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

Zie voor meer informatie, [File System-connector artikel](data-factory-onprem-file-system-connector.md#copy-activity-properties).

## <a name="ftp"></a>FTP

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde voor het definiëren van een FTP-service, stelt u de **type** van de gekoppelde service om **FtpServer**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist | Standaard |
| --- | --- | --- | --- |
| host |Naam of IP-adres van de FTP-Server |Ja |&nbsp; |
| authenticationType |Verificatietype opgeven |Ja |Basic-, anonieme |
| gebruikersnaam |Gebruikers die toegang tot de FTP-server heeft |Nee |&nbsp; |
| wachtwoord |Wachtwoord voor de gebruiker (gebruikersnaam) |Nee |&nbsp; |
| encryptedCredential |Versleutelde referenties voor toegang tot de FTP-server |Nee |&nbsp; |
| gatewayName |Naam van de Data Management Gateway verbinding maken met een on-premises FTP-server |Nee |&nbsp; |
| poort |Poort waarop de FTP-server luistert |Nee |21 |
| enableSsl |Geef op of u FTP via SSL/TLS-kanaal |Nee |true |
| enableServerCertificateValidation |Geef op of validatie van het servercertificaat SSL in te schakelen wanneer u met behulp van FTP via SSL/TLS-kanaal |Nee |true |

#### <a name="example-using-anonymous-authentication"></a>Voorbeeld: Met behulp van anonieme verificatie

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

#### <a name="example-using-encryptedcredential-for-authentication-and-gateway"></a>Voorbeeld: Met behulp van encryptedCredential voor verificatie en -gateway

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

Zie voor meer informatie, [FTP-connector](data-factory-ftp-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een FTP-gegevensset, stel de **type** van de gegevensset in **bestandsshare**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| folderPath |Subpad naar de map. Gebruik van escape-teken ' \ ' voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppelde service en de gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** naar de map paden op basis van het segment de status begin/einde en tijden. |Ja 
| fileName |Geef de naam van het bestand in de **folderPath** als u wilt dat de tabel om te verwijzen naar een specifiek bestand in de map. Als u een waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Als geen bestandsnaam is opgegeven voor een uitvoergegevensset, de naam van het gegenereerde bestand zou worden in de volgende notatie: <br/><br/>De gegevens. <Guid>.txt (voorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nee |
| fileFilter |Geef een filter op dat moet worden gebruikt voor het selecteren van een subset van de bestanden in het mappad in plaats van alle bestanden.<br/><br/>Toegestane waarden zijn: `*` (meerdere tekens) en `?` (Eén teken).<br/><br/>Voorbeeld 1: `"fileFilter": "*.log"`<br/>Voorbeeld 2: `"fileFilter": 2016-1-?.txt"`<br/><br/> fileFilter is van toepassing voor een invoergegevensset van de bestandsshare. Deze eigenschap wordt niet ondersteund met HDFS. |Nee |
| partitionedBy |partitionedBy kan worden gebruikt om op te geven van een dynamische folderPath, filename voor time series-gegevens. Bijvoorbeeld, folderPath geparametriseerde voor elk uur gegevens. |Nee |
| Indeling | De volgende bestandsindelingen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc-indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [Parquet-indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer. |Nee |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**; en ondersteunde niveaus zijn: **Optimale** en **snelste**. Zie voor meer informatie, [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |
| useBinaryTransfer |Opgeven of binaire overdrachtsmodus gebruiken. De waarde True voor binaire modus en de waarde false ASCII. Standaardwaarde: De waarde True. Deze eigenschap kan alleen worden gebruikt wanneer de bijbehorende gekoppelde servicetype is van het type: FtpServer. |Nee |

> [!NOTE]
> bestandsnaam en fileFilter kunnen niet tegelijkertijd worden gebruikt.

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

Zie voor meer informatie, [FTP-connector](data-factory-ftp-connector.md#dataset-properties) artikel.

### <a name="file-system-source-in-copy-activity"></a>Bestand System-bron in de Kopieeractiviteit
Als u gegevens van een FTP-server kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **FileSystemSource**, en geeft u de volgende eigenschappen in de **bron** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| recursieve |Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. |True, False (standaard) |Nee |

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

Zie voor meer informatie, [FTP-connector](data-factory-ftp-connector.md#copy-activity-properties) artikel.


## <a name="hdfs"></a>HDFS

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een HDFS, stelt u de **type** van de gekoppelde service om **Hdfs**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **Hdfs** |Ja |
| URL |URL naar het HDFS |Ja |
| authenticationType |Anoniem, of Windows. <br><br> Gebruik **Kerberos-verificatie** voor HDFS-connector, raadpleegt u [in deze sectie](#use-kerberos-authentication-for-hdfs-connector) voor het instellen van uw on-premises omgeving dienovereenkomstig. |Ja |
| Gebruikersnaam |Gebruikersnaam voor Windows-verificatie. |Ja (voor Windows-verificatie) |
| wachtwoord |Wachtwoord voor Windows-verificatie. |Ja (voor Windows-verificatie) |
| gatewayName |De naam van de gateway die de Data Factory-service gebruiken moet voor verbinding met het HDFS. |Ja |
| encryptedCredential |[Nieuwe AzureRMDataFactoryEncryptValue](https://docs.microsoft.com/powershell/module/azurerm.datafactories/new-azurermdatafactoryencryptvalue) uitvoer van de referentie op die toegang. |Nee |

#### <a name="example-using-anonymous-authentication"></a>Voorbeeld: Met behulp van anonieme verificatie

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

Zie voor meer informatie, [HDFS connector](#data-factory-hdfs-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een gegevensset HDFS, stel de **type** van de gegevensset in **bestandsshare**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| folderPath |Pad naar de map. Voorbeeld: `myfolder`<br/><br/>Gebruik van escape-teken ' \ ' voor speciale tekens in de tekenreeks. Bijvoorbeeld: voor folder\subfolder, geeft u de map\\\\submap en geef voor de d:\samplefolder, d:\\\\Voorbeeldmap.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** naar de map paden op basis van het segment de status begin/einde en tijden. |Ja |
| fileName |Geef de naam van het bestand in de **folderPath** als u wilt dat de tabel om te verwijzen naar een specifiek bestand in de map. Als u een waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Als geen bestandsnaam is opgegeven voor een uitvoergegevensset, de naam van het gegenereerde bestand zou worden in de volgende notatie: <br/><br/>De gegevens. <Guid>.txt (voorbeeld:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nee |
| partitionedBy |partitionedBy kan worden gebruikt om op te geven van een dynamische folderPath, filename voor time series-gegevens. Voorbeeld: folderPath geparametriseerde voor elk uur gegevens. |Nee |
| Indeling | De volgende bestandsindelingen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc-indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [Parquet-indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer. |Nee |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus zijn: **Optimale** en **snelste**. Zie voor meer informatie, [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

> [!NOTE]
> bestandsnaam en fileFilter kunnen niet tegelijkertijd worden gebruikt.

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

Zie voor meer informatie, [HDFS connector](#data-factory-hdfs-connector.md#dataset-properties) artikel. 

### <a name="file-system-source-in-copy-activity"></a>Bestand System-bron in de Kopieeractiviteit
Als u gegevens van HDFS kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **FileSystemSource**, en geeft u de volgende eigenschappen in de **bron** sectie:

**FileSystemSource** ondersteunt de volgende eigenschappen:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| recursieve |Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. |True, False (standaard) |Nee |

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

Zie voor meer informatie, [HDFS connector](#data-factory-hdfs-connector.md#copy-activity-properties) artikel.

## <a name="sftp"></a>SFTP


### <a name="linked-service"></a>Gekoppelde service
Gekoppelde voor het definiëren van een SFTP-service, stelt u de **type** van de gekoppelde service om **Sftp**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- | --- |
| host | Naam of IP-adres van de SFTP-server. |Ja |
| poort |De poort waarop de SFTP-server luistert. De standaardwaarde is: 21 |Nee |
| authenticationType |Geef het verificatietype. Toegestane waarden: **Basic**, **SshPublicKey**. <br><br> Raadpleeg [met basisverificatie](#using-basic-authentication) en [met behulp van SSH openbare sleutelverificatie](#using-ssh-public-key-authentication) respectievelijk de secties over meer eigenschappen en voorbeelden voor JSON. |Ja |
| skipHostKeyValidation | Geef op of moet worden overgeslagen sleutelvalidatie voor de host. | Nee. De standaardwaarde: false |
| hostKeyFingerprint | Geef de vingerafdruk van de hostsleutel. | Ja als de `skipHostKeyValidation` is ingesteld op false.  |
| gatewayName |De naam van de Data Management Gateway verbinding maken met een on-premises SFTP-server. | Ja als het kopiëren van gegevens uit een on-premises SFTP-server. |
| encryptedCredential | Versleutelde referenties voor toegang tot de SFTP-server. Automatisch gegenereerde wanneer u basisverificatie (gebruikersnaam en wachtwoord) of SshPublicKey verificatie (gebruikersnaam + pad naar de persoonlijke sleutel of inhoud) in de wizard kopiëren of de ClickOnce-pop-dialoogvenster opgeeft. | Nee. Gelden alleen wanneer het kopiëren van gegevens uit een on-premises SFTP-server. |

#### <a name="example-using-basic-authentication"></a>Voorbeeld: Met behulp van basisverificatie

Als u wilt gebruikmaken van basisverificatie instellen `authenticationType` als `Basic`, en geeft u de volgende eigenschappen naast de algemene die zijn geïntroduceerd in de laatste sectie van de SFTP-connector:

| Eigenschap | Description | Vereist |
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

#### <a name="example-basic-authentication-with-encrypted-credential"></a>Voorbeeld: **Basisverificatie wordt gebruikt met de versleutelde referentie**

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

#### <a name="using-ssh-public-key-authentication"></a>**Met behulp van SSH-verificatie voor openbare sleutel:**

Als u wilt gebruikmaken van basisverificatie instellen `authenticationType` als `SshPublicKey`, en geeft u de volgende eigenschappen naast de algemene die zijn geïntroduceerd in de laatste sectie van de SFTP-connector:

| Eigenschap | Description | Vereist |
| --- | --- | --- | --- |
| gebruikersnaam |Gebruikers die toegang tot de SFTP-server heeft |Ja |
| privateKeyPath | Geef het absolute pad naar het persoonlijke sleutelbestand dat de gateway toegankelijk. | Opgeven of de `privateKeyPath` of `privateKeyContent`. <br><br> Gelden alleen wanneer het kopiëren van gegevens uit een on-premises SFTP-server. |
| privateKeyContent | Een geserialiseerde tekenreeks van de inhoud van de persoonlijke sleutel. De Wizard kopiëren kunt lezen van het bestand met persoonlijke sleutel en de persoonlijke sleutel inhoud automatisch uit te pakken. Als u van andere hulpprogramma/SDK gebruikmaakt, in plaats daarvan de eigenschap privateKeyPath gebruiken. | Opgeven of de `privateKeyPath` of `privateKeyContent`. |
| Wachtwoordzin | Geef de pass-woordgroep en wachtwoord in voor het ontsleutelen van de persoonlijke sleutel als bestand met de sleutel is beveiligd met een wachtwoordzin. | Ja als het bestand met persoonlijke sleutel wordt beveiligd door een wachtwoordzin. |

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

#### <a name="example-sshpublickey-authentication-using-private-key-content"></a>Voorbeeld: **SshPublicKey verificatie met behulp van de persoonlijke sleutel inhoud**

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

Zie voor meer informatie, [SFTP-connector](data-factory-sftp-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een SFTP-gegevensset, stel de **type** van de gegevensset in **bestandsshare**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| folderPath |Subpad naar de map. Gebruik van escape-teken ' \ ' voor speciale tekens in de tekenreeks. Zie [voorbeeld gekoppelde service en de gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** naar de map paden op basis van het segment de status begin/einde en tijden. |Ja |
| fileName |Geef de naam van het bestand in de **folderPath** als u wilt dat de tabel om te verwijzen naar een specifiek bestand in de map. Als u een waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Als geen bestandsnaam is opgegeven voor een uitvoergegevensset, de naam van het gegenereerde bestand zou worden in de volgende notatie: <br/><br/>De gegevens. <Guid>.txt (voorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |Nee |
| fileFilter |Geef een filter op dat moet worden gebruikt voor het selecteren van een subset van de bestanden in het mappad in plaats van alle bestanden.<br/><br/>Toegestane waarden zijn: `*` (meerdere tekens) en `?` (Eén teken).<br/><br/>Voorbeeld 1: `"fileFilter": "*.log"`<br/>Voorbeeld 2: `"fileFilter": 2016-1-?.txt"`<br/><br/> fileFilter is van toepassing voor een invoergegevensset van de bestandsshare. Deze eigenschap wordt niet ondersteund met HDFS. |Nee |
| partitionedBy |partitionedBy kan worden gebruikt om op te geven van een dynamische folderPath, filename voor time series-gegevens. Bijvoorbeeld, folderPath geparametriseerde voor elk uur gegevens. |Nee |
| Indeling | De volgende bestandsindelingen worden ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Stel de **type** eigenschap onder indeling op een van deze waarden. Zie voor meer informatie, [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc-indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [Parquet-indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. <br><br> Als u wilt **bestanden als kopiëren-is** overslaan tussen op basis van bestanden (binaire kopie), het gedeelte indeling in beide definities van de gegevensset voor invoer en uitvoer. |Nee |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus zijn: **Optimale** en **snelste**. Zie voor meer informatie, [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |
| useBinaryTransfer |Opgeven of binaire overdrachtsmodus gebruiken. De waarde True voor binaire modus en de waarde false ASCII. Standaardwaarde: De waarde True. Deze eigenschap kan alleen worden gebruikt wanneer de bijbehorende gekoppelde servicetype is van het type: FtpServer. |Nee |

> [!NOTE]
> bestandsnaam en fileFilter kunnen niet tegelijkertijd worden gebruikt.

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

Zie voor meer informatie, [SFTP-connector](data-factory-sftp-connector.md#dataset-properties) artikel. 

### <a name="file-system-source-in-copy-activity"></a>Bestand System-bron in de Kopieeractiviteit
Als u gegevens van een SFTP-bron kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **FileSystemSource**, en geeft u de volgende eigenschappen in de **bron** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| recursieve |Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen voor de opgegeven map. |True, False (standaard) |Nee |



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

Zie voor meer informatie, [SFTP-connector](data-factory-sftp-connector.md#copy-activity-properties) artikel.


## <a name="http"></a>HTTP

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een HTTP, stelt u de **type** van de gekoppelde service om **Http**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| url | Basis-URL naar de webserver | Ja |
| authenticationType | Hiermee geeft u het verificatietype. Toegestane waarden zijn: **Anonieme**, **Basic**, **Digest**, **Windows**, **ClientCertificate**. <br><br> Zie de secties onder deze tabel op meer eigenschappen en JSON-voorbeelden voor deze verificatietypen respectievelijk. | Ja |
| enableServerCertificateValidation | Geef op of het inschakelen van validatie van het servercertificaat SSL als bron HTTPS-webserver is | Nee, de standaardinstelling is true |
| gatewayName | De naam van de Data Management Gateway verbinding maken met een on-premises HTTP-bron. | Ja als het kopiëren van gegevens uit een on-premises HTTP-bron. |
| encryptedCredential | Versleutelde referenties voor toegang tot het HTTP-eindpunt. Automatisch gegenereerde bij het configureren van de verificatie-informatie in de wizard kopiëren of de ClickOnce-pop-dialoogvenster. | Nee. Gelden alleen wanneer het kopiëren van gegevens uit een on-premises HTTP-server. |

#### <a name="example-using-basic-digest-or-windows-authentication"></a>Voorbeeld: Met behulp van basisverificatie, verificatiesamenvatting of Windows-verificatie
Stel `authenticationType` als `Basic`, `Digest`, of `Windows`, en geeft u de volgende eigenschappen naast de HTTP-connector algemeen die hierboven wordt geïntroduceerd:

| Eigenschap | Description | Vereist |
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

#### <a name="example-using-clientcertificate-authentication"></a>Voorbeeld: Met behulp van ClientCertificate verificatie

Als u wilt gebruikmaken van basisverificatie instellen `authenticationType` als `ClientCertificate`, en geeft u de volgende eigenschappen naast de HTTP-connector algemeen die hierboven wordt geïntroduceerd:

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| embeddedCertData | De met Base64 gecodeerde inhoud van binaire gegevens van de Personal Information Exchange (PFX)-bestand. | Opgeven of de `embeddedCertData` of `certThumbprint`. |
| certThumbprint | De vingerafdruk van het certificaat dat is geïnstalleerd op de gatewaycomputer certificatenstore. Gelden alleen wanneer het kopiëren van gegevens uit een on-premises HTTP-bron. | Opgeven of de `embeddedCertData` of `certThumbprint`. |
| wachtwoord | Het wachtwoord dat is gekoppeld aan het certificaat. | Nee |

Als u `certThumbprint` voor verificatie en het certificaat in het persoonlijke archief van de lokale computer is geïnstalleerd, moet u de leesmachtiging voor de gateway-service te verlenen:

1. Start Microsoft Management Console (MMC). Voeg de **certificaten** -module die is gericht op de **lokale Computer**.
2. Vouw **certificaten**, **persoonlijke**, en klikt u op **certificaten**.
3. Met de rechtermuisknop op het certificaat uit het persoonlijke archief, en selecteer **alle taken**->**persoonlijke sleutels beheren...**
3. Op de **Security** tabblad, voegt u het gebruikersaccount waarmee Data Management Gateway Host-Service wordt uitgevoerd met de leestoegang voor het certificaat.  

**Voorbeeld:-clientcertificaat gebruiken:** Deze gekoppelde service koppelt uw data factory op een on-premises HTTP-webserver. Het maakt gebruik van een clientcertificaat dat op de computer is geïnstalleerd met Data Management Gateway is geïnstalleerd.

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

#### <a name="example-using-client-certificate-in-a-file"></a>Voorbeeld:-clientcertificaat gebruiken in een bestand
Deze gekoppelde service koppelt uw data factory op een on-premises HTTP-webserver. Hierbij wordt een certificaatbestand op de computer met Data Management Gateway is geïnstalleerd.

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

Zie voor meer informatie, [HTTP-connector](data-factory-http-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een HTTP-gegevensset, stel de **type** van de gegevensset in **Http**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| relativeurl bevatten | Een relatieve URL naar de resource die de gegevens bevat. Als geen pad is opgegeven, worden alleen de URL die is opgegeven in de definitie van de gekoppelde service wordt gebruikt. <br><br> Kan de URL van de dynamische, kunt u [Data Factory-functies en systeemvariabelen](data-factory-functions-variables.md), voorbeeld: `"relativeUrl": "$$Text.Format('/my/report?month={0:yyyy}-{0:MM}&fmt=csv', SliceStart)"`. | Nee |
| requestMethod | HTTP-methode. Toegestane waarden zijn **ophalen** of **POST**. | Nee. De standaardwaarde is `GET`. |
| additionalHeaders | Extra kopteksten die HTTP-aanvraag. | Nee |
| RequestBody | De hoofdtekst van HTTP-aanvraag. | Nee |
| Indeling | Als u zich gewoon wilt **ophalen van de gegevens van het HTTP-eindpunt als-is** overslaan zonder deze parseren, deze instellingen bestandsindelingen. <br><br> Als u de inhoud van de HTTP-reactie parseren tijdens het kopiëren wilt, worden de volgende bestandsindelingen ondersteund: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Zie voor meer informatie, [tekstindeling](data-factory-supported-file-and-compression-formats.md#text-format), [Json-indeling](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-indeling](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc-indeling](data-factory-supported-file-and-compression-formats.md#orc-format), en [Parquet-indeling](data-factory-supported-file-and-compression-formats.md#parquet-format) secties. |Nee |
| Compressie | Geef het type en het niveau van compressie voor de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, **BZip2**, en **ZipDeflate**. Ondersteunde niveaus zijn: **Optimale** en **snelste**. Zie voor meer informatie, [bestands- en compressie indelingen in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nee |

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

#### <a name="example-using-the-post-method"></a>Voorbeeld: met behulp van de POST-methode

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
Zie voor meer informatie, [HTTP-connector](data-factory-http-connector.md#dataset-properties) artikel.

### <a name="http-source-in-copy-activity"></a>HTTP-bron in de Kopieeractiviteit
Als u gegevens van een HTTP-bron kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **HttpSource**, en geeft u de volgende eigenschappen in de **bron** sectie:

| Eigenschap | Description | Vereist |
| -------- | ----------- | -------- |
| httpRequestTimeout | De time-out (duur) voor de HTTP-aanvraag reageert. De time-out is een antwoord, niet de time-out voor lezen van gegevens van de reactie. | Nee. Standaardwaarde: 00:01:40 |


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

Zie voor meer informatie, [HTTP-connector](data-factory-http-connector.md#copy-activity-properties) artikel.

## <a name="odata"></a>OData

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde voor het definiëren van een OData-service, stelt u de **type** van de gekoppelde service om **OData**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| url |URL van de OData-service. |Ja |
| authenticationType |Het type verificatie gebruikt voor verbinding met de OData-bron. <br/><br/> Voor cloud OData zijn mogelijke waarden ingesteld op anoniem, basis en OAuth (Opmerking: momenteel alleen ondersteuning voor Azure Data Factory Azure Active Directory op basis van OAuth). <br/><br/> Voor on-premises OData zijn mogelijke waarden ingesteld op anoniem, basis en Windows. |Ja |
| gebruikersnaam |Geef de gebruikersnaam op als u basisverificatie gebruikt. |Ja (alleen als u basisverificatie gebruikt) |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Ja (alleen als u basisverificatie gebruikt) |
| authorizedCredential |Als u van OAuth gebruikmaakt, klikt u op **autoriseren** knop in de Kopieerwizard van Data Factory of Editor en voer uw referenties, en vervolgens de waarde van deze eigenschap wordt niet automatisch wordt gegenereerd. |Ja (alleen als u met behulp van OAuth-verificatie) |
| gatewayName |De naam van de gateway die de Data Factory-service gebruiken moet voor verbinding met de on-premises OData-service. Geef alleen als u gegevens van on-premises OData-bron kopieert. |Nee |

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

#### <a name="example---using-anonymous-authentication"></a>Voorbeeld: met behulp van anonieme verificatie

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

#### <a name="example---using-windows-authentication-accessing-on-premises-odata-source"></a>Bijvoorbeeld: met behulp van Windows-verificatie toegang tot on-premises OData-bron

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

#### <a name="example---using-oauth-authentication-accessing-cloud-odata-source"></a>Voorbeeld: met behulp van OAuth-verificatie verkrijgen van toegang tot cloud OData-bron
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

Zie voor meer informatie, [OData-connector](data-factory-odata-connector.md#linked-service-properties) artikel.

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een OData-gegevensset, stel de **type** van de gegevensset in **ODataResource**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| pad |Pad naar de OData-resource |Nee |

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

Zie voor meer informatie, [OData-connector](data-factory-odata-connector.md#dataset-properties) artikel.

### <a name="relational-source-in-copy-activity"></a>Relationele bron in de Kopieeractiviteit
Als u gegevens uit een OData-bron kopiëren wilt, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en geeft u de volgende eigenschappen in de **bron** sectie:

| Eigenschap | Description | Voorbeeld | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |'? $select = de naam, beschrijving en $top = 5 ' |Nee |

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

Zie voor meer informatie, [OData-connector](data-factory-odata-connector.md#copy-activity-properties) artikel.


## <a name="odbc"></a>ODBC


### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een ODBC, stelt u de **type** van de gekoppelde service om **OnPremisesOdbc**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| connectionString |Het niet-access-referentie-gedeelte van de verbindingsreeks en een optionele versleutelde referentie. Zie de voorbeelden in de volgende secties. |Ja |
| referenties |De access-referentie-gedeelte van de verbindingsreeks die is opgegeven in de indeling van de eigenschap / waarde-specifieke stuurprogramma's. Voorbeeld: "Uid =<user ID>; Pwd =<password>; RefreshToken =<secret refresh token>; ". |Nee |
| authenticationType |Het type verificatie gebruikt voor verbinding met de ODBC-gegevensopslag. Mogelijke waarden zijn: Anonieme en Basic. |Ja |
| gebruikersnaam |Geef de gebruikersnaam op als u basisverificatie gebruikt. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |
| gatewayName |De naam van de gateway die de Data Factory-service gebruiken moet voor verbinding met de ODBC-gegevensopslag. |Ja |

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
#### <a name="example---using-basic-authentication-with-encrypted-credentials"></a>Voorbeeld: met behulp van basisverificatie wordt gebruikt met de versleutelde referenties
Kunt u de referenties met behulp van versleutelen de [New-AzureRMDataFactoryEncryptValue](https://docs.microsoft.com/powershell/module/azurerm.datafactories/new-azurermdatafactoryencryptvalue) cmdlet (versie 1.0 van Azure PowerShell) of [New-AzureDataFactoryEncryptValue](https://msdn.microsoft.com/library/dn834940.aspx) (0,9 of een eerdere versie van Azure PowerShell).  

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

#### <a name="example-using-anonymous-authentication"></a>Voorbeeld: Met behulp van anonieme verificatie

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

Zie voor meer informatie, [ODBC connector](data-factory-odbc-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een ODBC-gegevensset, stel de **type** van de gegevensset in **RelationalTable**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| tableName |Naam van de tabel in de ODBC-gegevensopslag. |Ja |


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

Zie voor meer informatie, [ODBC connector](data-factory-odbc-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationele bron in de Kopieeractiviteit
Als u gegevens van een ODBC-gegevensopslag kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en geeft u de volgende eigenschappen in de **bron** sectie :

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |SQL-query-tekenreeks. Bijvoorbeeld: `select * from MyTable`. |Ja |

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

Zie voor meer informatie, [ODBC connector](data-factory-odbc-connector.md#copy-activity-properties) artikel.

## <a name="salesforce"></a>SalesForce


### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Salesforce, stelt u de **type** van de gekoppelde service om **Salesforce**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| environmentUrl | Geef de URL van de Salesforce-exemplaar. <br><br> -De standaardwaarde is "https://login.salesforce.com'. <br> -Om gegevens te kopiëren van sandbox, Geef 'https://test.salesforce.com'. <br> -Om gegevens te kopiëren uit aangepaste domein, opgeven, bijvoorbeeld 'https://[domain].my.salesforce.com'. |Nee |
| gebruikersnaam |Geef een gebruikersnaam voor het gebruikersaccount. |Ja |
| wachtwoord |Geef een wachtwoord voor het gebruikersaccount. |Ja |
| securityToken |Geef een beveiligingstoken voor het gebruikersaccount. Zie [security-token ophalen](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) voor instructies over het opnieuw instellen/ophalen van een beveiligingstoken. Zie voor meer informatie over beveiligingstokens het in het algemeen, [beveiligings- en de API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Ja |

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

Zie voor meer informatie, [Salesforce-connector](data-factory-salesforce-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een Salesforce-gegevensset, stel de **type** van de gegevensset in **RelationalTable**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| tableName |Naam van de tabel in Salesforce. |Nee (als een **query** van **RelationalSource** is opgegeven) |

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

Zie voor meer informatie, [Salesforce-connector](data-factory-salesforce-connector.md#dataset-properties) artikel. 

### <a name="relational-source-in-copy-activity"></a>Relationele bron in de Kopieeractiviteit
Als u gegevens van Salesforce kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **RelationalSource**, en geeft u de volgende eigenschappen in de **bron** sectie:

| Eigenschap | Description | Toegestane waarden | Vereist |
| --- | --- | --- | --- |
| query |De aangepaste query gebruiken om gegevens te lezen. |Een SQL-92-query of [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) query. Bijvoorbeeld `select * from MyTable__c`. |Nee (als de **tableName** van de **gegevensset** is opgegeven) |

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
> Het gedeelte '__c' van de API-naam is nodig voor een aangepaste object.

Zie voor meer informatie, [Salesforce-connector](data-factory-salesforce-connector.md#copy-activity-properties) artikel. 

## <a name="web-data"></a>Web-gegevens 

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een webpagina, stelt u de **type** van de gekoppelde service om **Web**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| URL |URL naar de Web-bron |Ja |
| authenticationType |Anoniem. |Ja |
 

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

Zie voor meer informatie, [Webtabel connector](data-factory-web-table-connector.md#linked-service-properties) artikel. 

### <a name="dataset"></a>Gegevensset
Voor het definiëren van een Web-gegevensset, stel de **type** van de gegevensset in **WebTable**, en geeft u de volgende eigenschappen in de **typeProperties** sectie: 

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| type |Het type van de gegevensset. moet worden ingesteld op **WebTable** |Ja |
| pad |Een relatieve URL naar de resource met de tabel. |Nee. Als geen pad is opgegeven, worden alleen de URL die is opgegeven in de definitie van de gekoppelde service wordt gebruikt. |
| index |De index van de tabel in de resource. Zie [Get index van een tabel in een HTML-pagina](#get-index-of-a-table-in-an-html-page) sectie voor stappen voor het ophalen van de index van een tabel in een HTML-pagina. |Ja |

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

Zie voor meer informatie, [Webtabel connector](data-factory-web-table-connector.md#dataset-properties) artikel. 

### <a name="web-source-in-copy-activity"></a>Webbron in de Kopieeractiviteit
Als u gegevens uit een webtabel kopieert, stelt u de **gegevensbrontype** van de kopieeractiviteit naar **WebSource**. Op dit moment, wanneer de bron in de kopieeractiviteit is van het type **WebSource**, geen extra eigenschappen worden ondersteund.

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

Zie voor meer informatie, [Webtabel connector](data-factory-web-table-connector.md#copy-activity-properties) artikel. 

## <a name="compute-environments"></a>COMPUTE-OMGEVINGEN
De volgende tabel bevat de compute-omgevingen wordt ondersteund door Data Factory en de activiteiten voor gegevenstransformatie die kunnen worden uitgevoerd op deze. Klik op de koppeling voor de berekening die u geïnteresseerd bent in om te zien van de JSON-schema voor de gekoppelde service om deze te koppelen aan een gegevensfactory. 

| Compute-omgeving | Activiteiten |
| --- | --- |
| [On-demand HDInsight-cluster](#on-demand-azure-hdinsight-cluster) of [uw eigen HDInsight-cluster](#existing-azure-hdinsight-cluster) |[Aangepaste .NET-activiteit](#net-custom-activity), [Hive-activiteit](#hdinsight-hive-activity), [Pig-activiteit](#hdinsight-pig-activity), [MapReduce-activiteit](#hdinsight-mapreduce-activity), [Hadoop-streaming-activiteit](#hdinsight-streaming-activityd), [Spark-activiteit](#hdinsight-spark-activity) |
| [Azure Batch](#azure-batch) |[.NET aangepaste activiteit](#net-custom-activity) |
| [Azure Machine Learning](#azure-machine-learning) | [Machine Learning-Batchuitvoeringsactiviteit](#machine-learning-batch-execution-activity), [Machine Learning-activiteit resources bijwerken](#machine-learning-update-resource-activity) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics) |[Data Lake Analytics U-SQL](#data-lake-analytics-u-sql-activity) |
| [Azure SQL Database](#azure-sql-database-1), [Azure SQL datawarehouse](#azure-sql-data-warehouse-1), [SQL Server](#sql-server-1) |[Opgeslagen procedure](#stored-procedure-activity) |

## <a name="on-demand-azure-hdinsight-cluster"></a>On-demand Azure HDInsight-cluster
De Azure Data Factory-service kan automatisch maken van een Windows/Linux gebaseerd HDInsight-cluster op aanvraag om gegevens te verwerken. Het cluster is gemaakt in dezelfde regio als het opslagaccount (met de eigenschap linkedServiceName in de JSON) die zijn gekoppeld aan het cluster. U kunt de volgende activiteiten voor gegevenstransformatie uitvoeren op deze gekoppelde service: [aangepaste .NET-activiteit](#net-custom-activity), [Hive-activiteit](#hdinsight-hive-activity), [Pig-activiteit](#hdinsight-pig-activity), [MapReduce activiteit](#hdinsight-mapreduce-activity), [Hadoop-streaming-activiteit](#hdinsight-streaming-activityd), [Spark-activiteit](#hdinsight-spark-activity). 

### <a name="linked-service"></a>Gekoppelde service 
De volgende tabel bevat beschrijvingen voor de eigenschappen die worden gebruikt in de Azure-JSON-definitie van een gekoppelde HDInsight-service op aanvraag.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op **HDInsightOnDemand**. |Ja |
| clusterSize |Het aantal worker/gegevensknooppunten in het cluster. Het HDInsight-cluster is gemaakt met 2 hoofdknooppunten samen met het aantal worker-knooppunten die u voor deze eigenschap opgeeft. De knooppunten zijn van de grootte Standard_D3 met 4 kernen, zodat een cluster met 4 worker-knooppunten 24 kernen wordt (4\*4 = 16 cores voor worker-knooppunten, plus 2\*4 = 8 cores voor hoofdknooppunten). Zie [maken Linux gebaseerde Hadoop-clusters in HDInsight](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) voor meer informatie over de Standard_D3-laag. |Ja |
| TimeToLive |De toegestane niet-actieve tijd voor het HDInsight-cluster op aanvraag. Hiermee geeft u op hoelang het HDInsight-cluster op aanvraag na voltooiing van een activiteit die wordt uitgevoerd als er geen andere actieve taken in het cluster actief blijft.<br/><br/>Bijvoorbeeld, als de uitvoering van een activiteit kan 6 minuten en timetolive is ingesteld op 5 minuten, blijft het cluster actief gedurende vijf minuten na het uitvoeren van de zes minuten van de verwerking van de activiteit. Als een andere activiteit die wordt uitgevoerd met het venster 6 minuten wordt uitgevoerd, wordt verwerkt door hetzelfde cluster.<br/><br/>Het maken van een on-demand HDInsight-cluster is een dure bewerking (kan even duren), dus gebruik deze instelling als die nodig zijn om prestaties te verbeteren van een data factory door opnieuw een on-demand HDInsight-cluster te gebruiken.<br/><br/>Als u timetolive waarde op 0 instelt, wordt het cluster wordt verwijderd zodra de uitvoering van activiteit verwerkt. Anderzijds, als u een hoge waarde hebt ingesteld, het cluster kan Blijf op de niet-actieve onnodig leidt tot hoge kosten. Het is daarom belangrijk dat u de juiste waarde op basis van uw behoeften.<br/><br/>Meerdere pijplijnen kunnen hetzelfde exemplaar van de HDInsight-cluster op aanvraag delen, als de waarde van de eigenschap timetolive op de juiste wijze is ingesteld |Ja |
| versie |De versie van het HDInsight-cluster. Zie voor meer informatie, [HDInsight-versies ondersteund in Azure Data Factory](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory). |Nee |
| linkedServiceName |Gekoppelde Azure Storage-service moet worden gebruikt door het cluster op aanvraag voor het opslaan en verwerken van gegevens. <p>U kunt geen op dit moment een on-demand HDInsight-cluster die gebruikmaakt van een Azure Data Lake Store als de opslag maken. Als u wilt voor het opslaan van de resulterende gegevens uit HDInsight verwerking in een Azure Data Lake Store, kunt u een Kopieeractiviteit gebruiken de gegevens van de Azure Blob-opslag kopiëren naar de Azure Data Lake Store.</p>  | Ja |
| additionallinkedservicenames |Hiermee geeft u extra opslagaccounts voor het HDInsight gekoppelde service zodat de Data Factory-service op uw rekening registreren kan. |Nee |
| besturingssysteemtype |Het type besturingssysteem. Toegestane waarden zijn: Windows (standaard)- en Linux |Nee |
| hcatalogLinkedServiceName |De naam van Azure SQL gekoppelde service die verwijzen naar de HCatalog-database. De on-demand HDInsight-cluster is gemaakt met behulp van de Azure SQL database als de metastore. |Nee |

### <a name="json-example"></a>Voorbeeld van JSON
De volgende JSON definieert een service op aanvraag gekoppeld HDInsight op basis van Linux. De Data Factory-service maakt automatisch een **op basis van Linux** HDInsight-cluster bij het verwerken van een gegevenssegment. 

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

Zie voor meer informatie, [gekoppelde services berekenen](data-factory-compute-linked-services.md) artikel. 

## <a name="existing-azure-hdinsight-cluster"></a>Bestaande Azure HDInsight-cluster
U kunt een Azure HDInsight gekoppelde service voor het registreren van uw eigen HDInsight-cluster met Data Factory maken. U kunt de volgende activiteiten voor gegevenstransformatie uitvoeren op deze gekoppelde service: [aangepaste .NET-activiteit](#net-custom-activity), [Hive-activiteit](#hdinsight-hive-activity), [Pig-activiteit](#hdinsight-pig-activity), [ MapReduce-activiteit](#hdinsight-mapreduce-activity), [Hadoop-streaming-activiteit](#hdinsight-streaming-activityd), [Spark-activiteit](#hdinsight-spark-activity). 

### <a name="linked-service"></a>Gekoppelde service
De volgende tabel bevat beschrijvingen voor de eigenschappen die worden gebruikt in de Azure-JSON-definitie van een Azure HDInsight gekoppelde service.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op **HDInsight**. |Ja |
| clusterUri |De URI van het HDInsight-cluster. |Ja |
| gebruikersnaam |Geef de naam van de gebruiker moet worden gebruikt om te verbinden met een bestaand HDInsight-cluster. |Ja |
| wachtwoord |Wachtwoord voor het gebruikersaccount opgeven. |Ja |
| linkedServiceName | De naam van de gekoppelde Azure Storage-service die naar de Azure blob-opslag die wordt gebruikt door het HDInsight-cluster verwijst. <p>Momenteel kunt opgeven u niet dat een Azure Data Lake Store gekoppelde service voor deze eigenschap. U kunt gegevens in de Azure Data Lake Store openen van Hive/Pig-scripts als het HDInsight-cluster toegang tot de Data Lake Store heeft. </p>  |Ja |

Zie voor versies van HDInsight-clusters ondersteund [ondersteunde versies van HDInsight](data-factory-compute-linked-services.md#supported-hdinsight-versions-in-azure-data-factory). 

#### <a name="json-example"></a>Voorbeeld van JSON

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
U kunt een gekoppelde Azure-Batch-service voor het registreren van een Batch-pool van virtuele machines (VM's) maken met een data factory. U kunt aangepaste .NET-activiteiten met behulp van Azure Batch of Azure HDInsight kunt uitvoeren. U kunt uitvoeren een [aangepaste .NET-activiteit](#net-custom-activity) op deze gekoppelde service. 

### <a name="linked-service"></a>Gekoppelde service
De volgende tabel bevat beschrijvingen voor de eigenschappen die worden gebruikt in de Azure-JSON-definitie van een gekoppelde Azure-Batch-service.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op **AzureBatch**. |Ja |
| accountName |De naam van de Azure Batch-account. |Ja |
| accessKey |De toegangssleutel voor de Azure Batch-account. |Ja |
| poolName |De naam van de pool van virtuele machines. |Ja |
| linkedServiceName |Naam van de Azure Storage gekoppelde service die is gekoppeld aan deze gekoppelde Azure-Batch-service. Deze gekoppelde service wordt gebruikt voor tijdelijke bestanden die zijn vereist voor het uitvoeren van de activiteit en het opslaan van de activiteitenlogboeken worden uitgevoerd. |Ja |


#### <a name="json-example"></a>Voorbeeld van JSON

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
U maakt een Azure Machine Learning gekoppelde service voor het registreren van een Machine Learning batch scoring-eindpunt met een data factory. Twee activiteiten voor gegevenstransformatie die kunnen worden uitgevoerd op deze gekoppelde service: [Machine Learning-Batchuitvoeringsactiviteit](#machine-learning-batch-execution-activity), [Machine Learning-activiteit resources bijwerken](#machine-learning-update-resource-activity). 

### <a name="linked-service"></a>Gekoppelde service
De volgende tabel bevat beschrijvingen voor de eigenschappen die worden gebruikt in de Azure-JSON-definitie van een service van Azure Machine Learning is gekoppeld.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| Type |De eigenschap type moet worden ingesteld op: **AzureML**. |Ja |
| mlEndpoint |Het batch scoring-URL. |Ja |
| ApiKey |Het gepubliceerde werkruimtemodel API. |Ja |

#### <a name="json-example"></a>Voorbeeld van JSON

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
U maakt een **Azure Data Lake Analytics** gekoppelde service om te koppelen van een Azure Data Lake Analytics compute-service aan een Azure data factory voordat u de [Data Lake Analytics U-SQL-activiteit](data-factory-usql-activity.md) in een pijplijn.

### <a name="linked-service"></a>Gekoppelde service

De volgende tabel bevat beschrijvingen voor de eigenschappen die worden gebruikt in de JSON-definitie van een Azure Data Lake Analytics gekoppelde service. 

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| Type |De eigenschap type moet worden ingesteld op: **AzureDataLakeAnalytics**. |Ja |
| accountName |Azure Data Lake Analytics-accountnaam. |Ja |
| dataLakeAnalyticsUri |Azure Data Lake Analytics-URI. |Nee |
| Autorisatie |Autorisatiecode wordt automatisch opgehaald nadat u hebt geklikt **autoriseren** knop in de Data Factory-Editor en de OAuth-aanmelding te voltooien. |Ja |
| subscriptionId |Azure-abonnement-id |Geen (indien niet opgegeven, abonnement van de data factory wordt gebruikt). |
| resourceGroupName |Naam van Azure-resourcegroep |Geen (indien niet opgegeven, de resourcegroep van de data factory wordt gebruikt). |
| sessie-id |sessie-id van de OAuth-autorisatie-sessie. Elke sessie-id is uniek en kan slechts eenmaal worden gebruikt. Wanneer u de Data Factory-Editor gebruikt, wordt deze ID wordt automatisch gegenereerd. |Ja |


#### <a name="json-example"></a>Voorbeeld van JSON
Het volgende voorbeeld bevat JSON-definitie voor een Azure Data Lake Analytics gekoppelde service.

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
U maakt een gekoppelde Azure SQL-service en deze gebruikt met de [opgeslagen-Procedureactiviteit](#stored-procedure-activity) om aan te roepen een opgeslagen procedure uit een Data Factory-pijplijn. 

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Azure SQL Database, stelt u de **type** van de gekoppelde service om **AzureSqlDatabase**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met de Azure SQL Database-exemplaar voor de connectionString-eigenschap. |Ja |

#### <a name="json-example"></a>Voorbeeld van JSON

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

Zie [Azure SQL-Connector](data-factory-azure-sql-connector.md#linked-service-properties) artikel voor meer informatie over deze gekoppelde service.

## <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse
U een Azure SQL Data Warehouse gekoppelde service maken en gebruiken met de [opgeslagen-Procedureactiviteit](data-factory-stored-proc-activity.md) om aan te roepen een opgeslagen procedure uit een Data Factory-pijplijn. 

### <a name="linked-service"></a>Gekoppelde service
Gekoppelde service voor het definiëren van een Azure SQL Data Warehouse, stelt u de **type** van de gekoppelde service om **AzureSqlDW**, en geeft u de volgende eigenschappen in de **typeProperties** sectie:  

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| connectionString |Geef informatie op die nodig zijn voor het verbinding maken met de Azure SQL Data Warehouse-exemplaar voor de connectionString-eigenschap. |Ja |

#### <a name="json-example"></a>Voorbeeld van JSON

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

Zie voor meer informatie, [Azure SQL Data Warehouse-connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) artikel. 

## <a name="sql-server"></a>SQL Server 
U maakt een gekoppelde SQL Server-service en deze gebruikt met de [opgeslagen-Procedureactiviteit](data-factory-stored-proc-activity.md) om aan te roepen een opgeslagen procedure uit een Data Factory-pijplijn. 

### <a name="linked-service"></a>Gekoppelde service
U maakt een gekoppelde service van het type **OnPremisesSqlServer** een on-premises SQL Server-database koppelen aan een data factory. De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor on-premises gekoppelde SQL Server-service.

De volgende tabel bevat een beschrijving op voor JSON-elementen die specifiek zijn voor de gekoppelde SQL Server-service.

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| type |De eigenschap type moet worden ingesteld op: **OnPremisesSqlServer**. |Ja |
| connectionString |Geef connectionString informatie die nodig zijn voor het verbinding maken met de on-premises SQL Server-database met behulp van SQL-verificatie of Windows-verificatie. |Ja |
| gatewayName |De naam van de gateway die de Data Factory-service gebruiken moet voor verbinding met de on-premises SQL Server-database. |Ja |
| gebruikersnaam |Geef de gebruikersnaam op als u van Windows-verificatie gebruikmaakt. Voorbeeld: **domainname\\gebruikersnaam**. |Nee |
| wachtwoord |Wachtwoord voor het gebruikersaccount dat u hebt opgegeven voor de gebruikersnaam opgeven. |Nee |

U kunt versleutelen referenties met behulp van de **New-AzureRmDataFactoryEncryptValue** cmdlet te gebruiken in de verbindingsreeks, zoals wordt weergegeven in het volgende voorbeeld (**EncryptedCredential** eigenschap):  

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

Als gebruikersnaam en wachtwoord zijn opgegeven, worden deze door gateway te imiteren van het opgegeven gebruikersaccount verbinding maken met de on-premises SQL Server-database gebruikt. Anders gateway maakt verbinding met de SQL-Server rechtstreeks met de beveiligingscontext van de Gateway (de opstartaccount).

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

Zie voor meer informatie, [SQL Server-connector](data-factory-sqlserver-connector.md#linked-service-properties) artikel.

## <a name="data-transformation-activities"></a>ACTIVITEITEN VOOR GEGEVENSTRANSFORMATIE

Activiteit | Description
-------- | -----------
[HDInsight Hive-activiteit](#hdinsight-hive-activity) | De HDInsight Hive-activiteit in een Data Factory-pijplijn voert een Hive-query's op uw eigen of Windows/Linux gebaseerd HDInsight-cluster op aanvraag. 
[HDInsight Pig-activiteit](#hdinsight-pig-activity) | De HDInsight Pig-activiteit in een Data Factory-pijplijn voert Pig-query's op uw eigen of Windows/Linux gebaseerd HDInsight-cluster op aanvraag.
[HDInsight MapReduce-activiteit](#hdinsight-mapreduce-activity) | De HDInsight MapReduce-activiteit in een Data Factory-pijplijn voert MapReduce-programma's op uw eigen of Windows/Linux gebaseerd HDInsight-cluster op aanvraag.
[HDInsight Streaming-activiteit](#hdinsight-streaming-activity) | De HDInsight Streaming-activiteit in een Data Factory-pijplijn voert Hadoop-Streaming-programma's op uw eigen of Windows/Linux gebaseerd HDInsight-cluster op aanvraag.
[HDInsight Spark-activiteit](#hdinsight-spark-activity) | De HDInsight Spark-activiteit in een Data Factory-pijplijn Spark-programma's op uw eigen HDInsight-cluster wordt uitgevoerd. 
[Machine Learning-batchuitvoeringsactiviteit](#machine-learning-batch-execution-activity) | Azure Data Factory kunt u eenvoudig pijplijnen die gebruikmaken van een gepubliceerde Azure Machine Learning-webservice voor voorspellende analyses te maken. Met behulp van de Batch Execution-activiteit in een Azure Data Factory-pijplijn, kunt u een Machine Learning-webservice om voorspellingen te maken van de gegevens in batch aanroepen. 
[Machine Learning-activiteit resources bijwerken](#machine-learning-update-resource-activity) | De voorspellende modellen in de Machine Learning experimenten scoren moeten opnieuw worden getraind met behulp van nieuwe invoergegevenssets na verloop van tijd. Als u klaar bent met het opnieuw trainen, die u wilt de scoringwebservice bijwerken met de retrained Machine Learning-model. Het bijwerken van de webservice met het zojuist getrainde model kunt u de activiteit resources bijwerken.
[Opgeslagen procedureactiviteit](#stored-procedure-activity) | U kunt de opgeslagen Procedure-activiteit in een Data Factory-pijplijn gebruiken om aan te roepen een opgeslagen procedure in een van de volgende gegevensarchieven: Azure SQL-Database, Azure SQL Data Warehouse, SQL Server-Database in uw onderneming of een Azure-VM. 
[Data Lake Analytics U-SQL-activiteit](#data-lake-analytics-u-sql-activity) | Data Lake Analytics U-SQL-activiteit wordt een U-SQL-script uitgevoerd op een Azure Data Lake Analytics-cluster.  
[.NET aangepaste activiteit](#net-custom-activity) | Als u nodig hebt voor het transformeren van gegevens op een manier die niet wordt ondersteund door Data Factory, kunt u een aangepaste activiteit maken met uw eigen logica gegevensverwerking en het gebruik van de activiteit in de pijplijn. U kunt de aangepaste .NET-activiteit om uit te voeren met behulp van een Azure Batch-service of een Azure HDInsight-cluster configureren. 

     
## <a name="hdinsight-hive-activity"></a>HDInsight Hive-activiteit
U kunt de volgende eigenschappen opgeven in een Hive-activiteit JSON-definitie. De eigenschap type voor de activiteit moet zijn: **HDInsightHive**. U moet eerst een gekoppelde HDInsight-service maken en geef de naam van het als een waarde voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** sectie als u het type van de activiteit ingesteld op HDInsightHive:

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| script |Geef de inline Hive-script |Nee |
| pad naar script |Het Hive-script Store in een Azure blob-opslag en het pad naar het bestand opgeven. Gebruik de eigenschap 'script' of 'scriptPath'. Beide kunnen niet samen worden gebruikt. De bestandsnaam is hoofdlettergevoelig. |Nee |
| Hiermee worden gedefinieerd |Geef parameters op als sleutel/waarde-paren voor verwijzende binnen het Hive-script met behulp van 'hiveconf' |Nee |

Deze eigenschappen zijn specifiek voor de Hive-activiteit. Andere eigenschappen (buiten de sectie typeProperties) worden ondersteund voor alle activiteiten.   

### <a name="json-example"></a>Voorbeeld van JSON
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

Zie voor meer informatie, [Hive-activiteit](data-factory-hive-activity.md) artikel. 

## <a name="hdinsight-pig-activity"></a>HDInsight Pig-activiteit
U kunt de volgende eigenschappen opgeven in een Pig-activiteit JSON-definitie. De eigenschap type voor de activiteit moet zijn: **HDInsightPig**. U moet eerst een gekoppelde HDInsight-service maken en geef de naam van het als een waarde voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** sectie als u het type van de activiteit ingesteld op HDInsightPig: 

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| script |Geef de inline Pig-script |Nee |
| pad naar script |Store de Pig-script in een Azure blob-opslag en het pad naar het bestand opgeven. Gebruik de eigenschap 'script' of 'scriptPath'. Beide kunnen niet samen worden gebruikt. De bestandsnaam is hoofdlettergevoelig. |Nee |
| Hiermee worden gedefinieerd |Geef parameters op als sleutel/waarde-paren voor verwijzende binnen de Pig-script |Nee |

Deze eigenschappen zijn specifiek voor de Pig-activiteit. Andere eigenschappen (buiten de sectie typeProperties) worden ondersteund voor alle activiteiten.   

### <a name="json-example"></a>Voorbeeld van JSON

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

Zie voor meer informatie, [Pig-activiteit](#data-factory-pig-activity.md) artikel. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce-activiteit
U kunt de volgende eigenschappen opgeven in een MapReduce-activiteit JSON-definitie. De eigenschap type voor de activiteit moet zijn: **HDInsightMapReduce**. U moet eerst een gekoppelde HDInsight-service maken en geef de naam van het als een waarde voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** sectie als u het type van de activiteit ingesteld op HDInsightMapReduce: 

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| jarLinkedService | De naam van de gekoppelde service voor de Azure-opslag die het JAR-bestand bevat. | Ja |
| jarFilePath | Pad naar het JAR-bestand in Azure Storage. | Ja | 
| Klassenaam | De naam van de main-klasse in het JAR-bestand. | Ja | 
| argumenten | Een lijst met door komma's gescheiden argumenten voor de MapReduce-programma. Tijdens runtime, ziet u een paar extra argumenten (bijvoorbeeld: mapreduce.job.tags) van de MapReduce-framework. Overweeg het gebruik van optie en de waarde als argumenten, zoals wordt weergegeven in het volgende voorbeeld om te onderscheiden van de argumenten met de argumenten MapReduce, (- s,--input,--output enz., zijn opties onmiddellijk wordt gevolgd door hun waarden) | Nee | 

### <a name="json-example"></a>Voorbeeld van JSON

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

Zie voor meer informatie, [MapReduce-activiteit](data-factory-map-reduce.md) artikel. 

## <a name="hdinsight-streaming-activity"></a>HDInsight-streamingactiviteit
U kunt de volgende eigenschappen opgeven in de definitie van een Hadoop Streaming Activity in JSON. De eigenschap type voor de activiteit moet zijn: **HDInsightStreaming**. U moet eerst een gekoppelde HDInsight-service maken en geef de naam van het als een waarde voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** sectie als u het type van de activiteit ingesteld op HDInsightStreaming: 

| Eigenschap | Description | 
| --- | --- |
| toewijzen | De naam van het uitvoerbare bestand toewijzen. In het voorbeeld is cat.exe het toewijzen van de uitvoerbare.| 
| reducer | De naam van het uitvoerbare bestand reducer. In het voorbeeld is wc.exe de uitvoerbare reducer. | 
| invoer | Invoerbestand (met inbegrip van locatie) voor het toewijzen van de. In het voorbeeld: "wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt": adfsample is de blob-container, gegevens-voorbeeld/Gutenberg is de map en davinci.txt wordt de blob. |
| output | Bestand voor uitvoer (met inbegrip van locatie) voor de reducer. De uitvoer van de Hadoop-Streaming-taak is geschreven naar de locatie die is opgegeven voor deze eigenschap. |
| filePaths | Paden voor de toewijzing en reducer uitvoerbare bestanden. In het voorbeeld: "adfsample/example/apps/wc.exe" adfsample wordt de blob-container, voorbeeld-apps is de map en wc.exe is het uitvoerbare bestand. | 
| fileLinkedService | Gekoppelde Azure Storage-service die de Azure-opslag met de bestanden die zijn opgegeven in de sectie filePaths vertegenwoordigt. | 
| argumenten | Een lijst met door komma's gescheiden argumenten voor de MapReduce-programma. Tijdens runtime, ziet u een paar extra argumenten (bijvoorbeeld: mapreduce.job.tags) van de MapReduce-framework. Overweeg het gebruik van optie en de waarde als argumenten, zoals wordt weergegeven in het volgende voorbeeld om te onderscheiden van de argumenten met de argumenten MapReduce, (- s,--input,--output enz., zijn opties onmiddellijk wordt gevolgd door hun waarden) | 
| getDebugInfo | Een optioneel element. Wanneer deze is ingesteld op mislukt, worden de logboeken alleen bij fout gedownload. Wanneer deze is ingesteld op Alles, worden logboeken worden altijd gedownload, ongeacht de uitvoeringsstatus van de. | 

> [!NOTE]
> U moet een uitvoergegevensset opgeven voor de Hadoop-Streamingactiviteit voor de **levert** eigenschap. Deze gegevensset kan alleen een dummy gegevensset die is vereist voor het station van de pijplijn planning (elk uur, dagelijks, enz.) zijn. Als de activiteit geen invoer, kunt u overslaan op te geven voor de activiteit voor een invoergegevensset wordt tijdens de **invoer** eigenschap.  

## <a name="json-example"></a>Voorbeeld van JSON

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

Zie voor meer informatie, [Hadoop-Streamingactiviteit](data-factory-hadoop-streaming-activity.md) artikel. 

## <a name="hdinsight-spark-activity"></a>HDInsight Spark-activiteit
U kunt de volgende eigenschappen opgeven in een Spark-activiteit JSON-definitie. De eigenschap type voor de activiteit moet zijn: **HDInsightSpark**. U moet eerst een gekoppelde HDInsight-service maken en geef de naam van het als een waarde voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** sectie als u het type van de activiteit ingesteld op HDInsightSpark: 

| Eigenschap | Description | Vereist |
| -------- | ----------- | -------- |
| rootPath | Het Azure Blob-container en de map waarin het Spark-bestand. De bestandsnaam is hoofdlettergevoelig. | Ja |
| entryFilePath | Relatief pad naar de hoofdmap van de Spark-code of pakket. | Ja |
| Klassenaam | Java/Spark-hoofdklasse van de toepassing | Nee | 
| argumenten | Een lijst met opdrachtregelargumenten op het Spark-programma. | Nee | 
| proxyUser | De account van de gebruiker te imiteren voor het uitvoeren van het Spark-programma | Nee | 
| sparkConfig | Spark-configuratie-eigenschappen. | Nee | 
| getDebugInfo | Hiermee geeft u aan bij de Spark-logboekbestanden worden gekopieerd naar de Azure-opslag die wordt gebruikt door HDInsight-cluster (of) opgegeven door sparkJobLinkedService. Toegestane waarden: Geen altijd of fout. Standaardwaarde: Geen. | Nee | 
| sparkJobLinkedService | Azure Storage gekoppelde service waarin de Spark taakbestand, afhankelijkheden en Logboeken.  Als u een waarde voor deze eigenschap niet opgeeft, wordt de opslag die is gekoppeld aan het HDInsight-cluster gebruikt. | Nee |

### <a name="json-example"></a>Voorbeeld van JSON

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
- De **rootPath** is ingesteld op **adfspark\\pyFiles** waarbij adfspark is de Azure Blob-container en pyFiles fijn map in die container. In dit voorbeeld is de Azure Blob-opslag die is gekoppeld aan het Spark-cluster. U kunt het bestand uploaden naar een ander Azure-Opslagaccount. Als u dit doet, maakt u een gekoppelde Azure Storage-service als u wilt dat opslagaccount koppelen aan de data factory. Geef de naam van de gekoppelde service vervolgens als een waarde voor de **sparkJobLinkedService** eigenschap. Zie [Spark-activiteit eigenschappen](#spark-activity-properties) voor meer informatie over deze eigenschap en andere eigenschappen die worden ondersteund door de Spark-activiteit.
- De **entryFilePath** is ingesteld op de **test.py**, dit is het python-bestand. 
- De **getDebugInfo** eigenschap is ingesteld op **altijd**, wat betekent dat de logboekbestanden altijd zijn gegenereerd (slagen of mislukken).  

    > [!IMPORTANT]
    > U wordt aangeraden dat u niet deze eigenschap op altijd in een productieomgeving stelt, tenzij u een probleem wilt oplossen. 
- De **levert** sectie heeft één uitvoergegevensset. U moet een uitvoergegevensset, zelfs als het spark-programma geen uitvoer wordt geproduceerd. De uitvoergegevensset stuurt het schema voor de pijplijn (elk uur, dagelijks, enz.).

Zie voor meer informatie over de activiteit [Spark-activiteit](data-factory-spark.md) artikel.  

## <a name="machine-learning-batch-execution-activity"></a>Machine Learning-batchuitvoeringsactiviteit
U kunt de volgende eigenschappen opgeven in een Azure ML Batch uitvoering Activity in JSON-definitie. De eigenschap type voor de activiteit moet zijn: **AzureMLBatchExecution**. U moet maken van een Azure Machine Learning-gekoppelde service eerst en geef de naam van het als een waarde voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** sectie als u het type van de activiteit ingesteld op AzureMLBatchExecution:

Eigenschap | Description | Vereist 
-------- | ----------- | --------
webServiceInput | De gegevensset worden doorgegeven als invoer voor de Azure ML-webservice. Deze gegevensset moet ook worden opgenomen in de invoer voor de activiteit. |WebServiceInput of webServiceInputs gebruiken. | 
webServiceInputs | Geef gegevenssets moeten worden doorgegeven als invoer voor de Azure ML-webservice. Als de webservice meerdere invoergegevens heeft, gebruikt u de eigenschap webServiceInputs in plaats van de webServiceInput-eigenschap. Gegevenssets waarnaar wordt verwezen door de **webServiceInputs** moet ook zijn opgenomen in de activiteit **invoer**. | WebServiceInput of webServiceInputs gebruiken. | 
webServiceOutputs | De gegevenssets die zijn toegewezen als uitvoer voor de Azure ML-webservice. De webservice retourneert uitvoergegevens in deze gegevensset. | Ja | 
globalParameters | Geef waarden voor parameters van de webservice in deze sectie. | Nee | 

### <a name="json-example"></a>Voorbeeld van JSON
In dit voorbeeld wordt de activiteit heeft de gegevensset **MLSqlInput** als invoer en **MLSqlOutput** als de uitvoer. De **MLSqlInput** wordt doorgegeven als invoer voor de webservice met behulp van de **webServiceInput** JSON-eigenschap. De **MLSqlOutput** wordt doorgegeven als een uitvoer aan de Web-service met behulp van de **webServiceOutputs** JSON-eigenschap. 

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

In het JSON-voorbeeld gebruikt de geïmplementeerde Azure Machine Learning-webservice een lezer en een schrijfmodule voor lezen/schrijven van gegevens van/naar een Azure SQL Database. Deze webservice wordt aangegeven dat de volgende vier parameters:  Database-servernaam, databasenaam, gebruikersnaam voor account Server en Server het wachtwoord voor gebruikersaccount.

> [!NOTE]
> Alleen de invoer en uitvoer van de activiteit AzureMLBatchExecution kunnen als parameters worden doorgegeven aan de webservice. In de bovenstaande JSON-codefragment is MLSqlInput bijvoorbeeld invoer voor de activiteit AzureMLBatchExecution, die wordt doorgegeven als invoer voor de webservice via webServiceInput-parameter.

## <a name="machine-learning-update-resource-activity"></a>Machine Learning-activiteit resources bijwerken
U kunt de volgende eigenschappen opgeven in een Azure ML bijwerken Resource Activity in JSON-definitie. De eigenschap type voor de activiteit moet zijn: **AzureMLUpdateResource**. U moet maken van een Azure Machine Learning-gekoppelde service eerst en geef de naam van het als een waarde voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** sectie als u het type van de activiteit ingesteld op AzureMLUpdateResource:

Eigenschap | Description | Vereist 
-------- | ----------- | --------
trainedModelName | De naam van het retrained model. | Ja |  
trainedModelDatasetName | De gegevensset die verwijst naar het iLearner-bestand dat is geretourneerd door de retraining-bewerking. | Ja | 

### <a name="json-example"></a>Voorbeeld van JSON
De pijplijn heeft twee activiteiten: **AzureMLBatchExecution** en **AzureMLUpdateResource**. De Azure ML-Batch Execution-activiteit neemt de trainingsgegevens als invoer en produceert een iLearner-bestand als uitvoer. De activiteit roept de training webservice (trainingsexperiment weergegeven als een webservice) met de invoer trainingsgegevens en ontvangt het ilearner-bestand van de webservice. De placeholderBlob is alleen een dummy uitvoergegevensset die door de Azure Data Factory-service is vereist voor het uitvoeren van de pijplijn.


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
U kunt de volgende eigenschappen opgeven in een U-SQL-activiteit JSON-definitie. De eigenschap type voor de activiteit moet zijn: **DataLakeAnalyticsU SQL**. U moet een Azure Data Lake Analytics gekoppelde service maken en geef de naam van het als een waarde voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** sectie als u het type van de activiteit ingesteld op DataLakeAnalyticsU-SQL: 

| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| scriptPath |Pad naar map met de U-SQL-script. Naam van het bestand is hoofdlettergevoelig. |Nee (als u een script gebruiken) |
| scriptLinkedService |Gekoppelde service die is gekoppeld aan de opslag met het script aan de data factory |Nee (als u een script gebruiken) |
| script |Geef inline script in plaats van scriptPath en scriptLinkedService op te geven. Bijvoorbeeld: "script": 'CREATE DATABASE test'. |Nee (als u gebruik scriptPath en scriptLinkedService) |
| degreeOfParallelism |Het maximale aantal knooppunten dat tegelijk wordt gebruikt voor het uitvoeren van de taak. |Nee |
| priority |Hiermee bepaalt u welke taken uit in de wachtrij moeten worden geselecteerd moeten eerst worden uitgevoerd. Des te lager het nummer, hoe hoger de prioriteit. |Nee |
| parameters |Parameters voor de U-SQL-script |Nee |

### <a name="json-example"></a>Voorbeeld van JSON

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

Zie voor meer informatie, [Data Lake Analytics U-SQL-activiteit](data-factory-usql-activity.md). 

## <a name="stored-procedure-activity"></a>Opgeslagen procedureactiviteit
U kunt de volgende eigenschappen opgeven in een opgeslagen Procedure Activity in JSON-definitie. De eigenschap type voor de activiteit moet zijn: **SqlServerStoredProcedure**. U moet een van de volgende gekoppelde services maken en de naam van de gekoppelde service opgeven als een waarde voor de **linkedServiceName** eigenschap:

- SQL Server 
- Azure SQL Database
- Azure SQL Data Warehouse

De volgende eigenschappen worden ondersteund in de **typeProperties** sectie als u het type van de activiteit ingesteld op SqlServerStoredProcedure:

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| storedProcedureName |Geef de naam van de opgeslagen procedure in de Azure SQL-database of Azure SQL Data Warehouse die wordt vertegenwoordigd door de gekoppelde service die gebruikmaakt van de uitvoertabel. |Ja |
| storedProcedureParameters |Geef waarden op voor de opgeslagen-procedureparameters. Als u moet null zijn voor een parameter door te geven, gebruikt u de syntaxis: "param1": null (alleen kleine letters). Zie het volgende voorbeeld voor meer informatie over het gebruik van deze eigenschap. |Nee |

Als u een invoergegevensset opgeeft, moet deze zijn beschikbaar (in de status 'Gereed') voor de opgeslagen procedure-activiteit om uit te voeren. De invoergegevensset kan niet worden gebruikt in de opgeslagen procedure als een parameter. Het wordt alleen gebruikt om te controleren van de afhankelijkheid voordat u begint met de opgeslagen procedure-activiteit. U moet een uitvoergegevensset voor een opgeslagen procedure-activiteit opgeven. 

Uitvoergegevensset Hiermee geeft u de **planning** voor de opgeslagen procedure-activiteit (elk uur, wekelijks, maandelijks, enzovoort). De uitvoergegevensset moet gebruiken een **gekoppelde service** die verwijst naar een Azure SQL Database of een Azure SQL Data Warehouse of een SQL Server-Database waarin u wilt dat de opgeslagen procedure om uit te voeren. De uitvoergegevensset kan fungeren als een manier om door te geven het resultaat van de opgeslagen procedure voor verdere verwerking door een andere activiteit ([Koppelingsactiviteiten](data-factory-scheduling-and-execution.md##multiple-activities-in-a-pipeline)) in de pijplijn. Echter, Data Factory wordt niet automatisch schrijven de uitvoer van een opgeslagen procedure aan deze gegevensset. Het is de opgeslagen procedure die naar een SQL-tabel die de uitvoergegevensset naar verwijst schrijft. In sommige gevallen kan de uitvoergegevensset mag een **dummy gegevensset**, die wordt gebruikt om alleen op te geven van de planning voor het uitvoeren van de activiteit opgeslagen procedure.  

### <a name="json-example"></a>Voorbeeld van JSON

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

Zie voor meer informatie, [opgeslagen-Procedureactiviteit](data-factory-stored-proc-activity.md) artikel. 

## <a name="net-custom-activity"></a>Aangepaste .NET-activiteit
U kunt de volgende eigenschappen opgeven in een aangepaste .NET-activiteit JSON-definitie. De eigenschap type voor de activiteit moet zijn: **DotNetActivity**. U moet een Azure HDInsight gekoppelde service maken of een gekoppeld Azure-Batch-service en de naam van de gekoppelde service opgeven als een waarde voor de **linkedServiceName** eigenschap. De volgende eigenschappen worden ondersteund in de **typeProperties** sectie als u het type van de activiteit ingesteld op DotNetActivity:
 
| Eigenschap | Description | Vereist |
|:--- |:--- |:--- |
| Assembly-naam | De naam van de assembly. In het voorbeeld is: **MyDotnetActivity.dll**. | Ja |
| EntryPoint |De naam van de klasse die de IDotNetActivity-interface implementeert. In het voorbeeld is: **MyDotNetActivityNS.MyDotNetActivity** waarbij MyDotNetActivityNS is de naamruimte en MyDotNetActivity is de klasse.  | Ja | 
| PackageLinkedService | Naam van de gekoppelde Azure Storage-service die verwijst naar de blob-opslag met de aangepaste activiteit zip-bestand. In het voorbeeld is: **AzureStorageLinkedService**.| Ja |
| PackageFile | De naam van het zip-bestand. In het voorbeeld is: **customactivitycontainer/MyDotNetActivity.zip**. | Ja |
| ExtendedProperties | Uitgebreide eigenschappen die u kunt definiëren en deze doorgeven aan de .NET-code. In dit voorbeeld wordt de **SliceStart** variabele is ingesteld op een waarde op basis van de systeemvariabele slicestart-waarde. | Nee | 

### <a name="json-example"></a>Voorbeeld van JSON

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

Zie voor gedetailleerde informatie [aangepaste activiteiten gebruiken in Data Factory](data-factory-use-custom-activities.md) artikel. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende zelfstudies: 

- [Zelfstudie: een pijplijn maken met een kopieeractiviteit](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Zelfstudie: een pijplijn maken met een hive-activiteit](data-factory-build-your-first-pipeline-using-editor.md)
