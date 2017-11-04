---
title: Taak hubs in duurzame functies - Azure
description: Meer informatie over welke taak hub is in de uitbreiding duurzame functies voor Azure Functions. Meer informatie over het configureren van taak hubs configureren.
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
ms.openlocfilehash: b241bad7b0060551eba5e78efbb1b729bf5d0098
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2017
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Taak hubs in duurzame functies (Azure-functies)

Een *taak hub* in [duurzame functies](durable-functions-overview.md) is een logische container voor Azure Storage-resources die worden gebruikt voor integraties. Orchestrator en activiteit functies kunnen alleen met elkaar communiceren als ze deel uitmaken van dezelfde taak hub.

Elke functie-app heeft een afzonderlijke taak hub. Als meerdere functie apps een opslagaccount delen, bevat het storage-account meerdere taak hubs. Het volgende diagram ziet u een taak hub per functie-app in gedeelde en speciale storage-accounts.

![Diagram waarin gedeeld en storage-accounts toegewezen.](media/durable-functions-task-hubs/task-hubs-storage.png)

## <a name="azure-storage-resources"></a>Azure Storage-resources

Een hub taak bestaat uit de volgende opslagbronnen: 

* Een of meer wachtrijen van control.
* Een werkitem-wachtrij.
* Van een geschiedenistabel.
* Een storage-container met een of meer lease blobs.

Van al deze resources worden automatisch gemaakt in het standaard Azure Storage-account als orchestrator of activiteit functies uitvoeren of zijn gepland. De [prestaties en schaalbaarheid](durable-functions-perf-and-scale.md) artikel wordt uitgelegd hoe u deze resources worden gebruikt.

## <a name="task-hub-names"></a>Namen van de hub taken

Taak hubs worden aangeduid met een naam die is gedeclareerd in de *host.json* -bestand, zoals wordt weergegeven in het volgende voorbeeld:

```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```

Taak hub namen moeten beginnen met een letter en bestaan uit alleen letters en cijfers. Als u niet opgeeft, wordt de standaardnaam is **DurableFunctionsHub**.

> [!NOTE]
> De naam is wat één taak hub onderscheidt van andere wanneer er meerdere taak hubs in een gedeelde storage-account. Als u meerdere functie apps delen van een gedeelde storage-account hebt, hebt u voor het configureren van verschillende namen voor elke taak hub in de *host.json* bestanden.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het afhandelen van versiebeheer](durable-functions-versioning.md)
