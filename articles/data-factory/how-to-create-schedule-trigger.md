---
title: Schema-triggers maken in Azure Data Factory | Microsoft Docs
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
ms.openlocfilehash: 6b92e8402d372e29e264dc70b128124973d66bb9
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="how-to-create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Het maken van een trigger die een pijplijn volgens een planning wordt uitgevoerd
Dit artikel bevat informatie over de planning trigger en stappen voor het maken, te starten en te bewaken van een trigger. Zie voor andere soorten triggers [Pipeline-uitvoering en triggers](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Aan de slag met versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

### <a name="schedule-trigger-json-definition"></a>Planning trigger JSON-definitie
Wanneer u een trigger schema maakt, kunt u de planning en een terugkeerpatroon met een JSON zoals weergegeven in het voorbeeld in deze sectie kunt opgeven.

Als u uw schema-trigger ere van een pijplijn die worden uitgevoerd, bevatten een pipeline-verwijzing van de bepaalde pijplijn in de definitie van de trigger. Pijplijnen en triggers hebben een veel-op-veel-relatie. Meerdere triggers kunnen één pijplijn activeren. Eén trigger kan meerdere pijplijnen activeren.

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Month>>,
        "interval": <<int>>,             // optional, how often to fire (default to 1)
        "startTime": <<datetime>>,
        "endTime": <<datetime - optional>>,
        "timeZone": "UTC"
        "schedule": {                    // optional (advanced scheduling specifics)
          "hours": [<<0-23>>],
          "weekDays": : [<<Monday-Sunday>>],
          "minutes": [<<0-59>>],
          "monthDays": [<<1-31>>],
          "monthlyOccurences": [
               {
                    "day": <<Monday-Sunday>>,
                    "occurrence": <<1-5>>
               }
           ]
        }
      }
    },
   "pipelines": [
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "<Name of your pipeline>"
                },
                "parameters": {
                    "<parameter 1 Name>": {
                        "type": "Expression",
                        "value": "<parameter 1 Value>"
                    },
                    "<parameter 2 Name>" : "<parameter 2 Value>"
                }
           }
      ]
  }
}
```

> [!IMPORTANT]
>  De eigenschap **parameters** is een verplichte eigenschap binnen **pijplijnen**. Zelfs als uw pijplijn geen parameters accepteert, voegt u een lege json toe voor 'parameters', aangezien de eigenschap moet bestaan.


### <a name="overview-schedule-trigger-schema"></a>Overzicht: Planning trigger schema
De volgende tabel bevat een overzicht van de belangrijkste elementen die betrekking hebben op het terugkeerpatroon en de planning in een trigger:

JSON-eigenschap |     Beschrijving
------------- | -------------
startTime | startTime is een datum en tijd. In eenvoudige planningen is de startTime de eerste gebeurtenis. In complexe planningen begint de trigger niet eerder dan de startTime.
endTime | Hiermee geeft u de einddatum en -tijd voor de trigger op. De trigger wordt niet meer uitgevoerd na deze tijd. U kunt geen endTime die in het verleden ligt opgeven. Dit is een optionele eigenschap.
timeZone | Op dit moment wordt alleen UTC ondersteund.
recurrence | Het object recurrence bepaalt de regels voor het terugkeerpatroon van de trigger. Het object recurrence ondersteunt de volgende elementen: frequency, interval, endTime, count en schedule. Als de recurrence is gedefinieerd, is het element frequency vereist. De andere elementen van de recurrence zijn optioneel.
frequency | Hiermee geeft u de frequentie-eenheid aan waarmee de trigger wordt uitgevoerd. Ondersteunde waarden zijn: `minute`, `hour`, `day`, `week` of `month`.
interval | Het interval is een positief geheel getal. Het geeft het interval aan voor de frequentie waarmee wordt bepaald hoe vaak de trigger wordt uitgevoerd. Als het interval bijvoorbeeld 3 is en de frequency 'week', wordt de trigger elke 3 weken uitgevoerd.
schedule | Een trigger met een opgegeven frequency wijzigt de recurrence op basis van een terugkerende schedule. Een schedule bevat wijzigingen op basis van de minuten, uren, weekdagen, dagen van de maand en het weeknummer.


### <a name="overview-schedule-trigger-schema-defaults-limits-and-examples"></a>Overzicht: Planning trigger schema standaardwaarden, limieten en voorbeelden

JSON-naam | Waardetype | Vereist? | Standaardwaarde | Geldige waarden | Voorbeeld
--------- | ---------- | --------- | ------------- | ------------ | -------
startTime | Tekenreeks | Ja | None | Datums en tijden volgens ISO 8601 | ```"startTime" : "2013-01-09T09:30:00-08:00"```
recurrence | Object | Ja | None | Recurrence-object | ```"recurrence" : { "frequency" : "monthly", "interval" : 1 }```
interval | Aantal | Nee | 1 | 1 tot 1000. | ```"interval":10```
endTime | Tekenreeks | Ja | None | Datum-/tijdwaarde van een moment in de toekomst | `"endTime" : "2013-02-09T09:30:00-08:00"`
schedule | Object | Nee | None | Schedule-object | `"schedule" : { "minute" : [30], "hour" : [8,17] }`

### <a name="deep-dive-starttime"></a>Gedetailleerde informatie over startTime
De volgende tabel wordt uitgelegd hoe startTime bepaalt hoe een trigger wordt uitgevoerd:

startTime-waarde | Recurrence zonder schedule | Recurrence met schedule
--------------- | --------------------------- | ------------------------
Starttijd in het verleden | Berekent de eerstvolgende uitvoering na de starttijd en voert deze op dat moment uit.<p>Voert volgende uitvoeringen uit op basis van de tijd waarop de laatste uitvoering heeft plaatsgevonden.</p><p>Zie het voorbeeld onder deze tabel.</p> | Trigger wordt _nooit voor_ de opgegeven begintijd geactiveerd. De eerste uitvoering is gebaseerd op de planning die wordt berekend op basis van de starttijd. <p>Volgende uitvoeringen worden op basis van de terugkeerplanning uitgevoerd</p>
De starttijd ligt in de toekomst, of op dit moment | Uitvoering vindt eenmaal plaats op de opgegeven starttijd. <p>Voert volgende uitvoeringen uit op basis van de tijd waarop de laatste uitvoering heeft plaatsgevonden.</p> | Trigger wordt _nooit voor_ de opgegeven begintijd geactiveerd. De eerste uitvoering is gebaseerd op de planning die wordt berekend op basis van de starttijd.<p>Volgende uitvoeringen worden op basis van de terugkeerplanning uitgevoerd.</p>

We bekijken een voorbeeld van wat er gebeurt wanneer de startTime in het verleden ligt en er een recurrence, maar geen schedule is opgegeven. We gaan ervan uit dat de huidige tijd `2017-04-08 13:00` is, dat de startTime `2017-04-07 14:00` is en dat de recurrence elke twee dagen is (gedefinieerd met een frequence van dag en een interval van 2.) U ziet dat de startTime in het verleden ligt en plaatsvindt voor de huidige tijd.

In deze omstandigheden vindt de eerste uitvoering plaats op `2017-04-09 at 14:00`. De scheduler-engine berekent uitvoeringen vanaf de startTime. Alle uitvoeringen in het verleden worden genegeerd. De engine gebruikt de eerstvolgende uitvoering die in de toekomst plaatsvindt. In dit geval is de startTime is `2017-04-07 at 2:00pm`, waardoor de volgende uitvoering twee dagen erna wordt gestart. Dit is op `2017-04-09 at 2:00pm`.

De eerste uitvoeringstijd is de hetzelfde, zelfs als de startTime `2017-04-05 14:00` of `2017-04-01 14:00` is. Na de eerste uitvoering worden volgende uitvoeringen berekend met behulp van de schedule. Deze vinden daarom plaats op `2017-04-11 at 2:00pm`, vervolgens `2017-04-13 at 2:00pm`, dan `2017-04-15 at 2:00pm`, enzovoort.

Wanneer een trigger een schedule heeft, maar de uren en/of minuten niet zijn niet ingesteld in de schedule, worden standaard de uren en/of de minuten van de eerste uitvoering gebruikt.

### <a name="deep-dive-schedule"></a>Gedetailleerde informatie over schedule
Enerzijds kan een schedule het aantal uitvoeringen door een trigger beperken. Als een trigger met de frequency 'maand' bijvoorbeeld een schedule heeft die alleen wordt uitgevoerd op dag 31, wordt de trigger alleen uitgevoerd in maanden die een 31e dag hebben.

Anderzijds kan een schedule het aantal uitvoeringen door een trigger ook uitbreiden. Als een trigger met de frequency 'maand' bijvoorbeeld een schedule heeft die wordt uitgevoerd op dag 1 en 2 van de maand, wordt de trigger uitgevoerd op de 1e en 2e dag van de maand in plaats van één keer per maand.

Als meerdere schedule-elementen worden opgegeven, worden deze op volgorde van de grootste waarde naar de kleinste geëvalueerd: weeknummer, maand, dag, uur en minuut.

In de volgende tabel worden de schedule-elementen in detail beschreven:


JSON-naam | Beschrijving | Geldige waarden
--------- | ----------- | ------------
minutes | Minuten van het uur waarop de trigger wordt uitgevoerd. | <ul><li>Geheel getal</li><li>Matrix van gehele getallen</li></ul>
hours | Uren van de dag waarop de trigger wordt uitgevoerd. | <ul><li>Geheel getal</li><li>Matrix van gehele getallen</li></ul>
weekDays | Dagen van de week waarop de trigger wordt uitgevoerd. Kan alleen worden opgegeven met de frequency wekelijks. | <ul><li>Maandag, dinsdag, woensdag, donderdag, vrijdag, zaterdag of zondag</li><li>Matrix van elk van de waarden (maximale matrixgrootte van 7)</li></p>Niet hoofdlettergevoelig</p>
monthlyOccurrences | Hiermee wordt bepaald op welke dagen van de maand de trigger wordt uitgevoerd. Kan alleen worden opgegeven met de frequency maandelijks. | Matrix met monthlyOccurence-objecten: `{ "day": day,  "occurrence": occurence }`. <p> Day is de dag van de week waarop de trigger wordt uitgevoerd. `{Sunday}` is bijvoorbeeld elke zondag van de maand. Vereist.<p>Occurrence geeft aan op welke dag de trigger wordt uitgevoerd. `{Sunday, -1}` is bijvoorbeeld de laatste zondag van de maand. Optioneel.
monthDays | Dag van de maand waarop de trigger wordt uitgevoerd. Kan alleen worden opgegeven met de frequency maandelijks. | <ul><li>Alle waarden < = -1 en > =-31</li><li>Alle waarden > = -1 en < =-31</li><li>Een matrix met waarden</li>


## <a name="examples-recurrence-schedules"></a>Voorbeelden: herhalingsplanningen
Deze sectie bevat voorbeelden van herhalingsplanningen, gericht op het object schedule en de bijbehorende subelementen.

In de voorbeeldplanningen wordt ervan uitgegaan dat het interval is ingesteld op 1. Ook wordt ervan uitgegaan dat de juiste frequency wordt gebruikt overeenkomstig met de schedule. U kunt bijvoorbeeld niet de frequency 'day' gebruiken en 'monthDays' wijzigen in de schedule. Deze beperkingen worden vermeld in de tabel in de vorige sectie.

Voorbeeld | Beschrijving
------- | -----------
`{"hours":[5]}` | Wordt elke dag uitgevoerd om 05:00
`{"minutes":[15], "hours":[5]}` | Wordt elke dag uitgevoerd om 05:15
`{"minutes":[15], "hours":[5,17]}` | Wordt elke dag uitgevoerd om 05:15 en 17:15
`{"minutes":[15,45], "hours":[5,17]}` | Wordt elke dag uitgevoerd om 05:15, 05:45, 17:15 en 17:45
`{"minutes":[0,15,30,45]}` | Wordt elke 15 minuten uitgevoerd
`{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Wordt elk uur uitgevoerd. Deze trigger wordt elk uur uitgevoerd. De minuut wordt bepaald door de startTime, als deze is opgegeven. Als er niets is opgegeven, wordt de aanmaaktijd gebruikt. Als de starttijd of aanmaaktijd (afhankelijk van wat van toepassing is) bijvoorbeeld 12:25 uur is, wordt de trigger uitgevoerd om 00:25, 01:25, 02:25,..., 23:25. Deze schedule komt overeen met een trigger met de frequency 'uur', een interval van 1 en geen schedule. Het verschil is dat deze schedule ook kan worden gebruikt met een andere frequency en interval om andere triggers te maken. Als de frequency bijvoorbeeld 'month' is, wordt de schedule slechts één keer per maand uitgevoerd, in plaats van elke dag bij een frequency van 'day'.
`{"minutes":[0]}` | Wordt elk uur op het hele uur uitgevoerd. Deze trigger wordt ook elk uur uitgevoerd, maar dan op het hele uur (bijvoorbeeld 00:00, 01:00, 02,00, enz.). Deze instelling is gelijk aan een trigger met de frequency 'uur', een startTime met nul minuten en geen schedule. Als de frequentie 'day' was, zou er heen schedule zijn, maar als de frequency 'week' of 'month' was, zou de schedule de trigger slechts respectievelijk één dag per week of maand uitvoeren.
`{"minutes":[15]}` | Wordt 15 minuten na elk uur uitgevoerd. Wordt elk uur uitgevoerd vanaf 00:15, 01:15, 02,15, enz. en eindigt om 22:15 en 23:15.
`{"hours":[17], "weekDays":["saturday"]}` | Wordt elke week om 17:00 uitgevoerd op zaterdag
`{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Wordt elke week om 17:00 uitgevoerd op maandag, woensdag en vrijdag
`{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Wordt elke week om 17:15 en 17:45 uitgevoerd op maandag, woensdag en vrijdag
`{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Wordt elke 15 minuten uitgevoerd op weekdagen
`{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Wordt elke 15 minuten uitgevoerd op weekdagen tussen 9:00 en 16:45
`{"weekDays":["tuesday", "thursday"]}` | Wordt uitgevoerd op dinsdag en donderdag op de opgegeven begintijd.
`{"minutes":[0], "hours":[6], "monthDays":[28]}` | Wordt uitgevoerd om 06.00 op de 28e dag van elke maand (ervan uitgaande de frequency 'month' is)
`{"minutes":[0], "hours":[6], "monthDays":[-1]}` | Wordt uitgevoerd om 06.00 op de laatste dag van de maand. Als u een trigger wilt uitvoeren op de laatste dag van een maand, gebruik dan -1 in plaats van dag 28, 29, 30 of 31.
`{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | Wordt uitgevoerd om 06.00 op de eerste en laatste dag van elke maand
`{monthDays":[1,14]}` | Wordt uitgevoerd op de eerste en veertiende dag van elke maand op de opgegeven begintijd.
`{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Wordt uitgevoerd op de eerste vrijdag van elke maand om 05:00
`{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Wordt uitgevoerd op de eerste vrijdag van elke maand op de opgegeven begintijd.
`{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Wordt elke maand uitgevoerd op de derde vrijdag voor het einde van de maand op de opgegeven begintijd
`{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Wordt uitgevoerd op de eerste en laatste vrijdag van elke maand om 05:15
`{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Wordt uitgevoerd op de eerste en laatste vrijdag van elke maand op de opgegeven begintijd
`{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Wordt uitgevoerd op de vijfde vrijdag van elke maand op de opgegeven begintijd. Als een maand geen vijfde vrijdag heeft, wordt de pijplijn niet uitgevoerd, omdat deze is ingesteld om alleen op de vijfde vrijdag te worden uitgevoerd.  Als u de trigger op de laatste vrijdag van de maand wilt uitvoeren, overweeg dan om -1 te gebruiken in plaats van 5.
`{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Wordt elke 15 minuten uitgevoerd op de laatste vrijdag van de maand.
`{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | Wordt uitgevoerd om 05:15, 05:45, 17:15 en 17:45 op de derde woensdag van elke maand.


## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken
Deze sectie wordt beschreven hoe u Azure PowerShell gebruiken voor het maken, het starten en het bewaken van een scheduler-trigger. Als u zien van dit voorbeeld werken wilt, eerst doorloopt de [Snelstartgids: Maak een gegevensfactory met Azure PowerShell](quickstart-create-data-factory-powershell.md). Voeg de volgende code aan de belangrijkste methode, die zijn gemaakt en gestart van een trigger planning die wordt uitgevoerd om de 15 minuten. De trigger is gekoppeld aan een pijplijn (**Adfv2QuickStartPipeline**) die u maakt als onderdeel van de Quick Start.

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
