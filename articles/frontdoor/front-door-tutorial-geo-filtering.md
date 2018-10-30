---
title: 'Zelfstudie: Geofilters configureren in een domein voor Azure Front Door Service (AFD) | Microsoft Docs'
description: In deze zelfstudie leert u hoe u een eenvoudig beleid voor geofilters maakt en het beleid koppelt aan uw bestaande front-endhost van uw Front Door
services: frontdoor
documentationcenter: ''
author: sharad4u
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/20/2018
ms.author: sharadag
ms.openlocfilehash: c4032f7c33cec7b7a7864ccff07a05b87c945949
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988586"
---
# <a name="how-to-set-up-a-geo-filtering-policy-for-your-front-door"></a>Beleid voor geofilters instellen voor uw Front Door
In deze zelfstudie leert u hoe u Azure PowerShell gebruikt om een voorbeeldbeleid voor geofilters te maken en het beleid koppelt aan uw bestaande front-endhost van uw Front Door. In dit voorbeeldbeleid voor geofilters worden aanvragen uit alle andere landen, met uitzondering van de Verenigde Staten, geblokkeerd.

## <a name="1-set-up-your-powershell-environment"></a>1. Uw PowerShell-omgeving instellen
Azure PowerShell voorziet in een set van cmdlets die gebruikmaken van het [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)-model om uw Azure-resources te beheren. 

U kunt [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) op uw lokale computer installeren en in elke PowerShell-sessie gebruiken. Volg de instructies op de pagina om u aan te melden met uw Azure-referenties en AzureRM te installeren.
```
# Connect to Azure with an interactive dialog for sign-in
Connect-AzureRmAccount
Install-Module -Name AzureRM
```
> [!NOTE]
>  Ondersteuning voor [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) komt binnenkort beschikbaar.

Voordat u de Front Door-module installeert, moet u controleren of u de nieuwste versie van PowerShellGet hebt geïnstalleerd. Voer de onderstaande opdracht uit en open PowerShell opnieuw.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

Installeer de AzureRM.FrontDoor-module. 

```
Install-Module -Name AzureRM.FrontDoor -AllowPrerelease
```

## <a name="2-define-geo-filtering-match-conditions"></a>2. Overeenkomstvoorwaarde(n) voor geofilters definiëren
Maak eerst een voorbeeld van een overeenkomstvoorwaarde die aanvragen die niet afkomstig zijn van "VS" selecteert. Raadpleeg de [handleiding](https://docs.microsoft.com/azure/frontdoor/new-azurermfrontdoormatchconditionobject) van PowerShell voor parameters wanneer u een overeenkomstvoorwaarde maakt. De uit twee letters bestaande koppeling tussen landcode en landtoewijzing vindt u [hier](/Protection/GeoFiltering).

```
$nonUSGeoMatchCondition = New-AzureRmFrontDoorMatchConditionObject -MatchVariable RemoteAddr -OperatorProperty GeoMatch -NegateCondition $true -MatchValue "US"
```
 
## <a name="3-add-geo-filtering-match-condition-to-a-rule-with-action-and-priority"></a>3. Overeenkomstvoorwaarde voor geofilter toevoegen aan een regel met Actie en Prioriteit

Maak vervolgens een CustomRule-object `nonUSBlockRule` op basis van de overeenkomstvoorwaarde, een Actie en een Prioriteit.  Een CustomRule kan meerdere overeenkomstvoorwaarden hebben.  In dit voorbeeld is Actie ingesteld op Blokkeren en is Prioriteit ingesteld op 1, de hoogste prioriteit.

```
$nonUSBlockRule = New-AzureRmFrontDoorCustomRuleObject -Name "geoFilterRule" -RuleType MatchRule -MatchCondition $nonUSGeoMatchCondition -Action Block -Priority 1
```

Raadpleeg de [handleiding](https://docs.microsoft.com/azure/frontdoor/new-azurermfrontdoorcustomruleobject) van PowerShell voor parameters wanneer u een CustomRuleObject maakt.

## <a name="4-add-rules-to-a-policy"></a>4. Regels toevoegen aan een beleid
In deze stap maakt u een `geoPolicy`-beleidsobject met `nonUSBlockRule` uit de vorige stap in de opgegeven resourcegroep. Gebruik `Get-AzureRmResourceGroup` om uw ResourceGroupName $resourceGroup te vinden.

```
$geoPolicy = New-AzureRmFrontDoorFireWallPolicy -Name "geoPolicyAllowUSOnly" -resourceGroupName $resourceGroup -Customrule $nonUSBlockRule  -Mode Prevention -EnabledState Enabled
```

Raadpleeg de [handleiding](https://docs.microsoft.com/azure/frontdoor/new-azurermfrontdoorfirewallpolicy) van PowerShell voor parameters wanneer u een beleid maakt.

## <a name="5-link-policy-to-a-front-door-frontend-host"></a>5. Beleid koppelen aan een front-endhost van Front Door
De laatste stappen bestaan uit het koppelen van het beveiligingsbeleidobject aan een bestaande front-endhost van Front Door en het bijwerken van de eigenschappen van Front Door. U haalt eerst uw Front Door-object op met behulp van [Get-AzureRmFrontDoor](https://docs.microsoft.com/azure/frontdoor/get-azurermfrontdoor), en stelt daarna de eigenschap WebApplicationFirewallPolicyLink van de front-end in op de resourceId van de `geoPolicy`.

```
$geoFrontDoorObjectExample = Get-AzureRmFrontDoor -ResourceGroupName $resourceGroup
$geoFrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $geoPolicy.Id
```

Gebruik de volgende [opdracht](https://docs.microsoft.com/azure/frontdoor/set-azurermfrontdoor) om uw Front Door-object bij te werken.

```
Set-AzureRmFrontDoor -InputObject $geoFrontDoorObjectExample[0]
```

> [!NOTE] 
> U hoeft de eigenschap WebApplicationFirewallPolicyLink maar één keer in te stellen om een beveiligingsbeleid te koppelen aan een front-endhost van Front Door. Volgende beleidsupdates worden automatisch toegepast op de front-endhost.

## <a name="next-steps"></a>Volgende stappen

- Informatie over [bescherming van de toepassingslaag met Front Door](front-door-application-security.md).
- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).
