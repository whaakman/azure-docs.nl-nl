---
title: Pijplijnen uitvoeren en triggers in Azure Data Factory | Microsoft Docs
description: Dit artikel bevat informatie over het uitvoeren van een pijplijn in Azure Data Factory, op aanvraag of door een trigger te maken.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/10/2017
ms.author: shlo
ms.openlocfilehash: c319979cce23da69965d4fbab037919461f67b3a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="pipeline-execution-and-triggers-in-azure-data-factory"></a>Pijplijnen uitvoeren en triggers in Azure Data Factory 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-scheduling-and-execution.md)
> * [Versie 2 - Preview](concepts-pipeline-execution-triggers.md)

Een **pijplijnuitvoering** is een term in versie 2 van Azure Data Factory waarmee een exemplaar van de uitvoering van een pijplijn wordt aangeduid. Stel dat u een pijplijn hebt die wordt uitgevoerd om 8 uur, 9 uur en 10 uur. In dit geval wordt de pijplijn drie keer afzonderlijk uitgevoerd (pijplijnuitvoeringen). Elke pijplijnuitvoering heeft een unieke pijplijn-ID. Dit is een unieke GUID die de betreffende specifieke pijplijnuitvoering definieert. Pijplijnuitvoeringen worden doorgaans geïnstantieerd doordat argumenten worden doorgegeven aan gedefinieerde parameters in de pijplijnen. Er zijn twee manieren om een pijplijn uit te voeren: **handmatig** of via een **trigger**. In dit artikel bevat informatie over beide manieren om een pijplijn uit te voeren. 

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt die algemeen beschikbaar is (GA), raadpleegt u [plannen en uitvoeren in Data Factory V1](v1/data-factory-scheduling-and-execution.md).

## <a name="run-pipeline-on-demand"></a>Pijplijn op aanvraag uitvoeren
Bij deze methode moet u de pijplijn handmatig uitvoeren. Dit wordt ook beschouwd als de uitvoering op aanvraag van een pijplijn. 

Stel dat u een pijplijn met de naam **copyPipeline** hebt die u wilt uitvoeren. De pijplijn is een eenvoudige pijplijn met één activiteit, waarmee items worden gekopieerd uit een bronmap in Azure Blob Storage naar een doelmap in dezelfde opslagplaats. Hier volgt de definitie van de voorbeeldpijplijn: 

```json
{
  "name": "copyPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
        "name": "CopyBlobtoBlob",
        "inputs": [
          {
            "referenceName": "sourceBlobDataset",
            "type": "DatasetReference"
          }
        ],
        "outputs": [
          {
            "referenceName": "sinkBlobDataset",
            "type": "DatasetReference"
          }
        ]
      }
    ],
    "parameters": {
      "sourceBlobContainer": {
        "type": "String"
      },
      "sinkBlobContainer": {
        "type": "String"
      }
    }
  }
}

```
De pijplijn heeft twee parameters: sourceBlobContainer en sinkBlobContainer, zoals weergegeven in de JSON-definitie. Tijdens runtime geeft u waarden door aan deze parameters. 

Als u de pijplijn handmatig wilt uitvoeren, kunt u een van de volgende manieren gebruiken: .NET, PowerShell, REST en Python. 

### <a name="rest-api"></a>REST API
Hier volgt een voorbeeldopdracht in REST:  

```
POST
https://management.azure.com/subscriptions/mySubId/resourceGroups/myResourceGroup/providers/Microsoft.DataFactory/factories/myDataFactory/pipelines/copyPipeline/createRun?api-version=2017-03-01-preview
```
Zie [Snelstartgids: een data factory maken met REST API](quickstart-create-data-factory-rest-api.md) voor een compleet voorbeeld.

### <a name="powershell"></a>PowerShell
Hier volgt een voorbeeldopdracht in PowerShell: 

```powershell
Invoke-AzureRmDataFactoryV2Pipeline -DataFactory $df -PipelineName "Adfv2QuickStartPipeline" -ParameterFile .\PipelineParameters.json
```

U kunt parameters doorgeven in de hoofdtekst van de nettolading van de aanvraag. In .NET, Powershell en Python kunt u waarden doorgeven in een woordenlijst die is doorgegeven als een argument voor de aanroep.

