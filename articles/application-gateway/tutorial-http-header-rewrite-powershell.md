---
title: Herschrijf de HTTP-headers in Azure Application Gateway
description: Dit artikel bevat informatie over het maken van een Azure Application Gateway en Herschrijf de HTTP-headers met behulp van Azure PowerShell
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 12/20/2018
ms.author: absha
ms.openlocfilehash: 8bc0d53080d0653ae630765d8a656cbe8d50b24c
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53971566"
---
# <a name="tutorial-create-an-application-gateway-and-rewrite-http-headers"></a>Zelfstudie: Een toepassingsgateway maken en te herschrijven van HTTP-headers

U kunt Azure PowerShell gebruiken om te configureren [regels voor het herschrijven van HTTP-aanvraag- en reactieheaders](rewrite-http-headers.md) bij het maken van de nieuwe [automatisch schalen en zone-redundante application gateway-SKU](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant)

> [!IMPORTANT] 
> De SKU met automatisch schalen en zone-redundantie voor toepassingsgateways is momenteel in openbare preview. Deze preview wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
>
> * Een virtueel netwerk automatisch schalen
> * Een gereserveerd openbaar IP-adres maken
> * De infrastructuur van de toepassingsgateway instellen
> * Geef de HTTP-header herschrijven regelconfiguratie
> * Automatisch schalen configureren
> * De toepassingsgateway maken
> * De toepassingsgateway testen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie moet u Azure PowerShell lokaal uitvoeren. U moet Az moduleversie 1.0.0 of hoger is geïnstalleerd. Voer `Import-Module Az` en vervolgens`Get-Module Az` de versie te vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Nadat u de versie van PowerShell hebt gecontroleerd, voert u `Login-AzAccount` uit om een verbinding op te zetten met Azure.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een resourcegroep op een van de beschikbare locaties.

```azurepowershell
$location = "East US 2"
$rg = "<rg name>"

#Create a new Resource Group
New-AzResourceGroup -Name $rg -Location $location
```

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Maak een virtueel netwerk met één speciaal subnet voor een toepassingsgateway met automatisch schalen. Op dit moment kan er in elk gereserveerd subnet maar één toepassingsgateway met automatisch schalen worden geïmplementeerd.

```azurepowershell
#Create VNet with two subnets
$sub1 = New-AzVirtualNetworkSubnetConfig -Name "AppGwSubnet" -AddressPrefix "10.0.0.0/24"
$sub2 = New-AzVirtualNetworkSubnetConfig -Name "BackendSubnet" -AddressPrefix "10.0.1.0/24"
$vnet = New-AzvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg `
       -Location $location -AddressPrefix "10.0.0.0/16" -Subnet $sub1, $sub2
```

## <a name="create-a-reserved-public-ip"></a>Een gereserveerd openbaar IP-adres maken

Stel de toewijzingsmethode van PublicIPAddress in op **Statisch**. De VIP van een toepassingsgateway met automatisch schalen kan alleen statisch zijn. Dynamische VIP's worden niet ondersteund. Alleen PublicIpAddress uit de standaard-SKU wordt ondersteund.

```azurepowershell
#Create static public IP
$pip = New-AzPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP" `
       -location $location -AllocationMethod Static -Sku Standard
```

## <a name="retrieve-details"></a>Gegevens ophalen

Haal gegevens op van de resourcegroep, het subnet en een IP in een lokaal object om de IP-configuratiegegevens van de toepassingsgateway te maken.

```azurepowershell
$resourceGroup = Get-AzResourceGroup -Name $rg
$publicip = Get-AzPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP"
$vnet = Get-AzvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name "AppGwSubnet" -VirtualNetwork $vnet
```

## <a name="configure-the-infrastructure"></a>Infrastructuur configureren

Configureer de IP-config, front-end-IP-config back-end-pool, HTTP-instellingen, certificaten, poort en listener in een indeling identiek zijn aan de bestaande standaard application-gateway. De nieuwe SKU volgt hetzelfde objectmodel als de standaard-SKU.

```azurepowershell
$ipconfig = New-AzApplicationGatewayIPConfiguration -Name "IPConfig" -Subnet $gwSubnet
$fip = New-AzApplicationGatewayFrontendIPConfig -Name "FrontendIPCOnfig" -PublicIPAddress $publicip
$pool = New-AzApplicationGatewayBackendAddressPool -Name "Pool1" `
       -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com
$fp01 = New-AzApplicationGatewayFrontendPort -Name "HTTPPort" -Port 80

$listener01 = New-AzApplicationGatewayHttpListener -Name "HTTPListener" `
             -Protocol Http -FrontendIPConfiguration $fip -FrontendPort $fp01

