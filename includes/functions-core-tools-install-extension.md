---
title: bestand opnemen
description: bestand opnemen
services: functions
author: craigshoemaker
ms.service: functions
ms.topic: include
ms.date: 09/25/2018
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: fc5b43dcdee394fea023124171fb42c1a18224dc
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67175801"
---
Extensie-bundels maken alle bindingen die zijn gepubliceerd door de Azure Functions-team beschikbaar via een instelling in de *host.json* bestand. Voor lokale ontwikkeling, zorg ervoor dat u hebt de nieuwste versie van [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md#install-the-azure-functions-core-tools).

Als u wilt gebruiken extensie bundels, de *host.json* bestand om op te nemen van de volgende vermelding voor `extensionBundle`:

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```

- De `id` eigenschap verwijst naar de naamruimte voor Microsoft Azure Functions-extensie bundels.
- De `version` verwijst naar de versie van de bundel.

Verhoging van de bundel versies als pakketten in de bundel wordt gewijzigd. Primaire versiewijzigingen optreden alleen wanneer pakketten in de bundel een primaire versie verplaatsen. De `version` eigenschap gebruikt de [interval notatie voor het opgeven van versiebereiken](https://docs.microsoft.com/nuget/reference/package-versioning#version-ranges-and-wildcards). De Functions-runtime kiest altijd de maximale toegestane versie gedefinieerd door het bereik van de versie of de interval.

Wanneer u verwijst naar de extensie-bundels in uw project, zijn alle standaard bindingen zijn beschikbaar voor uw functies. De bindingen die beschikbaar zijn in de [extensie bundel](https://github.com/Azure/azure-functions-extension-bundles/blob/master/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json) zijn:

|Pakket  |Version  |
|---------|---------|
|Microsoft.Azure.WebJobs.Extensions.CosmosDB|3.0.3|
|Microsoft.Azure.WebJobs.Extensions.DurableTask|1.8.0|
|Microsoft.Azure.WebJobs.Extensions.EventGrid|2.0.0|
|Microsoft.Azure.WebJobs.Extensions.EventHubs|3.0.3|
|Microsoft.Azure.WebJobs.Extensions.SendGrid|3.0.0|
|Microsoft.Azure.WebJobs.Extensions.ServiceBus|3.0.3|
|Microsoft.Azure.WebJobs.Extensions.SignalRService|1.0.0|
|Microsoft.Azure.WebJobs.Extensions.Storage|3.0.4|
|Microsoft.Azure.WebJobs.Extensions.Twilio|3.0.0|