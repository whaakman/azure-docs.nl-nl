---
title: Herschrijf de HTTP-headers in Azure Application Gateway
description: Dit artikel bevat informatie over het herschrijven van HTTP-headers in Azure Application Gateway met behulp van Azure PowerShell
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/12/2019
ms.author: absha
ms.openlocfilehash: 47fe6a5247622e3ad3b3720955068580e0329913
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64947193"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-powershell"></a>Herschrijf de HTTP-aanvraag- en reactieheaders met Azure Application Gateway - Azure PowerShell

In dit artikel wordt beschreven hoe u Azure PowerShell gebruiken voor het configureren van een [Application Gateway v2 SKU](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) exemplaar te herschrijven van de HTTP-headers in aanvragen en antwoorden.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="before-you-begin"></a>Voordat u begint

- U moet Azure PowerShell als de stappen in dit artikel lokaal wilt uitvoeren. U moet ook zijn Az moduleversie 1.0.0 of hoger is geïnstalleerd. Voer `Import-Module Az` en vervolgens `Get-Module Az` om te bepalen van de versie die u hebt geïnstalleerd. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](https://docs.microsoft.com/powershell/azure/install-az-ps). Nadat u de versie van PowerShell hebt gecontroleerd, voert u `Login-AzAccount` uit om een verbinding op te zetten met Azure.
- U moet een Application Gateway v2 SKU-exemplaar. Headers herschrijven wordt niet in de v1-SKU ondersteund. Als u de v2-SKU hebt, maakt u een [Application Gateway v2 SKU](https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps) exemplaar voordat u begint.

## <a name="create-required-objects"></a>Vereiste objecten maken

Voor het configureren van HTTP-header herschrijven, moet u deze stappen hebt voltooid.

1. Maak de objecten die vereist voor HTTP-header herschrijven zijn:

   - **RequestHeaderConfiguration**: Gebruikt om op te geven velden in de aanvraagheader die u van plan bent om te schrijven en de nieuwe waarde voor de headers.

   - **ResponseHeaderConfiguration**: Hiermee geeft de antwoord-header-velden die u van plan bent om te schrijven en de nieuwe waarde voor de headers.

   - **ActionSet**: Bevat de configuratie van de aanvraag- en reactieheaders die eerder zijn opgegeven.

   - **Voorwaarde**: Een optionele configuratie. Herschrijven voorwaarden evalueren de inhoud van HTTP (S) aanvragen en antwoorden. De actie herschrijven treedt op als de HTTP (S)-aanvraag of antwoord komt overeen met de voorwaarde herschrijven.

     Als u meer dan één voorwaarde aan een actie koppelen, wordt de actie optreedt, alleen wanneer alle voorwaarden wordt voldaan. De bewerking is met andere woorden, een logische AND-bewerking.

   - **RewriteRule**: Bevat meerdere herschrijven actie / Herschrijf de voorwaarde combinaties.

   - **RuleSequence**: Een optionele configuratie die helpt te bepalen in welke volgorde de regels schrijven uitvoeren. Deze configuratie is handig wanneer u meerdere herschrijvingsregels in een set herschrijven hebt. Eerste een herschrijvingsregel voor met een lagere waarde van de regel-reeks wordt gestart. Als u dezelfde regel reekswaarde aan twee herschrijvingsregels toewijzen, is de volgorde van de uitvoering van niet-deterministisch.

     Als u niet expliciet de RuleSequence opgeeft, wordt een standaardwaarde van 100 ingesteld.

   - **RewriteRuleSet**: Bevat meerdere herschrijvingsregels dat gekoppeld aan een regel voor het doorsturen van aanvragen is.

2. De RewriteRuleSet koppelen aan een regel voor doorsturen. De configuratie opnieuw schrijven is gekoppeld aan de bron-listener via de regel voor doorsturen. Wanneer u een regel voor het doorsturen van basic gebruikt, wordt de configuratie van de herschrijven header is gekoppeld aan de listener van een bron en is een algemene header herschrijven. Wanneer u een pad gebaseerde routering regel gebruikt, wordt de configuratie van de koptekst opnieuw schrijven is gedefinieerd in de URL-path-map. In dat geval van de toepassing alleen op het padgebied van het specifieke van een site.

U kunt meerdere sets van HTTP-header herschrijven maken en toepassen van elke opnieuw ingesteld op meerdere listeners. Maar u kunt alleen een is ingesteld op een specifieke listener opnieuw toepassen.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="specify-the-http-header-rewrite-rule-configuration"></a>Geef de HTTP-header herschrijven regelconfiguratie

In dit voorbeeld wijzigen we een Omleidings-URL door de location-header in het HTTP-antwoord voor het herschrijven wanneer de location-header een verwijzing naar azurewebsites.net bevat. Om dit te doen, gaan we een voorwaarde om te beoordelen of de location-header in het antwoord azurewebsites.net bevat toevoegen. We gebruiken het patroon `(https?):\/\/.*azurewebsites\.net(.*)$`. En we gebruiken `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` als waarde van de header. Deze waarde wordt vervangen door *azurewebsites.net* met *contoso.com* in de location-header.

```azurepowershell
$responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Location" -HeaderValue "{http_resp_Location_1}://contoso.com{http_resp_Location_2}"
$actionSet = New-AzApplicationGatewayRewriteRuleActionSet -RequestHeaderConfiguration $requestHeaderConfiguration -ResponseHeaderConfiguration $responseHeaderConfiguration
$condition = New-AzApplicationGatewayRewriteRuleCondition -Variable "http_resp_Location" -Pattern "(https?):\/\/.*azurewebsites\.net(.*)$" -IgnoreCase
$rewriteRule = New-AzApplicationGatewayRewriteRule -Name LocationHeader -ActionSet $actionSet
$rewriteRuleSet = New-AzApplicationGatewayRewriteRuleSet -Name LocationHeaderRewrite -RewriteRule $rewriteRule
```

## <a name="retrieve-the-configuration-of-your-application-gateway"></a>Ophalen van de configuratie van uw application gateway

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
```

## <a name="retrieve-the-configuration-of-your-request-routing-rule"></a>De configuratie van de regel voor het doorsturen van aanvraag ophalen

```azurepowershell
$reqRoutingRule = Get-AzApplicationGatewayRequestRoutingRule -Name rule1 -ApplicationGateway $appgw
```

## <a name="update-the-application-gateway-with-the-configuration-for-rewriting-http-headers"></a>Bijwerken van de application gateway met de configuratie voor het herschrijven van HTTP-headers

```azurepowershell
Add-AzApplicationGatewayRewriteRuleSet -ApplicationGateway $appgw -Name LocationHeaderRewrite -RewriteRule $rewriteRuleSet.RewriteRules
Set-AzApplicationGatewayRequestRoutingRule -ApplicationGateway $appgw -Name rule1 -RuleType $reqRoutingRule.RuleType -BackendHttpSettingsId $reqRoutingRule.BackendHttpSettings.Id -HttpListenerId $reqRoutingRule.HttpListener.Id -BackendAddressPoolId $reqRoutingRule.BackendAddressPool.Id -RewriteRuleSetId $rewriteRuleSet.Id
Set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="delete-a-rewrite-rule"></a>Een herschrijvingsregel voor verwijderen

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
Remove-AzApplicationGatewayRewriteRuleSet -Name "LocationHeaderRewrite" -ApplicationGateway $appgw
$requestroutingrule= Get-AzApplicationGatewayRequestRoutingRule -Name "rule1" -ApplicationGateway $appgw
$requestroutingrule.RewriteRuleSet= $null
set-AzApplicationGateway -ApplicationGateway $appgw
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het instellen van enkele veelvoorkomende use cases, [algemene header scenario's herschrijven](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).