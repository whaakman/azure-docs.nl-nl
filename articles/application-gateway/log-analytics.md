---
title: Azure Log Analytics gebruiken voor het onderzoeken van Application Gateway Web Application Firewall-logboeken
description: In dit artikel laat u zien hoe u Azure Log Analytics kunt gebruiken om te controleren van Application Gateway Web Application Firewall-logboeken
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 7/10/2019
ms.author: victorh
ms.openlocfilehash: aa867e33ef0faa96b6a66a9075a3a5b8b0b0bca4
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712174"
---
# <a name="use-log-analytics-to-examine-application-gateway-web-application-firewall-logs"></a>Log Analytics gebruiken voor het onderzoeken van Application Gateway Web Application Firewall-logboeken

Nadat uw Application Gateway WAF ingeschakeld is, kunt u Logboeken om te controleren wat er gebeurt bij elke aanvraag inschakelen. Firewall-logboeken geven inzicht in wat de WAF is geëvalueerd, die overeenkomt met en blokkeren. Met Log Analytics, kunt u de gegevens in de firewall-logboeken te geven van nog meer inzichten bekijken. Zie voor meer informatie over het maken van een Log Analytics-werkruimte [een Log Analytics-werkruimte maken in Azure portal](../azure-monitor/learn/quick-create-workspace.md). Zie voor meer informatie over de logboeken-query's, [overzicht van het logboek query's in Azure Monitor](../azure-monitor/log-query/log-query-overview.md).

## <a name="import-waf-logs"></a>WAF-logboeken importeren

Zie voor informatie over het importeren van uw firewall-Logboeken in Log Analytics [Back-endstatus, diagnostische logboeken en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md#diagnostic-logging). Wanneer u de firewall-Logboeken in uw Log Analytics-werkruimte hebt, kunt u gegevens bekijken, query's schrijven, visualisaties maken en deze toevoegen aan uw portal-dashboard.

## <a name="explore-data-with-examples"></a>Verken gegevens met voorbeelden

Als u wilt de onbewerkte gegevens weergeven in de firewall-logboek, kunt u de volgende query uitvoeren:

```
AzureDiagnostics 
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
```

Dit ziet eruit als de volgende query uit:

![Log Analytics-query](media/log-analytics/log-query.png)

U kunt inzoomen op de gegevens en grafieken uitgezet of visualisaties maken hier. Zie de volgende query's als uitgangspunt:

### <a name="matchedblocked-requests-by-ip"></a>Overeenkomende/geblokkeerde aanvragen per IP

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by clientIp_s, bin(TimeGenerated, 1m)
| render timechart
```

### <a name="matchedblocked-requests-by-uri"></a>Overeenkomende/geblokkeerde aanvragen via een URI

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by requestUri_s, bin(TimeGenerated, 1m)
| render timechart
```

### <a name="top-matched-rules"></a>Bovenste overeenkomende regels

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by ruleId_s, bin(TimeGenerated, 1m)
| where count_ > 10
| render timechart
```

### <a name="top-five-matched-rule-groups"></a>Top vijf overeenkomende regelgroepen

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize Count=count() by details_file_s, action_s
| top 5 by Count desc
| render piechart
```

## <a name="add-to-your-dashboard"></a>Toevoegen aan uw dashboard

Als u een query maakt, kunt u deze kunt toevoegen aan uw dashboard.  Selecteer de **vastmaken aan dashboard** in de rechterbovenhoek van de log analytics-werkruimte. Met de vorige vier query's vastgemaakt aan een dashboard voorbeeld, maar dit is de gegevens die u in een oogopslag kunt zien:

![Dashboard](media/log-analytics/dashboard.png)

## <a name="next-steps"></a>Volgende stappen

[Back-endstatus, diagnostische logboeken en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md)
