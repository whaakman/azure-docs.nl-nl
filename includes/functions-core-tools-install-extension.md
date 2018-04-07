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
ms.openlocfilehash: 4c9b579534d9a7f2c55e9c589b1738fe060b1cf2
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
Wanneer u lokaal functies ontwikkelt, kunt u de uitbreidingen die u nodig hebt met het gebruik van Azure Functions Core's vanaf de Terminal of vanaf een opdrachtprompt installeren. De volgende `func extensions install` opdracht installeert u de extensie Azure DB die Cosmos-binding:

```
func extensions install --package Microsoft.Azure.WebJobs.Extensions.CosmosDB --version <target_version>
```

Vervang `<taget_version>` met een specifieke versie van het pakket. Geldige versies worden vermeld op de afzonderlijke pakket's op de [NuGet.org](https://nuget.org).
