---
title: bestand opnemen
description: bestand opnemen
services: functions
author: nzthiago
ms.service: azure-functions
ms.topic: include
ms.date: 02/21/2018
ms.author: nzthiago
ms.custom: include file
ms.openlocfilehash: ffb29fc76313e8870b52cb0a63936da7853ea6ce
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58305072"
---
## <a name="timeout"></a>Duur van functie-app-time-out 

De duur van de time-out van een functie-app wordt gedefinieerd door de eigenschap functionTimeout in de [host.json](../articles/azure-functions/functions-host-json.md#functiontimeout) projectbestand. De volgende tabel ziet u de standaard- en maximale waarden in minuten voor beide abonnementen en beide runtimeversies:

| Plannen | Runtime-versie | Standaard | Maximum |
|------|---------|---------|---------|
| Verbruik | 1.x | 5 | 10 |
| Verbruik | 2.x | 5 | 10 |
| App Service | 1.x | Onbeperkt | Onbeperkt |
| App Service | 2.x | 30 | Onbeperkt |

> [!NOTE] 
> 230 seconden is, ongeacht de instelling van de time-out in de functie-app, de maximale hoeveelheid tijd die een door HTTP geactiveerde functie nemen kunt om te reageren op een aanvraag. Dit is vanwege de [time-out voor inactiviteit van Azure Load Balancer standaard](../articles/app-service/faq-availability-performance-application-issues.md#why-does-my-request-time-out-after-230-seconds). Voor langere verwerkingstijden, kunt u overwegen de [duurzame functies asynchrone patroon](../articles/azure-functions/durable/durable-functions-concepts.md#async-http) of [het echte werk uitstellen en direct een reactie terug](../articles/azure-functions/functions-best-practices.md#avoid-long-running-functions).
