---
title: Web application firewall-regels in Azure Application Gateway - PowerShell aanpassen | Microsoft Docs
description: In dit artikel bevat informatie over het aanpassen van web application firewall-regels in Application Gateway met PowerShell.
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: davidmu
ms.openlocfilehash: 4595864a7bc624375ba2ff6ace09ebae5b0f843a
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="customize-web-application-firewall-rules-through-powershell"></a>Web application firewall-regels via PowerShell aanpassen

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md)

De Azure Application Gateway web application firewall (WAF) biedt beveiliging voor webtoepassingen. Deze beveiligingen worden geleverd door de toepassing van Open Web Project beveiliging (OWASP) Core regel ingesteld (CRS). Sommige regels kunnen valse positieven veroorzaken en echte verkeer blokkeert. Daarom biedt Application Gateway de mogelijkheid om aan te passen regelgroepen en regels. Zie voor meer informatie over de specifieke regelgroepen en regels [lijst met web application firewall CRS regelgroepen en regels](application-gateway-crs-rulegroups-rules.md).

## <a name="view-rule-groups-and-rules"></a>Groepen van de regel weergeven en regels

De volgende codevoorbeelden laten zien hoe regels en groepen met regels die kunnen worden geconfigureerd op een gateway WAF-toepassing weergeven.

### <a name="view-rule-groups"></a>Groepen van de regel weergeven

Het volgende voorbeeld ziet u hoe groepen met regels weergeven:

```powershell
Get-AzureRmApplicationGatewayAvailableWafRuleSets
```

De volgende uitvoer is een afgekapte reactie van het vorige voorbeeld:

```
OWASP (Ver. 3.0):

    REQUEST-910-IP-REPUTATION:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            910011     Rule 910011
            910012     Rule 910012
            ...        ...

    REQUEST-911-METHOD-ENFORCEMENT:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            911011     Rule 911011
            ...        ...

OWASP (Ver. 2.2.9):

    crs_20_protocol_violations:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            960911     Invalid HTTP Request Line
            981227     Apache Error: Invalid URI in Request.
            960000     Attempted multipart/form-data bypass
            ...        ...
```

## <a name="disable-rules"></a>Regels uitschakelen

Het volgende voorbeeld wordt uitgeschakeld regels `910018` en `910017` op een toepassingsgateway:

```powershell
$disabledrules=New-AzureRmApplicationGatewayFirewallDisabledRuleGroupConfig -RuleGroupName REQUEST-910-IP-REPUTATION -Rules 910018,910017
Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -ApplicationGateway $gw -Enabled $true -FirewallMode Detection -RuleSetVersion 3.0 -RuleSetType OWASP -DisabledRuleGroups $disabledrules
```

## <a name="next-steps"></a>Volgende stappen

Nadat u uw uitgeschakelde regels hebt geconfigureerd, kunt u informatie over het weergeven van uw WAF Logboeken. Zie voor meer informatie [Application Gateway Diagnostics](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
