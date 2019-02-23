---
title: Regels voor web application firewall in Azure Application Gateway - PowerShell aanpassen
description: In dit artikel bevat informatie over het aanpassen van regels voor web application firewall in Application Gateway met PowerShell.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 2/22/2019
ms.author: victorh
ms.openlocfilehash: 1e1638d69915f16b9f30acc5b6a0265b25c2c561
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56728876"
---
# <a name="customize-web-application-firewall-rules-through-powershell"></a>Regels voor web application firewall via PowerShell aanpassen

De Azure Application Gateway web application firewall (WAF) biedt beveiliging voor webtoepassingen. Deze beveiligingen worden geleverd door de Open Web Application Security Project (OWASP) Core regel ingesteld (CRS). Sommige regels kunnen fout-positieven en echte verkeer blokkeert. Om deze reden biedt Application Gateway de mogelijkheid om aan te passen regelgroepen en -regels. Zie voor meer informatie over de specifieke regelgroepen en regels [lijst met web application firewall CRS-regelgroepen en -regels](application-gateway-crs-rulegroups-rules.md).

## <a name="view-rule-groups-and-rules"></a>Weergave-regelgroepen en -regels

De volgende codevoorbeelden laten zien hoe om regels en regelgroepen die kunnen worden geconfigureerd in een toepassingsgateway met WAF-functionaliteit weer te geven.

### <a name="view-rule-groups"></a>Groepen van de regel weergeven

Het volgende voorbeeld laat zien hoe om regelgroepen weer te geven:

```powershell
Get-AzureRmApplicationGatewayAvailableWafRuleSets
```

De volgende uitvoer is een afgekapte reactie van het vorige voorbeeld:

```
OWASP (Ver. 3.0):

    General:
        Description:

        Rules:
            RuleId     Description
            ------     -----------
            200004     Possible Multipart Unmatched Boundary.

    REQUEST-911-METHOD-ENFORCEMENT:
        Description:

        Rules:
            RuleId     Description
            ------     -----------
            911011     Rule 911011
            911012     Rule 911012
            911100     Method is not allowed by policy
            911013     Rule 911013
            911014     Rule 911014
            911015     Rule 911015
            911016     Rule 911016
            911017     Rule 911017
            911018     Rule 911018

    REQUEST-913-SCANNER-DETECTION:
        Description:

        Rules:
            RuleId     Description
            ------     -----------
            913011     Rule 913011
            913012     Rule 913012
            913100     Found User-Agent associated with security scanner
            913110     Found request header associated with security scanner
            913120     Found request filename/argument associated with security scanner
            913013     Rule 913013
            913014     Rule 913014
            913101     Found User-Agent associated with scripting/generic HTTP client
            913102     Found User-Agent associated with web crawler/bot
            913015     Rule 913015
            913016     Rule 913016
            913017     Rule 913017
            913018     Rule 913018

            ...        ...
```

## <a name="disable-rules"></a>Uitschakelen van regels

Het volgende voorbeeld wordt uitgeschakeld regels `911011` en `911012` in een toepassingsgateway:

```powershell
$disabledrules=New-AzureRmApplicationGatewayFirewallDisabledRuleGroupConfig -RuleGroupName REQUEST-911-METHOD-ENFORCEMENT -Rules 911011,911012
Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -ApplicationGateway $gw -Enabled $true -FirewallMode Detection -RuleSetVersion 3.0 -RuleSetType OWASP -DisabledRuleGroups $disabledrules
Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

## <a name="mandatory-rules"></a>Verplichte regels

De volgende lijst bevat de voorwaarden die ertoe leiden dat het WAF moet worden geblokkeerd dat de aanvraag in de Preventiemodus (in de detectiemodus die ze zijn aangemeld als uitzonderingen). Deze kunnen niet worden geconfigureerd of uitgeschakeld:

* Fout bij parseren van de hoofdtekst van de aanvraag resulteert in de aanvraag wordt geblokkeerd, tenzij de hoofdtekst van de controle is uitgeschakeld (XML, JSON, formuliergegevens)
* Gegevenslengte aanvraag-instantie (met geen bestanden) is groter dan de geconfigureerde limiet
* De aanvraag hoofdtekst (met inbegrip van bestanden) is groter dan de limiet
* Er is een interne fout opgetreden in de WAF-engine

CRS 3.x specifieke:

* Inkomende anomaliedetectie score overschreden drempelwaarde

## <a name="next-steps"></a>Volgende stappen

Nadat u uw uitgeschakelde regels hebt geconfigureerd, leert u hoe u om uw WAF-logboeken weer te geven. Zie voor meer informatie, [Application Gateway Diagnostics](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
