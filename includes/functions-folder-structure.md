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
ms.openlocfilehash: 2808264b4641bda49a53677ebe216a3b53b7d0d9
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293759"
---
De code voor alle functies in een specifieke functie-app bevindt zich in een hoofdmap van het project met een configuratiebestand voor de host en een of meer submappen. Elke submap bevat de code voor een afzonderlijke functie, zoals in de volgende afbeelding:

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

In versie 2.x van de Functions-runtime, alle functies in de functie-app moet dezelfde taal werknemer delen.  

De [host.json](../articles/azure-functions/functions-host-json.md) bestand, dat een runtime-specifieke configuraties bevat, is in de hoofdmap van de functie-app. Een `bin` map bevat pakketten en andere bibliotheekbestanden die vereist zijn voor de functie-app. Zie de taal-specifieke vereisten voor een functie-app-project:

* [C# klassebibliotheek (.csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
* [C# script (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
* [F #-script](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
* [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
* [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)



