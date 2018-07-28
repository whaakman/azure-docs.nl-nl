---
title: Tumblingvenstertriggers maken in Azure Data Factory | Microsoft Docs
description: Informatie over het maken van een trigger in Azure Data Factory die een pijplijn op een tumblingvenster wordt uitgevoerd.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/27/2018
ms.author: shlo
ms.openlocfilehash: c42d6235af8a5ab27fbd550b63c301fd9c6f15b1
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325030"
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-tumbling-window"></a>Een trigger die een pijplijn op een tumblingvenster uitvoert maken
In dit artikel bevat stappen voor het maken, starten en controleren van een tumblingvenstertrigger. Raadpleeg voor algemene informatie over triggers en de ondersteunde typen [pijplijnen uitvoeren en triggers](concepts-pipeline-execution-triggers.md).

Tumblingvenstertriggers zijn triggers die vanaf een opgegeven begintijd worden geactiveerd met een periodiek tijdsinterval en die hun status behouden. Tumblingvensters bestaan uit een reeks niet-overlappende en aaneengesloten tijdsintervallen van vaste duur. Een tumblingvenstertrigger heeft een-op-een-relatie met een pijplijn en kan alleen verwijzen naar een enkel pijplijn.

## <a name="data-factory-ui"></a>Gebruikersinterface van Data Factory

Als u wilt een tumblingvenstertrigger maken in Azure portal, selecteert u **Trigger > tumblingvenster > volgende**, en configureer vervolgens de eigenschappen die het tumblingvenster definiëren.

![Een tumblingvenstertrigger maken in Azure portal](media/how-to-create-tumbling-window-trigger/create-tumbling-window-trigger.png)

## <a name="tumbling-window-trigger-type-properties"></a>Tumbling venster trigger type-eigenschappen
Een tumblingvenster heeft de volgende eigenschappen van de trigger-type:

```  
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
        "runtimeState": "<<Started/Stopped/Disabled - readonly>>",
        "typeProperties": {
            "frequency": "<<Minute/Hour>>",
            "interval": <<int>>,
            "startTime": "<<datetime>>",
            "endTime: "<<datetime – optional>>"",
            "delay": "<<timespan – optional>>",
            “maxConcurrency”: <<int>> (required, max allowed: 50),
            "retryPolicy": {
                "count":  <<int - optional, default: 0>>,
                “intervalInSeconds”: <<int>>,
            }
        },
        "pipeline":
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "MyPipelineName"
                },
                "parameters": {
                    "parameter1": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    },
                    "parameter2": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    },
                    "parameter3": "https://mydemo.azurewebsites.net/api/demoapi"
                }
            }
      }    
}
```  

De volgende tabel bevat een overzicht van de belangrijkste JSON-elementen die betrekking hebben op het terugkeerpatroon en het schema van een tumblingvenstertrigger:

| JSON-element | Beschrijving | Type | Toegestane waarden | Vereist |
|:--- |:--- |:--- |:--- |:--- |
| **type** | Het type van de trigger. Het type is het vaste waarde "TumblingWindowTrigger." | Reeks | "TumblingWindowTrigger" | Ja |
| **runtimeState** | Uitvoeringstijd voor de huidige status van de trigger.<br/>**Houd er rekening mee**: dit element heeft de \<readOnly >. | Reeks | 'Gestart', 'gestopt,""Uitgeschakeld" | Ja |
| **frequency** | Een tekenreeks waarmee de frequentie-eenheid waarmee de trigger wordt uitgevoerd (minuten of uur). Als de **startTime** date-waarden zijn gedetailleerder zijn dan de **frequentie** waarde, de **startTime** datums worden beschouwd als wanneer de grenzen van het venster worden berekend. Bijvoorbeeld, als de **frequentie** waarde wordt per uur en de **startTime** waarde is 2016-04-01T10:10:10Z, het eerste venster is (2017-09-01T10:10:10Z, 2017-09-01T11:10:10Z). | Reeks | 'minuut', 'uur'  | Ja |
| **interval** | Een positief geheel getal dat het interval voor de waarde **frequency** aangeeft. Het bepaalt hoe vaak de trigger wordt uitgevoerd. Bijvoorbeeld, als de **interval** 3 is en de **frequentie** 'uur', wordt de trigger elke drie uur uitgevoerd. | Geheel getal | Een positief geheel getal zijn. | Ja |
| **startTime**| De eerste instantie, die in het verleden worden kan. Het interval voor de eerste trigger is (**startTime**, **startTime** + **interval**). | DateTime | Een datum / tijdwaarde. | Ja |
| **endTime**| Het laatste exemplaar, die in het verleden worden kan. | DateTime | Een datum / tijdwaarde. | Ja |
| **delay** | De hoeveelheid tijd aan het begin van de verwerking van gegevens van het venster van de wachttijd. Uitvoering van de pijplijn wordt gestart na de verwachte tijd voor de uitvoering van plus het bedrag van **vertraging**. De **vertraging** wordt gedefinieerd hoe lang de trigger moet wachten na de vervaldatum tijd voordat een nieuwe uitvoering wordt geactiveerd. De **vertraging** niet van invloed op het venster **startTime**. Bijvoorbeeld, een **vertraging** waarde van 00:10:00 impliceert een vertraging van 10 minuten. | Periode  | Een tijd-waarde waar de standaardwaarde 00:00:00 is. | Nee |
| **maxConcurrency** | Het aantal gelijktijdige triggeruitvoeringen die zijn geactiveerd voor windows die gereed zijn. Bijvoorbeeld: voor de achtergrondopvulling elk uur wordt uitgevoerd voor gisteren resulteert in 24 windows. Als **maxConcurrency** = 10, worden gebeurtenissen alleen voor de eerste 10 windows trigger (00:00:01: 00 - 09:00-10:00). Nadat de eerste 10 geactiveerde pijplijnuitvoeringen voltooid zijn, worden de triggeruitvoeringen geactiveerd voor de volgende 10 (10:00-11:00-19:00:20: 00) van windows. In dit voorbeeld van **maxConcurrency** = 10, als er 10 windows gereed is, er zijn 10 totale pijplijnuitvoeringen. Als er slechts 1 venster gereed is, is er slechts 1 pijplijnuitvoering. | Geheel getal | Een geheel getal tussen 1 en 50. | Ja |
| **retryPolicy: aantal** | Het aantal nieuwe pogingen voordat de uitvoering van de pijplijn is gemarkeerd als 'Mislukt'.  | Geheel getal | Een geheel getal, waarbij de standaardwaarde 0 (geen nieuwe pogingen is). | Nee |
| **retryPolicy: intervalInSeconds** | De vertraging tussen nieuwe pogingen in seconden opgegeven. | Geheel getal | Het aantal seconden, waar de standaardwaarde 30 is. | Nee |

