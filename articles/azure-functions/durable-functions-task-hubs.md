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
ms.openlocfilehash: 53c70d4407222a80a9bc948db51294cd3e4e1bb4
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092832"
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Taakhubs in duurzame functies (Azure Functions)

Een *taak hub* in [duurzame functies](durable-functions-overview.md) is een logische container voor Azure Storage-resources die worden gebruikt voor indelingen. Orchestrator en activiteit functies kunnen alleen met elkaar communiceren wanneer ze deel uitmaken van dezelfde taak hub.

Elke functie-app heeft een afzonderlijke taak hub. Als meerdere functie-apps een storage-account deelt, bevat het storage-account meerdere taakhubs. Het volgende diagram ziet u een taak hub per functie-app in de gedeelde en toegewijde storage-accounts.

![Diagram van gedeeld en storage-accounts toegewezen.](media/durable-functions-task-hubs/task-hubs-storage.png)

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

```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```

Namen van taken hub moeten beginnen met een letter en bestaan uit alleen letters en cijfers. Indien niet opgegeven, wordt de standaardnaam is **DurableFunctionsHub**.

> [!NOTE]
> De naam is wat wordt onderscheid gemaakt tussen een taak hub vanuit een ander wanneer er meerdere taakhubs in een gedeelde storage-account. Als u meerdere functie-apps delen van een gedeelde storage-account hebt, u moet verschillende namen voor elke taak hub in configureren de *host.json* bestanden.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over het afhandelen van versiebeheer](durable-functions-versioning.md)
