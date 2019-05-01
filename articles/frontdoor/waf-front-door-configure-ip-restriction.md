---
title: Een regel voor het IP-beperkingen configureren met web application firewall-regel voor Azure voordeur
description: Informatie over het configureren van een regel met WAF beperkingen voor IP-adres voor een bestaande voordeur-eindpunt.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/25/2019
ms.author: kumud;tyao
ms.openlocfilehash: 514c034c23eed3a87111331724f3a33104651a43
ms.sourcegitcommit: e729629331ae10097a081a03029398525f4147a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/25/2019
ms.locfileid: "64514902"
---
# <a name="configure-an-ip-restriction-rule-with-web-application-firewall-for-azure-front-door-preview"></a>Een regel voor het IP-beperkingen configureren met web application firewall voor Azure voordeur (Preview)
 In dit artikel wordt beschreven hoe u IP-beperking regels in de Azure-web application firewall (WAF) voor de voordeur configureren met behulp van Azure CLI, Azure PowerShell of Azure Resource Manager-sjabloon.

Een IP-adres op basis van toegangsregel is een aangepaste WAF-regel waarmee u toegang tot uw webtoepassingen beheren door een lijst met IP-adressen of IP-adresbereiken op te geven in de notatie (Classless Inter-Domain Routing).

Uw web-App is standaard toegankelijk is vanaf internet. Als u beperken van toegang tot uw webtoepassingen alleen voor clients in een lijst van bekende IP-adressen of IP-adresbereiken wilt, moet u twee IP-overeenkomende regels maken. Eerste IP-overeenkomende regel bevat de lijst met IP-adressen als overeenkomende waarden en de actie 'Toestaan'. De tweede waarde met een lagere prioriteit wordt voorkomen dat alle IP-adressen met behulp van de operator 'All' en de actie 'BLOCK' instellen. Zodra een regel voor het IP-beperkingen is toegepast, ontvangt verzoeken die afkomstig zijn van adressen buiten deze toegestane lijst een 403 (verboden)-antwoord.  

> [!IMPORTANT]
> De WAF-IP-beperking functie voordeur voor Azure is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="configure-waf-policy-with-azure-cli"></a>WAF-beleid configureren met Azure CLI

### <a name="prerequisites"></a>Vereisten
Voordat u begint met een IP-beperking-beleid configureren, instellen van de CLI-omgeving en een voordeur-profiel maken.

#### <a name="set-up-azure-cli-environment"></a>Azure CLI-omgeving instellen
1. Installeer de [Azure CLI](/cli/azure/install-azure-cli), of gebruik de Azure Cloud Shell. De Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. Dit is de Azure-CLI vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Selecteer de **uitproberen** knop in het CLI-opdrachten die volgen. Selecteren **uitproberen** een Cloud-Shell die u kunt zich aanmelden bij uw Azure-account met aanroept. Zodra een cloud shell-sessie start, voer `az extension add --name front-door` aan de voordeur extensie toevoegen.
 2. Als u de CLI lokaal in Bash, aanmelden bij Azure met `az login`.

#### <a name="create-front-door-profile"></a>Voordeur profiel maken
Een profiel voordeur maken door de instructies die worden beschreven in [Quick Start: Maak een profiel van de voordeur](quickstart-create-front-door.md)

### <a name="create-a-waf-policy"></a>Een WAF-beleid maken

