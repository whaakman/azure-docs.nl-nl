---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 04/06/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d166a77a0636efea3b63660fde2187e3f2ec15c0
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38940870"
---
Als u functies lokaal ontwikkelt, kunt u de uitbreidingen die u nodig hebt met behulp van Azure Functions Core Tools vanaf de Terminal of vanaf een opdrachtprompt installeren. 

Nadat u hebt bijgewerkt uw *function.json* bestand om op te nemen alle bindingen met de functie nodig heeft, de `func extensions install` opdracht in de projectmap. De opdracht leest de *function.json* bestand om te zien welke pakketten die u nodig hebt en installeert deze.

Als u wilt dat een bepaalde versie van een pakket te installeren of u wilt om pakketten te installeren voordat u bewerkt de *function.json* bestand, gebruikt u de `func extensions install` opdracht met de naam van het pakket, zoals wordt weergegeven in het volgende voorbeeld:

```
func extensions install --package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

Vervang `<target_version>` met een specifieke versie van het pakket, zoals `3.0.0-beta5`. Geldige versies worden weergegeven op de afzonderlijke pakket's op de [NuGet.org](https://nuget.org).
