---
title: Onderliggende indelingen voor duurzame functies - Azure
description: Over het aanroepen van de indelingen van indelingen in de extensie duurzame functies voor Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 59e8eb41b7e9fe3d57196f6844d1a768c3ef598b
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44094429"
---
# <a name="sub-orchestrations-in-durable-functions-azure-functions"></a>Onderliggende indelingen in duurzame functies (Azure Functions)

Naast het aanroepen van activiteitsfuncties, kunnen orchestrator-functies aanroepen van andere orchestrator-functies. U kunt bijvoorbeeld een grotere orchestration uit een bibliotheek van orchestrator-functies bouwen. Of u kunt meerdere exemplaren van een orchestrator-functie parallel uitvoeren.

Een orchestrator-functie kan een andere orchestrator-functie aanroepen door het aanroepen van de [CallSubOrchestratorAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorAsync_) of de [CallSubOrchestratorWithRetryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorWithRetryAsync_) methode. De [vorm van foutafhandeling & compensatie](durable-functions-error-handling.md#automatic-retry-on-failure) artikel vindt u meer informatie over automatische nieuwe pogingen.

Onderliggende orchestrator-functies werken net als bij activiteitsfuncties vanuit het perspectief van de oproepende functie. Ze kunnen een waarde retourneren een uitzondering en door de bovenliggende orchestrator-functie kan worden gestopt.

> [!NOTE]
> De `CallSubOrchestratorAsync` en `CallSubOrchestratorWithRetryAsync` methoden zijn nog niet beschikbaar in JavaScript.

## <a name="example"></a>Voorbeeld

Het volgende voorbeeld wordt een IoT ('Internet of Things")-scenario waarbij er meerdere apparaten die moeten worden ingericht. Er is een bepaalde indeling die moet worden opgestart voor elk van de apparaten, ziet er ongeveer als volgt uit:

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

Deze functie orchestrator kan worden gebruikt als-is voor eenmalige apparaatinrichting of deel van een grotere orchestration uitmaken kunnen. In dat laatste geval, de bovenliggende orchestrator-functie kunt plannen dat exemplaren van `DeviceProvisioningOrchestration` met behulp van de `CallSubOrchestratorAsync` API.

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
> [Leer wat taakhubs zijn en hoe u ze configureren](durable-functions-task-hubs.md)
