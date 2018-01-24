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
ms.date: 01/23/2018
ms.author: shlo
ms.openlocfilehash: 51e2dddbe66ca372d89fc8efeb24bdab9fe6a442
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Een trigger die volgens een schema uitgevoerd van een pijplijn maken
Dit artikel bevat informatie over de trigger schema en de stappen voor het maken, start en controleren van een schema-trigger. Zie voor andere soorten triggers [Pipeline-uitvoering en triggers](concepts-pipeline-execution-triggers.md).

Wanneer u de trigger van een planning maakt, u een planning opgeven (de begindatum, terugkeerpatroon, einddatum enz.) voor de trigger en koppelen met een pipeline. Pijplijnen en triggers hebben een veel-op-veel-relatie. Meerdere triggers kunnen één pijplijn activeren. Eén trigger kan meerdere pijplijnen activeren.

> [!NOTE]
> In dit artikel is van toepassing op Azure Data Factory versie 2, momenteel in preview is. Als u Azure Data Factory versie 1, die algemeen beschikbaar (GA), Zie [aan de slag met Azure Data Factory versie 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

De volgende secties bieden stappen voor het maken van een schema-trigger op verschillende manieren. 

## <a name="data-factory-ui"></a>Gebruikersinterface van Data Factory
Kunt u een **planning trigger** plannen van een pijplijn periodiek wordt uitgevoerd (elk uur, dagelijks, enzovoort). 

> [!NOTE]
> Zie voor een volledige procedure voor het maken van een pijplijn en een schema worden geactiveerd, de trigger koppelen aan de pijplijn, en de uitgevoerd en de pijplijn bewaken [Snelstartgids: Maak een gegevensfactory met behulp van Data Factory-gebruikersinterface](quickstart-create-data-factory-portal.md).

1. Ga naar het tabblad **Bewerken**. 

    ![Naar het tabblad Bewerken gaan](./media/how-to-create-schedule-trigger/switch-edit-tab.png)
1. Klik in het menu op **Activeren** en klik op **Nieuw/bewerken**. 

    ![Menu Nieuwe trigger](./media/how-to-create-schedule-trigger/new-trigger-menu.png)
2. Klik op de pagina **Triggers toevoegen** op **Trigger kiezen...** , en klik op **Nieuw**. 

    ![Triggers toevoegen - nieuwe trigger](./media/how-to-create-schedule-trigger/add-trigger-new-button.png)
3. In de **nieuwe Trigger** pagina, voert u de volgende stappen uit: 

    1. Bevestig dat **planning** is geselecteerd voor **Type**. 
    2. Geef de begindatum/tijd van de trigger voor **Start datum (UTC)**. Dit standaard ingesteld op de huidige datum/tijd. 
    3. Geef **terugkeerpatroon** voor de trigger. Selecteer een van de waarden in de vervolgkeuzelijst (elke minuut, uur, dagelijks, wekelijks, en maandelijks). De vermenigvuldigingsfactor invoeren in het tekstvak. Bijvoorbeeld, als u wilt dat de trigger wordt eenmaal uitvoeren voor elke 15 minuten, u **elke minuut**, en voer **15** in het tekstvak. 
    4. Voor de **End** veld, als u niet wilt opgeven van een einddatum/tijd voor de trigger, selecteer **Nee End**. Als u een end datum / tijd, schakelt u **op datum**, geef de einddatum/tijd en klikt u op **toepassen**. Er zijn kosten gekoppeld aan elke uitvoering van de pijplijn. Als u testen wilt, kunt u om ervoor te zorgen dat de pijplijn wordt geactiveerd slechts een paar keer. Zorg er echter wel voor dat er voldoende tijd is om de pijplijn uit te voeren tussen de publicatietijd en de eindtijd. De trigger gaat pas van kracht nadat u de oplossing hebt gepubliceerd in Data Factory, niet wanneer u de trigger opslaat in de UI.

        ![Triggerinstellingen](./media/how-to-create-schedule-trigger/trigger-settings.png)
4. In de **nieuwe Trigger** venster, Controleer de **geactiveerd** optie en klik op **volgende**. Dit selectievakje kunt u later de trigger deactiveren. 

    ![Triggerinstellingen - knop Volgende](./media/how-to-create-schedule-trigger/trigger-settings-next.png)
5. Lees op de pagina **Nieuwe trigger** de waarschuwing en klik op **Voltooien**.

    ![Triggerinstellingen - knop Voltooien](./media/how-to-create-schedule-trigger/new-trigger-finish.png)
6. Klik op **Publiceren** om wijzigingen te publiceren in Data Factory. Totdat u de wijzigingen naar Data Factory publiceert, start de trigger niet activering van de pijplijn wordt uitgevoerd. 

    ![De knop Publiceren](./media/how-to-create-schedule-trigger/publish-2.png)
8. Ga naar het tabblad **Controleren** aan de linkerkant. Klik op **Vernieuwen** om de lijst te vernieuwen. U ziet in dat de pijplijn wordt uitgevoerd door de geplande gebeurtenis geactiveerd. Bekijk de waarden in de kolom **Geactiveerd door**. Als u **Trigger nu** optie, ziet u de handmatige trigger uitvoeren in de lijst. 

    ![Geactiveerde uitvoeringen controleren](./media/how-to-create-schedule-trigger/monitor-triggered-runs.png)
9. Klik op de pijl-omlaag naast **Pijplijnuitvoeringen** om naar de weergave **Triggeruitvoeringen** te gaan. 

    ![Triggeruitvoeringen controleren](./media/how-to-create-schedule-trigger/monitor-trigger-runs.png)

## <a name="azure-powershell"></a>Azure PowerShell
Deze sectie wordt beschreven hoe u Azure PowerShell gebruiken voor het maken, start en controleren van een schema-trigger. In dit voorbeeld werken eerst Ga via de [Snelstartgids: Maak een gegevensfactory met Azure PowerShell](quickstart-create-data-factory-powershell.md). Voeg de volgende code aan de belangrijkste methode, die zijn gemaakt en gestart van een trigger planning die wordt uitgevoerd om de 15 minuten. De trigger wordt gekoppeld aan een pijplijn met de naam **Adfv2QuickStartPipeline** die u maakt als onderdeel van de Quick Start.

1. Maak een JSON-bestand met de naam **MyTrigger.json** in de map C:\ADFv2QuickStartPSH\ met de volgende inhoud:

    > [!IMPORTANT]
    > Voordat u het JSON-bestand opslaan, stel de waarde van de **startTime** element in de huidige UTC-tijd. Stel de waarde van de **endTime** element aan één uur na de huidige UTC-tijd.

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
    - De **type** element van de trigger is ingesteld op 'ScheduleTrigger'.
    - De **frequentie** element is ingesteld op 'Minuut' en de **interval** element is ingesteld op 15. De trigger voert de pijplijn daarom om de 15 minuten tussen de begin- en eindtijden.
    - De **endTime** -element is één uur nadat de waarde van de **startTime** element. De trigger voert de pijplijn daarom 15 minuten, 30 minuten en 45 minuten na de begintijd. Vergeet niet bijwerken van de begintijd voor de huidige UTC-tijd en de eindtijd aan één uur na de begintijd. 
    - De trigger is gekoppeld aan de **Adfv2QuickStartPipeline** pijplijn. Als u wilt meerdere pijplijnen koppelen aan een trigger, meer toevoegen **pipelineReference** secties.
    - De pijplijn in de Quick Start heeft twee **parameters** waarden: **inputPath** en **outputPath**. U kunt daarom waarden voor deze parameters doorgeven van de trigger.

2. Geen trigger maken met behulp van de **Set AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```

3. Controleer of de status van de trigger **gestopt** met behulp van de **Get-AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

4. De trigger wordt gestart via de **Start AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

5. Controleer of de status van de trigger **gestart** met behulp van de **Get-AzureRmDataFactoryV2Trigger** cmdlet:

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

6.  Get-de trigger wordt uitgevoerd in Azure PowerShell met behulp van de **Get-AzureRmDataFactoryV2TriggerRun** cmdlet. Als u de informatie over de trigger wordt uitgevoerd, moet u de volgende opdracht periodiek uitvoeren. Update de **TriggerRunStartedAfter** en **TriggerRunStartedBefore** in overeenstemming met de waarden in de definitie van de trigger:

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
    
    Voor het bewaken van de trigger wordt uitgevoerd en de pijplijn wordt uitgevoerd in de Azure portal, Zie [pijplijn bewaken wordt uitgevoerd](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).


## <a name="net-sdk"></a>.NET SDK
Deze sectie wordt beschreven hoe u met de .NET SDK maakt, starten en controleren van een trigger. In dit voorbeeld werken eerst Ga via de [Snelstartgids: Maak een gegevensfactory met behulp van de .NET SDK](quickstart-create-data-factory-dot-net.md). Voeg de volgende code aan de belangrijkste methode, die zijn gemaakt en gestart van een trigger planning die wordt uitgevoerd om de 15 minuten. De trigger wordt gekoppeld aan een pijplijn met de naam **Adfv2QuickStartPipeline** die u maakt als onderdeel van de Quick Start.

Als u wilt maken en starten van een trigger planning die wordt uitgevoerd om de 15 minuten, voeg de volgende code aan de belangrijkste methode:

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

Voor het bewaken van een trigger worden uitgevoerd, voeg de volgende code voordat de laatste `Console.WriteLine` instructie in de steekproef:

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

Voor het bewaken van de trigger wordt uitgevoerd en de pijplijn wordt uitgevoerd in de Azure portal, Zie [pijplijn bewaken wordt uitgevoerd](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).


## <a name="python-sdk"></a>Python-SDK
Deze sectie wordt beschreven hoe u met de SDK voor Python maken, te starten en te bewaken van een trigger. In dit voorbeeld werken eerst Ga via de [Snelstartgids: Maak een gegevensfactory met behulp van de SDK voor Python](quickstart-create-data-factory-python.md). Vervolgens voegt u het volgende codeblok na het codeblok 'bewaken de pijplijn uitvoeren' in het Python-script. Deze code maakt een trigger planning die wordt uitgevoerd om de 15 minuten tussen de opgegeven start- en eindtijd. Update de **start_time** variabele in de huidige UTC-tijd en de **end_time** variabele aan één uur na de huidige UTC-tijd.

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

Voor het bewaken van de trigger wordt uitgevoerd en de pijplijn wordt uitgevoerd in de Azure portal, Zie [pijplijn bewaken wordt uitgevoerd](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon
Een Azure Resource Manager-sjabloon kunt u geen trigger maken. Zie voor stapsgewijze instructies [maken van een Azure data factory met Resource Manager-sjabloon](quickstart-create-data-factory-resource-manager-template.md).  

## <a name="pass-the-trigger-start-time-to-a-pipeline"></a>De begintijd van de trigger doorgeven aan een pijplijn
Azure Data Factory versie 1 ondersteunt lezen of schrijven van gepartitioneerde gegevens met behulp van de systeemvariabelen: **SliceStart**, **SliceEnd**, **WindowStart**, en **WindowEnd**. U kunt dit gedrag bereiken met behulp van de parameter van een pijplijn in Azure Data Factory versie 2. De begintijd en de geplande tijd voor de trigger worden ingesteld als de waarde voor de pipeline-parameter. In het volgende voorbeeld wordt de geplande tijd voor de trigger is doorgegeven als een waarde voor de pijplijn **scheduledRunTime** parameter:

```json
"parameters": {
    "scheduledRunTime": "@trigger().scheduledTime"
}
```    

Voor meer informatie, Zie de instructies in [het lezen of schrijven gepartitioneerd gegevens](how-to-read-write-partitioned-data.md).

## <a name="json-schema"></a>JSON-schema
De volgende JSON-definitie laat zien hoe u een trigger schema maken met plannen en het terugkeerpatroon:

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
>  De **parameters** eigenschap is een verplichte eigenschap van de **pijplijnen** element. Als uw pijplijn niet alle parameters, moet u een leeg JSON-definitie voor opnemen de **parameters** eigenschap.


### <a name="schema-overview"></a>Schema-overzicht
De volgende tabel bevat een overzicht van de belangrijkste schema-elementen die gerelateerd aan terugkeerpatroon en planning van een trigger zijn:

| JSON-eigenschap | Beschrijving |
|:--- |:--- |
| **startTime** | Een datum / tijd-waarde. Voor eenvoudige schema's van de waarde van de **startTime** eigenschap is van toepassing op het eerste exemplaar. Voor complexe schema's, begint de trigger niet eerder dan de opgegeven **startTime** waarde. |
| **endTime** | De einddatum en -tijd voor de trigger. De trigger uitvoeren niet nadat de opgegeven einddatum en -tijd. De waarde voor de eigenschap kan niet in het verleden. Deze eigenschap is optioneel. |
| **timeZone** | De tijdzone. Alleen de UTC-tijdzone wordt momenteel ondersteund. |
| **terugkeerpatroon** | Een object van het terugkeerpatroon dat Hiermee geeft u het terugkeerpatroon regels voor de trigger. Het terugkeerpatroon object ondersteunt de **frequentie**, **interva**l, **endTime**, **aantal**, en **planning**elementen. Als een terugkeerpatroon-object is gedefinieerd, de **frequentie** element is vereist. Andere elementen van het object van het terugkeerpatroon zijn optioneel. |
| **frequentie** | De eenheid van de frequentie waarmee de trigger terugkeert. De ondersteunde waarden zijn 'minute', 'uur', 'day', 'week' en 'month'. |
| **interval** | Een positief geheel getal dat geeft het interval voor de **frequentie** waarde waarmee wordt bepaald hoe vaak de trigger wordt uitgevoerd. Bijvoorbeeld, als de **interval** 3 en de **frequentie** 'week', is de trigger wordt herhaald om de drie weken. |
| **planning** | De terugkeerplanning voor de trigger. Een trigger met een opgegeven **frequentie** waarde wijzigt het terugkeerpatroon op basis van een terugkerende planning. De **planning** eigenschap bevat wijzigingen voor het terugkeerpatroon die zijn gebaseerd op minuten, uren, weekdagen, dagen van de maand en weeknummer.


### <a name="schema-defaults-limits-and-examples"></a>Schema standaardwaarden, limieten en voorbeelden

| JSON-eigenschap | Type | Vereist | Standaardwaarde | Geldige waarden | Voorbeeld |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **startTime** | Tekenreeks | Ja | Geen | Datums en tijden volgens ISO 8601 | `"startTime" : "2013-01-09T09:30:00-08:00"` |
| **terugkeerpatroon** | Object | Ja | Geen | Recurrence-object | `"recurrence" : { "frequency" : "monthly", "interval" : 1 }` |
| **interval** | Aantal | Nee | 1 | 1-1000 | `"interval":10` |
| **endTime** | Tekenreeks | Ja | None | Een datum / tijd-waarde die een tijd in de toekomst vertegenwoordigt. | `"endTime" : "2013-02-09T09:30:00-08:00"` |
| **planning** | Object | Nee | Geen | Schedule-object | `"schedule" : { "minute" : [30], "hour" : [8,17] }` |

### <a name="starttime-property"></a>eigenschap startTime
De volgende tabel ziet u hoe de **startTime** eigenschap bepaalt een trigger worden uitgevoerd:

| startTime-waarde | Recurrence zonder schedule | Recurrence met schedule |
|:--- |:--- |:--- |
| Starttijd in het verleden | Berekent de toekomstige uitvoeringstijd na de begintijd en wordt uitgevoerd op dat moment.<br/><br/>Wordt uitgevoerd bij een volgende uitvoering op basis van het berekenen van de laatste uitvoeringstijd.<br/><br/>Zie het voorbeeld onder deze tabel. | De trigger wordt gestart _niet sooner dan_ de opgegeven begintijd. Het eerste exemplaar is gebaseerd op de planning die wordt berekend op basis van de begintijd.<br/><br/>Wordt uitgevoerd bij een volgende uitvoering op basis van de terugkerende planning. |
| De starttijd ligt in de toekomst, of op dit moment | Voert eenmaal op de opgegeven begintijd.<br/><br/>Wordt uitgevoerd bij een volgende uitvoering op basis van het berekenen van de laatste uitvoeringstijd. | De trigger wordt gestart _niet sneller_ dan de opgegeven begintijd. Het eerste exemplaar is gebaseerd op de planning die wordt berekend op basis van de begintijd.<br/><br/>Wordt uitgevoerd bij een volgende uitvoering op basis van de terugkerende planning. |

We bekijken een voorbeeld van wat er gebeurt wanneer de begintijd in het verleden heeft een terugkeer, maar zonder schema is. Wordt ervan uitgegaan dat de huidige tijd `2017-04-08 13:00`, de starttijd is `2017-04-07 14:00`, en het terugkeerpatroon wordt elke twee dagen. (De **terugkeerpatroon** waarde wordt gedefinieerd door in te stellen de **frequentie** eigenschap om 'day' en de **interval** eigenschap in op 2.) U ziet dat de **startTime** waarde in het verleden en vindt plaats voordat de huidige tijd.

In deze omstandigheden vindt de eerste uitvoering plaats op `2017-04-09 at 14:00`. De scheduler-engine berekent uitvoeringen vanaf de startTime. Alle uitvoeringen in het verleden worden genegeerd. De engine gebruikt de eerstvolgende uitvoering die in de toekomst plaatsvindt. In dit scenario wordt de begintijd is `2017-04-07 at 2:00pm`, waardoor het volgende exemplaar twee dagen na de tijd die is `2017-04-09 at 2:00pm`.

De eerste uitvoeringstijd is de dezelfde zelfs als de **startTime** waarde is `2017-04-05 14:00` of `2017-04-01 14:00`. Na de eerste uitvoering worden de volgende uitvoering berekend met behulp van de planning. De volgende uitvoering zijn daarom op `2017-04-11 at 2:00pm`, klikt u vervolgens `2017-04-13 at 2:00pm`, klikt u vervolgens `2017-04-15 at 2:00pm`, enzovoort.

Ten slotte, als de uren of minuten zijn niet ingesteld in de planning voor een trigger, de uren of minuten van de eerste uitvoering gebruikt als de standaardwaarden.

### <a name="schedule-property"></a>schema-eigenschap
Enerzijds kunt het gebruik van een planning beperken het aantal uitvoeringen trigger. Bijvoorbeeld, als een trigger met een maandelijkse frequentie is gepland laten uitvoeren op dag 31, kan de trigger alleen in deze maanden die een 31e dag hebben.

Anderzijds kan een schedule het aantal uitvoeringen door een trigger ook uitbreiden. Bijvoorbeeld een trigger met een maandelijkse frequentie die is gepland op dagen van de maand 1 en 2, uitgevoerd op de 1e en 2e dagen van de maand, in plaats van één keer per maand.

Als er meerdere **planning** elementen zijn opgegeven, is de volgorde van de evaluatie van de grootste naar de kleinste schema-instelling. De evaluatie begint met week nummer en vervolgens de dag maand, dag, uur, en ten slotte minuut.

De volgende tabel beschrijft de **planning** elementen in detail:


| JSON-element | Beschrijving | Geldige waarden |
|:--- |:--- |:--- |
| **minuten** | Minuten van het uur waarop de trigger wordt uitgevoerd. | <ul><li>Geheel getal</li><li>Matrix van gehele getallen</li></ul>
| **uren** | Uren van de dag waarop de trigger wordt uitgevoerd. | <ul><li>Geheel getal</li><li>Matrix van gehele getallen</li></ul> |
| **Weekdagen** | Dagen van de week waarop de trigger wordt uitgevoerd. De waarde kan worden opgegeven met een wekelijkse frequentie alleen. | <ul><li>Maandag, dinsdag, woensdag, donderdag, vrijdag, zaterdag, zondag</li><li>Matrix van Dagwaarden (Maximale matrixgrootte is 7)</li><li>Dag-waarden zijn niet hoofdlettergevoelig</li></ul> |
| **monthlyOccurrences** | Dagen van de maand op waarop de trigger wordt uitgevoerd. De waarde kan worden opgegeven met alleen een maandelijkse frequentie. | <ul><li>Matrix van **monthlyOccurence** objecten: `{ "day": day,  "occurrence": occurence }`.</li><li>De **dag** kenmerk is de dag van de week waarop de trigger wordt uitgevoerd. Bijvoorbeeld, een **monthlyOccurrences** eigenschap met een **dag** waarde van `{Sunday}` betekent dat elke zondag van de maand. De **dag** kenmerk is vereist.</li><li>De **exemplaar** kenmerk is het exemplaar van de opgegeven **dag** tijdens de maand. Bijvoorbeeld, een **monthlyOccurrences** eigenschap met de **dag** en **exemplaar** waarden van `{Sunday, -1}` betekent dat de laatste zondag van de maand. De **exemplaar** kenmerk is optioneel.</li></ul> |
| **maanddagen** | Dag van de maand op waarop de trigger wordt uitgevoerd. De waarde kan worden opgegeven met alleen een maandelijkse frequentie. | <ul><li>Alle waarden < = -1 en > =-31</li><li>Alle waarden > = -1 en < =-31</li><li>Matrix van waarden</li></ul> |


## <a name="examples-of-trigger-recurrence-schedules"></a>Voorbeelden van trigger terugkerende planning
Deze sectie bevat voorbeelden van terugkerende schema's en richt zich op de **planning** object en de bijbehorende elementen.

De voorbeelden wordt ervan uitgegaan dat de **interval** waarde is 1 en dat de **frequentie** waarde volgens de schemadefinitie juist is. Bijvoorbeeld, er geen een **frequentie** waarde van 'day' en hebben een 'maanddagen' wijziging in de **planning** object. Beperkingen zoals deze worden vermeld in de tabel in de vorige sectie.

| Voorbeeld | Beschrijving |
|:--- |:--- |
| `{"hours":[5]}` | Op elke dag 5:00 uur uitgevoerd. |
| `{"minutes":[15], "hours":[5]}` | Op elke dag 5:15 uur uitgevoerd. |
| `{"minutes":[15], "hours":[5,17]}` | Op 5:15 uur en 17:15:00 uur elke dag worden uitgevoerd. |
| `{"minutes":[15,45], "hours":[5,17]}` | Uitvoeren op 5:15 uur, 5:45 uur 17:15:00 uur en 17:45 uur elke dag. |
| `{"minutes":[0,15,30,45]}` | Elke 15 minuten wordt uitgevoerd. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Elk uur uitgevoerd. Deze trigger wordt elk uur uitgevoerd. De minuten worden beheerd door de **startTime** waarde, wanneer een waarde wordt opgegeven. Als er een waarde niet is opgegeven, wordt de minuten worden beheerd door de aanmaaktijd. Bijvoorbeeld, als de begintijd of Aanmaaktijd (afhankelijk van wat van toepassing) 12:25 uur, de trigger wordt uitgevoerd op 00:25, 01:25 02:25,..., en 23:25.<br/><br/>Dit schema komt overeen met een trigger met een **frequentie** waarde van 'uur', een **interval** waarde van 1, maar er is geen **planning**.  Dit schema kan worden gebruikt met andere **frequentie** en **interval** waarden voor het maken van andere triggers. Bijvoorbeeld, wanneer de **frequentie** waarde is "maand", de planning wordt uitgevoerd, maar één keer per maand, in plaats van elke dag waarop de **frequentie** waarde is 'dag'. |
| `{"minutes":[0]}` | Elk uur worden uitgevoerd op het uur. Deze trigger elk uur wordt uitgevoerd op het uur vanaf 12:00 uur, 1:00 uur, 2:00 uur, enzovoort.<br/><br/>Dit schema is gelijk aan een trigger met een **frequentie** waarde van 'uur' en een **startTime** waarde van nul minuten of op Nee **planning** maar er is een **frequentie**  waarde van 'day'. Als de **frequentie** waarde is 'week' of 'maand' één dag een week of maand één dag van de planning wordt uitgevoerd alleen, respectievelijk. |
| `{"minutes":[15]}` | Op 15 minuten na elk uur uitgevoerd. Deze trigger elk uur wordt uitgevoerd op 15 minuten de vanaf 00:15 uur, 01:15:00, 2:15 uur, minuut en dergelijke, en eindigt bij 23:15 uur. |
| `{"hours":[17], "weekDays":["saturday"]}` | Voer op 17:00 uur op zaterdag elke week. |
| `{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Voer op 17:00 uur op maandag, woensdag en vrijdag elke week. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Voer op 17:15:00 uur en 17:45 uur op maandag, woensdag en vrijdag elke week. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Om de 15 minuten worden uitgevoerd op weekdagen. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Om de 15 minuten worden uitgevoerd op weekdagen tussen 9:00 uur en 4:45 uur. |
| `{"weekDays":["tuesday", "thursday"]}` | Voer op dinsdag en donderdag op de opgegeven begintijd. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` | Op 6:00 uur op de 28 dag van elke maand worden uitgevoerd (ervan uitgaande dat een **frequentie** waarde van 'maand'). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` | 6:00 uur op de laatste dag van de maand uitvoeren. Gebruik een trigger op de laatste dag van een maand uitgevoerd, -1 in plaats van de dag 28, 29, 30 of 31. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | 6:00 uur op de eerste en laatste dag van elke maand uitvoeren. |
| `{monthDays":[1,14]}` | Voer op de eerste en 14 dag van elke maand in de opgegeven begintijd. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Voer op de eerste vrijdag van elke maand in 5:00 uur. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Voer op de eerste vrijdag van elke maand in de opgegeven begintijd. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Voer op de derde vrijdag vanaf het einde van de maand, elke maand, om de opgegeven begintijd. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Op de eerste en laatste vrijdag van elke maand 5:15 uur uitvoeren. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Op de eerste en laatste vrijdag van elke maand uitvoeren op de opgegeven begintijd. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Voer op de vijfde vrijdag van elke maand in de opgegeven begintijd. Wanneer er geen vijfde vrijdag in een maand, de pijplijn niet wordt uitgevoerd, omdat deze is gepland voor uitvoering alleen op de vijfde vrijdag. De trigger wordt uitgevoerd op de laatste vrijdag van de maand plaatsvinden Overweeg het gebruik van -1 in plaats van 5 voor de **exemplaar** waarde. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Om de 15 minuten worden uitgevoerd op de laatste vrijdag van de maand. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | Uitvoeren op 5:15 uur, 5:45 uur 17:15:00 uur en 17:45 uur op de derde woensdag van elke maand. |


## <a name="next-steps"></a>Volgende stappen
Zie voor gedetailleerde informatie over triggers [Pipeline-uitvoering en triggers](concepts-pipeline-execution-triggers.md#triggers).
