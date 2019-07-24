---
title: Bewaking en logboek registratie van Azure Web Application Firewall
description: Meer informatie over Web Application Firewall (WAF) met-ingang controle en logboek registratie
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: 675d06f3d2071022da3867a4c45137efb818980d
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67849143"
---
# <a name="azure-web-application-firewall-monitoring-and-logging"></a>Bewaking en logboek registratie van Azure Web Application Firewall 

Bewaking en logboek registratie van Azure Web Application Firewall (WAF) worden verschaft via logboek registratie en integratie met Azure Monitor en Azure Monitor Logboeken.

## <a name="azure-monitor"></a>Azure Monitor

WAF met-ingang-logboek is geïntegreerd met [Azure monitor](../azure-monitor/overview.md). Met Azure Monitor kunt u Diagnostische gegevens bijhouden, waaronder WAF-waarschuwingen en-Logboeken. U kunt WAF-bewaking configureren binnen de voor deur van de portal op het tabblad **Diagnostische gegevens** of via de Azure Monitor-service rechtstreeks.

Ga vanuit Azure Portal naar het resource type voor deur. Op /het tabblad bewakings**gegevens** aan de linkerkant kunt u **WebApplicationFirewallRequestCount** toevoegen om het aantal aanvragen bij te houden dat overeenkomt met WAF-regels. Aangepaste filters kunnen worden gemaakt op basis van actie typen en regel namen.

![WAFMetrics](./media//waf-front-door-monitor/waf-frontdoor-metrics.png)

## <a name="logs-and-diagnostics"></a>Logboeken en diagnostische gegevens

WAF met de voor deur biedt gedetailleerde rapportage over elke bedreiging die wordt gedetecteerd. Logboekregistratie is geïntegreerd met de logboeken van Azure Diagnostics en waarschuwingen worden vastgelegd in de JSON-indeling. Deze logboeken kunnen worden geïntegreerd met [Azure monitor](../azure-monitor/insights/azure-networking-analytics.md)-Logboeken.

![WAFDiag](./media/waf-front-door-monitor/waf-frontdoor-diagnostics.png)

FrontdoorAccessLog registreert alle aanvragen die worden doorgestuurd naar de back-ends van de klant. FrontdoorWebApplicationFirewallLog registreert alle aanvragen die overeenkomen met een WAF-regel.

Met de volgende voorbeeld query worden WAF-logboeken op geblokkeerde aanvragen opgehaald:

``` WAFlogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"

```

Met de volgende voorbeeld query worden AccessLogs-vermeldingen opgehaald:

``` AccessLogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorAccessLog"


```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [voor deur](front-door-overview.md).

