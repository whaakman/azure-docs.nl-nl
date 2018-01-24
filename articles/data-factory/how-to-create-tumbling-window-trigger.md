---
title: Daling venster triggers maken in Azure Data Factory | Microsoft Docs
description: Informatie over het maken van een trigger in Azure Data Factory dat een pijplijn op een tumblingvenster wordt uitgevoerd.
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
ms.date: 01/05/2018
ms.author: shlo
ms.openlocfilehash: 1f026683ebc9b3d2bc935cd78aa9d16684e7db40
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-tumbling-window"></a>Een trigger die op een tumblingvenster wordt uitgevoerd van een pijplijn maken
Dit artikel bevat stappen voor het maken, het starten en het bewaken van een venster daling trigger. Raadpleeg voor algemene informatie over triggers en de ondersteunde typen [Pipeline-uitvoering en triggers](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> In dit artikel is van toepassing op Azure Data Factory versie 2, momenteel in preview is. Als u Azure Data Factory versie 1, die algemeen beschikbaar (GA), Zie [aan de slag met Azure Data Factory versie 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Tumblingvenstertriggers zijn triggers die vanaf een opgegeven begintijd worden geactiveerd met een periodiek tijdsinterval en die hun status behouden. Windows daling zijn een reeks met vaste grootte en niet-overlappende en aaneengesloten tijdsintervallen. Een venster daling trigger heeft een-op-een relatie met een pipeline en kan alleen verwijzen naar een enkelvoud pijplijn.

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

De volgende tabel bevat een overzicht van de belangrijke JSON-elementen die gerelateerd aan terugkeerpatroon en planning van een trigger daling-venster zijn:

| JSON-element | Beschrijving | Type | Toegestane waarden | Vereist |
|:--- |:--- |:--- |:--- |:--- |
| **type** | Het type van de trigger. Het type is de vaste waarde 'TumblingWindowTrigger'. | Tekenreeks | "TumblingWindowTrigger" | Ja |
| **runtimeState** | Uitvoeringstijd van de huidige status van de trigger.<br/>**Opmerking**: dit element heeft de \<readOnly >. | Tekenreeks | 'Gestart', 'is gestopt,' "Uitgeschakeld" | Ja |
| **frequentie** | Een tekenreeks met de frequentie-eenheid waarmee de trigger terugkeert (minuten of uur). Als de **startTime** date-waarden zijn meer gedetailleerd dan de **frequentie** waarde, de **startTime** datums worden beschouwd wanneer de grenzen van het venster worden berekend. Bijvoorbeeld, als de **frequentie** waarde per uur is en de **startTime** waarde is 2016-04-01T10:10:10Z, het eerste venster is (2017-09-01T10:10:10Z, 2017-09-01T11:10:10Z). | Tekenreeks | "minute', 'uur"  | Ja |
| **interval** | Een positief geheel getal dat geeft het interval voor de **frequentie** waarde waarmee wordt bepaald hoe vaak de trigger wordt uitgevoerd. Bijvoorbeeld, als de **interval** 3 en de **frequentie** 'uur', is de trigger wordt herhaald elke drie uur. | Geheel getal | Een positief geheel getal. | Ja |
| **startTime**| Het eerste exemplaar, die in het verleden kan zijn. De eerste trigger-interval (**startTime**, **startTime** + **interval**). | Datum en tijd | Een datum / tijdwaarde. | Ja |
| **endTime**| Het laatste exemplaar, die in het verleden kan zijn. | Datum en tijd | Een datum / tijdwaarde. | Ja |
| **delay** | De hoeveelheid tijd aan die het begin van het verwerken van gegevens voor het venster verstrijkt. De pijplijn die uitgevoerd wordt gestart nadat de verwachte uitvoeringstijd plus de hoeveelheid **vertraging**. De **vertraging** definieert hoe lang de trigger moet wachten na de vervaldatum tijd voordat een nieuw run. De **vertraging** niet van invloed op het venster **startTime**. Bijvoorbeeld, een **vertraging** waarde van 00:10:00 impliceert een vertraging van 10 minuten. | Periode  | Een tijdwaarde waar de standaardwaarde 00:00:00 is. | Nee |
| **maxConcurrency** | Het aantal gelijktijdige trigger wordt uitgevoerd die zijn gestart voor windows die gereed zijn. Bijvoorbeeld als u wilt back-opvulling elk uur wordt uitgevoerd voor gisteren resulteert in 24 windows. Als **maxConcurrency** = 10, worden gebeurtenissen alleen voor de eerste 10 windows trigger (00:00-01:00 - 09:00 – 10:00). Nadat de eerste 10 triggered pijplijn wordt uitgevoerd, voltooid zijn, worden de trigger wordt uitgevoerd gestart voor de volgende 10 windows (10:00 – 11:00-19:00 – 20:00). In dit voorbeeld van **maxConcurrency** = 10, als er 10 windows gereed is, zijn er 10 totale pijplijn wordt uitgevoerd. Als er slechts 1 venster gereed is, is er slechts 1 pijplijn uitvoeren. | Geheel getal | Een geheel getal tussen 1 en 50. | Ja |
| **het retryPolicy: aantal** | Het aantal nieuwe pogingen voordat de uitvoering van de pijplijn is gemarkeerd als 'Mislukt'.  | Geheel getal | Een geheel getal, waarbij de standaardwaarde 0 (geen herhaalde pogingen is). | Nee |
| **retryPolicy: intervalInSeconds** | De vertraging tussen pogingen opgegeven (in seconden). | Geheel getal | Het aantal seconden, waarbij de standaardwaarde 30 is. | Nee |

### <a name="windowstart-and-windowend-system-variables"></a>WindowStart en WindowEnd systeemvariabelen

U kunt de **WindowStart** en **WindowEnd** systeemvariabelen van de trigger van het venster daling in uw **pijplijn** definitie (dat wil zeggen, voor een deel van een query). De systeemvariabelen als parameters doorgeven aan de pijplijn in de **trigger** definitie. Het volgende voorbeeld ziet u hoe u kunt deze variabelen doorgeven als parameters:

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

Gebruik de **WindowStart** en **WindowEnd** systeemwaarden van variabelen in de definitie van de pipeline, de parameters 'MyWindowStart' en 'MyWindowEnd' dienovereenkomstig gebruiken.

### <a name="execution-order-of-windows-in-a-backfill-scenario"></a>De volgorde van uitvoering van windows in een scenario backfill
Wanneer er meerdere vensters voor uitvoering (met name in een scenario backfill), is de volgorde van de uitvoering voor windows deterministisch, uit oud naar nieuw intervallen. Op dit moment is kan niet dit gedrag worden gewijzigd.

### <a name="existing-triggerresource-elements"></a>Bestaande TriggerResource elementen
De volgende punten van toepassing op bestaande **TriggerResource** elementen:

* Als de waarde voor de **frequentie** element (of venstergrootte) van de trigger wijzigingen, de status van de vensters die al zijn verwerkt is *niet* opnieuw instellen. De trigger blijft gestart voor de vensters van het laatste venster dat wordt uitgevoerd met behulp van de grootte van het nieuwe venster.
* Als de waarde voor de **endTime** element van de trigger wijzigingen (toegevoegd of bijgewerkt), de status van de windows dat al is verwerkt is *niet* opnieuw instellen. De trigger zich houdt aan de nieuwe **endTime** waarde. Als de nieuwe **endTime** waarde is voordat de vensters die al worden uitgevoerd, de trigger stopt. Anders wordt de trigger stopt wanneer de nieuwe **endTime** waarde is aangetroffen.

## <a name="sample-for-azure-powershell"></a>Voorbeeld voor Azure PowerShell
Deze sectie wordt beschreven hoe u Azure PowerShell gebruikt te maken, te starten en te bewaken van een trigger.

1. Maak een JSON-bestand met de naam **MyTrigger.json** in de map C:\ADFv2QuickStartPSH\ met de volgende inhoud:

   > [!IMPORTANT]
   > Voordat u het JSON-bestand opslaan, stel de waarde van de **startTime** element in de huidige UTC-tijd. Stel de waarde van de **endTime** element aan één uur na de huidige UTC-tijd.

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

6. Get-de trigger wordt uitgevoerd in Azure PowerShell met behulp van de **Get-AzureRmDataFactoryV2TriggerRun** cmdlet. Als u informatie over de trigger wordt uitgevoerd, moet u de volgende opdracht periodiek uitvoeren. Update de **TriggerRunStartedAfter** en **TriggerRunStartedBefore** in overeenstemming met de waarden in de definitie van de trigger:

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
    
Voor het bewaken van de trigger wordt uitgevoerd en de pijplijn wordt uitgevoerd in de Azure portal, Zie [pijplijn bewaken wordt uitgevoerd](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="next-steps"></a>Volgende stappen
Zie voor gedetailleerde informatie over triggers [Pipeline-uitvoering en triggers](concepts-pipeline-execution-triggers.md#triggers).
