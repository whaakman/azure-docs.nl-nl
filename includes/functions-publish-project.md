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
ms.openlocfilehash: 9fefba9c79e9c58956243de8db67ca4cf316251c
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68949991"
---
## <a name="deploy-the-function-app-project-to-azure"></a>Het functie-appproject implementeren in Azure

Nadat de functie-app is gemaakt in azure, kunt u de [`func azure functionapp publish`](../articles/azure-functions/functions-run-local.md#project-file-deployment) opdracht kern hulpprogramma's gebruiken om uw project code te implementeren in Azure. In deze voor beelden vervangt `<APP_NAME>` u door de naam van uw app uit de vorige stap.

### <a name="c--javascript"></a>C\# /java script

```command
func azure functionapp publish <APP_NAME>
```

### <a name="typescript"></a>TypeScript

```command
npm run build:production 
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