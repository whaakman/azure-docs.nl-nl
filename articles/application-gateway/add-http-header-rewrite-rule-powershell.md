---
title: Herschrijf de HTTP-headers in een bestaande Azure Application Gateway
description: Dit artikel bevat informatie over het herschrijven van HTTP-headers in een bestaande Azure-toepassingsgateway met behulp van Azure PowerShell
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 12/20/2018
ms.author: absha
ms.openlocfilehash: 68da63bcad3c670c5e8bda62dda656e29c41f899
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55692912"
---
# <a name="rewrite-http-headers-in-an-existing-application-gateway"></a>Herschrijf de HTTP-headers in een bestaande toepassingsgateway

U kunt Azure PowerShell gebruiken om te configureren [regels voor het herschrijven van HTTP-aanvraag- en reactieheaders](rewrite-http-headers.md) van een bestaande [automatisch schalen en zone-redundante application gateway-SKU](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant)

> [!IMPORTANT]
> De SKU met automatisch schalen en zone-redundantie voor toepassingsgateways is momenteel in openbare preview. Deze preview wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
>
> * Configuratie van een bestaande toepassingsgateway ophalen
> * Geef de HTTP-header herschrijven regelconfiguratie
> * Bijwerken van de application gateway met de bovenstaande configuratie voor het herschrijven van http-headers

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie moet u Azure PowerShell lokaal uitvoeren. U moet Az moduleversie 1.0.0 of hoger is geïnstalleerd. Voer `Import-Module Az` en vervolgens`Get-Module Az` de versie te vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps). Nadat u de versie van PowerShell hebt gecontroleerd, voert u `Login-AzAccount` uit om een verbinding op te zetten met Azure.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="specify-your-http-header-rewrite-rule-configuration"></a>**Geef de HTTP-header herschrijven regelconfiguratie**

De nieuwe objecten die zijn vereist voor het herschrijven van de http-headers configureren:

- **RequestHeaderConfiguration**: dit object wordt gebruikt om velden in de aanvraagheader die u van plan bent om te schrijven en de nieuwe waarde die de oorspronkelijke kopteksten worden herschreven moeten aan te geven.
- **ResponseHeaderConfiguration**: dit object wordt gebruikt om de antwoord-header-velden die u van plan bent te herschrijven en de nieuwe waarde die de oorspronkelijke kopteksten worden herschreven moeten aan te geven.
- **ActionSet**: dit object bevat de configuratie van de aanvraag- en reactieheaders hierboven opgegeven.
- **RewriteRule**: dit object bevat alle de *actionSets* hierboven opgegeven.
- **RewriteRuleSet**-dit object bevat alle de *rewriteRules* en moet worden gekoppeld aan een aanvraag routeringsregel - basis of op basis van een pad.

```azurepowershell
$requestHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "X-isThroughProxy" -HeaderValue "True"
$responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Strict-Transport-Security" -HeaderValue "max-age=31536000"
$actionSet = New-AzApplicationGatewayRewriteRuleActionSet -RequestHeaderConfiguration $requestHeaderConfiguration -ResponseHeaderConfiguration $responseHeaderConfiguration
$rewriteRule = New-AzApplicationGatewayRewriteRule -Name rewriteRule1 -ActionSet $actionSet
$rewriteRuleSet = New-AzApplicationGatewayRewriteRuleSet -Name rewriteRuleSet1 -RewriteRule $rewriteRule
```

## <a name="retrieve-configuration-of-your-existing-application-gateway"></a>Configuratie van uw bestaande toepassingsgateway ophalen

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
```

## <a name="retrieve-configuration-of-your-existing-request-routing-rule"></a>Configuratie van de bestaande regel voor het doorsturen van aanvraag ophalen

```azurepowershell
$reqRoutingRule = Get-AzApplicationGatewayRequestRoutingRule -Name Rule1 -ApplicationGateway $appgw
```

## <a name="update-the-application-gateway-with-the-configuration-for-rewriting-http-headers"></a>Bijwerken van de application gateway met de configuratie voor het herschrijven van http-headers

```azurepowershell
Add-AzApplicationGatewayRewriteRuleSet -ApplicationGateway $appgw -Name rewriteRuleSet1 -RewriteRule $rewriteRuleSet.RewriteRules
Set-AzApplicationGatewayRequestRoutingRule -ApplicationGateway $appgw -Name rule1 -RuleType $reqRoutingRule.RuleType -BackendHttpSettingsId $reqRoutingRule.BackendHttpSettings.Id -HttpListenerId $reqRoutingRule.HttpListener.Id -BackendAddressPoolId $reqRoutingRule.BackendAddressPool.Id -RewriteRuleSetId $rewriteRuleSet.Id
Set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="delete-a-rewrite-rule"></a>Een herschrijvingsregel voor verwijderen

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
Remove-AzApplicationGatewayRewriteRuleSet -Name "rewriteRuleSet1" -ApplicationGateway $appgw
$requestroutingrule= Get-AzApplicationGatewayRequestRoutingRule -Name "rule1" -ApplicationGateway $appgw
$requestroutingrule.RewriteRuleSet= $null
set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een toepassingsgateway maken met omleidingsregels op basis van een URL-pad](./tutorial-url-route-powershell.md)
