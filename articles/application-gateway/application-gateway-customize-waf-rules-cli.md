---
title: Regels voor web application firewall in Azure Application Gateway - Azure CLI aanpassen
description: In dit artikel bevat informatie over het aanpassen van regels voor web application firewall in Application Gateway met de Azure CLI.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 2/22/2019
ms.author: victorh
ms.openlocfilehash: 5e364c597b8c524e95297f279003462f2d16abe1
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56726258"
---
# <a name="customize-web-application-firewall-rules-through-the-azure-cli"></a>Regels voor web application firewall via de Azure CLI aanpassen

De Azure Application Gateway web application firewall (WAF) biedt beveiliging voor webtoepassingen. Deze beveiligingen worden geleverd door de Open Web Application Security Project (OWASP) Core regel ingesteld (CRS). Sommige regels kunnen fout-positieven en echte verkeer blokkeert. Om deze reden biedt Application Gateway de mogelijkheid om aan te passen regelgroepen en -regels. Zie voor meer informatie over de specifieke regelgroepen en regels [lijst met web application firewall CRS-regelgroepen en -regels](application-gateway-crs-rulegroups-rules.md).

## <a name="view-rule-groups-and-rules"></a>Weergave-regelgroepen en -regels

De volgende codevoorbeelden laten zien hoe regels en regelgroepen die kunnen worden geconfigureerd.

### <a name="view-rule-groups"></a>Groepen van de regel weergeven

Het volgende voorbeeld laat zien hoe de regelgroepen weergeven:

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --type OWASP
```

De volgende uitvoer is een afgekapte reactie van het vorige voorbeeld:

```json
[
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_3.0",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "REQUEST-910-IP-REPUTATION",
        "rules": null
      },
      ...
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  },
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_2.2.9",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
   "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "crs_20_protocol_violations",
        "rules": null
      },
      ...
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "2.2.9",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  }
]
```

### <a name="view-rules-in-a-rule-group"></a>Regels weergeven in de regelgroep van een

Het volgende voorbeeld laat zien hoe om regels in een opgegeven regel-groep weer te geven:

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --group "REQUEST-910-IP-REPUTATION"
```

De volgende uitvoer is een afgekapte reactie van het vorige voorbeeld:

```json
[
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_3.0",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "REQUEST-910-IP-REPUTATION",
        "rules": [
          {
            "description": "Rule 910011",
            "ruleId": 910011
          },
          ...
        ]
      }
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  }
]
```

## <a name="disable-rules"></a>Uitschakelen van regels

Het volgende voorbeeld wordt uitgeschakeld regels `910018` en `910017` in een toepassingsgateway:

```azurecli-interactive
az network application-gateway waf-config set --resource-group AdatumAppGatewayRG --gateway-name AdatumAppGateway --enabled true --rule-set-version 3.0 --disabled-rules 910018 910017
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

Nadat u uw uitgeschakelde regels hebt geconfigureerd, leert u hoe u om uw WAF-logboeken weer te geven. Zie voor meer informatie, [Application Gateway diagnostics](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
