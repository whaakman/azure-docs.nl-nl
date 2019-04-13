---
title: Herschrijf de HTTP-headers in Azure Application Gateway
description: Dit artikel bevat informatie over het herschrijven van HTTP-headers in Azure Application Gateway met behulp van Azure PowerShell
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/12/2019
ms.author: absha
ms.openlocfilehash: bfafc74cbcb97f28cc085196a2cbaf4e9bf2e871
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/13/2019
ms.locfileid: "59548316"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-powershell"></a>Herschrijf de HTTP-aanvraag- en reactieheaders met Azure Application Gateway - Azure PowerShell

Dit artikel ziet u hoe u Azure PowerShell gebruiken voor het configureren van een [Application Gateway v2 SKU](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) te herschrijven van de HTTP-headers in de aanvragen en antwoorden.

> [!IMPORTANT]
> De SKU met automatisch schalen en zone-redundantie voor toepassingsgateways is momenteel in openbare preview. Deze preview wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

- Voor deze zelfstudie moet u Azure PowerShell lokaal uitvoeren. U moet Az moduleversie 1.0.0 of hoger is geïnstalleerd. Voer `Import-Module Az` en vervolgens`Get-Module Az` de versie te vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](https://docs.microsoft.com/powershell/azure/install-az-ps). Nadat u de versie van PowerShell hebt gecontroleerd, voert u `Login-AzAccount` uit om een verbinding op te zetten met Azure.
- U moet een Application Gateway-v2 SKU sinds de mogelijkheid tot herschrijven van de koptekst wordt niet ondersteund voor de v1-SKU. Als u de v2-SKU hebt, maakt u een [Application Gateway v2 SKU](https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps>) voordat u begint.

## <a name="what-is-required-to-rewrite-a-header"></a>Wat is vereist voor het herschrijven van een koptekst

Voor het configureren van HTTP-header herschrijven, moet u naar:

1. De nieuwe objecten die zijn vereist voor het herschrijven van de http-headers maken:

   - **RequestHeaderConfiguration**: dit object wordt gebruikt om velden in de aanvraagheader die u van plan bent om te schrijven en de nieuwe waarde die de oorspronkelijke kopteksten worden herschreven moeten aan te geven.

   - **ResponseHeaderConfiguration**: dit object wordt gebruikt om de antwoord-header-velden die u van plan bent te herschrijven en de nieuwe waarde die de oorspronkelijke kopteksten worden herschreven moeten aan te geven.

   - **ActionSet**: dit object bevat de configuratie van de aanvraag- en reactieheaders hierboven opgegeven.

   - **Voorwaarde**: Het is een optionele configuratie. Als een voorwaarde herschrijven wordt toegevoegd, wordt de inhoud van het HTTP (S) aanvragen en antwoorden geëvalueerd. De beslissing om uit te voeren van de herschrijven actie die is gekoppeld aan de voorwaarde herschrijven zijn gebaseerd of het HTTP (S)-aanvraag of antwoord met de voorwaarde herschrijven overeenkomt. 

     Als meer dan één voorwaarden zijn gekoppeld aan een actie, en vervolgens de actie wordt uitgevoerd alleen als alle voorwaarden wordt voldaan, dat wil zeggen, een logische AND-bewerking wordt uitgevoerd.

   - **RewriteRule**: bevat meerdere herschrijven van de virtuele - herschrijven voorwaarde combinaties.

   - **RuleSequence**: Dit is een optionele configuratie. Het helpt te bepalen van de volgorde waarin de verschillende herschrijvingsregels uitgevoerd. Dit is handig wanneer er meerdere herschrijvingsregels in een set herschrijven. De herschrijvingsregel met minder regel reekswaarde wordt eerst uitgevoerd. Als u de dezelfde volgorde van de regel aan twee herschrijvingsregels wordt de volgorde van de uitvoering van niet-deterministisch zijn.

     Als u niet expliciet de RuleSequence opgeeft, wordt een standaardwaarde van 100 worden ingesteld.

   - **RewriteRuleSet**: dit object bevat meerdere herschrijvingsregels die wordt gekoppeld aan een regel voor het doorsturen van aanvragen.

2. U moet de rewriteRuleSet met een regel voor doorsturen koppelen. Dit is omdat de configuratie opnieuw schrijven is gekoppeld aan de bron-listener via de regel voor doorsturen. Wanneer u een basisregel voor routering, wordt de configuratie van de herschrijven header is gekoppeld aan de listener van een bron en is een algemene header herschrijven. Wanneer een regel op pad gebaseerde routering wordt gebruikt, wordt de configuratie van de koptekst opnieuw schrijven is gedefinieerd in de URL-path-map. Dus dit alleen van toepassing op het padgebied van het specifieke van een site.

U kunt meerdere sets van http-header herschrijven maken en elke set herschrijven kan worden toegepast op meerdere listeners. U kunt echter alleen een is ingesteld op een specifieke listener opnieuw toepassen.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="specify-your-http-header-rewrite-rule-configuration"></a>**Geef de HTTP-header herschrijven regelconfiguratie**

In dit voorbeeld wijzigen we de omleidings-URL voor de location-header in het http-antwoord herschrijven wanneer de location-header een verwijzing naar 'azurewebsites.net bevat'. U doet dit door een voorwaarde om te beoordelen of de location-header in het antwoord azurewebsites.net bevat met behulp van het patroon wordt toegevoegd `(https?):\/\/.*azurewebsites\.net(.*)$`. We gebruiken `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` als waarde van de header. Dit wordt vervangen door *azurewebsites.net* met *contoso.com* in de location-header.

```azurepowershell
$responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Location" -HeaderValue "{http_resp_Location_1}://contoso.com{http_resp_Location_2}"
$actionSet = New-AzApplicationGatewayRewriteRuleActionSet -RequestHeaderConfiguration $requestHeaderConfiguration -ResponseHeaderConfiguration $responseHeaderConfiguration
$condition = New-AzApplicationGatewayRewriteRuleCondition -Variable "http_resp_Location" -Pattern "(https?):\/\/.*azurewebsites\.net(.*)$" -IgnoreCase
$rewriteRule = New-AzApplicationGatewayRewriteRule -Name LocationHeader -ActionSet $actionSet
$rewriteRuleSet = New-AzApplicationGatewayRewriteRuleSet -Name LocationHeaderRewrite -RewriteRule $rewriteRule
```

## <a name="retrieve-configuration-of-your-existing-application-gateway"></a>Configuratie van uw bestaande toepassingsgateway ophalen

```azurepowershell
$appgw = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
```

## <a name="retrieve-configuration-of-your-existing-request-routing-rule"></a>Configuratie van de bestaande regel voor het doorsturen van aanvraag ophalen

```azurepowershell
$reqRoutingRule = Get-AzApplicationGatewayRequestRoutingRule -Name rule1 -ApplicationGateway $appgw
```

## <a name="update-the-application-gateway-with-the-configuration-for-rewriting-http-headers"></a>Bijwerken van de application gateway met de configuratie voor het herschrijven van http-headers

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

Zie voor meer informatie over de configuratie is vereist om uit te voeren sommige van de algemene gebruiksvoorbeelden [algemene header scenario's herschrijven](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).