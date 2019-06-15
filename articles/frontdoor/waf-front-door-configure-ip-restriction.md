---
title: Een regel voor het IP-beperkingen configureren met een web application firewall-regel voor Azure voordeur Service
description: Informatie over het configureren van een web application firewall-regel voor het IP-adressen voor een bestaande Azure voordeur Service-eindpunt beperken.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: kumud;tyao
ms.openlocfilehash: 73ef16aeb9a6014e98c0d40314bc174c6b5bf307
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66808352"
---
# <a name="configure-an-ip-restriction-rule-with-a-web-application-firewall-for-azure-front-door-service"></a>Een regel voor het IP-beperkingen configureren met een web application firewall voor Azure voordeur Service
In dit artikel leest u hoe het configureren van IP-beperking regels in een web application firewall (WAF) voor Azure voordeur Service met behulp van de Azure CLI, Azure PowerShell of een Azure Resource Manager-sjabloon.

Een IP-adres gebaseerde toegangsregel is een aangepaste WAF-regel waarmee u toegang tot uw webtoepassingen beheren. Dit gebeurt door op te geven een lijst met IP-adressen of IP-adresbereiken in de notatie (Classless Inter-Domain Routing).

Uw web-App is standaard toegankelijk is vanaf internet. Als u toegang beperken tot clients in een lijst met bekende IP-adressen of IP-adresbereiken wilt, moet u twee IP-overeenkomende regels maken. Het eerste IP-overeenkomende regel bevat de lijst met IP-adressen als de overeenkomende waarden en stelt u de actie op **toestaan**. De tweede waarde, met een lagere prioriteit, blokkeert alle IP-adressen met behulp van de **alle** operator en het instellen van de actie op **blok**. Nadat u een regel voor het IP-beperkingen is toegepast, betekent dit dat aanvragen die afkomstig van adressen buiten deze toegestane lijst zijn 403 verboden reactie ontvangen.  

## <a name="configure-a-waf-policy-with-the-azure-cli"></a>Een WAF-beleid configureren met de Azure CLI

### <a name="prerequisites"></a>Vereisten
Voordat u begint met een IP-beperking-beleid configureren, instellen van de CLI-omgeving en een Azure voordeur Service-profiel maken.

#### <a name="set-up-the-azure-cli-environment"></a>De Azure CLI-omgeving instellen
1. Installeer de [Azure CLI](/cli/azure/install-azure-cli), of gebruik van Azure Cloud Shell. Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. In deze shell is de Azure CLI vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Selecteer de **uitproberen** knop in het CLI-opdrachten die volgen en meld aan uw Azure-account in de Cloud Shell-sessie die wordt geopend. Nadat de sessie wordt gestart, voert u `az extension add --name front-door` om toe te voegen van de extensie Azure voordeur Service.
 2. Als u de CLI lokaal in Bash, aanmelden bij Azure met behulp van `az login`.

#### <a name="create-an-azure-front-door-service-profile"></a>Een Azure voordeur Service-profiel maken
Een Azure voordeur Service-profiel maken door de instructies die worden beschreven in [Quick Start: Maken van een voordeur voor een maximaal beschikbare globale webtoepassing](quickstart-create-front-door.md).

### <a name="create-a-waf-policy"></a>Een WAF-beleid maken

