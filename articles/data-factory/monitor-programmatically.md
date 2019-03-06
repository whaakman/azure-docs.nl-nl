---
title: Een Azure data factory programmatisch bewaken | Microsoft Docs
description: Meer informatie over het bewaken van een pijplijn in een data factory met behulp van verschillende software development kits (SDK's).
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: douglasl
ms.openlocfilehash: 938392cea3e6fe001e662a60e17b936415e7e4f2
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57439278"
---
# <a name="programmatically-monitor-an-azure-data-factory"></a>Een Azure data factory programmatisch bewaken
In dit artikel wordt beschreven hoe u een pijplijn in een data factory bewaken met behulp van verschillende software development kits (SDK's). 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="data-range"></a>Het bereik

Data Factory worden alleen gegevens voor pijplijnuitvoering voor 45 dagen opgeslagen. Wanneer u een query via een programma voor gegevens over pijplijnuitvoeringen Data Factory - bijvoorbeeld: met de PowerShell-opdracht `Get-AzDataFactoryV2PipelineRun` -er zijn geen maximale datums voor de optionele `LastUpdatedAfter` en `LastUpdatedBefore` parameters. Maar als u een query uitvoeren voor gegevens van het afgelopen jaar, bijvoorbeeld de query geen fout retourneert, maar alleen retourneert pijplijn uitvoeren gegevens van de afgelopen 45 dagen.

Als u wilt om vast te leggen van de pijplijnuitvoering gegevens langer dan 45 dagen, instellen van uw eigen Diagnostische logboekregistratie met [Azure Monitor](monitor-using-azure-monitor.md).

## <a name="net"></a>.NET
Zie voor een volledige procedure voor het maken en controleren van een pijplijn met behulp van .NET SDK, [maken van een data factory en pijplijn met behulp van .NET](quickstart-create-data-factory-dot-net.md).

1. Voeg de volgende code om te controleren continu de status van de pijplijn worden uitgevoerd totdat het kopiëren van de gegevens is voltooid.

    ```csharp
    // Monitor the pipeline run
    Console.WriteLine("Checking pipeline run status...");
    PipelineRun pipelineRun;
    while (true)
    {
        pipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, runResponse.RunId);
        Console.WriteLine("Status: " + pipelineRun.Status);
        if (pipelineRun.Status == "InProgress")
            System.Threading.Thread.Sleep(15000);
        else
            break;
    }
    ```

2. Voeg de volgende code toe die haalt kopie details uitvoering van activiteit, bijvoorbeeld: grootte van de gelezen of weggeschreven gegevens.

    ```csharp
    // Check the copy activity run details
    Console.WriteLine("Checking copy activity run details...");
   
    List<ActivityRun> activityRuns = client.ActivityRuns.ListByPipelineRun(
    resourceGroup, dataFactoryName, runResponse.RunId, DateTime.UtcNow.AddMinutes(-10), DateTime.UtcNow.AddMinutes(10)).ToList(); 
    if (pipelineRun.Status == "Succeeded")
        Console.WriteLine(activityRuns.First().Output);
    else
        Console.WriteLine(activityRuns.First().Error);
    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
    ```

Zie voor volledige documentatie over .NET SDK [Data Factory .NET SDK-verwijzing](/dotnet/api/microsoft.azure.management.datafactory?view=azure-dotnet).

## <a name="python"></a>Python
Zie voor een volledige procedure voor het maken en controleren van een pijplijn met behulp van Python-SDK, [maken van een data factory en pijplijn met behulp van Python](quickstart-create-data-factory-python.md).

Voeg de volgende code voor het controleren van de pijplijnuitvoering:

```python
#Monitor the pipeline run
time.sleep(30)
pipeline_run = adf_client.pipeline_runs.get(rg_name, df_name, run_response.run_id)
print("\n\tPipeline run status: {}".format(pipeline_run.status))
activity_runs_paged = list(adf_client.activity_runs.list_by_pipeline_run(rg_name, df_name, pipeline_run.run_id, datetime.now() - timedelta(1),  datetime.now() + timedelta(1)))
print_activity_run_details(activity_runs_paged[0])
```

Zie voor volledige documentatie over Python SDK [naslaginformatie over Data Factory Python SDK](/python/api/overview/azure/datafactory?view=azure-python).

## <a name="rest-api"></a>REST-API
Zie voor een volledige procedure voor het maken en controleren van een pijplijn met behulp van REST-API, [maken van een data factory en pijplijn met behulp van REST-API](quickstart-create-data-factory-rest-api.md).
 
1. Voer het volgende script uit om continu de status van de pijplijnuitvoering te controleren totdat het kopiëren van de gegevens is voltooid.

    ```powershell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}?api-version=${apiVersion}"
    while ($True) {
        $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
        Write-Host  "Pipeline run status: " $response.Status -foregroundcolor "Yellow"

        if ($response.Status -eq "InProgress") {
            Start-Sleep -Seconds 15
        }
        else {
            $response | ConvertTo-Json
            break
        }
    }
    ```
2. Voer het volgende script uit om uitvoeringsdetails van de kopieeractiviteit op te halen, zoals de omvang van de gelezen of weggeschreven gegevens.

    ```PowerShell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}/activityruns?api-version=${apiVersion}&startTime="+(Get-Date).ToString('yyyy-MM-dd')+"&endTime="+(Get-Date).AddDays(1).ToString('yyyy-MM-dd')+"&pipelineName=Adfv2QuickStartPipeline"
    $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
    $response | ConvertTo-Json
    ```

Zie voor volledige documentatie over REST-API, [Data Factory-REST API-verwijzing](/rest/api/datafactory/).

## <a name="powershell"></a>PowerShell
Zie voor een volledige procedure voor het maken en controleren van een pijplijn met behulp van PowerShell, [maken van een data factory en pijplijn met behulp van PowerShell](quickstart-create-data-factory-powershell.md).

1. Voer het volgende script uit om continu de status van de pijplijnuitvoering te controleren totdat het kopiëren van de gegevens is voltooid.

    ```powershell
    while ($True) {
        $run = Get-AzDataFactoryV2PipelineRun -ResourceGroupName $resourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $runId

        if ($run) {
            if ($run.Status -ne 'InProgress') {
                Write-Host "Pipeline run finished. The status is: " $run.Status -foregroundcolor "Yellow"
                $run
                break
            }
            Write-Host  "Pipeline is running...status: InProgress" -foregroundcolor "Yellow"
        }

        Start-Sleep -Seconds 30
    }
    ```
2. Voer het volgende script uit om uitvoeringsdetails van de kopieeractiviteit op te halen, zoals de omvang van de gelezen of weggeschreven gegevens.

    ```powershell
    Write-Host "Activity run details:" -foregroundcolor "Yellow"
    $result = Get-AzDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    $result
    
    Write-Host "Activity 'Output' section:" -foregroundcolor "Yellow"
    $result.Output -join "`r`n"
    
    Write-Host "\nActivity 'Error' section:" -foregroundcolor "Yellow"
    $result.Error -join "`r`n"
    ```

Zie voor volledige documentatie over PowerShell-cmdlets [naslaginformatie over Data Factory PowerShell-cmdlets](/powershell/module/az.datafactory).

## <a name="next-steps"></a>Volgende stappen
Zie [pijplijnen bewaken met Azure Monitor](monitor-using-azure-monitor.md) artikel voor meer informatie over het gebruik van Azure Monitor om Data Factory-pijplijnen te bewaken. 

