---
title: Een Azure data factory programmatisch bewaken | Microsoft Docs
description: Informatie over het bewaken van een pijplijn in een data factory met behulp van verschillende-SDK's (SDK).
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: spelluru
ms.openlocfilehash: 8d47ebe5ad40aee8289420e8424ec4d745baf2df
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="programmatically-monitor-an-azure-data-factory"></a>Een Azure data factory programmatisch bewaken
Dit artikel wordt beschreven hoe u een pijplijn in een gegevensfactory bewaken met behulp van verschillende-SDK's (SDK). 

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [bewaken en beheren van pijplijnen in Data Factory version1 gedefinieerd](v1/data-factory-monitor-manage-pipelines.md).

## <a name="net"></a>.NET
Zie voor een volledige procedure voor het maken en bewaken van een pijplijn met .NET SDK [Maak een gegevensfactory en de pijplijn met .NET](quickstart-create-data-factory-dot-net.md).

1. Voeg de volgende code u voortdurend de status van de pijplijn uitgevoerd totdat deze is voltooid met het kopiëren van de gegevens.

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

2. De volgende code toevoegen aan die haalt kopieeractiviteit bijvoorbeeld details uitvoering van grootte van de gegevens gelezen of weggeschreven.

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

Zie voor volledige documentatie over .NET SDK [Data Factory .NET SDK verwijzing](/dotnet/api/microsoft.azure.management.datafactory?view=azure-dotnet).

## <a name="python"></a>Python
Zie voor een volledige procedure voor het maken en bewaken van een pijplijn met behulp van Python SDK [Maak een gegevensfactory en de pijplijn met behulp van Python](quickstart-create-data-factory-python.md).

Voeg de volgende code voor het controleren van de pijplijn uitvoeren:

```python
#Monitor the pipeline run
time.sleep(30)
pipeline_run = adf_client.pipeline_runs.get(rg_name, df_name, run_response.run_id)
print("\n\tPipeline run status: {}".format(pipeline_run.status))
activity_runs_paged = list(adf_client.activity_runs.list_by_pipeline_run(rg_name, df_name, pipeline_run.run_id, datetime.now() - timedelta(1),  datetime.now() + timedelta(1)))
print_activity_run_details(activity_runs_paged[0])
```

Zie voor volledige documentatie over Python SDK [Data Factory Python SDK verwijzing](/python/api/overview/azure/datafactory?view=azure-python).

## <a name="rest-api"></a>REST-API
Zie voor een volledige procedure voor het maken en bewaken van een pijplijn met REST-API [Maak een gegevensfactory en de pijplijn met REST-API](quickstart-create-data-factory-rest-api.md).
 
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

Zie voor volledige documentatie over REST API [naslaginformatie over REST API van Data Factory](/rest/api/datafactory/).

## <a name="powershell"></a>PowerShell
Zie voor een volledige procedure voor het maken en bewaken van een pijplijn met behulp van PowerShell [Maak een gegevensfactory en de pijplijn met behulp van PowerShell](quickstart-create-data-factory-powershell.md).

1. Voer het volgende script uit om continu de status van de pijplijnuitvoering te controleren totdat het kopiëren van de gegevens is voltooid.

    ```powershell
    while ($True) {
        $run = Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $resourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $runId

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
    $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    $result
    
    Write-Host "Activity 'Output' section:" -foregroundcolor "Yellow"
    $result.Output -join "`r`n"
    
    Write-Host "\nActivity 'Error' section:" -foregroundcolor "Yellow"
    $result.Error -join "`r`n"
    ```

Zie voor volledige documentatie over PowerShell-cmdlets [Data Factory PowerShell-cmdlet-verwijzing](/powershell/module/azurerm.datafactoryv2/?view=azurermps-4.4.1).

## <a name="next-steps"></a>Volgende stappen
Zie [Monitor pijplijnen met behulp van Azure Monitor](monitor-using-azure-monitor.md) artikel voor meer informatie over het gebruik van de Monitor voor Azure Data Factory-pijplijnen bewaken. 

