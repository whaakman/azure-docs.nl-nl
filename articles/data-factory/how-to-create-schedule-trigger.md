---
title: Schematriggers maken in Azure Data Factory | Microsoft Docs
description: Informatie over het maken van een trigger in Azure Data Factory die een pijplijn volgens een schema wordt uitgevoerd.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/23/2018
ms.author: shlo
ms.openlocfilehash: 70f8533843668a86607e31a551e6ebf9abeab6c4
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54016627"
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Maken van een trigger die een pijplijn volgens een schema wordt uitgevoerd
In dit artikel bevat informatie over schematriggers en de stappen voor het maken, starten en controleren van een planningstrigger. Zie voor andere soorten triggers, [pijplijnen uitvoeren en triggers](concepts-pipeline-execution-triggers.md).

Wanneer u een schematrigger maakt, u een planning opgeven (de begindatum, terugkeerpatroon, datum enz.) voor de trigger en koppelen met een pijplijn. Pijplijnen en triggers hebben een veel-op-veel-relatie. Meerdere triggers kunnen één pijplijn activeren. Eén trigger kan meerdere pijplijnen activeren.

De volgende secties bieden stappen voor het maken van een schematrigger op verschillende manieren. 

## <a name="data-factory-ui"></a>Gebruikersinterface van Data Factory
U kunt maken een **schematrigger** voor het plannen van een pijplijn periodiek wordt uitgevoerd (elk uur, dagelijks, enzovoort). 

> [!NOTE]
> Zie voor een volledige procedure voor het maken van een pijplijn en een planningstrigger, koppelt de trigger de pijplijn, en wordt uitgevoerd en bewaking van de pijplijn [Snelstartgids: een data factory maken met Data Factory-UI](quickstart-create-data-factory-portal.md).

1. Ga naar het tabblad **Bewerken**. 

    ![Naar het tabblad Bewerken gaan](./media/how-to-create-schedule-trigger/switch-edit-tab.png)
1. Klik in het menu op **Activeren** en klik op **Nieuw/bewerken**. 

    ![Menu Nieuwe trigger](./media/how-to-create-schedule-trigger/new-trigger-menu.png)
2. Klik op de pagina **Triggers toevoegen** op **Trigger kiezen...** , en klik op **Nieuw**. 

    ![Triggers toevoegen - nieuwe trigger](./media/how-to-create-schedule-trigger/add-trigger-new-button.png)
3. In de **nieuwe Trigger** pagina, de volgende stappen uit: 

    1. Bevestig dat **planning** is geselecteerd voor **Type**. 
    2. Geef de begindatum/tijd van de trigger voor **Start datum (UTC)**. Deze wordt standaard ingesteld op de huidige datum/tijd. 
    3. Geef **terugkeerpatroon** voor de trigger. Selecteer een van de waarden in de vervolgkeuzelijst (elke minuut, uur, dagelijks, wekelijks, en maandelijks). Voer de vermenigvuldiger in het tekstvak in. Bijvoorbeeld, als u wilt dat de trigger eenmaal voor elke 15 minuten wordt uitgevoerd, u **elke minuut**, en voer **15** in het tekstvak in. 
    4. Voor de **End** veld, als u niet wilt opgeven van een datum en tijd voor de trigger, selecteer **geen End**. Als u een end datum en tijd, schakelt u **op datum**, en geef de datum en tijd en klikt u op **toepassen**. Er zijn kosten gekoppeld aan elke uitvoering van de pijplijn. Als u testen wilt, kunt u om ervoor te zorgen dat de pijplijn wordt geactiveerd slechts een paar keer. Zorg er echter wel voor dat er voldoende tijd is om de pijplijn uit te voeren tussen de publicatietijd en de eindtijd. De trigger gaat pas van kracht nadat u de oplossing hebt gepubliceerd in Data Factory, niet wanneer u de trigger opslaat in de UI.

        ![Triggerinstellingen](./media/how-to-create-schedule-trigger/trigger-settings.png)
4. In de **nieuwe Trigger** venster, Controleer de **geactiveerd** optie en klik op **volgende**. U kunt dit selectievakje in voor het deactiveren van de trigger later gebruiken. 

    ![Triggerinstellingen - knop Volgende](./media/how-to-create-schedule-trigger/trigger-settings-next.png)
