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
ms.openlocfilehash: a3b056ae4bb4eda26fec58ca3b6bed7f0744e36e
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="how-to-create-a-trigger-that-runs-a-pipeline-on-a-tumbling-window"></a>Het maken van een trigger die wordt uitgevoerd een pijplijn op een tumblingvenster
Dit artikel bevat stappen voor het maken, het starten en het bewaken van een venster daling trigger. Raadpleeg voor algemene informatie over triggers en de typen die er ondersteuning geboden voor [Pipeline-uitvoering en triggers](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Aan de slag met versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Daling venster triggers zijn een type van de trigger die wordt geactiveerd met een periodieke tijdsinterval van een opgegeven begintijd, terwijl de status behouden. Windows daling zijn een reeks met vaste grootte en niet-overlappende en aaneengesloten tijdsintervallen. Een venster daling trigger heeft een 1:1-relatie met een pipeline en kan alleen verwijzen naar een enkelvoud pijplijn.

## <a name="tumbling-window-trigger-type-properties"></a>Tumbling venster Trigger Type-eigenschappen
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

De volgende tabel bevat een overzicht van de belangrijkste onderdelen die betrekking hebben op terugkeerpatroon en planning in een venster daling trigger.

| **De naam van de JSON** | **Beschrijving** | **Toegestane waarden** | **Vereist** |
|:--- |:--- |:--- |:--- |
| **type** | Type van de trigger. Dit probleem wordt opgelost als 'TumblingWindowTrigger'. | Tekenreeks | Ja |
| **runtimeState** | <readOnly>Mogelijke waarden: Gestart, gestopt, uitgeschakeld | Tekenreeks | Ja, alleen-lezen |
| **frequentie** |De *frequentie* de eenheid van de frequentie waarmee de trigger terugkeert type string. Ondersteunde waarden zijn 'minuut' en "uur." Als de begintijd de onderdelen van de datum die meer gedetailleerd dan de frequentie heeft, wordt ze in aanmerking worden genomen voor het berekenen van de grenzen van het venster. Voor bijvoorbeeld: als de frequentie per uur en de begintijd 2016 is-04-01T10:10:10Z, het eerste venster is (2017-09-01T10:10:10Z, 2017-09-01T11:10:10Z.)  | De tekenreeks. Ondersteunde typen 'minute', 'uur" | Ja |
| **interval** |De *interval* een positief geheel getal is en geeft het interval voor de *frequentie* die bepaalt hoe vaak de trigger wordt uitgevoerd. Bijvoorbeeld, als *interval* 3 en *frequentie* 'uur', is de trigger wordt herhaald elke drie uur. | Geheel getal | Ja |
| **startTime**|*startTime* is een datum / tijd. *startTime* is het eerste exemplaar en kan in het verleden. De eerste trigger-interval is (startTime, startTime + interval). | Datum en tijd | Ja |
| **Eindtijd**|*endTime* is een datum / tijd. *endTime* is het laatste exemplaar en kan in het verleden. | Datum en tijd | Ja |
| **vertraging** | Geef de vertraging optreden voordat de verwerking van het venster wordt gestart. De pijplijn die uitgevoerd wordt gestart nadat de verwachte uitvoeringstijd + vertraging. Vertraging definieert hoe lang de trigger moet wachten vervallen tijd voordat nieuwe uitvoeren. Het wijzigen venster begintijd niet. | TimeSpan (voorbeeld: 00:10:00 impliceert vertraging van 10 minuten) |  Nee. Standaardwaarde is "00: 00:00 ' |
| **maximale gelijktijdigheid van taken** | Het aantal gelijktijdige trigger wordt uitgevoerd die zijn gestart voor windows die gereed zijn. Voorbeeld: als we backfill per uur voor gisteren willen, dat is 24 windows. Als gelijktijdigheid = 10, worden gebeurtenissen alleen voor de eerste 10 windows trigger (00:00-01:00 - 09:00 – 10:00). Nadat de eerste 10 triggered pijplijn wordt uitgevoerd, voltooid zijn, worden de trigger wordt uitgevoerd gestart voor de volgende 10 (10:00 – 11:00-19:00 – 20:00). Voorbeeld van gelijktijdigheid = 10, als er 10 windows gereed is, zal er 10 pijplijn wordt uitgevoerd. Als er slechts 1 venster gereed is, er wordt niet meer dan 1 pijplijn uitvoeren. | Geheel getal | Ja. Mogelijke waarden 1-50 |
| **het retryPolicy: aantal** | Het aantal nieuwe pogingen voordat de uitvoering van de pijplijn is gemarkeerd als 'Mislukt'  | Geheel getal |  Nee. De standaardwaarde is 0 pogingen |
| **het retryPolicy: intervalInSeconds** | De vertraging tussen nieuwe pogingen in seconden | Geheel getal |  Nee. De standaardwaarde is 30 seconden |

### <a name="using-system-variables-windowstart-and-windowend"></a>Met behulp van systeemvariabelen: WindowStart en WindowEnd

Als u wilt gebruiken WindowStart en WindowEnd van de trigger van het venster daling in uw **pijplijn** definition (dat wil zeggen voor die deel uitmaken van een query), moet u de variabelen doorgeeft als parameters voor de pijplijn in de **trigger**definitie als volgt te werk:
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

In de definitie van de pijplijn voor het gebruik van de waarden voor WindowStart en WindowEnd gebruik vervolgens de parameters dienovereenkomstig van 'MyWindowStart' en 'MyWindowEnd'

### <a name="notes-on-backfill"></a>Opmerkingen bij Backfill
Wanneer er meerdere vensters voor uitvoering (esp. in backfill scenario), wordt de volgorde van de uitvoering van windows is deterministisch en afkomstig van oud naar nieuw intervallen. Er is geen manier om dit gedrag vanaf nu te wijzigen.

### <a name="updating-an-existing-triggerresource"></a>Bijwerken van een bestaande TriggerResource
* Als de frequentie (of venstergrootte) van de trigger is gewijzigd, de status van windows al wordt verwerkt *niet* opnieuw worden ingesteld. De trigger blijven starten voor het windows vanaf de laatste die het uitgevoerd met behulp van de grootte van het nieuwe venster.
* Als de eindtijd van de trigger wijzigingen (toegevoegd of bijgewerkt), de status van windows al wordt verwerkt *niet* opnieuw worden ingesteld. De nieuwe eindtijd gewoon aan de trigger worden geaccepteerd. Als de eindtijd voor de windows al uitgevoerd is, stopt de trigger. Anders stopt wanneer de nieuwe eindtijd is opgetreden.

## <a name="sample-using-azure-powershell"></a>Voorbeeld met behulp van Azure PowerShell
Deze sectie wordt beschreven hoe u Azure PowerShell gebruikt te maken, te starten en te bewaken van een trigger.

1. Maak een JSON-bestand met de naam MyTrigger.json in de map C:\ADFv2QuickStartPSH\ met de volgende inhoud:

   > [!IMPORTANT]
   > Stel **startTime** in de huidige UTC-tijd en **endTime** aan één uur na de huidige UTC-tijd voordat de JSON-bestand wordt opgeslagen.

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
2. Geen trigger maken met behulp van de **Set AzureRmDataFactoryV2Trigger** cmdlet.

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    
3. Confirm that the status of the trigger is **Stopped** by using the **Get-AzureRmDataFactoryV2Trigger** cmdlet.

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

## <a name="next-steps"></a>Volgende stappen
Zie voor gedetailleerde informatie over triggers [Pipeline-uitvoering en triggers](concepts-pipeline-execution-triggers.md#triggers).