Een WAF-beleid maken met behulp van de [az waf-netwerkbeleid maken](/cli/azure/ext/front-door/network/waf-policy?view=azure-cli-latest#ext-front-door-az-network-waf-policy-create) opdracht. In het voorbeeld dat de naam van het beleid voor het vervangen van volgt, *IPAllowPolicyExampleCLI* met een unieke beleidsnaam op.

```azurecli-interactive 
az network waf-policy create \
  --resource-group <resource-group-name> \
  --subscription <subscription ID> \
  --name IPAllowPolicyExampleCLI
  ```
### <a name="add-a-custom-ip-access-control-rule"></a>Een aangepaste toegangsbeheerregel van IP-toevoegen

Gebruik de [az netwerk waf-beleid voor aangepaste regel maken](/cli/azure/ext/front-door/network/waf-policy/custom-rule?view=azure-cli-latest#ext-front-door-az-network-waf-policy-custom-rule-create) opdracht voor het toevoegen van een aangepaste IP-toegangsbeheerregel voor de WAF-beleid u zojuist hebt gemaakt.

In de volgende voorbeelden:
-  Vervang *IPAllowPolicyExampleCLI* aan uw unieke beleid eerder hebt gemaakt.
-  Vervang *ip-adres-bereik-1*, *ip-adres-bereik-2* met uw eigen bereik.

Maak eerst het IP-adres-regel voor de opgegeven adressen toestaan.

```azurecli
az network waf-policy custom-rule create \
  --name IPAllowListRule \
  --priority 1 \
  --rule-type MatchRule \
  --match-condition RemoteAddr IPMatch ("<ip-address-range-1>","<ip-address-range-2>") \
  --action Allow \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI
```
Maak vervolgens een **alle blokkeren** regel met een lagere prioriteit dan de vorige **toestaan** regel. Vervang weer *IPAllowPolicyExampleCLI* in het volgende voorbeeld aan uw unieke beleid dat u eerder hebt gemaakt.

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
    
### <a name="find-the-id-of-a-waf-policy"></a>De ID van een beleid voor WAF vinden 
Een WAF-beleids-ID vinden met behulp van de [az netwerk waf-policy show](/cli/azure/ext/front-door/network/waf-policy?view=azure-cli-latest#ext-front-door-az-network-waf-policy-show) opdracht. Vervang *IPAllowPolicyExampleCLI* in het volgende voorbeeld aan uw unieke beleid dat u eerder hebt gemaakt.

   ```azurecli
   az network waf-policy show \
     --resource-group <resource-group-name> \
     --name IPAllowPolicyExampleCLI
   ```

### <a name="link-a-waf-policy-to-an-azure-front-door-service-front-end-host"></a>Een WAF-beleid koppelen aan een Azure voordeur front-end ServiceHost

Stel de voordeur Azure Service *WebApplicationFirewallPolicyLink* ID moet de beleids-ID met behulp van de [az netwerk voordeur update](/cli/azure/ext/front-door/network/front-door?view=azure-cli-latest#ext-front-door-az-network-front-door-update) opdracht. Vervang *IPAllowPolicyExampleCLI* aan uw unieke beleid dat u eerder hebt gemaakt.

   ```azurecli
   az network front-door update \
     --set FrontendEndpoints[0].WebApplicationFirewallPolicyLink.id=/subscriptions/<subscription ID>/resourcegroups/<resource- name>/providers/Microsoft.Network/frontdoorwebapplicationfirewallpolicies/IPAllowPolicyExampleCLI \
     --name <frontdoor-name>
     --resource-group <resource-group-name>
   ```
In dit voorbeeld wordt de WAF-beleid wordt toegepast op **FrontendEndpoints [0]** . U kunt de WAF-beleid koppelen aan een van de front-ends.
> [!Note]
> U moet instellen de **WebApplicationFirewallPolicyLink** eigenschap slechts één keer een WAF-beleid koppelen aan een Azure voordeur front-endservice. Voor latere beleidsupdates worden automatisch toegepast op de front-end.

## <a name="configure-a-waf-policy-with-azure-powershell"></a>Een WAF-beleid configureren met Azure PowerShell

### <a name="prerequisites"></a>Vereisten
Voordat u begint met een IP-beperking-beleid configureren, instellen van uw PowerShell-omgeving en een Azure voordeur Service-profiel maken.

#### <a name="set-up-your-powershell-environment"></a>Uw PowerShell-omgeving instellen
Azure PowerShell biedt een set cmdlets die gebruikmaken van de [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) model voor het beheren van Azure-resources.

U kunt [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) op uw lokale computer installeren en in elke PowerShell-sessie gebruiken. Volg de instructies op de pagina met behulp van uw Azure-referenties aanmelden bij PowerShell en installeer vervolgens de Az-module.

1. Verbinding maken met Azure met behulp van de volgende opdracht, en gebruik vervolgens een dialoogvenster met een interactief aan te melden.
    ```
    Connect-AzAccount
    ```
 2. Zorg voordat u een Azure voordeur Service-module installeert, hebt u de huidige versie van de PowerShellGet-module geïnstalleerd. Voer de volgende opdracht uit en open PowerShell.

    ```
    Install-Module PowerShellGet -Force -AllowClobber
    ``` 

3. De Az.FrontDoor-module installeren met behulp van de volgende opdracht uit. 
    
    ```
    Install-Module -Name Az.FrontDoor
    ```
### <a name="create-an-azure-front-door-service-profile"></a>Een Azure voordeur Service-profiel maken
Een Azure voordeur Service-profiel maken door de instructies die worden beschreven in [Quick Start: Maken van een voordeur voor een maximaal beschikbare globale webtoepassing](quickstart-create-front-door.md).

### <a name="define-an-ip-match-condition"></a>Een IP-voorwaarde definiëren
Gebruik de [New-AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject) opdracht voor het definiëren van een IP-voorwaarde voor overeenkomst.
Vervang in het volgende voorbeeld *ip-adres-bereik-1*, *ip-adres-bereik-2* met uw eigen bereik.    
```powershell
$IPMatchCondition = New-AzFrontDoorWafMatchConditionObject `
-MatchVariable  RemoteAddr `
-OperatorProperty IPMatch `
-MatchValue "ip-address-range-1", "ip-address-range-2"
```
Een IP-adres maken *overeen met alle voorwaarde* regel met behulp van de volgende opdracht uit:
```powershell
$IPMatchALlCondition = New-AzFrontDoorWafMatchConditionObject `
-MatchVariable  RemoteAddr `
-OperatorProperty Any        
  ```
    
### <a name="create-a-custom-ip-allow-rule"></a>Maken van een aangepaste regel voor geven van IP

Gebruik de [New-AzFrontDoorCustomRuleObject](/powershell/module/Az.FrontDoor/New-azfrontdoorwafcustomruleobject) opdracht voor het definiëren van een actie en stel een prioriteit. In het volgende voorbeeld worden aanvragen van client-IP-adressen die overeenkomen met de lijst worden toegestaan.

```powershell
$IPAllowRule = New-AzFrontDoorCustomRuleObject `
-Name "IPAllowRule" `
-RuleType MatchRule `
-MatchCondition $IPMatchCondition `
-Action Allow -Priority 1
```
Maak een **alle blokkeren** regel met een lagere prioriteit dan het vorige IP-adres **toestaan** regel.
```powershell
$IPBlockAll = New-AzFrontDoorCustomRuleObject `
-Name "IPDenyAll" `
-RuleType MatchRule `
-MatchCondition $IPMatchALlCondition `
-Action Block `
-Priority 2
```

### <a name="configure-a-waf-policy"></a>Een WAF-beleid configureren
Zoek de naam van de resourcegroep met de Azure voordeur Service-profiel met behulp van `Get-AzResourceGroup`. Configureer vervolgens een WAF-beleid met het IP-adres **alle blokkeren** regel met behulp van [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy).

```powershell
  $IPAllowPolicyExamplePS = New-AzFrontDoorWafPolicy `
    -Name "IPRestrictionExamplePS" `
    -resourceGroupName <resource-group-name> `
    -Customrule $IPAllowRule $IPBlockAll `
    -Mode Prevention `
    -EnabledState Enabled
   ```

### <a name="link-a-waf-policy-to-an-azure-front-door-service-front-end-host"></a>Een WAF-beleid koppelen aan een Azure voordeur front-end ServiceHost

Een WAF policy object koppelen aan een bestaande front-host en update Azure voordeur Service-eigenschappen. Eerst het serviceobject Azure voordeur ophalen met behulp van [Get-AzFrontDoor](/powershell/module/Az.FrontDoor/Get-AzFrontDoor). Vervolgens stelt de **WebApplicationFirewallPolicyLink** eigenschap in op de resource-ID van *$IPAllowPolicyExamplePS*, gemaakt in de vorige stap, met behulp van de [Set-AzFrontDoor](/powershell/module/Az.FrontDoor/Set-AzFrontDoor)opdracht.

```powershell
  $FrontDoorObjectExample = Get-AzFrontDoor `
    -ResourceGroupName <resource-group-name> `
    -Name $frontDoorName
  $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $IPBlockPolicy.Id
  Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
```

> [!NOTE]
> In dit voorbeeld wordt de WAF-beleid wordt toegepast op **FrontendEndpoints [0]** . U kunt een WAF-beleid koppelen aan een van de front-ends. U moet instellen de **WebApplicationFirewallPolicyLink** eigenschap slechts één keer een WAF-beleid koppelen aan een Azure voordeur front-endservice. Voor latere beleidsupdates worden automatisch toegepast op de front-end.


## <a name="configure-a-waf-policy-with-a-resource-manager-template"></a>Een WAF-beleid configureren met Resource Manager-sjabloon
Als u de sjabloon die wordt gemaakt van een beleid voor Azure voordeur Service en een WAF met aangepaste regels voor IP-beperking, gaat u naar [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-waf-clientip).


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [maken van een Azure-voordeur serviceprofiel](quickstart-create-front-door.md).
