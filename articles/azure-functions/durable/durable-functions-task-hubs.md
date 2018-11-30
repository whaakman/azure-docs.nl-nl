---
title: Taakhubs in duurzame functies - Azure
description: Leer wat een hub voor de taak is in de extensie duurzame functies voor Azure Functions. Meer informatie over het configureren van taakhubs configureren.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 7a6346e594c5a7cc4cf02f3ea658aac4977e641a
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52642618"
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Taakhubs in duurzame functies (Azure Functions)

Een *taak hub* in [duurzame functies](durable-functions-overview.md) is een logische container voor Azure Storage-resources die worden gebruikt voor indelingen. Orchestrator en activiteit functies kunnen alleen met elkaar communiceren wanneer ze deel uitmaken van dezelfde taak hub.

Elke functie-app heeft een afzonderlijke taak hub. Als meerdere functie-apps een storage-account deelt, bevat het storage-account meerdere taakhubs. Het volgende diagram ziet u een taak hub per functie-app in de gedeelde en toegewijde storage-accounts.

![Diagram van gedeeld en storage-accounts toegewezen.](./media/durable-functions-task-hubs/task-hubs-storage.png)

## <a name="azure-storage-resources"></a>Azure Storage-resources

Een hub taak bestaat uit de volgende opslagresources: 

* Een of meer controle-wachtrijen.
* Een werkitem wachtrij.
* Van één geschiedenistabel.
* Tabel met één exemplaren.
* Een storage-container met een of meer lease-blobs.

Al deze resources worden automatisch gemaakt in de standaard Azure Storage-account als orchestrator of activiteitsfuncties zijn gepland uitvoeren. De [prestaties en schaal](durable-functions-perf-and-scale.md) artikel wordt uitgelegd hoe deze resources worden gebruikt.

## <a name="task-hub-names"></a>Namen van de hub taken

Taakhubs worden aangeduid met een naam die is gedeclareerd in de *host.json* bestand, zoals weergegeven in het volgende voorbeeld:

### <a name="hostjson-functions-v1"></a>host.JSON (functies v1)
```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```
### <a name="hostjson-functions-v2"></a>host.JSON (functies v2)
```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "HubName": "MyTaskHub"
    }
  }
}
```
Taakhubs kunnen ook worden geconfigureerd met behulp van app-instellingen, zoals wordt weergegeven in de volgende *host.json* voorbeeld van een bestand:

### <a name="hostjson-functions-v1"></a>host.JSON (functies v1)
```json
{
  "durableTask": {
    "HubName": "%MyTaskHub%"
  }
}
```
### <a name="hostjson-functions-v2"></a>host.JSON (functies v2)
```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "HubName": "%MyTaskHub%"
    }
  }
}
```
De naam van de taak hub wordt ingesteld op de waarde van de `MyTaskHub` app-instelling. De volgende `local.settings.json` ziet u hoe u voor het definiëren van de `MyTaskHub` instellen als `samplehubname`:

```json
{
  "IsEncrypted": false,
  "Values": {
    "MyTaskHub" :  "samplehubname" 
  }
}
```

Namen van taken hub moeten beginnen met een letter en bestaan uit alleen letters en cijfers. Indien niet opgegeven, wordt de standaardnaam is **DurableFunctionsHub**.

> [!NOTE]
> De naam is wat wordt onderscheid gemaakt tussen een taak hub vanuit een ander wanneer er meerdere taakhubs in een gedeelde storage-account. Als u meerdere functie-apps delen van een gedeelde storage-account hebt, u moet verschillende namen voor elke taak hub in configureren de *host.json* bestanden.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het afhandelen van versiebeheer](durable-functions-versioning.md)
