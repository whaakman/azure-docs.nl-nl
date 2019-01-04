---
author: mrbullwinkle
ms.service: application-insights
ms.topic: include
ms.date: 11/09/2018
ms.author: mbullwin
ms.openlocfilehash: 91141e314bf276d9138157c8a9d85d5262ac5907
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53728979"
---
Verzameling van aangepaste metingen. Gebruik deze verzameling naar een rapport met de naam meting die is gekoppeld aan de telemetrie-item. Er zijn typische gebruiksvoorbeelden:
- de grootte van de nettolading van Afhankelijkheidstelemetrie
- het aantal wachtrij-items die zijn verwerkt door aanvragen telemetrie
- tijd die klanten nodig was voor het voltooien van de stap in de wizard stap voltooiing telemetrie van gebeurtenissen.

U kunt een query [aangepaste metingen](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) in Application Analytics:

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > Aangepaste metingen zijn gekoppeld aan de telemetrie-item waartoe ze behoren. Ze vallen onder steekproef nemen met de telemetrie-item met deze metingen. Gebruiken voor het volgen van een meting met een waarde die onafhankelijk van andere telemetrietypen [metrische telemetrie](../articles/azure-monitor/app/api-custom-events-metrics.md).

Maximale sleutellengte: 150