$setting = New-AzApplicationGatewayBackendHttpSettings -Name "BackendHttpSetting1" `
          -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

## <a name="specify-your-http-header-rewrite-rule-configuration"></a>Geef de HTTP-header herschrijven regelconfiguratie

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

## <a name="specify-the-routing-rule"></a>Geef de regel voor doorsturen

Maak een regel voor het doorsturen van aanvragen. Eenmaal gemaakt, wordt deze configuratie opnieuw schrijven is gekoppeld aan de bron-listener via de regel voor doorsturen. Wanneer u een basisregel voor routering, wordt de configuratie van de herschrijven header is gekoppeld aan de listener van een bron en is een algemene header herschrijven. Wanneer een regel op pad gebaseerde routering wordt gebruikt, wordt de configuratie van de koptekst opnieuw schrijven is gedefinieerd in de URL-path-map. Dus dit alleen van toepassing op het padgebied van het specifieke van een site. Hieronder is een eenvoudige regel voor doorsturen wordt gemaakt en de regelset herschrijven is gekoppeld.

```azurepowershell
$rule01 = New-AzApplicationGatewayRequestRoutingRule -Name "Rule1" -RuleType basic `
         -BackendHttpSettings $setting -HttpListener $listener01 -BackendAddressPool $pool -RewriteRuleSet $rewriteRuleSet
```

## <a name="specify-autoscale"></a>Automatisch schalen configureren

U kunt nu de configuratie voor automatisch schalen opgeven voor de toepassingsgateway. Er worden twee configuratietypen voor automatisch schalen ondersteund:

* **Modus voor vaste capaciteit**. In deze modus wordt er geen automatisch schalen uitgevoerd voor de toepassingsgateway en werkt deze met een vaste schaaleenheid-capaciteit.

   ```azurepowershell
   $sku = New-AzApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2 -Capacity 2
   ```

* **Modus voor automatisch schalen**. In deze modus wordt de schaal van de toepassingsgateway automatisch aangepast op basis van het patroon van het toepassingsverkeer.

   ```azurepowershell
   $autoscaleConfig = New-AzApplicationGatewayAutoscaleConfiguration -MinCapacity 2
   $sku = New-AzApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2
   ```

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

Maak de toepassingsgateway en neem redundantie-zones en de configuratie voor automatisch schalen op.

```azurepowershell
$appgw = New-AzApplicationGateway -Name "AutoscalingAppGw" -Zone 1,2,3 -ResourceGroupName $rg -Location $location -BackendAddressPools $pool -BackendHttpSettingsCollection $setting -GatewayIpConfigurations $ipconfig -FrontendIpConfigurations $fip -FrontendPorts $fp01 -HttpListeners $listener01 -RequestRoutingRules $rule01 -Sku $sku -AutoscaleConfiguration $autoscaleConfig -RewriteRuleSet $rewriteRuleSet
```

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

Gebruik Get-AzureRmPublicIPAddress om het openbare IP-adres van de toepassingsgateway op te halen. Kopieer het openbare IP-adres of de DNS-naam en plak het adres of de naam in de adresbalk van de browser.

```azurepowershell
Get-AzPublicIPAddress -ResourceGroupName $rg -Name AppGwVIP
```



## <a name="clean-up-resources"></a>Resources opschonen

Verken eerst de resources die samen met de toepassingsgateway zijn gemaakt. U kunt de resourcegroep, de toepassingsgateway en alle gerelateerde resources verwijderen met de opdracht `Remove-AzResourceGroup` als u deze niet meer nodig hebt.

`Remove-AzResourceGroup -Name $rg`

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een toepassingsgateway maken met omleidingsregels op basis van een URL-pad](./tutorial-url-route-powershell.md)
