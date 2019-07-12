---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 09/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: aad66a91f7de8380ac7e87f0ce8e35ed43cac4a6
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594544"
---
De code voor alle functies in een specifieke functie-app bevindt zich in een hoofdmap van het project met een configuratiebestand voor de host en een of meer submappen. Elke submap bevat de code voor een afzonderlijke functie. De mapstructuur wordt weergegeven in de volgende afbeelding:

```
FunctionApp
 | - host.json
 | - Myfirstfunction
 | | - function.json
 | | - ...  
 | - mysecondfunction
 | | - function.json
 | | - ...  
 | - SharedCode
 | - bin
```

In versie 2.x van de Functions-runtime, alle functies in de functie-app moeten delen de dezelfde taal-stack.  

De [host.json](../articles/azure-functions/functions-host-json.md) bestand bevat de runtime-specifieke configuraties en is in de hoofdmap van de functie-app. Een *bin* map bevat pakketten en andere bibliotheekbestanden waarvoor de functie-app is vereist. Zie de taal-specifieke vereisten voor een functie-app-project:

* [C# class library (.csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
* [C# script (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
* [F#script](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
* [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
* [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)