```json
{
  “sourceBlobContainer”: “MySourceFolder”,
  “sinkBlobCountainer”: “MySinkFolder”
}
```

De nettolading van de reactie is een unieke ID van de pijplijnuitvoering:

```json
{
  "runId": "0448d45a-a0bd-23f3-90a5-bfeea9264aed"
}
```


Zie [Snelstartgids: een data factory maken met PowerShell](quickstart-create-data-factory-powershell.md) voor een compleet voorbeeld.

### <a name="net"></a>.NET 
Hier volgt een voorbeeldaanroep in .NET: 

```csharp
client.Pipelines.CreateRunWithHttpMessagesAsync(resourceGroup, dataFactoryName, pipelineName, parameters)
```

Zie [Snelstartgids: een data factory maken met .NET](quickstart-create-data-factory-dot-net.md) voor een compleet voorbeeld.

> [!NOTE]
> U kunt de .NET API gebruiken om Data Factory-pijplijnen vanuit Azure Functions, uw eigen webservices, enzovoort aan te roepen.

## <a name="triggers"></a>Triggers
Triggers zijn de tweede manier om een pijplijnuitvoering te activeren. Triggers zijn verwerkingseenheden die bepalen wanneer een pijplijnuitvoering moet worden gestart. Data Factory ondersteunt momenteel een trigger die een pijplijn volgens een wandklokschema aanroept. Dit heet een **scheduler-trigger**. Op dit moment ondersteunt Data Factory geen triggers op basis van gebeurtenissen, zoals een trigger van een pijplijnuitvoering wanneer een bestand wordt ontvangen.

Pijplijnen en triggers hebben een 'n-m-relatie'. Meerdere triggers kunnen één pijplijn starten en één trigger kan meerdere pijplijnen starten. In de volgende JSON-definitie van een trigger verwijst de eigenschap **pijplijnen** naar een lijst van de pijplijnen die worden geactiveerd door de betreffende trigger, en naar waarden voor pijplijnparameters.

### <a name="basic-trigger-definition"></a>Basisdefinitie voor triggers: 
```json
    "properties": {
        "name": "MyTrigger",
        "type": "<type of trigger>",
        "typeProperties": {
            …
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
```

## <a name="scheduler-trigger"></a>Scheduler-trigger
Scheduler-trigger voert pijplijnen uit volgens een wandklokschema. Deze trigger ondersteunt periodieke en geavanceerde kalenderopties (wekelijks, maandag om 17:00 uur en donderdag om 21:00). Deze is flexibel doordat hij losstaat van het gegevenssetpatroon en er geen onderscheid wordt gemaakt tussen gegevens uit tijdreeksen en gegevens die niet uit tijdreeksen afkomstig zijn.

### <a name="scheduler-trigger-json-definition"></a>JSON-definitie van een scheduler-trigger
Wanneer u een scheduler-trigger maakt, kunt u de planning en een terugkeerpatroon opgeven met JSON, zoals weergegeven in het voorbeeld in deze sectie. 