Maak een WAF-beleid met de [az waf-netwerkbeleid maken](/cli/azure/ext/front-door/network/waf-policy?view=azure-cli-latest#ext-front-door-az-network-waf-policy-create) opdracht. In het onderstaande voorbeeld, vervangen de beleidsnaam *IPAllowPolicyExampleCLI* met een unieke beleidsnaam op.

```azurecli-interactive 
az network waf-policy create \
  --resource-group <resource-group-name> \
  --subscription <subscription ID> \
  --name IPAllowPolicyExampleCLI
  ```
### <a name="add-custom-ip-access-control-rule"></a>Toegangsbeheerregel van aangepaste IP-toevoegen

Een aangepaste toegangsbeheerregel van IP-toevoegen aan het WAF-beleid dat is gemaakt in de vorige stap met de [az netwerk waf-beleid voor aangepaste regel maken](/cli/azure/ext/front-door/network/waf-policy/custom-rule?view=azure-cli-latest#ext-front-door-az-network-waf-policy-custom-rule-create) opdracht. 

In het onderstaande voorbeeld:
-  Vervang *IPAllowPolicyExampleCLI* aan uw unieke beleid eerder hebt gemaakt.
-  Vervang *ip-adres-bereik-1*, *ip-adres-bereik-2* met uw eigen bereik.

Maak eerst het IP-adres-regel voor de opgegeven adressen toestaan.

```azurecli
az network waf-policy custom-rule create \
  --name IPAllowListRule \
  --priority 1 \
  --rule-type MatchRule \
  --match-condition RemoteAddr IPMatch ["<ip-address-range-1>","<ip-address-range-2>"] \
  --action Allow \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI
```
Maak vervolgens een blok alle IP-regel met een lagere prioriteit dan de regel voor geven van het vorige IP-adres. Vervang de *IPAllowPolicyExampleCLI* aan uw unieke beleid eerder hebt gemaakt.

```azurecli
az network waf-policy custom-rule create \
  --name IPDenyAllRule\
  --priority 2 \
  --rule-type MatchRule \
  --match-condition RemoteAddr Any
  --action Block \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI
 ```

### <a name="find-waf-policy-id"></a>Zoeken naar WAF beleids-ID
De ID van een beleid met een WAF vinden de [az netwerk waf-policy show](/cli/azure/ext/front-door/network/waf-policy?view=azure-cli-latest#ext-front-door-az-network-waf-policy-show) opdracht. Vervang de *IPAllowPolicyExampleCLI* aan uw unieke beleid eerder hebt gemaakt.

   ```azurecli
   az network waf-policy show \
     --resource-group <resource-group-name> \
     --name IPAllowPolicyExampleCLI
   ```

### <a name="link-waf-policy-to-a-front-door-front-end-host"></a>Koppeling WAF-beleid met een front-host van de voordeur

Stel de voordeur *WebApplicationFirewallPolicyLink* ID moet de beleids-ID met de [az netwerk voordeur update](/cli/azure/ext/front-door/network/front-door?view=azure-cli-latest#ext-front-door-az-network-front-door-update) opdracht. Vervang de *IPAllowPolicyExampleCLI* aan uw unieke beleid eerder hebt gemaakt.

   ```azurecli
   az network front-door update \
     --set FrontendEndpoints[0].WebApplicationFirewallPolicyLink.id=/subscriptions/<subscription ID>/resourcegroups/<resource- name>/providers/Microsoft.Network/frontdoorwebapplicationfirewallpolicies/IPAllowPolicyExampleCLI \
     --name <frontdoor-name>
     --resource-group <resource-group-name>
   ```
In dit voorbeeld wordt de WAF-beleid toegepast op FrontendEndpoints [0]. U mag WAF beleid koppelen aan een van de front-ends.
> [!Note]
> U hoeft alleen om in te stellen de **WebApplicationFirewallPolicyLink** eigenschap eenmaal een WAF-beleid koppelen aan een front-end-voordeur. Voor latere beleidsupdates worden automatisch toegepast op de front-end.

## <a name="configure-waf-policy-with-azure-powershell"></a>WAF-beleid configureren met Azure PowerShell

### <a name="prerequisites"></a>Vereisten
Voordat u begint met een IP-beperking-beleid configureren, instellen van uw PowerShell-omgeving en een voordeur-profiel maken.

#### <a name="set-up-your-powershell-environment"></a>Uw PowerShell-omgeving instellen
Azure PowerShell voorziet in een set van cmdlets die gebruikmaken van het [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)-model om uw Azure-resources te beheren. 

U kunt [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) op uw lokale computer installeren en in elke PowerShell-sessie gebruiken. Volg de instructies op de pagina, meld u aan met uw Azure-referenties, en installeer Az PowerShell-module.

##### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Verbinding maken met Azure met een interactieve dialoogvenster voor aanmelding
```
Connect-AzAccount

```
Zorg voordat u voordeur module installeert, hebt u de huidige versie van PowerShellGet geïnstalleerd. De onderstaande opdracht en open deze opnieuw uitvoeren PowerShell.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

##### <a name="install-azfrontdoor-module"></a>Az.FrontDoor-module installeren 

```
Install-Module -Name Az.FrontDoor
```
### <a name="create-a-front-door-profile"></a>Maak een profiel van de voordeur
Een profiel voordeur maken door de instructies die worden beschreven in [Quick Start: Maak een profiel van de voordeur](quickstart-create-front-door.md)

### <a name="define-ip-match-condition"></a>IP-voorwaarde definiëren
Gebruik de [New-AzFrontDoorMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoormatchconditionobject) opdracht voor het definiëren van een IP-voorwaarde voor overeenkomst. In het onderstaande voorbeeld, Vervang *ip-adres-bereik-1*, *ip-adres-bereik-2* met uw eigen bereik.

```powershell
  $IPMatchCondition = New-AzFrontDoorMatchConditionObject `
    -MatchVariable  RemoteAddr `
    -OperatorProperty IPMatch `
    -MatchValue ["ip-address-range-1", "ip-address-range-2"]
```
Een IP-overeenkomst alle voorwaarde regel maken
```powershell
  $IPMatchALlCondition = New-AzFrontDoorMatchConditionObject `
    -MatchVariable  RemoteAddr `
    -OperatorProperty Any
    
```

### <a name="create-a-custom-ip-allow-rule"></a>Maken van een aangepaste regel voor geven van IP
   Gebruik de [New-AzFrontDoorCustomRuleObject](/powershell/module/Az.FrontDoor/New-AzFrontDoorCustomRuleObject) opdracht voor het definiëren van een actie en stel een prioriteit. In het volgende voorbeeld worden aanvragen van client-IP-adressen die overeenkomen met de lijst worden toegestaan. 

```powershell
  $IPAllowRule = New-AzFrontDoorCustomRuleObject `
    -Name "IPAllowRule" `
    -RuleType MatchRule `
    -MatchCondition $IPMatchCondition `
    -Action Allow -Priority 1
```
Een blok alle IP-regel met een lagere prioriteit dan de regel voor geven van het vorige IP-adres maken.

```powershell
  $IPBlockAll = New-AzFrontDoorCustomRuleObject `
    -Name "IPDenyAll" `
    -RuleType MatchRule `
    -MatchCondition $IPMatchALlCondition `
    -Action Block `
    -Priority 2
   ```

### <a name="configure-waf-policy"></a>WAF-beleid configureren
Zoek de naam van de resourcegroep waarin de voordeur via `Get-AzResourceGroup`. Configureer vervolgens een WAF-beleid met de IP-blok regel met behulp van [New-AzFrontDoorFireWallPolicy](/powershell/module/Az.FrontDoor/New-AzFrontDoorFireWallPolicy).

```powershell
  $IPAllowPolicyExamplePS = New-AzFrontDoorFireWallPolicy `
    -Name "IPRestrictionExamplePS" `
    -resourceGroupName <resource-group-name> `
    -Customrule $IPAllowRule $IPBlockAll `
    -Mode Prevention `
    -EnabledState Enabled
   ```

### <a name="link-waf-policy-to-a-front-door-front-end-host"></a>Koppeling WAF-beleid met een front-host van de voordeur

De WAF-groepsbeleidsobject koppelen aan een bestaande voordeur front-host en de voordeur eigenschappen bijwerken. Eerst ophalen van de voordeur object via [Get-AzFrontDoor](/powershell/module/Az.FrontDoor/Get-AzFrontDoor). Vervolgens stelt de front-end *WebApplicationFirewallPolicyLink* eigenschap in op de resourceId van de *$IPAllowPolicyExamplePS* gemaakt in de vorige stap met de [Set-AzFrontDoor](/powershell/module/Az.FrontDoor/Set-AzFrontDoor) opdracht.

```powershell
  $FrontDoorObjectExample = Get-AzFrontDoor `
    -ResourceGroupName <resource-group-name> `
    -Name $frontDoorName
  $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $IPBlockPolicy.Id
  Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
```

> [!NOTE]
> In dit voorbeeld wordt de WAF-beleid toegepast op FrontendEndpoints [0]. U mag WAF beleid koppelen aan een van de front-ends. U hoeft alleen om in te stellen *WebApplicationFirewallPolicyLink* eigenschap eenmaal een WAF-beleid koppelen aan een front-end-voordeur. Voor latere beleidsupdates worden automatisch toegepast op de front-end.


## <a name="configure-waf-policy-with-resource-manager-template"></a>WAF-beleid configureren met Resource Manager-sjabloon
Bekijk de sjabloon waarmee u een voordeur en een WAF-beleid met aangepaste regels voor IP-beperking maakt [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-waf-clientip).


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [maakt u een profiel voordeur](quickstart-create-front-door.md).
