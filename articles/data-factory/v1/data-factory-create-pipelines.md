---
title: Pijplijnen maken/schema, de keten van activiteiten in de Data Factory | Microsoft Docs
description: Informatie over het maken van een pijplijn gegevens in Azure Data Factory verplaatsen en gegevens transformeren. Maak een gegevensgestuurde werkstroom voor het produceren van gereed om informatie te gebruiken.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 13b137c7-1033-406f-aea7-b66f25b313c0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: c8ddd2b49ca48f3bf232a8650d870a8b7159f66a
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="pipelines-and-activities-in-azure-data-factory"></a>Pijplijnen en activiteiten in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](data-factory-create-pipelines.md)
> * [Versie 2 - Preview](../concepts-pipelines-activities.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [pijplijnen in V2](../concepts-pipelines-activities.md).

Met behulp van dit artikel krijgt u inzicht in de pijplijnen en activiteiten in Azure Data Factory en in de wijze waarop u deze kunt gebruiken om end-to-end gegevensgestuurde werkstromen te maken voor uw gegevensverplaatsingen en scenario’s voor gegevensverwerking.  

> [!NOTE]
> In dit artikel wordt ervan uitgegaan dat u hebt doorlopen [Inleiding tot Azure Data Factory](data-factory-introduction.md). Als u geen hands-op-ervaring met het maken van data Factory, gaan via [data transformation zelfstudie](data-factory-build-your-first-pipeline.md) en/of [data movement zelfstudie](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) zou u in dit artikel beter inzicht te geven.  

## <a name="overview"></a>Overzicht
Een gegevensfactory kan één of meer pijplijnen hebben. Een pijplijn is een logische groep activiteiten die samen een taak uitvoeren. Met activiteiten in een pijplijn definieert u welk acties moeten worden uitgevoerd voor uw gegevens. U kunt bijvoorbeeld een kopieeractiviteit gebruiken om gegevens van een on-premises SQL Server naar Azure Blob Storage te kopiëren. Vervolgens kunt u een Hive-activiteit gebruiken waarmee een Hive-script op een Azure HDInsight-cluster wordt uitgevoerd om gegevens uit Blob Storage te verwerken/transformeren en uitvoergegevens te produceren. Gebruik tot slot een tweede kopieeractiviteit voor het kopiëren van de uitvoergegevens naar een Azure SQL Data Warehouse waarop rapportageoplossingen voor business intelligence (BI) zijn gebouwd. 

Een activiteit kan nul of meer [invoergegevenssets](data-factory-create-datasets.md) hebben en een of meer [uitvoergegevenssets](data-factory-create-datasets.md) produceren. Het volgende diagram toont de relatie tussen de pijplijn, activiteit en gegevensset in Data Factory: 

![Relatie tussen pipeline, activiteit en gegevensset](media/data-factory-create-pipelines/relationship-pipeline-activity-dataset.png)

Een pijplijn, kunt u activiteiten als een set in plaats van elk afzonderlijk beheren. U kunt bijvoorbeeld implementeren, plannen, onderbreken en hervatten van een pijplijn, in plaats van onafhankelijk omgaan met activiteiten in de pijplijn.

Data Factory ondersteunt twee soorten activiteiten: activiteiten voor gegevensverplaatsing en activiteiten voor gegevenstransformatie. Elke activiteit kunt hebben van nul of meer invoer [gegevenssets](data-factory-create-datasets.md) en een of meer uitvoergegevenssets te produceren.

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
Als u verplaatsen naar/van een gegevensarchief wilt dat de Kopieeractiviteit niet ondersteunen, of gegevens met behulp van uw eigen logica te transformeren, maken een **aangepaste .NET-activiteit**. Zie [Aangepaste activiteiten gebruiken in een Azure Data Factory-pijplijn](data-factory-use-custom-activities.md) voor meer informatie over het maken en gebruiken van een aangepaste activiteit.

## <a name="schedule-pipelines"></a>Planning pijplijnen
Een pijplijn is active alleen tussen de **start** tijd en **end** tijd. Het is niet uitgevoerd vóór de begintijd of na de eindtijd. Als de pijplijn wordt onderbroken, wordt deze niet uitgevoerd ongeacht de begin- en -tijd. Voor een pijplijn om uit te voeren, moet deze niet worden onderbroken. Zie [planning en uitvoering](data-factory-scheduling-and-execution.md) om te begrijpen hoe plannen en uitvoeren in Azure Data Factory werkt.

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

| Label | Beschrijving | Vereist |
| --- | --- | --- |
| naam |Naam van de pijplijn. Geef een naam op die staat voor de actie die de pijplijn uitvoert. <br/><ul><li>Maximum aantal tekens: 260</li><li>Moet beginnen met een letter, cijfer of onderstrepingsteken (_)</li><li>Volgende tekens zijn niet toegestaan: '. ', '+ ','?', '/', '< ',' >', ' * ', "%", "&", ":","\\'</li></ul> |Ja |
| description | Voer een beschrijving in van het doel waarvoor de pijplijn wordt gebruikt. |Ja |
| activities | De sectie **activities** kan één of meer activiteiten bevatten die zijn gedefinieerd binnen de activiteit. Zie de volgende sectie voor meer informatie over de activiteiten JSON-element. | Ja |  
| start | Datum-begintijd voor de pijplijn. Moet in [ISO-indeling](http://en.wikipedia.org/wiki/ISO_8601). Bijvoorbeeld: `2016-10-14T16:32:41Z`. <br/><br/>Het is mogelijk om op te geven van een lokale tijd, bijvoorbeeld een geschatte tijd. Hier volgt een voorbeeld: `2016-02-27T06:00:00-05:00`", namelijk 6 uur geschatte<br/><br/>De begin- en -eigenschappen opgeven samen actieve periode voor de pijplijn. Uitvoer segmenten worden alleen gemaakt met deze actieve periode. |Nee<br/><br/>Als u een waarde op voor de end-eigenschap opgeeft, moet u de waarde voor de eigenschap start opgeven.<br/><br/>De begin- en eindtijden kunnen niet leeg zijn voor het maken van een pijplijn. U moet beide waarden om in te stellen van een actieve periode voor de pijplijn om uit te voeren. Als u geen begin- en eindtijden opgeeft wanneer u een pijplijn maakt, kunt u deze met de cmdlet Set-AzureRmDataFactoryPipelineActivePeriod later instellen. |
| einde | Einddatum en-tijd voor de pijplijn. Als in de ISO-indeling moet worden opgegeven. Bijvoorbeeld: `2016-10-14T17:32:41Z` <br/><br/>Het is mogelijk om op te geven van een lokale tijd, bijvoorbeeld een geschatte tijd. Hier volgt een voorbeeld: `2016-02-27T06:00:00-05:00`, namelijk 6 AM EST.<br/><br/>Als u wilt de pijplijn voor onbepaalde tijd uitvoeren, geef 9999-09-09 als de waarde voor de end-eigenschap. <br/><br/> Er is een pijplijn active alleen tussen de begintijd en eindtijd. Het is niet uitgevoerd vóór de begintijd of na de eindtijd. Als de pijplijn wordt onderbroken, wordt deze niet uitgevoerd ongeacht de begin- en -tijd. Voor een pijplijn om uit te voeren, moet deze niet worden onderbroken. Zie [planning en uitvoering](data-factory-scheduling-and-execution.md) om te begrijpen hoe plannen en uitvoeren in Azure Data Factory werkt. |Nee <br/><br/>Als u een waarde voor de start-eigenschap opgeeft, moet u de waarde voor de end-eigenschap opgeven.<br/><br/>Zie de opmerkingen voor de **start** eigenschap. |
| isPaused | Indien ingesteld op true, de pijplijn wordt niet uitgevoerd. Het is in de status onderbroken. Standaardwaarde = false. U kunt deze eigenschap in- of uitschakelen van een pijplijn. |Nee |
| pipelineMode | De methode voor de planning wordt uitgevoerd voor de pijplijn. Toegestane waarden zijn: (standaard), gepland eenmalige.<br/><br/>De geplande' geeft aan dat de pijplijn met een opgegeven tijdsinterval volgens de actieve periode (begin- en -tijd) wordt uitgevoerd. 'Eenmalige' geeft aan dat de pijplijn wordt slechts één keer uitgevoerd. Eenmalige pijplijnen eenmaal gemaakt kunnen niet worden gewijzigd/bijgewerkt op dit moment. Zie [Onetime pijplijn](#onetime-pipeline) voor meer informatie over het instellen van eenmalige. |Nee |
| expirationTime | Tijdsduur na het maken waarvoor de [eenmalige pijplijn](#onetime-pipeline) is geldig en moet worden ingericht. Als er geen elk actief is, is mislukt, of in behandeling wordt uitgevoerd, de pijplijn automatisch is bereikt één keer verwijderd de verlooptijd. De standaardwaarde:`"expirationTime": "3.00:00:00"`|Nee |
| Gegevenssets |Lijst met gegevenssets kunnen worden gebruikt door de activiteiten die zijn gedefinieerd in de pijplijn. Deze eigenschap kan worden gebruikt voor het definiëren van gegevenssets die specifiek zijn voor deze pipeline en niet gedefinieerd in de gegevensfactory. Gegevenssets die zijn gedefinieerd in deze pijplijn kan alleen worden gebruikt door deze pipeline en kan niet worden gedeeld. Zie [binnen het bereik van gegevenssets](data-factory-create-datasets.md#scoped-datasets) voor meer informatie. |Nee |

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

| Label | Beschrijving | Vereist |
| --- | --- | --- |
| naam | De naam van de activiteit. Geef een naam op die staat voor de actie die de activiteit uitvoert. <br/><ul><li>Maximum aantal tekens: 260</li><li>Moet beginnen met een letter, cijfer of onderstrepingsteken (_)</li><li>Volgende tekens zijn niet toegestaan: '. ', '+ ','?', '/', '< ',' >', ' * ', "%", "&", ":","\\'</li></ul> |Ja |
| description | Beschrijving van het doel waarvoor de activiteit of wordt gebruikt |Ja |
| type | Type activiteit. Zie de [activiteiten voor gegevensverplaatsing](#data-movement-activities) en [activiteiten voor gegevenstransformatie](#data-transformation-activities) secties voor verschillende soorten activiteiten. |Ja |
| Invoer |Invoertabellen die wordt gebruikt door de activiteit<br/><br/>`// one input table`<br/>`"inputs":  [ { "name": "inputtable1"  } ],`<br/><br/>`// two input tables` <br/>`"inputs":  [ { "name": "inputtable1"  }, { "name": "inputtable2"  } ],` |Ja |
| uitvoer |Uitvoergegevens tabellen die worden gebruikt door de activiteit.<br/><br/>`// one output table`<br/>`"outputs":  [ { "name": "outputtable1" } ],`<br/><br/>`//two output tables`<br/>`"outputs":  [ { "name": "outputtable1" }, { "name": "outputtable2" }  ],` |Ja |
| linkedServiceName |De naam van de gekoppelde service die door de activiteit wordt gebruikt. <br/><br/>Een activiteit kan vereisen dat u de gekoppelde service opgeeft die is gekoppeld aan de vereiste rekenomgeving. |Ja voor HDInsight-activiteit en Azure Machine Learning-score-activiteit <br/><br/>Nee voor alle andere |
| typeProperties |Eigenschappen in de **typeProperties** sectie, is afhankelijk van het type van de activiteit. Klik op koppelingen naar de activiteiten in de vorige sectie om typeProperties voor een activiteit te bekijken. | Nee |
| policy |Beleidsregels die van invloed zijn op het runtimegedrag van de activiteit. Als deze niet is opgegeven, worden de standaardbeleidsregels gebruikt. |Nee |
| Scheduler | 'scheduler'-eigenschap wordt gebruikt voor het definiëren van de gewenste planning voor de activiteit. Bijbehorende subeigenschappen zijn hetzelfde als de waarden in de [eigenschap beschikbaarheid in een dataset](data-factory-create-datasets.md#dataset-availability). |Nee |


### <a name="policies"></a>Beleidsregels
Beleid geldt voor de run-time-gedrag van een activiteit specifiek wanneer het segment van een tabel wordt verwerkt. De volgende tabel bevat de details.

| Eigenschap | Toegestane waarden | Standaardwaarde | Beschrijving |
| --- | --- | --- | --- |
| Gelijktijdigheid van taken |Geheel getal <br/><br/>De maximale waarde: 10 |1 |Het aantal gelijktijdige uitvoeringen van de activiteit.<br/><br/>Bepaalt het aantal parallelle activiteit uitvoeringen die kunnen ontstaan op verschillende segmenten. Bijvoorbeeld, als een activiteit moet doorlopen sneller een groot aantal beschikbare gegevens, een grotere waarde voor gelijktijdigheid het verwerken van gegevens. |
| executionPriorityOrder |NewestFirst<br/><br/>OldestFirst |OldestFirst |Bepaalt de volgorde van de gegevenssegmenten dat wordt verwerkt.<br/><br/>Als er 2 segmenten (één gebeurt om 4 uur en 17: 00 uur een andere één voor één) en beide zijn in behandeling kan worden uitgevoerd. Als u de executionPriorityOrder NewestFirst worden ingesteld, wordt het segment om 17.00 eerst verwerkt. Op dezelfde manier als u de executionPriorityORder OldestFIrst worden ingesteld, klikt u vervolgens het segment om 4 uur is verwerkt. |
| retry |Geheel getal<br/><br/>De maximale waarde is 10 |0 |Aantal nieuwe pogingen voordat de verwerking van de gegevens voor het segment is gemarkeerd als mislukt. Uitvoering van de activiteit voor een gegevenssegment wordt herhaald tot maximaal het aantal pogingen opgegeven. De nieuwe poging wordt gedaan zo snel mogelijk na de fout. |
| timeout |TimeSpan |00:00:00 |Time-out voor de activiteit. Voorbeeld: 00:10:00 (impliceert time-out 10 minuten)<br/><br/>Als een waarde niet opgegeven is of 0 is, is de time-out van oneindig.<br/><br/>Als de verwerkingstijd van de gegevens op een segment de time-outwaarde overschrijdt, deze wordt geannuleerd en wordt geprobeerd om opnieuw te proberen de verwerking. Het aantal nieuwe pogingen, is afhankelijk van de eigenschap probeer het opnieuw. Wanneer een time-out optreedt, wordt de status ingesteld op een time-out opgetreden bij. |
| Vertraging |TimeSpan |00:00:00 |Geef de vertraging optreden voordat de verwerking van het segment wordt gestart.<br/><br/>De uitvoering van de activiteit voor een gegevenssegment wordt gestart nadat de vertraging voorbij de verwachte tijd voor uitvoering is.<br/><br/>Voorbeeld: 00:10:00 (betekent vertraging van 10 minuten) |
| longRetry |Geheel getal<br/><br/>De maximale waarde: 10 |1 |Het aantal pogingen lang voordat de uitvoering van het segment is mislukt.<br/><br/>longRetry pogingen door longRetryInterval gelijkmatig verdeeld. Als u een tijd tussen pogingen opgeven moet, gebruikt u dus longRetry. Als zowel de nieuwe pogingen en de longRetry worden opgegeven, elke poging longRetry bevat nieuwe pogingen en het maximale aantal pogingen is opnieuw * longRetry.<br/><br/>Bijvoorbeeld, als we hebben de volgende instellingen in het activiteitenbeleid voor:<br/>Probeer: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>Wordt ervan uitgegaan dat er is slechts één segment uit te voeren (status Waiting) en de activiteit is uitgevoerd elke keer mislukt. Er zou worden in eerste instantie 3 opeenvolgende uitvoering pogingen. De status van het segment is na elke poging probeer het opnieuw. Nadat de eerste 3 pogingen hebben bekeken, zou de status van het segment LongRetry zijn.<br/><br/>Na een uur (dat wil zeggen, de longRetryInteval waarde), zou er een andere set 3 opeenvolgende uitvoering pogingen. Hierna is de status van het segment zou niet en geen pogingen meer zou worden uitgevoerd. Daarom is algemene 6 geprobeerd.<br/><br/>Als een uitvoering is voltooid, het segment de status gereed zijn en er worden geen pogingen meer geprobeerd.<br/><br/>longRetry kan worden gebruikt in situaties waarbij afhankelijke gegevens aankomen op niet-deterministische tijdstippen of de hele omgeving flaky onder welke gegevensverwerking plaatsvindt. In dergelijke gevallen tijd nieuwe pogingen achter elkaar niet kunt doen, en dit na een interval van resultaten in de gewenste uitvoer.<br/><br/>Waarschuwing: geen hoge waarden voor de longRetry of longRetryInterval instelt. Hogere waarden dat normaal gesproken andere al problemen. |
| longRetryInterval |TimeSpan |00:00:00 |De vertraging tussen pogingen lang |

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
* In het gedeelte **typeProperties** is **BlobSource** opgegeven als het brontype en **SqlSink** als het sink-type. In de [activiteiten voor gegevensverplaatsing](#data-movement-activities) sectie, klikt u op dat u gebruiken als een bron of een sink wilt voor meer informatie over het verplaatsen van gegevens naar/van die gegevensarchief gegevensarchief. 

Zie voor een volledige procedure voor het maken van deze pipeline [zelfstudie: gegevens kopiëren van Blob-opslag naar SQL-Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

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

De sectie **typeProperties** verschilt voor elke transformatieactiviteit. Voor meer informatie over ondersteunde voor een transformatieactiviteit type-eigenschappen, klikt u op de transformatieactiviteit in de [activiteiten voor gegevenstransformatie](#data-transformation-activities) tabel. 

Zie voor een volledige procedure voor het maken van deze pipeline [zelfstudie: uw eerste pijplijn om gegevens te verwerken met behulp van Hadoop-cluster](data-factory-build-your-first-pipeline.md). 

## <a name="multiple-activities-in-a-pipeline"></a>Meerdere activiteiten in een pijplijn
De vorige twee voorbeeldpijplijnen bevatten slechts één activiteit. Een pijplijn kan echter meer dan één activiteit hebben.  

Als er meerdere activiteiten in een pijplijn en uitvoer van een activiteit geen invoer van een andere activiteit is, de activiteiten kunnen parallel worden uitgevoerd als invoergegevens segmenten voor de activiteiten klaar bent. 

U kunt twee activiteiten koppelen door de uitvoergegevensbron van één activiteit als invoergegevensset van de andere activiteit. De tweede activiteit wordt uitgevoerd alleen wanneer het eerste beheerpunt is voltooid.

![Activiteiten in de dezelfde pijplijn-koppeling](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

In dit voorbeeld wordt de pijplijn heeft twee activiteiten: activiteit1 en activiteit2. De activiteit1 neemt Dataset1 als invoer en uitvoer produceert Dataset2. De activiteit neemt Dataset2 als invoer en uitvoer produceert Dataset3. Sinds de uitvoer van activiteit1 (Dataset2) is de invoer van activiteit2, de activiteit2 wordt uitgevoerd nadat de activiteit voltooid is en het Dataset2-segment wordt geproduceerd. Als de activiteit1 voor een bepaalde reden mislukt en het segment Dataset2 geen produceert, de activiteit 2 kan niet worden uitgevoerd voor die segment (bijvoorbeeld: 9 uur aan 10 uur). 

U kunt ook de activiteiten in verschillende pijplijnen koppelen.

![Koppeling van activiteiten in twee pijplijnen](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

In dit voorbeeld heeft Pipeline1 slechts één activiteit die ervoor zorgt dat Dataset1 als invoer Dataset2 als uitvoer produceert. De Pipeline2 heeft ook slechts één activiteit die Dataset2 als invoer en Dataset3 als uitvoer neemt. 

Zie voor meer informatie [plannen en uitvoeren](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

## <a name="create-and-monitor-pipelines"></a>Maken en pijplijnen bewaken
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
- [Maken van een pijplijn met een activiteit van de verplaatsing van gegevens](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

Zodra een pijplijn gemaakt/geïmplementeerd, kunt u deze kunt beheren en uw pijplijnen bewaken met behulp van de Azure portal-blades of Monitor and Manage App gebruiken. Zie de volgende onderwerpen voor stapsgewijze instructies. 

- [Bewaken en beheren van pijplijnen via Azure portal-blades](data-factory-monitor-manage-pipelines.md).
- [Bewaken en beheren van pijplijnen met behulp van de Monitor and Manage App](data-factory-monitor-manage-app.md)


## <a name="onetime-pipeline"></a>Eenmalige pijplijn
U kunt maken en plannen van een pijplijn periodiek wordt uitgevoerd (bijvoorbeeld: elk uur of dagelijks) binnen de begin- en eindtijden die u in de definitie van de pijplijn opgeeft. Zie [activiteiten plannen](#scheduling-and-execution) voor meer informatie. Ook kunt u een pijplijn die wordt slechts eenmaal uitgevoerd. Om dit te doen, stelt u de **pipelineMode** eigenschap in de pipeline-definitie **eenmalige** zoals weergegeven in het volgende JSON-voorbeeld. De standaardwaarde voor deze eigenschap is **geplande**.

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

* **Start** en **end** tijden voor de pijplijn niet zijn opgegeven.
* **Beschikbaarheid** van invoer en uitvoer gegevenssets is opgegeven (**frequentie** en **interval**), ook al Data Factory niet de waarden gebruikt.  
* Diagramweergave worden geen eenmalige pijplijnen weergegeven. Dit gedrag is standaard.
* Eenmalige pijplijnen kunnen niet worden bijgewerkt. U kunt een eenmalige pijplijn klonen, wijzigt u de naam, eigenschappen bijwerken en implementeren om te maken van een andere naam.


## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over gegevenssets [gegevenssets maken](data-factory-create-datasets.md) artikel. 
- Zie voor meer informatie over hoe pijplijnen worden gepland en uitgevoerd, [planning en uitvoering in Azure Data Factory](data-factory-scheduling-and-execution.md) artikel. 
  