### <a name="windowstart-and-windowend-system-variables"></a>WindowStart en WindowEnd systeemvariabelen

U kunt de **WindowStart** en **WindowEnd** systeemvariabelen van de tumblingvenstertrigger in uw **pijplijn** definitie (dat wil zeggen, voor een deel van een query). De systeemvariabelen als parameters doorgeven aan de pijplijn in de **trigger** definitie. Het volgende voorbeeld ziet u hoe u kunt deze variabelen doorgeven als parameters:

```  
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
            ...
        "pipeline":
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "MyPipelineName"
                },
                "parameters": {
                    "MyWindowStart": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    },
                    "MyWindowEnd": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    }
                }
            }
      }    
}
```  

Gebruik de **WindowStart** en **WindowEnd** systeemwaarden van variabelen in het pijplijndefinitie van de gebruiken de parameters "MyWindowStart" en "MyWindowEnd" dienovereenkomstig.

### <a name="execution-order-of-windows-in-a-backfill-scenario"></a>De volgorde van uitvoering van windows in een backfill-scenario
Wanneer er meerdere vensters voor uitvoering (met name in een backfill-scenario), is de volgorde van de uitvoering voor windows is deterministisch, van oudste naar nieuwste intervallen. Dit gedrag worden niet op dit moment gewijzigd.

### <a name="existing-triggerresource-elements"></a>Bestaande TriggerResource elementen
De volgende punten van toepassing op bestaande **TriggerResource** elementen:

* Als de waarde voor de **frequentie** -element (of venstergrootte) van de trigger-wijzigingen, de status van de vensters die al zijn verwerkt is *niet* opnieuw instellen. De trigger wordt geactiveerd voor de windows-van het laatste venster dat wordt uitgevoerd met behulp van de grootte van het nieuwe venster voortgezet.
* Als de waarde voor de **endTime** element van de trigger wijzigingen (toegevoegd of bijgewerkt), de status van de windows die al zijn verwerkt *niet* opnieuw instellen. De trigger zich houdt aan de nieuwe **endTime** waarde. Als de nieuwe **endTime** waarde is voordat u de vensters die al worden uitgevoerd, de trigger reageert. Anders wordt de trigger reageert wanneer de nieuwe **endTime** waarde is aangetroffen.

## <a name="sample-for-azure-powershell"></a>Voorbeeld van Azure PowerShell
Deze sectie leest u hoe u Azure PowerShell gebruiken om te maken, starten en controleren van een trigger.

1. Maak een JSON-bestand met de naam **MyTrigger.json** in de map C:\ADFv2QuickStartPSH\ met de volgende inhoud:

   > [!IMPORTANT]
   > Voordat u het JSON-bestand opslaat, stel de waarde van de **startTime** element op de huidige UTC-tijd. Stel de waarde van de **endTime** element naar één uur na de huidige UTC-tijd.

    ```json   
    {
      "name": "PerfTWTrigger",
      "properties": {
        "type": "TumblingWindowTrigger",
        "typeProperties": {
          "frequency": "Minute",
          "interval": "15",
          "startTime": "2017-09-08T05:30:00Z",
          "delay": "00:00:01",
          "retryPolicy": {
            "count": 2,
            "intervalInSeconds": 30
          },
          "maxConcurrency": 50
        },
        "pipeline": {
          "pipelineReference": {
            "type": "PipelineReference",
            "referenceName": "DynamicsToBlobPerfPipeline"
          },
          "parameters": {
            "windowStart": "@trigger().outputs.windowStartTime",
            "windowEnd": "@trigger().outputs.windowEndTime"
          }
        },
        "runtimeState": "Started"
      }
    }
    ```  

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

6. Ontvang de trigger wordt uitgevoerd in Azure PowerShell met behulp van de **Get-AzureRmDataFactoryV2TriggerRun** cmdlet. Voor informatie over de trigger wordt uitgevoerd, moet u de volgende opdracht periodiek uitvoeren. Update de **TriggerRunStartedAfter** en **TriggerRunStartedBefore** waarden zodat deze overeenkomen met de waarden in de Triggerdefinitie van de:

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
    
Voor het bewaken van de trigger wordt uitgevoerd en de pijplijn wordt uitgevoerd in Azure portal, Zie [pijplijnuitvoeringen controleren](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over triggers [pijplijnen uitvoeren en triggers](concepts-pipeline-execution-triggers.md#triggers).