5. Lees op de pagina **Nieuwe trigger** de waarschuwing en klik op **Voltooien**.

    ![Triggerinstellingen - knop Voltooien](./media/how-to-create-schedule-trigger/new-trigger-finish.png)
6. Klik op **Publiceren** om wijzigingen te publiceren in Data Factory. Totdat u de wijzigingen naar Data Factory publiceert, start de trigger niet activeren van de pijplijn wordt uitgevoerd. 

    ![De knop Publiceren](./media/how-to-create-schedule-trigger/publish-2.png)
8. Ga naar het tabblad **Controleren** aan de linkerkant. Klik op **Vernieuwen** om de lijst te vernieuwen. U ziet dat de pijplijn wordt geactiveerd door de geplande trigger uitgevoerd. Bekijk de waarden in de kolom **Geactiveerd door**. Als u **nu activeren** optie, ziet u de handmatige triggeruitvoering in de lijst. 

    ![Geactiveerde uitvoeringen controleren](./media/how-to-create-schedule-trigger/monitor-triggered-runs.png)
9. Klik op de pijl-omlaag naast **Pijplijnuitvoeringen** om naar de weergave **Triggeruitvoeringen** te gaan. 

    ![Triggeruitvoeringen controleren](./media/how-to-create-schedule-trigger/monitor-trigger-runs.png)

## <a name="azure-powershell"></a>Azure PowerShell
Deze sectie leest u hoe u Azure PowerShell gebruiken om te maken, starten en controleren van een planningstrigger. In dit voorbeeld werken eerst Ga via de [Quick Start: Een data factory maken met behulp van Azure PowerShell](quickstart-create-data-factory-powershell.md). Voeg de volgende code aan de belangrijkste methode, die wordt gemaakt en een planningstrigger die wordt uitgevoerd om de 15 minuten wordt gestart. De trigger is gekoppeld aan een pijplijn met de naam **Adfv2QuickStartPipeline** die u maakt als onderdeel van de Quick Start.

1. Maak een JSON-bestand met de naam **MyTrigger.json** in de map C:\ADFv2QuickStartPSH\ met de volgende inhoud:

    > [!IMPORTANT]
    > Voordat u het JSON-bestand opslaat, stel de waarde van de **startTime** element op de huidige UTC-tijd. Stel de waarde van de **endTime** element naar één uur na de huidige UTC-tijd.

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

    In de JSON-fragment:
    - De **type** element van de trigger is ingesteld op "ScheduleTrigger."
    - De **frequentie** element is ingesteld op 'Minuut' en de **interval** element is ingesteld op 15. Daarom voert de trigger de pijplijn om de 15 minuten tussen de begin- en eindtijden.
    - De **endTime** -element is één uur na de waarde van de **startTime** element. Daarom voert de trigger de pijplijn 15 minuten, 30 minuten en 45 minuten na de begintijd vallen. Vergeet niet om bij te werken van de begintijd voor de huidige UTC-tijd en de eindtijd op één uur na de begintijd. 
    - De trigger is gekoppeld aan de **Adfv2QuickStartPipeline** pijplijn. Als u wilt koppelen meerdere pijplijnen met een trigger, meer toevoegen **pipelineReference** secties.
    - De pijplijn in de Quick Start worden twee **parameters** waarden: **inputPath** en **outputPath**. Daarom geeft u de waarden voor deze parameters van de trigger.

