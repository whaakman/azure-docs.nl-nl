---
title: Een web application firewall (WAF)-beleid configureren met aangepaste regels en standaard Ruse ingesteld voor de voordeur - Azure PowerShell
description: Informatie over het configureren van een WAF beleid bestaan uit zowel beheerde als aangepaste regels voor een bestaande voordeur-eindpunt.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/21/2019
ms.author: kumud;tyao
ms.openlocfilehash: ff8330ab8aec7f0e9aa92409ce1eafd5be5ceeaf
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605759"
---
# <a name="configure-a-web-application-firewall-policy-using-azure-powershell"></a>Configureren van een web application firewall-beleid met behulp van Azure PowerShell
Azure-web application firewall (WAF)-beleid definieert inspecties vereist wanneer een aanvraag bij de voordeur aankomt.
In dit artikel laat zien hoe een WAF-beleid configureren dat bestaat uit van bepaalde aangepaste regels en met Azure beheerde standaard Ruse Set ingeschakeld.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten
Voordat u begint met het instellen van een beleid voor frequentielimiet, instellen van uw PowerShell-omgeving en een voordeur-profiel maken.
### <a name="set-up-your-powershell-environment"></a>Uw PowerShell-omgeving instellen
Azure PowerShell voorziet in een set van cmdlets die gebruikmaken van het [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)-model om uw Azure-resources te beheren. 

U kunt [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) op uw lokale computer installeren en in elke PowerShell-sessie gebruiken. Volg de instructies op de pagina, meld u aan met uw Azure-referenties, en installeer Az PowerShell-module.

#### <a name="sign-in-to-azure"></a>Aanmelden bij Azure
```
Connect-AzAccount

```
Voordat u de Front Door-module installeert, moet u controleren of u de nieuwste versie van PowerShellGet hebt geïnstalleerd. Voer de onderstaande opdracht uit en open PowerShell opnieuw.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

#### <a name="install-azfrontdoor-module"></a>Az.FrontDoor-module installeren 

```
Install-Module -Name Az.FrontDoor
```
### <a name="create-a-front-door-profile"></a>Maak een profiel van de voordeur
Een profiel voordeur maken door de instructies die worden beschreven in [Quick Start: Maak een profiel van de voordeur](quickstart-create-front-door.md)

## <a name="custom-rule-based-on-http-parameters"></a>Aangepaste regel op basis van HTTP-parameters

Het volgende voorbeeld laat zien hoe het configureren van een aangepaste regel met twee criteria voor overeenkomst met behulp van [New-AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject). Aanvragen zijn vanaf een opgegeven site, zoals gedefinieerd door de verwijzende site en querytekenreeks bevat geen 'wachtwoord'. 

```powershell-interactive
$referer = New-AzFrontDoorWafMatchConditionObject -MatchVariable RequestHeader -OperatorProperty Equal -Selector "Referer" -MatchValue "www.mytrustedsites.com/referpage.html"
$password = New-AzFrontDoorWafMatchConditionObject -MatchVariable QueryString -OperatorProperty Contains -MatchValue "password"
$AllowFromTrustedSites = New-AzFrontDoorWafCustomRuleObject -Name "AllowFromTrustedSites" -RuleType MatchRule -MatchCondition $referer,$password -Action Allow -Priority 1
```

## <a name="custom-rule-based-on-http-request-method"></a>Aangepaste regel gebaseerd op http-aanvraagmethode
Maak een regel blokkeren 'Plaats' methode met behulp van [New-AzFrontDoorWafCustomRuleObject](/powershell/module/az.frontdoor/new-azfrontdoorwafcustomruleobject) als volgt:

```powershell-interactive
$put = New-AzFrontDoorWafMatchConditionObject -MatchVariable RequestMethod -OperatorProperty Equal -MatchValue PUT
$BlockPUT = New-AzFrontDoorWafCustomRuleObject -Name "BlockPUT" -RuleType MatchRule -MatchCondition $put -Action Block -Priority 2
```

## <a name="create-a-custom-rule-based-on-size-constraint"></a>Een aangepaste regel gebaseerd op de formaat beperking maken

Het volgende voorbeeld wordt een regel voor het blokkeren van aanvragen met de Url die langer is dan 100 tekens met behulp van Azure PowerShell:
```powershell-interactive
$url = New-AzFrontDoorWafMatchConditionObject -MatchVariable RequestUri -OperatorProperty GreaterThanOrEqual -MatchValue 100
$URLOver100 = New-AzFrontDoorWafCustomRuleObject -Name "URLOver100" -RuleType MatchRule -MatchCondition $url -Action Block -Priority 3
```
## <a name="add-managed-default-rule-set"></a>Beheerde standaard regelset toevoegen

Het volgende voorbeeld wordt een beheerde standaard regel ingesteld met behulp van Azure PowerShell:
```powershell-interactive
$managedRules =  New-AzFrontDoorWafManagedRuleObject -Type DefaultRuleSet -Version 1.0
```
## <a name="configure-a-security-policy"></a>Een beveiligingsbeleid configureren

Zoek de naam van de resourcegroep waarin de voordeur via `Get-AzResourceGroup`. Configureer vervolgens een beveiligingsbeleid met gemaakte regels in de vorige stappen [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy) in de opgegeven resourcegroep gemaakt met de voordeur-profiel.

```powershell-interactive
$myWAFPolicy=New-AzFrontDoorWafPolicy -Name $policyName -ResourceGroupName $resourceGroupName -Customrule $AllowFromTrustedSites,$BlockPUT,$URLOver100 -ManagedRule $managedRules -EnabledState Enabled -Mode Prevention
```

## <a name="link-policy-to-a-front-door-front-end-host"></a>Voor de koppeling naar een front-end-host van de voordeur
De security policy object koppelen aan een bestaande voordeur front-host en de voordeur eigenschappen bijwerken. Eerst, halen de voordeur object via [Get-AzFrontDoor](/powershell/module/Az.FrontDoor/Get-AzFrontDoor).
Vervolgens stelt de front-end *WebApplicationFirewallPolicyLink* eigenschap in op de *resourceId* van de "$myWAFPolicy$ ' gemaakt in de vorige stap met [Set-AzFrontDoor](/powershell/module/Az.FrontDoor/Set-AzFrontDoor). 

Het onderstaande voorbeeld gebruikt u de naam van de Resource *myResourceGroupFD1* profiel met de veronderstelling dat u de voordeur hebt gemaakt met behulp van instructies hiervoor vindt u de [Quick Start: Maken van een voordeur](quickstart-create-front-door.md) artikel. Ook in het onderstaande voorbeeld, vervangt u $frontDoorName met de naam van uw profiel voordeur. 

```powershell-interactive
   $FrontDoorObjectExample = Get-AzFrontDoor `
     -ResourceGroupName myResourceGroupFD1 `
     -Name $frontDoorName
   $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $myWAFPolicy.Id
   Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
 ```

> [!NOTE]
> U hoeft alleen om in te stellen *WebApplicationFirewallPolicyLink* eigenschap eenmaal een beveiligingsbeleid koppelen aan een front-end-voordeur. Voor latere beleidsupdates worden automatisch toegepast op de front-end.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [voordeur](front-door-overview.md) 
- Meer informatie over [WAF voor de voordeur](waf-overview.md)
