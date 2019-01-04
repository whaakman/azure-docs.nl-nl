---
title: Pijplijnen maken/schema, de activiteiten in Data Factory | Microsoft Docs
description: Leer hoe u een pijplijn maken in Azure Data Factory te verplaatsen en transformeren van gegevens. Maak een gegevensgestuurde werkstroom voor het produceren van gereed om informatie te gebruiken.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 13b137c7-1033-406f-aea7-b66f25b313c0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: e1fe5af1769a0a1a83a3ce849a7eb1874369ce9a
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54023363"
---
# <a name="pipelines-and-activities-in-azure-data-factory"></a>Pijplijnen en activiteiten in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1:](data-factory-create-pipelines.md)
> * [Versie 2 (huidige versie)](../concepts-pipelines-activities.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [pijplijnen in V2](../concepts-pipelines-activities.md).

Met behulp van dit artikel krijgt u inzicht in de pijplijnen en activiteiten in Azure Data Factory en in de wijze waarop u deze kunt gebruiken om end-to-end gegevensgestuurde werkstromen te maken voor uw gegevensverplaatsingen en scenario’s voor gegevensverwerking.  

> [!NOTE]
> In dit artikel wordt ervan uitgegaan dat u hebt doorgenomen [Inleiding tot Azure Data Factory](data-factory-introduction.md). Als u geen komen-op-ervaring met het maken van data factory's, doorlopen [data transformation zelfstudie](data-factory-build-your-first-pipeline.md) en/of [data movement zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kunt u beter begrijpen van dit artikel.  

## <a name="overview"></a>Overzicht
Een gegevensfactory kan één of meer pijplijnen hebben. Een pijplijn is een logische groep activiteiten die samen een taak uitvoeren. Met activiteiten in een pijplijn definieert u welk acties moeten worden uitgevoerd voor uw gegevens. U kunt bijvoorbeeld een kopieeractiviteit gebruiken om gegevens van een on-premises SQL Server naar Azure Blob Storage te kopiëren. Vervolgens kunt u een Hive-activiteit gebruiken waarmee een Hive-script op een Azure HDInsight-cluster wordt uitgevoerd om gegevens uit Blob Storage te verwerken/transformeren en uitvoergegevens te produceren. Gebruik tot slot een tweede kopieeractiviteit voor het kopiëren van de uitvoergegevens naar een Azure SQL Data Warehouse waarop rapportageoplossingen voor business intelligence (BI) zijn gebouwd. 

Een activiteit kan nul of meer [invoergegevenssets](data-factory-create-datasets.md) hebben en een of meer [uitvoergegevenssets](data-factory-create-datasets.md) produceren. Het volgende diagram toont de relatie tussen de pijplijn, activiteit en gegevensset in Data Factory: 

![Relatie tussen pijplijn, activiteit en gegevensset](media/data-factory-create-pipelines/relationship-pipeline-activity-dataset.png)

Een pijplijn kunt u activiteiten afzonderlijk als een set in plaats van afzonderlijke beheren. U kunt bijvoorbeeld implementeren, plannen, onderbreken en hervatten van een pijplijn, in plaats van omgaan met activiteiten in de pijplijn onafhankelijk van elkaar.

Data Factory ondersteunt twee soorten activiteiten: activiteiten voor gegevensverplaatsing en activiteiten voor gegevenstransformatie. Elke activiteit kan nul of meer hebben [gegevenssets](data-factory-create-datasets.md) en een of meer uitvoergegevenssets produceren.

Een invoergegevensset vertegenwoordigt de invoer voor een activiteit in de pijplijn en een uitvoergegevensset vertegenwoordigt de uitvoer voor de activiteit. Met gegevenssets worden gegevens binnen andere gegevensarchieven geïdentificeerd, waaronder tabellen, bestanden, mappen en documenten. Nadat u een gegevensset hebt gemaakt, kunt u deze gebruiken voor activiteiten in een pijplijn. Een gegevensset kan bijvoorbeeld een gegevensset voor invoer/uitvoer van een kopieeractiviteit of een HDInsightHive-activiteit zijn. Zie het artikel [Gegevenssets in Azure Data Factory](data-factory-create-datasets.md) voor meer informatie over gegevenssets.

### <a name="data-movement-activities"></a>Activiteiten voor gegevensverplaatsing
De kopieeractiviteit in Data Factory kopieert gegevens van een brongegevensarchief naar een sinkgegevensarchief. Data Factory ondersteunt de volgende gegevensarchieven. Gegevens vanuit elke willekeurige bron kunnen naar een sink worden geschreven. Klik op een gegevensarchief voor informatie over het kopiëren van gegevens naar en van dat archief.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Gegevensarchieven met een * kunnen zich on-premises of op Azure IaaS bevinden. Hiervoor moet u [Data Management Gateway](data-factory-data-management-gateway.md) installeren op een on-premises/Azure IaaS-computer.

Zie het artikel [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) voor meer informatie.

### <a name="data-transformation-activities"></a>Activiteiten voor gegevenstransformatie
[!INCLUDE [data-factory-transformation-activities](../../../includes/data-factory-transformation-activities.md)]

Zie het artikel [Activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) voor meer informatie.

### <a name="custom-net-activities"></a>Aangepaste .NET-activiteiten 
Als u verplaatsen naar/van een gegevensarchief wilt dat de Kopieeractiviteit niet ondersteunen, of gegevens transformeren met behulp van uw eigen logica, maakt u een **aangepaste .NET-activiteit**. Zie [Aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn](data-factory-use-custom-activities.md) voor meer informatie over het maken en gebruiken van een aangepaste activiteit.

## <a name="schedule-pipelines"></a>Pijplijnen plannen
Een pijplijn is actief waren alleen tussen de **start** tijd en **end** tijd. Het is niet uitgevoerd voor de begintijd liggen of na de eindtijd. Als de pijplijn is onderbroken, wordt deze niet wordt uitgevoerd, ongeacht de begin- en -tijd. Voor een pijplijn om uit te voeren, moet deze niet worden onderbroken. Zie [planning en uitvoering](data-factory-scheduling-and-execution.md) om te begrijpen hoe plannen en uitvoeren in Azure Data Factory werkt.

## <a name="pipeline-json"></a>Pijplijn in JSON-indeling
We gaan dieper in op hoe een pijplijn wordt gedefinieerd in JSON-indeling. De algemene structuur voor een pijplijn ziet er als volgt uit:

```json
{
    "name": "PipelineName",
    "properties": 
    {
        "description" : "pipeline description",
        "activities":
        [

        ],
        "start": "<start date-time>",
        "end": "<end date-time>",
        "isPaused": true/false,
        "pipelineMode": "scheduled/onetime",
        "expirationTime": "15.00:00:00",
        "datasets": 
        [
        ]
    }
}
```

| Label | Description | Vereist |
| --- | --- | --- |
| naam |Naam van de pijplijn. Geef een naam op die staat voor de actie die de pijplijn uitvoert. <br/><ul><li>Maximum aantal tekens: 260</li><li>Moet beginnen met een letter, cijfer of een onderstrepingsteken (\_)</li><li>De volgende tekens zijn niet toegestaan: '. ', '+ ','?', '/', '< ',' >', ' * ', '%', '&', ': ','\\"</li></ul> |Ja |
| description | Voer een beschrijving in van het doel waarvoor de pijplijn wordt gebruikt. |Ja |
| activities | De sectie **activities** kan één of meer activiteiten bevatten die zijn gedefinieerd binnen de activiteit. Zie de volgende sectie voor meer informatie over de JSON-element activities. | Ja |  
| start | Startdatum en-tijd voor de pijplijn. Moet zich in [ISO-indeling](http://en.wikipedia.org/wiki/ISO_8601). Bijvoorbeeld: `2016-10-14T16:32:41Z`. <br/><br/>Het is mogelijk om op te geven een plaatselijke tijd, bijvoorbeeld een geschatte tijd. Hier volgt een voorbeeld: `2016-02-27T06:00:00-05:00`", die is 6 uur EST<br/><br/>De begin- en -eigenschappen opgeven samen actieve periode voor de pijplijn. Uitvoersegmenten worden alleen gemaakt met deze actieve periode. |Nee<br/><br/>Als u een waarde voor de end-eigenschap opgeeft, moet u de waarde voor de eigenschap QueryDefinition opgeven.<br/><br/>De begin- en eindtijden kunnen beide niet leeg zijn om een pijplijn te maken. U moet beide waarden om in te stellen van een actieve periode voor de pijplijn om uit te voeren. Als u geen begin- en eindtijden opgeeft wanneer u een pijplijn maakt, kunt u instellen met behulp van de cmdlet Set-AzureRmDataFactoryPipelineActivePeriod later opnieuw. |
| einde | Einddatum en-tijd voor de pijplijn. Als u opgeeft moet zich in de ISO-indeling. Bijvoorbeeld: `2016-10-14T17:32:41Z` <br/><br/>Het is mogelijk om op te geven een plaatselijke tijd, bijvoorbeeld een geschatte tijd. Hier volgt een voorbeeld: `2016-02-27T06:00:00-05:00`, die is 6 AM EST.<br/><br/>Als u wilt de pijplijn voor onbepaalde tijd worden uitgevoerd, geef 9999-09-09 als de waarde voor de end-eigenschap. <br/><br/> Een pijplijn is actief waren alleen tussen de begin- en eindtijd. Het is niet uitgevoerd voor de begintijd liggen of na de eindtijd. Als de pijplijn is onderbroken, wordt deze niet wordt uitgevoerd, ongeacht de begin- en -tijd. Voor een pijplijn om uit te voeren, moet deze niet worden onderbroken. Zie [planning en uitvoering](data-factory-scheduling-and-execution.md) om te begrijpen hoe plannen en uitvoeren in Azure Data Factory werkt. |Nee <br/><br/>Als u een waarde voor de eigenschap QueryDefinition opgeeft, moet u de waarde voor de end-eigenschap opgeven.<br/><br/>Zie de opmerkingen voor de **start** eigenschap. |
| isPaused | Indien ingesteld op true, de pijplijn wordt niet uitgevoerd. Het is in de status onderbroken ingeschakeld. Standaardwaarde = false. U kunt deze eigenschap gebruiken of een pijplijn uit te schakelen. |Nee |
| pipelineMode | De methode voor de planning wordt uitgevoerd voor de pijplijn. Toegestane waarden zijn: (standaard), gepland eenmalig.<br/><br/>'Gepland' geeft aan dat de pijplijn met een opgegeven tijdsinterval op basis van de actieve periode (begin- en -tijd) wordt uitgevoerd. 'Eenmalige' geeft aan dat de pijplijn slechts één keer wordt uitgevoerd. Eenmalige pijplijnen eenmaal gemaakt kunnen niet worden gewijzigd/bijgewerkt op dit moment. Zie [Onetime pijplijn](#onetime-pipeline) voor meer informatie over eenmalige instelling. |Nee |
| expirationTime | Duur van de tijd na het maken van waarvoor de [eenmalige pijplijn](#onetime-pipeline) geldig is en ingericht moet blijven. Als er geen een actieve, is mislukt, of opnieuw wordt uitgevoerd, de pijplijn automatisch is bereikt verwijderd nadat het verlooptijd van de. De standaardwaarde: `"expirationTime": "3.00:00:00"`|Nee |
| gegevenssets |Lijst met gegevenssets moet worden gebruikt door de activiteiten die zijn gedefinieerd in de pijplijn. Deze eigenschap kan worden gebruikt voor het definiëren van gegevenssets die specifiek zijn voor deze pijplijn en niet zijn gedefinieerd in de data factory. Gegevenssets die zijn gedefinieerd in deze pijplijn kan alleen worden gebruikt door deze pijplijn en kan niet worden gedeeld. Zie [binnen het bereik van gegevenssets](data-factory-create-datasets.md#scoped-datasets) voor meer informatie. |Nee |

## <a name="activity-json"></a>Activity in JSON
De sectie **activities** kan één of meer activiteiten bevatten die zijn gedefinieerd binnen de activiteit. Elke activiteit heeft de volgende structuur op het hoogste niveau:

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
    },
    "scheduler":
    {
    }
}
```

De volgende tabel beschrijft de eigenschappen in de JSON-definitie activity:

| Label | Description | Vereist |
| --- | --- | --- |
| naam | De naam van de activiteit. Geef een naam op die staat voor de actie die de activiteit uitvoert. <br/><ul><li>Maximum aantal tekens: 260</li><li>Moet beginnen met een letter, cijfer of een onderstrepingsteken (\_)</li><li>De volgende tekens zijn niet toegestaan: '. ', '+ ','?', '/', '< ',' >', ' * ', '%', '&', ': ','\\"</li></ul> |Ja |
| description | Beschrijving van het doel waarvoor de activiteit of wordt gebruikt |Ja |
| type | Type activiteit. Zie de [activiteiten voor gegevensverplaatsing](#data-movement-activities) en [activiteiten voor gegevenstransformatie](#data-transformation-activities) secties voor verschillende soorten activiteiten. |Ja |
| invoer |Invoer tabellen die worden gebruikt door de activiteit<br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |Ja |
| uitvoer |Uitvoer tabellen die worden gebruikt door de activiteit.<br/><br/>`// one output table`<br/>`"outputs":  [ { "name": "outputtable1" } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": "outputtable1" }, { "name": "outputtable2" }  ],` |Ja |
| linkedServiceName |De naam van de gekoppelde service die door de activiteit wordt gebruikt. <br/><br/>Een activiteit kan vereisen dat u de gekoppelde service opgeeft die is gekoppeld aan de vereiste rekenomgeving. |Ja voor HDInsight-activiteit en Azure Machine Learning Batch Scoring Activity <br/><br/>Nee voor alle andere |
| typeProperties |Eigenschappen in de **typeProperties** sectie, is afhankelijk van het type van de activiteit. Klik op koppelingen naar de activiteiten in de vorige sectie om typeProperties voor een activiteit te bekijken. | Nee |
| policy |Beleidsregels die van invloed zijn op het runtimegedrag van de activiteit. Als deze niet is opgegeven, worden de standaardbeleidsregels gebruikt. |Nee |
| Scheduler | ''-plannereigenschap wordt gebruikt voor het definiëren van de gewenste planning voor de activiteit. De subeigenschappen zijn hetzelfde als blobs in de [beschikbaarheid-eigenschap in een gegevensset](data-factory-create-datasets.md#dataset-availability). |Nee |


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

## <a name="sample-copy-pipeline"></a>Voorbeeld van kopieerpijplijn
De volgende voorbeeldpijplijn bevat een activiteit van het type **Copy** in de sectie **activities**. In dit voorbeeld kopieert de [kopieeractiviteit](data-factory-data-movement-activities.md) gegevens vanuit een Azure-blobopslag naar een Azure SQL-database. 

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
    "start": "2016-07-12T00:00:00Z",
    "end": "2016-07-13T00:00:00Z"
  }
} 
```

Houd rekening met de volgende punten:

* In het gedeelte Activiteiten is er slechts één activiteit waarvan **type** is ingesteld op **Copy**.
* De invoer voor de activiteit is ingesteld op **InputDataset** en de uitvoer voor de activiteit is ingesteld op **OutputDataset**. Zie het artikel [Gegevenssets](data-factory-create-datasets.md) voor informatie over het definiëren van gegevenssets in JSON. 
* In het gedeelte **typeProperties** is **BlobSource** opgegeven als het brontype en **SqlSink** als het sink-type. In de [activiteiten voor gegevensverplaatsing](#data-movement-activities) sectie, klikt u op het gegevensarchief dat u gebruiken als een bron of een sink wilt voor meer informatie over het verplaatsen van gegevens naar/van de betreffende gegevensarchief. 

Zie voor een volledige procedure voor het maken van deze pijplijn, [zelfstudie: Gegevens kopiëren van Blob Storage naar SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="sample-transformation-pipeline"></a>Voorbeeld van pijplijn voor transformatie
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
        "start": "2016-04-01T00:00:00Z",
        "end": "2016-04-02T00:00:00Z",
        "isPaused": false
    }
}
```

Houd rekening met de volgende punten: 

* In het gedeelte activities is er slechts één activiteit waarvan **type** is ingesteld op **HDInsightHive**.
* Het Hive-scriptbestand **partitionweblogs.hql** wordt opgeslagen in het Azure-opslagaccount (opgegeven door de scriptLinkedService met de naam **AzureStorageLinkedService**) en in de map **script** van de container **adfgetstarted**.
* De `defines` sectie wordt gebruikt om op te geven van de runtime-instellingen die worden doorgegeven aan het hive-script als Hive-configuratiewaarden (bijvoorbeeld `${hiveconf:inputtable}`, `${hiveconf:partitionedtable}`).

De sectie **typeProperties** verschilt voor elke transformatieactiviteit. Voor meer informatie over ondersteunde typeproperties voor een transformatieactiviteit, klikt u op de transformatieactiviteit in de [activiteiten voor gegevenstransformatie](#data-transformation-activities) tabel. 

Zie voor een volledige procedure voor het maken van deze pijplijn, [zelfstudie: Uw eerste pijplijn om gegevens te verwerken met behulp van Hadoop-cluster maken](data-factory-build-your-first-pipeline.md). 

## <a name="multiple-activities-in-a-pipeline"></a>Meerdere activiteiten in een pijplijn
De vorige twee voorbeeldpijplijnen bevatten slechts één activiteit. Een pijplijn kan echter meer dan één activiteit hebben.  

Als u meerdere activiteiten in een pijplijn hebt en uitvoer van een activiteit geen invoer van een andere activiteit is, de activiteiten kunnen parallel worden uitgevoerd als invoergegevens segmenten voor de activiteiten klaar bent. 

U kunt twee activiteiten koppelen door de uitvoergegevensset van één activiteit als invoergegevensset van de andere activiteit. De tweede activiteit wordt uitgevoerd alleen als het eerste item is voltooid.

![Koppelen van activiteiten in de dezelfde pijplijn](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

In dit voorbeeld heeft de pijplijn twee activiteiten: Activiteit1 en activiteit2. De activiteit1 neemt Dataset1 als invoer en uitvoer produceert Dataset2. De activiteit neemt Dataset2 als invoer en uitvoer produceert Dataset3. Sinds de uitvoer van activiteit1 (Dataset2) is de invoer van activiteit2, de activiteit2 wordt uitgevoerd nadat de activiteit voltooid is en het Dataset2 segment wordt geproduceerd. Als de activiteit1 om een bepaalde reden mislukt en geen het Dataset2 segment wordt geproduceerd, de activiteit 2 niet uitgevoerd voor dat segment (bijvoorbeeld: 9 BEN tot en met 10 uur). 

U kunt ook de activiteiten die zich in andere pijplijnen koppelen.

![Koppelen van activiteiten in twee pijplijnen](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

In dit voorbeeld heeft Pipeline1 slechts één activiteit, waarmee neemt Dataset1 als invoer en produceert Dataset2 als uitvoer. De Pipeline2 heeft ook maar één activiteit die wordt gebruikt Dataset2 als invoer en Dataset3 als uitvoer. 

Zie voor meer informatie, [plannings- en](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

## <a name="create-and-monitor-pipelines"></a>Maken en controleren van pijplijnen
U kunt pijplijnen maken met behulp van een van deze hulpprogramma's of de SDK's. 

- De Wizard kopiëren. 
- Azure Portal
- Visual Studio
- Azure PowerShell
- Azure Resource Manager-sjabloon
- REST-API
- .NET API

Zie de volgende zelfstudies voor stapsgewijze instructies voor het maken van pijplijnen met behulp van een van deze hulpprogramma's of de SDK's.
 
- [Een pijplijn met een transformatieactiviteit voor gegevens bouwen](data-factory-build-your-first-pipeline.md)
- [Een pijplijn met een activiteit van de verplaatsing van gegevens maken](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

Zodra een pijplijn gemaakt/geïmplementeerd is, kunt u deze kunt beheren en bewaken van uw pijplijnen met behulp van de Azure portal-blades of Monitor and Manage App gebruiken. Zie de volgende onderwerpen voor stapsgewijze instructies. 

- [Pijplijnen bewaken en beheren met behulp van Azure portal-blades](data-factory-monitor-manage-pipelines.md).
- [Pijplijnen bewaken en beheren met behulp van de Monitor and Manage App gebruiken](data-factory-monitor-manage-app.md)


## <a name="onetime-pipeline"></a>Eenmalige pijplijn
U kunt maken en plannen van een pijplijn periodiek wordt uitgevoerd (bijvoorbeeld: per uur of dagelijks) binnen de begin- en eindtijden die u in het pijplijndefinitie van de opgeeft. Zie [activiteiten taakplanning](#scheduling-and-execution) voor meer informatie. U kunt ook een pijplijn maken die wordt slechts één keer uitgevoerd. Om dit te doen, stelt u de **pipelineMode** eigenschap in de pijplijndefinitie **eenmalige** zoals wordt weergegeven in de volgende JSON-voorbeeld. De standaardwaarde voor deze eigenschap is **geplande**.

```json
{
    "name": "CopyPipeline",
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
                        "name": "InputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ]
                "name": "CopyActivity-0"
            }
        ]
        "pipelineMode": "OneTime"
    }
}
```

Houd rekening met het volgende:

* **Start** en **end** voor de pijplijn zijn niet opgegeven.
* **Beschikbaarheid** van invoer en uitvoer gegevenssets is opgegeven (**frequentie** en **interval**), hoewel Data Factory niet de waarden gebruikt.  
* Diagramweergave weergegeven geen eenmalige pijplijnen. Dit gedrag is standaard.
* Eenmalige pijplijnen kunnen niet worden bijgewerkt. U kunt klonen van een eenmalige pijplijn, wijzig de naam, eigenschappen bij te werken en implementeer deze voor het maken van een andere naam.


## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over gegevenssets [gegevenssets](data-factory-create-datasets.md) artikel. 
- Zie voor meer informatie over hoe pijplijnen worden gepland en uitgevoerd, [planning en uitvoering in Azure Data Factory](data-factory-scheduling-and-execution.md) artikel. 
  