2. Een trigger maken met behulp van de **Set-AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```

3. Controleer of de status van de trigger **gestopt** met behulp van de **Get-AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

4. Start de trigger met behulp van de **Start-AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

5. Controleer of de status van de trigger **gestart** met behulp van de **Get-AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

6.  Ontvang de trigger wordt uitgevoerd in Azure PowerShell met behulp van de **Get-AzureRmDataFactoryV2TriggerRun** cmdlet. Als u de informatie over de trigger wordt uitgevoerd, moet u de volgende opdracht periodiek uitvoeren. Update de **TriggerRunStartedAfter** en **TriggerRunStartedBefore** waarden zodat deze overeenkomen met de waarden in de Triggerdefinitie van de:

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
    
    Voor het bewaken van de trigger wordt uitgevoerd en de pijplijn wordt uitgevoerd in Azure portal, Zie [pijplijnuitvoeringen controleren](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).


## <a name="net-sdk"></a>.NET SDK
Deze sectie leest u hoe u de .NET SDK gebruiken om te maken, starten en controleren van een trigger. In dit voorbeeld werken eerst Ga via de [Quick Start: Een data factory maken met behulp van de .NET SDK](quickstart-create-data-factory-dot-net.md). Voeg de volgende code aan de belangrijkste methode, die wordt gemaakt en een planningstrigger die wordt uitgevoerd om de 15 minuten wordt gestart. De trigger is gekoppeld aan een pijplijn met de naam **Adfv2QuickStartPipeline** die u maakt als onderdeel van de Quick Start.

Als u wilt maken en starten van een planningstrigger die elk kwartier wordt uitgevoerd, voeg de volgende code toe aan de belangrijkste methode:

```csharp
            // Create the trigger
            Console.WriteLine("Creating the trigger");

            // Set the start time to the current UTC time
            DateTime startTime = DateTime.UtcNow;

            // Specify values for the inputPath and outputPath parameters
            Dictionary<string, object> pipelineParameters = new Dictionary<string, object>();
            pipelineParameters.Add("inputPath", "adftutorial/input");
            pipelineParameters.Add("outputPath", "adftutorial/output");

            // Create a schedule trigger
            string triggerName = "MyTrigger";
            ScheduleTrigger myTrigger = new ScheduleTrigger()
            {
                Pipelines = new List<TriggerPipelineReference>()
                {
                    // Associate the Adfv2QuickStartPipeline pipeline with the trigger
                    new TriggerPipelineReference()
                    {
                        PipelineReference = new PipelineReference(pipelineName),
                        Parameters = pipelineParameters,
                    }
                },
                Recurrence = new ScheduleTriggerRecurrence()
                {
                    // Set the start time to the current UTC time and the end time to one hour after the start time
                    StartTime = startTime,
                    TimeZone = "UTC",
                    EndTime = startTime.AddHours(1),
                    Frequency = RecurrenceFrequency.Minute,
                    Interval = 15,
                }
            };

            // Now, create the trigger by invoking the CreateOrUpdate method
            TriggerResource triggerResource = new TriggerResource()
            {
                Properties = myTrigger
            };
            client.Triggers.CreateOrUpdate(resourceGroup, dataFactoryName, triggerName, triggerResource);

            // Start the trigger
            Console.WriteLine("Starting the trigger");
            client.Triggers.Start(resourceGroup, dataFactoryName, triggerName);
```

Voor het controleren van uitvoering van een trigger, voeg de volgende code vóór de laatste `Console.WriteLine` -instructie in het voorbeeld:

```csharp
            // Check that the trigger runs every 15 minutes
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

Voor het bewaken van de trigger wordt uitgevoerd en de pijplijn wordt uitgevoerd in Azure portal, Zie [pijplijnuitvoeringen controleren](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).


## <a name="python-sdk"></a>Python-SDK
Deze sectie leest u hoe u de Python SDK gebruikt om te maken, starten en controleren van een trigger. In dit voorbeeld werken eerst Ga via de [Quick Start: Een data factory maken met behulp van de Python-SDK](quickstart-create-data-factory-python.md). Vervolgens voegt u het volgende codeblok na het codeblok "de pijplijnuitvoering controleren" in het Python-script. Deze code maakt een planningstrigger die wordt uitgevoerd om de 15 minuten tussen de opgegeven begintijd en eindtijd ervan. Update de **start_time** variabele op de huidige UTC-tijd, en de **end_time** variabele naar één uur na de huidige UTC-tijd.

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

    # Start the trigger
    adf_client.triggers.start(rg_name, df_name, tr_name)
