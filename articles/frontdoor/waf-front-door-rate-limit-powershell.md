---
title: Een web application firewall tarief limiet regel configureren voor voordeur - Azure PowerShell
description: Informatie over het configureren van een regel voor het beperken van tarief voor een bestaande voordeur-eindpunt.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/16/2019
ms.author: kumud;tyao
ms.openlocfilehash: 3701a69ab72abf20a4f1608a1cee56c9cea38aca
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65523640"
---
# <a name="configure-a-web-application-firewall-rate-limit-rule-using-azure-powershell"></a>Een web application tarief limiet firewallregel configureren met behulp van Azure PowerShell
De Azure-web application firewall (WAF) tarief limiet regel voor Azure voordeur bepaalt het aantal aanvragen dat is toegestaan in een enkele client-IP-adres tijdens een duur van één minuut.
In dit artikel laat zien hoe het configureren van een WAF tarief limiet regel die bepaalt het aantal aanvragen dat is toegestaan in een enkele client naar een webtoepassing met */promo* in de URL met behulp van Azure PowerShell.

> [!IMPORTANT]
> De functie voor WAF tarief limiet regel voordeur voor Azure is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten
Voordat u begint met het instellen van een beleid voor frequentielimiet, instellen van uw PowerShell-omgeving en een voordeur-profiel maken.
### <a name="set-up-your-powershell-environment"></a>Uw PowerShell-omgeving instellen
Azure PowerShell voorziet in een set van cmdlets die gebruikmaken van het [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)-model om uw Azure-resources te beheren. 

U kunt [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) op uw lokale computer installeren en in elke PowerShell-sessie gebruiken. Volg de instructies op de pagina, meld u aan met uw Azure-referenties, en installeer Az PowerShell-module.

#### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Verbinding maken met Azure met een interactieve dialoogvenster voor aanmelding bij
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

## <a name="define-url-match-conditions"></a>Url-criteria voor overeenkomst definiëren
Een voorwaarde voor URL-overeenkomst (URL bevat /promo) definiëren met behulp van [New-AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject).
Het volgende voorbeeld komt overeen met */promo* als de waarde van de *RequestUri* variabele:

```powershell-interactive
   $promoMatchCondition = New-AzFrontDoorWafMatchConditionObject `
     -MatchVariable RequestUri `
     -OperatorProperty Contains `
     -MatchValue "/promo"
```
## <a name="create-a-custom-rate-limit-rule"></a>Een aangepaste tarief limiet-regel maken
Stel een snelheid beperken met [New-AzFrontDoorCustomRuleObject](/powershell/module/az.frontdoor/new-azfrontdoorwafcustomruleobject). In het volgende voorbeeld wordt is de limiet ingesteld op 1000. Aanvragen van een client naar de pagina van de aanbieding van meer dan 1000 gedurende één minuut worden geblokkeerd totdat de volgende minuut wordt gestart.

```powershell-interactive
   $promoRateLimitRule = New-AzFrontDoorCustomRuleObject `
     -Name "rateLimitRule" `
     -RuleType RateLimitRule `
     -MatchCondition $promoMatchCondition `
     -RateLimitThreshold 1000 `
     -Action Block -Priority 1
```


## <a name="configure-a-security-policy"></a>Een beveiligingsbeleid configureren

Zoek de naam van de resourcegroep waarin de voordeur via `Get-AzureRmResourceGroup`. Vervolgens wordt een beveiligingsbeleid configureren met een aangepaste tarief limiet regel met [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy) in de opgegeven resourcegroep gemaakt met de voordeur-profiel.

Het onderstaande voorbeeld gebruikt u de naam van de Resource *myResourceGroupFD1* profiel met de veronderstelling dat u de voordeur hebt gemaakt met behulp van instructies hiervoor vindt u de [Quick Start: Maken van een voordeur](quickstart-create-front-door.md) artikel.

 using [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy).

```powershell-interactive
   $ratePolicy = New-AzFrontDoorWafPolicy `
     -Name "RateLimitPolicyExamplePS" `
     -resourceGroupName myResourceGroupFD1 `
     -Customrule $promoRateLimitRule `
     -Mode Prevention `
     -EnabledState Enabled
```
## <a name="link-policy-to-a-front-door-front-end-host"></a>Voor de koppeling naar een front-end-host van de voordeur
De security policy object koppelen aan een bestaande voordeur front-host en de voordeur eigenschappen bijwerken. Eerst ophalen van de voordeur object via [Get-AzFrontDoor](/powershell/module/Az.FrontDoor/Get-AzFrontDoor) opdracht.
Vervolgens stelt de front-end *WebApplicationFirewallPolicyLink* eigenschap in op de *resourceId* van de '$ratePolicy' gemaakt in de vorige stap met [Set AzFrontDoor](/powershell/module/Az.FrontDoor/Set-AzFrontDoor) de opdracht. 

Het onderstaande voorbeeld gebruikt u de naam van de Resource *myResourceGroupFD1* profiel met de veronderstelling dat u de voordeur hebt gemaakt met behulp van instructies hiervoor vindt u de [Quick Start: Maken van een voordeur](quickstart-create-front-door.md) artikel. Ook in het onderstaande voorbeeld, vervangt u $frontDoorName met de naam van uw profiel voordeur. 

```powershell-interactive
   $FrontDoorObjectExample = Get-AzFrontDoor `
     -ResourceGroupName myResourceGroupFD1 `
     -Name $frontDoorName
   $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $ratePolicy.Id
   Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
 ```

> [!NOTE]
> U hoeft alleen om in te stellen *WebApplicationFirewallPolicyLink* eigenschap eenmaal een beveiligingsbeleid koppelen aan een front-end-voordeur. Voor latere beleidsupdates worden automatisch toegepast op de front-end.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [voordeur](front-door-overview.md) 


