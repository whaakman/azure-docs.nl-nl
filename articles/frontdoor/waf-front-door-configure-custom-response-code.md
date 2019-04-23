---
title: Een aangepast antwoord voor de web application firewall configureren op Azure voordeur
description: Informatie over het configureren van een aangepast antwoordcode en een bericht wanneer de web application firewall (WAF) een aanvraag wordt geblokkeerd.
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/08/2019
ms.author: tyao;kumud
ms.openlocfilehash: 2d16893420f27caf4f8b00dc32069e3296d7c236
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59795743"
---
# <a name="configure-a-custom-response-for-azure-web-application-firewall"></a>Een aangepast antwoord voor de Azure-web application firewall configureren

Standaard als Azure web application firewall (WAF) met Azure voordeur een verzoek wordt geblokkeerd vanwege een overeenkomende regel wordt een 403-statuscode met **de aanvraag is geblokkeerd** bericht. In dit artikel wordt beschreven hoe u een aangepaste antwoordstatuscode en een antwoordbericht configureren wanneer een aanvraag wordt geblokkeerd door WAF.

## <a name="set-up-your-powershell-environment"></a>Uw PowerShell-omgeving instellen
Azure PowerShell voorziet in een set van cmdlets die gebruikmaken van het [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)-model om uw Azure-resources te beheren. 

U kunt [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) op uw lokale computer installeren en in elke PowerShell-sessie gebruiken. Volg de instructies op de pagina, meld u aan met uw Azure-referenties, en installeer de Az-PowerShell-module.

### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Verbinding maken met Azure met een interactieve dialoogvenster voor aanmelding bij
```
Connect-AzAccount
Install-Module -Name Az
```
Zorg ervoor dat u de huidige versie van PowerShellGet geïnstalleerd hebben. Voer de onderstaande opdracht uit en open PowerShell opnieuw.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 
### <a name="install-azfrontdoor-module"></a>Az.FrontDoor-module installeren 

```
Install-Module -Name Az.FrontDoor
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

In Azure kunt u verwante resources toewijzen aan een resourcegroep. In dit voorbeeld maakt u een resourcegroep met behulp van [New-AzResourceGroup](/powershell/module/Az.resources/new-Azresourcegroup).

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupWAF
```

## <a name="create-a-new-waf-policy-with-custom-response"></a>Maak een nieuwe WAF-beleid met aangepast antwoord 

Hieronder volgt een voorbeeld van het maken van een nieuwe WAF-beleid met de aangepaste reactie statuscode 405 en bericht dat moet worden ingesteld **u worden geblokkeerd.** using [New-AzFrontDoorFireWallPolicy](/powershell/module/az.frontdoor/new-azfrontdoorfirewallPolicy).

```azurepowershell
# WAF policy setting
New-AzFrontDoorFireWallPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-EnabledState enabled `
-Mode Detection `
-CustomBlockResponseStatusCode 405 `
-CustomBlockResponseBody "<html><head><title>You are blocked.</title></head><body></body></html>"
```

Responscode van de aangepaste of antwoord hoofdtekst van de instellingen van een beleidsregel van WAF aanpassen met behulp van [Set AzFrontDoor](/powershell/module/az.frontdoor/set-azfrontdoor).

```azurepowershell
# modify WAF response code
Set-AzFrontDoorFireWallPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-EnabledState enabled `
-Mode Detection `
-CustomBlockResponseStatusCode 403
```

```azurepowershell
# modify WAF response body
Set-AzFrontDoorFireWallPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-CustomBlockResponseBody "<html><head><title> Forbidden</title></head><body></body></html>"
```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [voordeur](front-door-overview.md)