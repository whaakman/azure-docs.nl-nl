---
title: Bedreigingsinformatie van Azure Firewall filteren op basis van
description: Meer informatie over Azure-Firewall threat intelligence filteren
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 3/2/2019
ms.author: victorh
ms.openlocfilehash: 38b60536a05fc726ddcd06b631078c0a5f3d89ce
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57248898"
---
# <a name="azure-firewall-threat-intelligence-based-filtering---public-preview"></a>Azure Firewall threat intelligence gebaseerd filteren - openbare Preview

Threat intelligence filteren kan worden ingeschakeld voor de firewall op de hoogte en verkeer van/naar bekende schadelijke IP-adressen en domeinen weigeren. Het IP-adressen en domeinen zijn afkomstig van de Microsoft-Bedreigingsinformatie-feed. [Intelligent Security Graph](https://www.microsoft.com/en-us/security/operations/intelligence) wordt gebruikt door bedreigingsinformatie van Microsoft en wordt gebruikt door meerdere services, met inbegrip van Azure Security Center.

![Firewall-bedreigingsinformatie](media/threat-intel/firewall-threat.png)

> [!IMPORTANT]
> Threat intelligence filteren op basis van is momenteel in openbare preview en is voorzien van een Preview-versie service level agreement. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben.  Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
>Threat intelligence filteren is beschikbaar in alle openbare regio's. De gebruikersinterface van de bijbehorende configuratie stapsgewijs in regio's wordt toegevoegd en is beschikbaar in alle regio's in de nabije toekomst.

Als threat intelligence gebaseerde filtering is ingeschakeld, worden de gekoppelde regels worden verwerkt voordat een van de NAT-regels, netwerkregels of regels voor application. Tijdens de Preview-versie, zijn alleen de hoogste betrouwbaarheid records zijn opgenomen.

U kunt aan te melden een waarschuwing wanneer een regel wordt geactiveerd, of u kunt kiezen waarschuwing en weigeren modus.

Standaard is threat intelligence gebaseerde filtering ingeschakeld in de modus voor waarschuwingen. Deze functie uitschakelen kan of de modus wijzigen totdat de interface van de portal beschikbaar in uw regio.

![Informatie over bedreigingen op basis van filteren interface van de portal](media/threat-intel/threat-intel-ui.png)

## <a name="logs"></a>Logboeken

Het volgende fragment in de logboekbestanden ziet u een geactiveerde regel:

```
{
    "category": "AzureFirewallNetworkRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallThreatIntelLog",
    "properties": {
         "msg": "HTTP request from 10.0.0.5:54074 to somemaliciousdomain.com:80. Action: Alert. ThreatIntel: Bot Networks"
    }
}
```

## <a name="testing"></a>Testen

- **Uitgaande testen** -waarschuwingen voor uitgaand verkeer moet een zeldzame exemplaar, zoals betekent dit dat uw omgeving is aangetast. Om te werkt testen uitgaande waarschuwingen, test FQDN-naam is gemaakt die een waarschuwing wordt geactiveerd. Gebruik **testmaliciousdomain.eastus.cloudapp.azure.com** voor uw uitgaande tests.

- **Inkomende testen** -u kunt verwachten om waarschuwingen te zien op het binnenkomende verkeer als DNAT regels zijn geconfigureerd op de firewall. Dit geldt zelfs als alleen specifieke bronnen zijn toegestaan voor de regel DNAT en verkeer anders is geweigerd. Firewall van Azure niet wordt gewaarschuwd voor alle bekende poort scanners; alleen op scanners waarvan bekend is dat ook via een schadelijke activiteiten.

## <a name="next-steps"></a>Volgende stappen

- Zie [Azure Firewall Log Analytics-voorbeelden](log-analytics-samples.md)
- Meer informatie over het [implementeren en configureren van een Azure-Firewall](tutorial-firewall-deploy-portal.md)
- Controleer de [Microsoft Security intelligence report](https://www.microsoft.com/en-us/security/operations/security-intelligence-report)