---
title: Azure PowerShell-voorbeeldscript - het maken van aangepaste regels voor WAF
description: Azure PowerShell-voorbeeldscript - het maken van aangepaste Web Application Firewall-regels
author: vhorne
ms.service: application-gateway
ms.topic: sample
ms.date: 6/7/2019
ms.author: victorh
ms.openlocfilehash: ffdde80598322222e2a8f000eee8be269becdd11
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743421"
---
# <a name="create-waf-custom-rules-with-azure-powershell"></a>WAF aangepaste regels maken met Azure PowerShell

Dit script maakt u een Application Gateway Web Application Firewall die gebruikmaakt van aangepaste regels. De aangepaste regel blokken verkeer als de kop van de aanvraag gebruikersagent bevat *evilbot*.

## <a name="prerequisites"></a>Vereisten

### <a name="azure-powershell-module"></a>Azure PowerShell-module

Als u wilt installeren en gebruiken van Azure PowerShell lokaal, met dit script is vereist voor de Azure PowerShell-moduleversie 2.1.0 of hoger.

1. Voer `Get-Module -ListAvailable Az` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps).
2. Voer `Connect-AzAccount` uit om een verbinding met Azure tot stand te brengen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/application-gateway/waf-rules/waf-custom-rules.ps1 "Custom WAF rules")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Gebruik de volgende opdracht om de resourcegroep, de toepassingsgateway en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzResourceGroup -Name CustomRulesTest
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt om de implementatie te maken. Elk item in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Hiermee maakt u de subnetconfiguratie. |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Hiermee maakt u het virtuele netwerk met de subnetconfiguraties. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Hiermee maakt u het openbare IP-adres voor de toepassingsgateway. |
| [New-AzApplicationGatewayIPConfiguration](/powershell/module/az.network/new-azapplicationgatewayipconfiguration) | Hiermee maakt u de configuratie die een subnet koppelt aan de toepassingsgateway. |
| [New-AzApplicationGatewayFrontendIPConfig](/powershell/module/az.network/new-azapplicationgatewayfrontendipconfig) | Hiermee maakt u de configuratie die een openbaar IP-adres toewijst aan de toepassingsgateway. |
| [New-AzApplicationGatewayFrontendPort](/powershell/module/az.network/new-azapplicationgatewayfrontendport) | Hiermee wijst u een poort toe die moet worden gebruikt voor toegang tot de toepassingsgateway. |
| [New-AzApplicationGatewayBackendAddressPool](/powershell/module/az.network/new-azapplicationgatewaybackendaddresspool) | Hiermee maakt u een back-endpool voor een toepassingsgateway. |
| [New-AzApplicationGatewayBackendHttpSettings](/powershell/module/az.network/new-azapplicationgatewaybackendhttpsetting) | Hiermee configureert u instellingen voor een back-endpool. |
| [New-AzApplicationGatewayHttpListener](/powershell/module/az.network/new-azapplicationgatewayhttplistener) | Hiermee maakt u een listener. |
| [New-AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/new-azapplicationgatewayrequestroutingrule) | Hiermee maakt u een routeringsregel. |
| [New-AzApplicationGatewaySku](/powershell/module/az.network/new-azapplicationgatewaysku) | Hiermee geeft u de laag en capaciteit voor een toepassingsgateway op. |
| [New-AzApplicationGateway](/powershell/module/az.network/new-azapplicationgateway) | Maak een toepassingsgateway. |
|[Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Hiermee verwijdert u een resourcegroep en alle daarin opgenomen resources. |
|[New-AzApplicationGatewayAutoscaleConfiguration](/powershell/module/az.network/New-AzApplicationGatewayAutoscaleConfiguration)|Hiermee maakt u een configuratie voor automatisch schalen voor de toepassingsgateway.|
|[New-AzApplicationGatewayFirewallMatchVariable](/powershell/module/az.network/New-AzApplicationGatewayFirewallMatchVariable)|Hiermee maakt u een variabele overeenkomst voor firewall-voorwaarde.|
|[New-AzApplicationGatewayFirewallCondition](/powershell/module/az.network/New-AzApplicationGatewayFirewallCondition)|Hiermee maakt u een voorwaarde voor overeenkomst voor aangepaste regel.|
|[New-AzApplicationGatewayFirewallCustomRule](/powershell/module/az.network/New-AzApplicationGatewayFirewallCustomRule)|Hiermee maakt u een nieuwe aangepaste regel voor de application gateway firewall-beleid.|
|[New-AzApplicationGatewayFirewallPolicy](/powershell/module/az.network/New-AzApplicationGatewayFirewallPolicy)|Hiermee maakt u een application gateway firewall-beleid.|
|[New-AzApplicationGatewayWebApplicationFirewallConfiguration](/powershell/module/az.network/New-AzApplicationGatewayWebApplicationFirewallConfiguration)|Hiermee maakt u een WAF-configuratie voor een toepassingsgateway.|

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over aangepaste regels voor WAF [aangepaste regels voor Web Application Firewall](../custom-waf-rules-overview.md)
- Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).
- Voorbeelden van extra PowerShell-voorbeeldscripts voor de toepassingsgateway zijn te vinden in de [documentatie over Azure Application Gateway](../powershell-samples.md).
