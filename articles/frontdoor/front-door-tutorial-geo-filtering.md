---
title: Zelfstudie - geo-filtering configureren web application firewall-beleid voor de voordeur Azure-service
description: In deze zelfstudie leert u hoe u een eenvoudig beleid voor geofilters maakt en het beleid koppelt aan uw bestaande front-endhost van uw Front Door
services: frontdoor
documentationcenter: ''
author: KumudD
manager: twooley
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: kumud;tyao
ms.openlocfilehash: 371347149b3c3f14784ba62365cfd6224ded99d1
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407331"
---
# <a name="how-to-set-up-a-geo-filtering-waf-policy-for-your-front-door"></a>Over het instellen van een filterfunctie voor geografische WAF-beleid voor de voordeur
In deze zelfstudie leert u hoe u Azure PowerShell gebruikt om een voorbeeldbeleid voor geofilters te maken en het beleid koppelt aan uw bestaande front-endhost van uw Front Door. In dit voorbeeldbeleid voor geofilters worden aanvragen uit alle andere landen, met uitzondering van de Verenigde Staten, geblokkeerd.

Als u nog geen abonnement op Azure hebt, maak dan nu een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Vereisten
Voordat u begint met het instellen van een beleid voor geo-filter, instellen van uw PowerShell-omgeving en een voordeur-profiel maken.
### <a name="set-up-your-powershell-environment"></a>Uw PowerShell-omgeving instellen
Azure PowerShell voorziet in een set van cmdlets die gebruikmaken van het [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)-model om uw Azure-resources te beheren. 

U kunt [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) op uw lokale computer installeren en in elke PowerShell-sessie gebruiken. Volg de instructies op de pagina, meld u aan met uw Azure-referenties, en installeer Az PowerShell-module.

#### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Verbinding maken met Azure met een interactieve dialoogvenster voor aanmelding bij
```
Connect-AzAccount
Install-Module -Name Az
```
Zorg ervoor dat u de huidige versie van PowerShellGet geïnstalleerd hebben. Voer de onderstaande opdracht uit en open PowerShell opnieuw.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

### <a name="create-a-front-door-profile"></a>Maak een profiel van de voordeur
Een profiel voordeur maken door de instructies die worden beschreven in [Quick Start: Maak een profiel voordeur](quickstart-create-front-door.md).

## <a name="define-geo-filtering-match-condition"></a>Geografisch filteren definiëren overeenkomen met de voorwaarde

Maken van een voorbeeld-voorwaarde voor overeenkomst dat aanvragen die niet afkomstig zijn van het gebruik van 'VS' selecteert [New-AzFrontDoorMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoormatchconditionobject) van de parameters bij het maken van een voorwaarde voor overeenkomst. Vindt u twee letter landcodes land toewijzen aan [hier](front-door-geo-filtering.md).

```azurepowershell-interactive
$nonUSGeoMatchCondition = New-AzFrontDoorMatchConditionObject `
-MatchVariable RemoteAddr `
-OperatorProperty GeoMatch `
-NegateCondition $true `
-MatchValue "US"
```
 
## <a name="add-geo-filtering-match-condition-to-a-rule-with-action-and-priority"></a>Overeenkomstvoorwaarde voor geofilter toevoegen aan een regel met Actie en Prioriteit

Maken van een object kan CustomRule `nonUSBlockRule` op basis van de voorwaarde voor overeenkomst, een actie en een prioriteit met [New-AzFrontDoorCustomRuleObject](/powershell/module/az.frontdoor/new-azfrontdoorcustomruleobject).  Een CustomRule kan meerdere overeenkomstvoorwaarden hebben.  In dit voorbeeld is Actie ingesteld op Blokkeren en is Prioriteit ingesteld op 1, de hoogste prioriteit.

```
$nonUSBlockRule = New-AzFrontDoorCustomRuleObject `
-Name "geoFilterRule" `
-RuleType MatchRule `
-MatchCondition $nonUSGeoMatchCondition `
-Action Block `
-Priority 1
```

## <a name="add-rules-to-a-policy"></a>Regels toevoegen aan een beleid
Zoek de naam van de resourcegroep waarin de voordeur via `Get-AzResourceGroup`. Maak vervolgens een `geoPolicy` beleid object met `nonUSBlockRule` met behulp van [New-AzFrontDoorFireWallPolicy](/powershell/module/az.frontdoor/new-azfrontdoorfirewallPolicy) in de opgegeven resourcegroep gemaakt met de voordeur-profiel. U moet een unieke naam voor de geo-beleid opgeven. 

Het onderstaande voorbeeld gebruikt u de naam van de Resource *myResourceGroupFD1* profiel met de veronderstelling dat u de voordeur hebt gemaakt met behulp van instructies hiervoor vindt u de [Quick Start: Maken van een voordeur](quickstart-create-front-door.md) artikel.

```
$geoPolicy = New-AzFrontDoorFireWallPolicy `
-Name "geoPolicyAllowUSOnly" `
-resourceGroupName myResourceGroupFD1 `
-Customrule $nonUSBlockRule  `
-Mode Prevention `
-EnabledState Enabled
```

## <a name="link-waf-policy-to-a-front-door-frontend-host"></a>Koppeling WAF-beleid op een host van de front-end voordeur
De WAF-groepsbeleidsobject koppelen aan de bestaande voordeur frontend-host en de voordeur eigenschappen bijwerken. 

Om dit te doen, eerst de ophalen voordeur object met [Get-AzFrontDoor](/powershell/module/az.frontdoor/get-azfrontdoor). 

```
$geoFrontDoorObjectExample = Get-AzFrontDoor -ResourceGroupName myResourceGroupFD1
$geoFrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $geoPolicy.Id
```

Vervolgens de front-end WebApplicationFirewallPolicyLink-eigenschap instellen op de resourceId van de `geoPolicy`met behulp van [Set AzFrontDoor](/powershell/module/az.frontdoor/set-azfrontdoor).

```
Set-AzFrontDoor -InputObject $geoFrontDoorObjectExample[0]
```

> [!NOTE] 
> U moet alleen WebApplicationFirewallPolicyLink eigenschap eenmaal een WAF-beleid koppelen aan een front-end voordeur host instellen. Voor latere beleidsupdates worden automatisch toegepast op de front-host.

## <a name="next-steps"></a>Volgende stappen

- Informatie over [bescherming van de toepassingslaag met Front Door](front-door-application-security.md).
- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).
