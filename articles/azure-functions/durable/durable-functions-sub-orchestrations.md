---
title: Onderliggende indelingen voor duurzame functies - Azure
description: Over het aanroepen van de indelingen van indelingen in de extensie duurzame functies voor Azure Functions.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: df4cfd8cdf720dd085c3f14ad518c557f270ffa4
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53340858"
---
# <a name="sub-orchestrations-in-durable-functions-azure-functions"></a>Onderliggende indelingen in duurzame functies (Azure Functions)

Naast het aanroepen van activiteitsfuncties, kunnen orchestrator-functies aanroepen van andere orchestrator-functies. U kunt bijvoorbeeld een grotere orchestration uit een bibliotheek van orchestrator-functies bouwen. Of u kunt meerdere exemplaren van een orchestrator-functie parallel uitvoeren.

Een orchestrator-functie kan een andere orchestrator-functie aanroepen door het aanroepen van de [CallSubOrchestratorAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorAsync_) of de [CallSubOrchestratorWithRetryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorWithRetryAsync_) methoden in .NET of de `callSubOrchestrator` of `callSubOrchestratorWithRetry` methoden in JavaScript. De [vorm van foutafhandeling & compensatie](durable-functions-error-handling.md#automatic-retry-on-failure) artikel vindt u meer informatie over automatische nieuwe pogingen.

Onderliggende orchestrator-functies werken net als bij activiteitsfuncties vanuit het perspectief van de oproepende functie. Ze kunnen een waarde retourneren een uitzondering en door de bovenliggende orchestrator-functie kan worden gestopt.

## <a name="example"></a>Voorbeeld

Het volgende voorbeeld wordt een IoT ('Internet of Things")-scenario waarbij er meerdere apparaten die moeten worden ingericht. Er is een bepaalde indeling die moet worden opgestart voor elk van de apparaten, ziet er ongeveer als volgt uit:

### <a name="c"></a>C#

```csharp
public static async Task DeviceProvisioningOrchestration(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string deviceId = context.GetInput<string>();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    Uri sasUrl = await context.CallActivityAsync<Uri>("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    await context.CallActivityAsync("SendPackageUrlToDevice", Tuple.Create(deviceId, sasUrl));

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    await context.WaitForExternalEvent<bool>("DownloadCompletedAck");

    // Step 4: ...
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceId = context.df.getInput();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    const sasUrl = yield context.df.callActivity("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    yield context.df.callActivity("SendPackageUrlToDevice", { id: deviceId, url: sasUrl });

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    yield context.df.waitForExternalEvent("DownloadCompletedAck");

    // Step 4: ...
});
```

Deze functie orchestrator kan worden gebruikt als-is voor eenmalige apparaatinrichting of deel van een grotere orchestration uitmaken kunnen. In dat laatste geval, de bovenliggende orchestrator-functie kunt plannen dat exemplaren van `DeviceProvisioningOrchestration` met behulp van de `CallSubOrchestratorAsync` (C#) of `callSubOrchestrator` (JavaScript) API.

Hier volgt een voorbeeld waarin wordt uitgelegd hoe u meerdere orchestrator-functies parallel worden uitgevoerd.

### <a name="c"></a>C#

```csharp
[FunctionName("ProvisionNewDevices")]
public static async Task ProvisionNewDevices(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string[] deviceIds = await context.CallActivityAsync<string[]>("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    var provisioningTasks = new List<Task>();
    foreach (string deviceId in deviceIds)
    {
        Task provisionTask = context.CallSubOrchestratorAsync("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.Add(provisionTask);
    }

    await Task.WhenAll(provisioningTasks);

    // ...
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (werkt alleen 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceIds = yield context.df.callActivity("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    const provisioningTasks = [];
    for (const deviceId of deviceIds) {
        const provisionTask = context.df.callSubOrchestrator("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.push(provisionTask);
    }

    yield context.df.Task.all(provisioningTasks);

    // ...
});
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Leer wat taakhubs zijn en hoe u ze configureren](durable-functions-task-hubs.md)
