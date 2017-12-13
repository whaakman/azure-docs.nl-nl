---
title: Triggers maken in Azure Data Factory | Microsoft Docs
description: Informatie over het maken van een trigger in Azure Data Factory die een pijplijn volgens een planning wordt uitgevoerd.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: shlo
ms.openlocfilehash: eed286c01604ab0e9ac2113d56cbce268503668d
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="how-to-create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Het maken van een trigger die een pijplijn volgens een planning wordt uitgevoerd
Dit artikel bevat stappen voor het maken, te starten en te bewaken van een trigger. Raadpleeg voor algemene informatie over triggers [Pipeline-uitvoering en triggers](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Aan de slag met versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken
Deze sectie wordt beschreven hoe u Azure PowerShell gebruikt te maken, te starten en te bewaken van een trigger. Als u zien van dit voorbeeld werken wilt, eerst doorloopt de [Snelstartgids: Maak een gegevensfactory met Azure PowerShell](quickstart-create-data-factory-powershell.md). Voeg de volgende code aan de belangrijkste methode, die zijn gemaakt en gestart van een trigger planning die wordt uitgevoerd om de 15 minuten. De trigger is gekoppeld aan een pijplijn (**Adfv2QuickStartPipeline**) die u maakt als onderdeel van de Quick Start.

1. Maak een JSON-bestand met de naam MyTrigger.json in de map C:\ADFv2QuickStartPSH\ met de volgende inhoud: 

    > [!IMPORTANT]
    > Stel **startTime** in de huidige UTC-tijd en **endTime** aan één uur na de huidige UTC-tijd voordat de JSON-bestand wordt opgeslagen.

    ```json   
    {
        "properties": {
            "name": "MyTrigger",
            "type": "ScheduleTrigger",
            "typeProperties": {
                "recurrence": {
                    "frequency": "Minute",
                    "interval": 15,
                    "startTime": "2017-12-08T00:00:00",
                    "endTime": "2017-12-08T01:00:00"
                }
            },
            "pipelines": [{
                    "pipelineReference": {
                        "type": "PipelineReference",
                        "referenceName": "Adfv2QuickStartPipeline"
                    },
                    "parameters": {
                        "inputPath": "adftutorial/input",
                        "outputPath": "adftutorial/output"
                    }
                }
            ]
        }
    }
    ```
    
    In het JSON-fragment: 
    - De **type** van de trigger is ingesteld op **ScheduleTrigger**. 
    - De **frequentie** is ingesteld op **minuut** en **interval** is ingesteld op **15**. De trigger voert de pijplijn daarom om de 15 minuten tussen de begin- en eindtijden. 
    - De **endTime** is één uur nadat de **startTime**, zodat de trigger wordt uitgevoerd de pijplijn 15 minuten, 30 minuten en 45 minuten na de starttijd. Vergeet niet de startTime bijwerken naar de huidige UTC-tijd en endTime aan één uur na de starttijd.  
    - De trigger is gekoppeld aan de **Adfv2QuickStartPipeline** pijplijn. Als u wilt meerdere pijplijnen koppelen aan een trigger, meer toevoegen **pipelineReference** secties. 
    - De pijplijn in de Quick Start heeft twee **parameters**. U kunt daarom waarden voor die parameters doorgeven van de trigger. 
2. Geen trigger maken met behulp van de **Set AzureRmDataFactoryV2Trigger** cmdlet.

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```
3. Controleer of de status van de trigger **gestopt** met behulp van de **Get-AzureRmDataFactoryV2Trigger** cmdlet. 

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" 
    ```
4. De trigger wordt gestart via de **Start AzureRmDataFactoryV2Trigger** cmdlet: 

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" 
    ```
5. Controleer of de status van de trigger **gestart** met behulp van de **Get-AzureRmDataFactoryV2Trigger** cmdlet.

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" 
    ```
6.  Ophalen van de trigger wordt uitgevoerd met behulp van PowerShell met behulp van de **Get-AzureRmDataFactoryV2TriggerRun** cmdlet. Als u de informatie over de trigger wordt uitgevoerd, voer de volgende opdracht periodiek: Update **TriggerRunStartedAfter** en **TriggerRunStartedBefore** in overeenstemming met de waarden in de definitie van de trigger . 

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```

    Zie voor het controleren van de trigger wordt uitgevoerd/pijplijn wordt uitgevoerd in de Azure portal [pijplijn bewaken wordt uitgevoerd](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="use-net-sdk"></a>.NET SDK gebruiken
Deze sectie wordt beschreven hoe u met .NET SDK maakt, starten en controleren van een trigger. Als u zien van deze code werkt wilt, eerst doorloopt de [Quick Start voor het maken van een gegevensfactory met .NET SDK](quickstart-create-data-factory-dot-net.md). Voeg de volgende code aan de belangrijkste methode, die zijn gemaakt en gestart van een trigger planning die wordt uitgevoerd om de 15 minuten. De trigger is gekoppeld aan een pijplijn (**Adfv2QuickStartPipeline**) die u maakt als onderdeel van de Quick Start. 

```csharp
            //create the trigger
            Console.WriteLine("Creating the trigger");

            // set the start time to the current UTC time
            DateTime startTime = DateTime.UtcNow;

            // specify values for the inputPath and outputPath parameters
            Dictionary<string, object> pipelineParameters = new Dictionary<string, object>();
            pipelineParameters.Add("inputPath", "adftutorial/input");
            pipelineParameters.Add("outputPath", "adftutorial/output");

            // create a schedule trigger
            string triggerName = "MyTrigger";
            ScheduleTrigger myTrigger = new ScheduleTrigger()
            {
                Pipelines = new List<TriggerPipelineReference>()
                {
                    // associate the Adfv2QuickStartPipeline pipeline with the trigger
                    new TriggerPipelineReference()
                    {
                        PipelineReference = new PipelineReference(pipelineName),
                        Parameters = pipelineParameters,
                    }
                },
                Recurrence = new ScheduleTriggerRecurrence()
                {
                    // set the start time to current UTC time and the end time to be one hour after.
                    StartTime = startTime,
                    TimeZone = "UTC",
                    EndTime = startTime.AddHours(1),
                    Frequency = RecurrenceFrequency.Minute,
                    Interval = 15,
                }
            };

            // now, create the trigger by invoking CreateOrUpdate method. 
            TriggerResource triggerResource = new TriggerResource()
            {
                Properties = myTrigger
            };
            client.Triggers.CreateOrUpdate(resourceGroup, dataFactoryName, triggerName, triggerResource);

            //start the trigger
            Console.WriteLine("Starting the trigger");
            client.Triggers.Start(resourceGroup, dataFactoryName, triggerName);

```

Voor het bewaken van een trigger worden uitgevoerd, voeg de volgende code voordat de laatste `Console.WriteLine` instructie: 

```csharp
            // Check the trigger runs every 15 minutes
            Console.WriteLine("Trigger runs. You see the output every 15 minutes");

            for (int i = 0; i < 3; i++)
            {
                System.Threading.Thread.Sleep(TimeSpan.FromMinutes(15));
                List<TriggerRun> triggerRuns = client.Triggers.ListRuns(resourceGroup, dataFactoryName, triggerName, DateTime.UtcNow.AddMinutes(-15 * (i + 1)), DateTime.UtcNow.AddMinutes(2)).ToList();
                Console.WriteLine("{0} trigger runs found", triggerRuns.Count);

                foreach (TriggerRun run in triggerRuns)
                {
                    foreach (KeyValuePair<string, string> triggeredPipeline in run.TriggeredPipelines)
                    {
                        PipelineRun triggeredPipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, triggeredPipeline.Value);
                        Console.WriteLine("Pipeline run ID: {0}, Status: {1}", triggeredPipelineRun.RunId, triggeredPipelineRun.Status);
                        List<ActivityRun> runs = client.ActivityRuns.ListByPipelineRun(resourceGroup, dataFactoryName, triggeredPipelineRun.RunId, run.TriggerRunTimestamp.Value, run.TriggerRunTimestamp.Value.AddMinutes(20)).ToList();
                    }
                }
            }
