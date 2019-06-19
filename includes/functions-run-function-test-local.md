---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 10/20/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 74e14e36b1ac0979da31203a2d16e2396ed821d0
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67175809"
---
## <a name="run-the-function-locally"></a>De functie lokaal uitvoeren

Met de volgende opdracht start u de functie-app. De app wordt uitgevoerd met dezelfde Azure Functions-runtime als die van Azure.

```bash
func host start --build
```

De optie `--build` is vereist voor het compileren van C#-projecten. U hebt deze optie niet nodig voor een JavaScript-project.

Wanneer de Functions-host start, wordt ongeveer de volgende uitvoer weergegeven. Deze is voor de leesbaarheid afgekapt:

```output

                  %%%%%%
                 %%%%%%
            @   %%%%%%    @
          @@   %%%%%%      @@
       @@@    %%%%%%%%%%%    @@@
     @@      %%%%%%%%%%        @@
       @@         %%%%       @@
         @@      %%%       @@
           @@    %%      @@
                %%
                %

...

Content root path: C:\functions\MyFunctionProj
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

...

Http Functions:

        HttpTrigger: http://localhost:7071/api/MyHttpTrigger

[8/27/2018 10:38:27 PM] Host started (29486ms)
[8/27/2018 10:38:27 PM] Job host started
```

Kopieer de URL van uw `HttpTrigger`-functie uit de uitvoer van de runtime en plak deze in de adresbalk van uw browser. Voeg de queryreeks `?name=<yourname>` toe aan de URL en voer de aanvraag uit. Hieronder ziet u de reactie op de GET-aanvraag die door de lokale functie wordt geretourneerd, weergegeven in de browser:

![Lokaal testen in de browser](./media/functions-run-function-test-local/functions-test-local-browser.png)

Nu u de functie lokaal hebt uitgevoerd, kunt u de functie-app en andere vereiste resources maken in Azure.