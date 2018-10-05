---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/27/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 1b553cbd720fcb76899844712ce5053af46f7ccb
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452952"
---
## <a name="deploy-the-function-app-project-to-azure"></a>Het functie-appproject implementeren in Azure

Nadat de functie-app is gemaakt in Azure, kunt u de [`func azure functionapp publish`](../articles/azure-functions/functions-run-local.md#project-file-deployment)-opdracht gebruiken om uw projectcode in Azure te implementeren.

```bash
func azure functionapp publish <FunctionAppName>
```

U ziet ongeveer de volgende uitvoer, die voor de leesbaarheid is afgekapt.

```output
Getting site publishing info...

...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
```

U kunt nu uw functies in Azure testen.