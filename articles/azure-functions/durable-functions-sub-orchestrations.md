---
title: Onderliggende integraties voor duurzame functies - Azure
description: Klik hier voor meer informatie over het aanroepen van integraties vanuit integraties in de uitbreiding duurzame functies voor Azure Functions.
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 5184bef81d1cd6ca7b41c1634def24031a4a5942
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2017
---
# <a name="sub-orchestrations-in-durable-functions-azure-functions"></a>Onderliggende integraties in duurzame functies (Azure-functies)

Naast het aanroepen van functies van de activiteit kunnen orchestrator-functies andere orchestrator-functies aanroepen. U kunt bijvoorbeeld een grotere orchestration buiten een tapewisselaar van orchestrator-functies maken. Of u kunt meerdere exemplaren van een orchestrator-functie parallel worden uitgevoerd.

Een orchestrator-functie kan een andere orchestrator-functie aanroepen door het aanroepen van de [CallSubOrchestratorAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorAsync_) of de [CallSubOrchestratorWithRetryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorWithRetryAsync_) methode. De [fouten afhandelen & compensatie](durable-functions-error-handling.md#automatic-retry-on-failure) artikel bevat meer informatie over automatische probeer het opnieuw.

Onderliggende orchestrator-functies werken net zo als functies vanuit het perspectief van de aanroeper van activiteit. Ze kunnen een waarde retourneren Veroorzaak een exception en door de bovenliggende orchestrator-functie kan worden afgewacht.

## <a name="example"></a>Voorbeeld

Het volgende voorbeeld wordt een scenario IoT ('Internet der dingen') wanneer er meerdere apparaten die moeten worden ingericht. Er is een bepaalde orchestration die moeten worden uitgevoerd voor elk van de apparaten, kunnen er als volgt uitzien:

```csharp
public static async Task DeviceProvisioningOrchestration(
    [OrchestrationTrigger] DurableOrchestrationContext ctx)
{
    string deviceId = ctx.GetInput<string>();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    Uri sasUrl = await ctx.CallActivityAsync<Uri>("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    await ctx.CallActivityAsync("SendPackageUrlToDevice", Tuple.Create(deviceId, sasUrl));

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    await ctx.WaitForExternalEvent<bool>("DownloadCompletedAck");

    // Step 4: ...
}
```

Deze functie orchestrator kan worden gebruikt als-is voor het inrichten van eenmalige-apparaat of deel van een grotere orchestration uitmaken kan. In dat geval wordt de functie van de orchestrator bovenliggende exemplaren van kunt plannen `DeviceProvisioningOrchestration` met behulp van de `CallSubOrchestratorAsync` API.

Hier volgt een voorbeeld waarin wordt uitgelegd hoe u meerdere orchestrator-functies parallel worden uitgevoerd.

```csharp
[FunctionName("ProvisionNewDevices")]
public static async Task ProvisionNewDevices(
    [OrchestrationTrigger] DurableOrchestrationContext ctx)
{
    string[] deviceIds = await ctx.CallActivityAsync<string[]>("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    var provisioningTasks = new List<Task>();
    foreach (string deviceId in deviceIds)
    {
        Task provisionTask = ctx.CallSubOrchestratorAsync("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.Add(provisionTask);
    }

    await Task.WhenAll(provisioningTasks);

    // ...
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over wat taak hubs zijn en hoe ze configureren](durable-functions-task-hubs.md)
