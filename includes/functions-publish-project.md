---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: fbb537c9584c948af37694b3bfc77a7c345e084d
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639111"
---
## <a name="deploy-the-function-app-project-to-azure"></a>Het functie-appproject implementeren in Azure

Nadat de functie-app is gemaakt in azure, kunt u de [`func azure functionapp publish`](../articles/azure-functions/functions-run-local.md#project-file-deployment) opdracht kern hulpprogramma's gebruiken om uw project code te implementeren in Azure. Vervang `<APP_NAME>` in de volgende opdracht door de naam van uw app uit de vorige stap.

```bash
func azure functionapp publish <APP_NAME>
```

De uitvoer ziet er ongeveer als volgt uit, die is afgekapt voor de Lees baarheid:

```output
Getting site publishing info...
...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
Functions in myfunctionapp:
    HttpTrigger - [httpTrigger]
        Invoke url: https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....
```

Kopieer de `Invoke url` waarde voor uw `HttpTrigger`, die u nu kunt gebruiken om uw functie in azure te testen. De URL bevat een `code` query teken reeks waarde die uw functie sleutel is. Met deze sleutel kunnen anderen moeilijk uw HTTP trigger-eind punt aanroepen in Azure.