```

Voor het bewaken van de trigger wordt uitgevoerd en de pijplijn wordt uitgevoerd in Azure portal, Zie [pijplijnuitvoeringen controleren](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon
U kunt een Azure Resource Manager-sjabloon gebruiken om te maken van een trigger. Zie voor stapsgewijze instructies [een Azure data factory maken met behulp van Resource Manager-sjabloon](quickstart-create-data-factory-resource-manager-template.md).  

## <a name="pass-the-trigger-start-time-to-a-pipeline"></a>De begintijd van trigger doorgeven aan een pijplijn
Azure Data Factory versie 1 ondersteunt lezen of schrijven van gepartitioneerde gegevens met behulp van de systeemvariabelen: **SliceStart**, **SliceEnd**, **WindowStart**, en **WindowEnd**. In de huidige versie van Azure Data Factory, kunt u dit gedrag bewerkstelligen met behulp van een pijplijnparameter. De begintijd en de geplande tijd voor de trigger zijn ingesteld als de waarde voor de pijplijnparameter. In het volgende voorbeeld wordt de geplande tijd voor de trigger wordt doorgegeven als een waarde aan de pijplijn **scheduledRunTime** parameter:

```json
"parameters": {
    "scheduledRunTime": "@trigger().scheduledTime"
}
```    

Zie voor meer informatie de instructies in [lezen of schrijven gegevens gepartitioneerd](how-to-read-write-partitioned-data.md).

## <a name="json-schema"></a>JSON-schema
De volgende JSON-definitie wordt beschreven hoe u een planningstrigger maken met het schema en een terugkeerpatroon:

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Month>>,
        "interval": <<int>>,             // Optional, specifies how often to fire (default to 1)
        "startTime": <<datetime>>,
        "endTime": <<datetime - optional>>,
        "timeZone": "UTC"
        "schedule": {                    // Optional (advanced scheduling specifics)
          "hours": [<<0-23>>],
          "weekDays": : [<<Monday-Sunday>>],
          "minutes": [<<0-59>>],
          "monthDays": [<<1-31>>],
          "monthlyOccurrences": [
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
>  De eigenschap **parameters** is een verplichte eigenschap van het element **pijplijnen**. Als de pijplijn geen parameters aanneemt, dient u een lege JSON-definitie op te nemen voor de eigenschap **parameters**.


### <a name="schema-overview"></a>Schemaoverzicht
De volgende tabel bevat een overzicht van de belangrijkste schema-elementen die betrekking hebben op het terugkeerpatroon en het schema van een trigger:

| JSON-eigenschap | Description |
|:--- |:--- |
| **startTime** | Een datum/tijdwaarde. Voor eenvoudige schema's is de waarde **startTime** van toepassing op de eerste gebeurtenis. In complexe schema's begint de trigger niet eerder dan de opgegeven waarde voor **startTime**. |
| **endTime** | De einddatum en -tijd voor de trigger. De trigger wordt na de opgegeven einddatum en -tijd niet uitgevoerd. De waarde voor de eigenschap kan niet in het verleden liggen. Deze eigenschap is optioneel. |
| **timeZone** | De tijdzone. Momenteel wordt alleen de tijdzone UTC ondersteund. |
| **recurrence** | Een recurrence-object bepaalt de regels voor het terugkeerpatroon van de trigger. Het recurrence-object ondersteunt de elementen **frequency**, **interval**, **endTime**, **count** en **schedule**. Als een recurrence-object wordt gedefinieerd, is het element **frequency** vereist. De overige elementen van het recurrence-object zijn optioneel. |
| **frequency** | Hiermee geeft u de frequentie aan waarmee de trigger wordt uitgevoerd. De ondersteunde waarden omvatten 'minuut', 'uur', 'dag', 'week' en 'maand'. |
| **interval** | Een positief geheel getal dat het interval voor de waarde **frequency** aangeeft. Het bepaalt hoe vaak de trigger wordt uitgevoerd. Als **interval** bijvoorbeeld 3 is en **frequency** 'week', dan wordt de trigger elke 3 weken uitgevoerd. |
| **schedule** | Het terugkeerschema voor de trigger. Een trigger met een opgegeven waarde voor **frequency** wijzigt het terugkeerpatroon op basis van een terugkeerschema. De eigenschap **property** bevat wijzigingen voor het terugkeerpatroon en zijn gebaseerd op minuten, uren, weekdagen, maanddagen en weeknummer.


### <a name="schema-defaults-limits-and-examples"></a>Standaardschemawaarden, limieten en voorbeelden

| JSON-eigenschap | Type | Vereist | Standaardwaarde | Geldige waarden | Voorbeeld |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **startTime** | Reeks | Ja | Geen | Datums en tijden volgens ISO 8601 | `"startTime" : "2013-01-09T09:30:00-08:00"` |
| **recurrence** | Object | Ja | Geen | Recurrence-object | `"recurrence" : { "frequency" : "monthly", "interval" : 1 }` |
| **interval** | Aantal | Nee | 1 | 1 tot 1000 | `"interval":10` |
| **endTime** | Reeks | Ja | Geen | Een datum/tijdwaarde die een toekomstig tijdstip voorstelt. | `"endTime" : "2013-02-09T09:30:00-08:00"` |
| **schedule** | Object | Nee | Geen | Schedule-object | `"schedule" : { "minute" : [30], "hour" : [8,17] }` |

### <a name="starttime-property"></a>Eigenschap startTime
In de volgende tabel ziet u hoe de eigenschap **startTime** de uitvoering van een trigger bepaalt:

| startTime-waarde | Recurrence zonder schedule | Recurrence met schedule |
|:--- |:--- |:--- |
| Starttijd in het verleden | Berekent de eerstvolgende uitvoering na de starttijd en voert deze op dat moment uit.<br/><br/>Voert volgende uitvoeringen uit op basis van de tijd waarop de laatste uitvoering heeft plaatsgevonden.<br/><br/>Zie het voorbeeld onder deze tabel. | De trigger wordt _nooit vóór_ de opgegeven begintijd geactiveerd. De eerste uitvoering is gebaseerd op het schema dat wordt berekend op basis van de starttijd.<br/><br/>Volgende uitvoeringen worden op basis van het terugkeerschema uitgevoerd. |
| De starttijd ligt in de toekomst, of op dit moment | Uitvoering vindt eenmaal plaats op de opgegeven starttijd.<br/><br/>Voert volgende uitvoeringen uit op basis van de tijd waarop de laatste uitvoering heeft plaatsgevonden. | De trigger wordt _nooit vóór_ de opgegeven begintijd geactiveerd. De eerste uitvoering is gebaseerd op het schema dat wordt berekend op basis van de starttijd.<br/><br/>Volgende uitvoeringen worden op basis van het terugkeerschema uitgevoerd. |

We bekijken een voorbeeld van wat er gebeurt wanneer de startTime in het verleden ligt en er een terugkeerpatroon (recurrence), maar geen schema (schedule) is opgegeven. Neem aan dat de huidige tijd `2017-04-08 13:00` is, de starttijd `2017-04-07 14:00` en het terugkeerpatroon om de dag. (De waarde **recurrence** wordt gedefinieerd door de eigenschap **frequency** in te stellen op 'dag' en de eigenschap **interval** op 2.) U ziet dat de waarde **startTime** in het verleden ligt en plaatsvindt vóór de huidige tijd.

In deze omstandigheden vindt de eerste uitvoering plaats op `2017-04-09 at 14:00`. De scheduler-engine berekent uitvoeringen vanaf de startTime. Alle uitvoeringen in het verleden worden genegeerd. De engine gebruikt de eerstvolgende uitvoering die in de toekomst plaatsvindt. In dit scenario is de starttijd (startTime) `2017-04-07 at 2:00pm`, waardoor de volgende uitvoering twee dagen erna wordt gestart. Dit is op `2017-04-09 at 2:00pm`.

De eerste uitvoeringstijd is dezelfde, ook als **startTime** `2017-04-05 14:00` of `2017-04-01 14:00` is. Na de eerste uitvoering worden volgende uitvoeringen berekend met behulp van het schema. De volgende uitvoeringen vinden daarom plaats op `2017-04-11 at 2:00pm`, `2017-04-13 at 2:00pm`, `2017-04-15 at 2:00pm` enzovoort.

Als de uren of minuten niet in het schema voor een trigger zijn ingesteld, worden deze waarden van de eerste uitvoering als standaardwaarden gebruikt.

### <a name="schedule-property"></a>Eigenschap schedule
Enerzijds kan met een schema (schedule) het aantal uitvoeringen door een trigger worden beperkt. Als een trigger met de frequency 'maand' bijvoorbeeld een schedule-waarde heeft die alleen wordt uitgevoerd op dag 31, wordt de trigger alleen uitgevoerd in maanden die een 31e dag hebben.

Anderzijds kan een schedule het aantal uitvoeringen door een trigger ook uitbreiden. Bijvoorbeeld: een trigger met een geplande maandfrequentie voor uitvoering op de maanddagen 1 en 2, wordt uitgevoerd op de eerste en tweede dag van de maand, in plaats van eenmaal per maand.

Als meerdere **schedule**-elementen worden opgegeven, is de volgorde voor de evaluatie van de hoogste naar de laagste instelling voor de waarde schedule. De evaluatie begint met weeknummer, waarna maanddag, weekdag, uur en minuut volgen.

In de volgende tabel worden de **schedule**-elementen in detail beschreven:


| JSON-element | Description | Geldige waarden |
|:--- |:--- |:--- |
| **minutes** | Minuten van het uur waarop de trigger wordt uitgevoerd. | <ul><li>Geheel getal</li><li>Matrix van gehele getallen</li></ul>
| **hours** | Uren van de dag waarop de trigger wordt uitgevoerd. | <ul><li>Geheel getal</li><li>Matrix van gehele getallen</li></ul> |
| **weekDays** | Dagen van de week waarop de trigger wordt uitgevoerd. De waarde kan alleen worden opgegeven met een weekfrequentie. | <ul><li>Maandag, dinsdag, woensdag, donderdag, vrijdag, zaterdag, zondag</li><li>Array met dagwaarden (maximale grootte van de array is 7)</li><li>Dagwaarden zijn niet hoofdlettergevoelig</li></ul> |
| **monthlyOccurrences** | Dagen van de maand waarop de trigger wordt uitgevoerd. De waarde kan alleen worden opgegeven met een maandfrequentie. | <ul><li>Matrix van **monthlyOccurrence** objecten: `{ "day": day,  "occurrence": occurrence }`.</li><li>Het attribuut **day** is de dag van de week waarop de trigger wordt uitgevoerd. Zo betekent de eigenschap **monthlyOccurrences** met een waarde **day** van `{Sunday}` dat er elke zondag van de maand een uitvoering is. Het attribuut **day** is verplicht.</li><li>Het attribuut **occurrence** slaat op het uitvoeren van de trigger op de opgegeven dag, **day**, tijdens de maand. Zo betekent de eigenschap **monthlyOccurrences** met de waarden **day** en **occurrence** van `{Sunday, -1}` dat er elke laatste zondag van de maand een uitvoering is. Het attribuut **occurrence** is optioneel.</li></ul> |
| **monthDays** | Dagen van de maand waarop de trigger wordt uitgevoerd. De waarde kan alleen worden opgegeven met een maandfrequentie. | <ul><li>Alle waarden < = -1 en > =-31</li><li>Alle waarden > = -1 en < =-31</li><li>Array met waarden</li></ul> |


## <a name="examples-of-trigger-recurrence-schedules"></a>Voorbeelden van schema's voor uitvoeringen van triggers
Deze sectie bevat voorbeelden van terugkeerschema's en is gericht op het object **schedule** en de bijbehorende elementen.

In het voorbeeld wordt ervan uitgegaan dat de waarde **interval** 1 is en de waarde **frequency** correct is volgens de definitie van het schema. De waarde voor **frequency** kan bijvoorbeeld niet tegelijkertijd 'day' zijn én een wijziging 'monthDays' in het **schedule**-object hebben. Dergelijke beperkingen staan vermeld in de tabel in de vorige sectie.

| Voorbeeld | Description |
|:--- |:--- |
| `{"hours":[5]}` | Wordt elke dag om 5:00 uur uitgevoerd. |
| `{"minutes":[15], "hours":[5]}` | Wordt elke dag om 5:15 uur uitgevoerd. |
| `{"minutes":[15], "hours":[5,17]}` | Wordt elke dag om 05:15 en 17:15 uur uitgevoerd. |
| `{"minutes":[15,45], "hours":[5,17]}` | Wordt elke dag om 05:15, 5:45, 17:15 en 17:45 uur uitgevoerd. |
| `{"minutes":[0,15,30,45]}` | Wordt elke 15 minuten uitgevoerd. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Wordt elk uur uitgevoerd. Deze trigger wordt elk uur uitgevoerd. De minuten worden door de waarde **startTime** bepaald (indien opgegeven). Als er geen waarde is opgegeven, worden de minuten bepaald door de aanmaaktijd. Als de starttijd of aanmaaktijd (afhankelijk van wat van toepassing is) bijvoorbeeld 12:25 uur is, wordt de trigger uitgevoerd om 00:25, 01:25, 02:25, ..., en 23:25 uur.<br/><br/>Dit schema komt overeen met een trigger met **frequency** 'uur', **interval** 1 en geen waarde voor **schedule**.  Dit schema kan worden gebruikt met verschillende waarden voor **frequency** en **interval** om andere triggers te maken. Als bijvoorbeeld de waarde **frequency** 'maand' is, dan wordt het schema slechts eenmaal per maand uitgevoerd in plaats van elke dag (als **frequency** 'dag' is). |
| `{"minutes":[0]}` | Wordt elk uur op het hele uur uitgevoerd. Deze trigger wordt elk uur op het hele uur uitgevoerd, te beginnen om 00:00 uur en vervolgens om 1:00 uur, 2:00 uur enzovoort.<br/><br/>Dit schema is gelijkwaardig met een trigger met **frequency** 'uur' en **startTime** nul minuten, of met **schedule** zonder waarde en **frequency** 'dag'. Als de waarde voor **frequency** 'week' of 'maand' is, wordt het schema respectievelijk één dag per week of één dag per maand uitgevoerd. |
| `{"minutes":[15]}` | Wordt 15 minuten na elk uur uitgevoerd. Deze trigger wordt elke 15 minuten na het hele uur uitgevoerd, te beginnen om 00:15 uur, en vervolgens om 1:15 uur, 2:15 uur, met de laatste uitvoering om 23:15 uur. |
| `{"hours":[17], "weekDays":["saturday"]}` | Wordt elke week op zaterdag om 17:00 uur uitgevoerd. |
| `{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Wordt elke week op maandag, woensdag en vrijdag om 17:00 uur uitgevoerd. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Wordt elke week op maandag, woensdag en vrijdag om 17:15 en 17:45 uur uitgevoerd. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Wordt op weekdagen elke 15 minuten uitgevoerd. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Wordt op weekdagen elke 15 minuten tussen 9:00 en 16:45 uur uitgevoerd. |
| `{"weekDays":["tuesday", "thursday"]}` | Wordt op dinsdag en donderdag op de opgegeven begintijd uitgevoerd. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` | Wordt op dag 28 van elke maand om 6:00 uur uitgevoerd (bij een waarde voor **frequency** van 'maand'). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` | Wordt op de laatste dag van de maand om 6:00 uur uitgevoerd. Als u een trigger wilt uitvoeren op de laatste dag van een maand, gebruik dan -1 in plaats van dag 28, 29, 30 of 31. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | Wordt op de eerste en laatste dag van elke maand om 6:00 uur uitgevoerd. |
| `{monthDays":[1,14]}` | Wordt op de eerste en veertiende dag van elke maand op de opgegeven begintijd uitgevoerd. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Wordt op de eerste vrijdagdag van elke maand om 5:00 uur uitgevoerd. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Wordt op de eerste vrijdag van elke maand op de opgegeven begintijd uitgevoerd. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Wordt op de derde vrijdag vanaf het eind van de maand elke maand op de opgegeven begintijd uitgevoerd. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Wordt op de eerste en laatste vrijdagdag van elke maand om 5:15 uur uitgevoerd. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Wordt op de eerste en laatste vrijdag van elke maand op de opgegeven begintijd uitgevoerd. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Wordt op de vijfde vrijdag van elke maand op de opgegeven begintijd uitgevoerd. Als een maand geen vijfde vrijdag heeft, wordt de pijplijn niet uitgevoerd, omdat deze is ingesteld om alleen op de vijfde vrijdag te worden uitgevoerd. Als u de trigger op de laatste vrijdag van de maand wilt uitvoeren, kunt u voor **occurrence** de waarde -1 gebruiken in plaats van 5. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Wordt op de laatste vrijdag van de maand elke 15 minuten uitgevoerd. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | Wordt elke maand op de derde woensdag om 5:15, 5:45, 17:15 en 17:45 uur uitgevoerd. |


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over triggers [pijplijnen uitvoeren en triggers](concepts-pipeline-execution-triggers.md#triggers).