```

Zie voor het controleren van de trigger wordt uitgevoerd/pijplijn wordt uitgevoerd in de Azure portal [pijplijn bewaken wordt uitgevoerd](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="use-python-sdk"></a>Python SDK gebruiken
Deze sectie leest u hoe u Python SDK voor het maken, te starten en te bewaken van een trigger. Als u zien van deze code werkt wilt, eerst doorloopt de [Quick Start voor het maken van een gegevensfactory met behulp van Python SDK](quickstart-create-data-factory-python.md). Vervolgens voegt u het volgende codeblok na het codeblok 'bewaken de pijplijn uitvoeren' in het pythonscript. Deze code maakt een trigger planning die wordt uitgevoerd om de 15 minuten tussen de opgegeven start- en eindtijd. De start_time worden bijgewerkt naar de huidige UTC-tijd en de end_time aan één uur na de huidige UTC-tijd. 

```python
    # Create a trigger
    tr_name = 'mytrigger'
    scheduler_recurrence = ScheduleTriggerRecurrence(frequency='Minute', interval='15',start_time='2017-12-12T04:00:00', end_time='2017-12-12T05:00:00', time_zone='UTC') 
    pipeline_parameters = {'inputPath':'adftutorial/input', 'outputPath':'adftutorial/output'}
    pipelines_to_run = []
    pipeline_reference = PipelineReference('copyPipeline')
    pipelines_to_run.append(TriggerPipelineReference(pipeline_reference, pipeline_parameters))
    tr_properties = ScheduleTrigger(description='My scheduler trigger', pipelines = pipelines_to_run, recurrence=scheduler_recurrence)    
    adf_client.triggers.create_or_update(rg_name, df_name, tr_name, tr_properties)

    # start the trigger
    adf_client.triggers.start(rg_name, df_name, tr_name)
```
Zie voor het controleren van de trigger wordt uitgevoerd/pijplijn wordt uitgevoerd in de Azure portal [pijplijn bewaken wordt uitgevoerd](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="use-resource-manager-template"></a>Gebruik Resource Manager-sjabloon
Een Azure Resource Manager-sjabloon kunt u geen trigger maken. Zie voor stapsgewijze instructies [maken van een Azure data factory met Resource Manager-sjabloon](quickstart-create-data-factory-resource-manager-template.md).  

## <a name="pass-the-trigger-start-time-to-a-pipeline"></a>De begintijd van de trigger doorgeven aan een pijplijn
Azure Data Factory ondersteund in versie 1, lezen of schrijven van gepartitioneerde gegevens met behulp van de SliceStart/SliceEnd/WindowStart/WindowEnd systeemvariabelen. In versie 2, kunt u dit gedrag kunt bereiken met behulp van een pipeline-parameter en van de trigger geplande tijd/begintijd als een waarde van de parameter. In het volgende voorbeeld wordt de geplande tijd van de trigger als een waarde doorgegeven aan de pijplijn parameter scheduledRunTime. 

```json
"parameters": {
    "scheduledRunTime": "@trigger().scheduledTime"
}
```    
Zie voor meer informatie [het lezen of schrijven gepartitioneerd gegevens](how-to-read-write-partitioned-data.md).

## <a name="next-steps"></a>Volgende stappen
Zie voor gedetailleerde informatie over triggers [Pipeline-uitvoering en triggers](concepts-pipeline-execution-triggers.md#triggers).