---
title: Azure-web application firewall, controle en logboekregistratie
description: Informatie over de web application firewall (WAF) met FrontDoor controle en logboekregistratie
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: tyao;kumud
ms.openlocfilehash: e4ba6cca679ce4910ea941d9578939721514b2ec
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66478966"
---
# <a name="azure-web-application-firewall-monitoring-and-logging"></a>Azure-web application firewall, controle en logboekregistratie 

Azure-web application firewall (WAF) bewaking en logboekregistratie worden geleverd via logboekregistratie en integratie met Azure Monitor en Azure Monitor Logboeken.

## <a name="azure-monitor"></a>Azure Monitor

WAF met FrontDoor logboek is geïntegreerd met [Azure Monitor](../azure-monitor/overview.md). Azure Monitor kunt u voor het bijhouden van diagnostische gegevens, met inbegrip van WAF waarschuwingen en Logboeken. U kunt bewaking met WAF binnen de voordeur resource in de portal onder de **Diagnostics** tabblad of de service rechtstreeks via de Azure Monitor.

Vanuit Azure-portal, gaat u naar de voordeur resourcetype. Van **bewaking**/**metrische gegevens** tabblad aan de linkerkant, kunt u toevoegen **WebApplicationFirewallRequestCount** voor het bijhouden van het aantal aanvragen die overeenkomen met de WAF-regels. Aangepaste filters kunnen worden gemaakt op basis van de actietypen en regelnamen.

![WAFMetrics](./media//waf-front-door-monitor/waf-frontdoor-metrics.png)

## <a name="logs-and-diagnostics"></a>Logboeken en diagnostische gegevens

WAF met voordeur biedt gedetailleerde rapporten voor elke bedreiging die wordt gedetecteerd. Logboekregistratie is geïntegreerd met de logboeken van Azure Diagnostics en waarschuwingen worden vastgelegd in de JSON-indeling. Deze logboeken kunnen worden geïntegreerd met [logboeken van Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md).

![WAFDiag](./media/waf-front-door-monitor/waf-frontdoor-diagnostics.png)

FrontdoorAccessLog registreert alle aanvragen die worden doorgestuurd naar de klant back-ends. FrontdoorWebApplicationFirewallLog registreert een aanvraag die overeenkomt met een WAF-regel.

De volgende voorbeeldquery verkrijgt WAF-logboeken op geblokkeerde aanvragen:

``` WAFlogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"

```

De volgende voorbeeldquery verkrijgt AccessLogs vermeldingen:

``` AccessLogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorAccessLog"


```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [voordeur](front-door-overview.md).