Als u wilt dat de scheduler-trigger een pijplijnuitvoering activeert, moet u een verwijzing naar de betreffende pijplijn opnemen in de definitie van de trigger. Pijplijnen en triggers hebben een 'n-m-relatie'. Meerdere triggers kunnen één pijplijn activeren. Eén trigger kan meerdere pijplijnen activeren.

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Year>>,
        "interval": <<int>>,             // optional, how often to fire (default to 1)
        "startTime": <<datetime>>,
        "endTime": <<datetime>>,
        "timeZone": <<default UTC>>
        "schedule": {                    // optional (advanced scheduling specifics)
          "hours": [<<0-24>>],
          "weekDays": ": [<<Monday-Sunday>>],
          "minutes": [<<0-60>>],
          "monthDays": [<<1-31>>],
          "monthlyOccurences": [
               {
                    "day": <<Monday-Sunday>>,
                    "occurrence": <<1-5>>
               }
           ] 
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
                    "<parameter 2 Name> : "<parameter 2 Value>"
                }
           }
      ]
  }
}
```

### <a name="overview-scheduler-trigger-schema"></a>Overzicht: scheduler-triggerschema
De volgende tabel bevat een overzicht van de belangrijkste elementen die betrekking hebben op het terugkeerpatroon en de planning in een trigger:

JSON-eigenschap |     Beschrijving
------------- | -------------
startTime | startTime is een datum en tijd. In eenvoudige planningen is de startTime de eerste gebeurtenis. In complexe planningen begint de trigger niet eerder dan de startTime.
recurrence | Het object recurrence bepaalt de regels voor het terugkeerpatroon van de trigger. Het object recurrence ondersteunt de volgende elementen: frequency, interval, endTime, count en schedule. Als de recurrence is gedefinieerd, is het element frequency vereist. De andere elementen van de recurrence zijn optioneel.
frequency | Hiermee geeft u de frequentie-eenheid aan waarmee de trigger wordt uitgevoerd. Ondersteunde waarden zijn: `minute`, `hour`, `day`, `week` of `month`.
interval | Het interval is een positief geheel getal. Het geeft het interval aan voor de frequentie waarmee wordt bepaald hoe vaak de trigger wordt uitgevoerd. Als het interval bijvoorbeeld 3 is en de frequency 'week', wordt de trigger elke 3 weken uitgevoerd.
endTime | Hiermee geeft u de einddatum en -tijd voor de trigger op. De trigger wordt niet meer uitgevoerd na deze tijd. U kunt geen endTime die in het verleden ligt opgeven.
schedule | Een trigger met een opgegeven frequency wijzigt de recurrence op basis van een terugkerende schedule. Een schedule bevat wijzigingen op basis van de minuten, uren, weekdagen, dagen van de maand en het weeknummer.

### <a name="overview-scheduler-trigger-schema-defaults-limits-and-examples"></a>Overzicht: standaardplanning, limieten en voorbeelden voor scheduler-trigger

JSON-naam | Waardetype | Vereist? | Standaardwaarde | Geldige waarden | Voorbeeld
--------- | ---------- | --------- | ------------- | ------------ | -------
startTime | Tekenreeks | Ja | Geen | Datums en tijden volgens ISO 8601 | ```"startTime" : "2013-01-09T09:30:00-08:00"```
recurrence | Object | Ja | Geen | Recurrence-object | ```"recurrence" : { "frequency" : "monthly", "interval" : 1 }```
interval | Aantal | Nee | 1 | 1 tot 1000. | ```"interval":10```
endTime | Tekenreeks | Ja | Geen | Datum-/tijdwaarde van een moment in de toekomst | `"endTime" : "2013-02-09T09:30:00-08:00"`
schedule | Object | Nee | Geen | Schedule-object | `"schedule" : { "minute" : [30], "hour" : [8,17] }`

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
weekDays | Dagen van de week waarop de trigger wordt uitgevoerd. Kan alleen worden opgegeven met de frequency wekelijks. | <ul><li>Maandag, dinsdag, woensdag, donderdag, vrijdag, zaterdag of zondag</li><li>Matrix van elk van de bovenstaande waarden (maximale matrixgrootte van 7)</li></p>Niet hoofdlettergevoelig</p>
monthlyOccurrences | Hiermee wordt bepaald op welke dagen van de maand de trigger wordt uitgevoerd. Kan alleen worden opgegeven met de frequency maandelijks. | Matrix met monthlyOccurence-objecten: `{ "day": day,  "occurrence": occurence }`. <p> Day is de dag van de week waarop de trigger wordt uitgevoerd. `{Sunday}` is bijvoorbeeld elke zondag van de maand. Vereist.<p>Occurrence geeft aan op welke dag de trigger wordt uitgevoerd. `{Sunday, -1}` is bijvoorbeeld de laatste zondag van de maand. Optioneel.
monthDays | Dag van de maand waarop de trigger wordt uitgevoerd. Kan alleen worden opgegeven met de frequency maandelijks. | <ul><li>Alle waarden < = -1 en > =-31</li><li>Alle waarden > = -1 en < =-31</li><li>Een matrix met bovenstaande waarden</li>


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




## <a name="next-steps"></a>Volgende stappen
Zie de volgende zelfstudies: 

- [Snelstartgids: een gegevensfactory maken met .NET](quickstart-create-data-factory-dot-net.md)
