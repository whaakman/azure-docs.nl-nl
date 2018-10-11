---
title: Een automatisch schalende, zoneredundante toepassingsgateway met een gereserveerde IP-adres maken met Azure PowerShell
description: Leer hoe u een automatisch schalende, zoneredundante toepassingsgateway met een gereserveerde IP-adres maakt met Azure PowerShell.
services: application-gateway
author: amitsriva
ms.service: application-gateway
ms.topic: tutorial
ms.date: 9/26/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: d86ce2e1bac2fb58df8df748381a00eac21e65cb
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48016931"
---
# <a name="tutorial-create-an-autoscaling-zone-redundant-application-gateway-with-a-reserved-virtual-ip-address-using-azure-powershell"></a>Zelfstudie: Een automatisch schalende, zoneredundante toepassingsgateway met een gereserveerde IP-adres maken met Azure PowerShell

In deze zelfstudie wordt beschreven hoe u in Azure een toepassingsgateway maakt met behulp van Azure PowerShell-cmdlets en het Azure Resource Manager-implementatiemodel. In deze zelfstudie wordt vooral gekeken naar de verschillen tussen de nieuwe SLU met automatische schaalaanpassing en de bestaande standaard-SKU, en dan met name de functies voor automatisch schalen, zoneredundantie en gereserveerde VIP's (statische IP-adressen).

Meer informatie over automatisch schalen en zoneredundantie voor toepassingsgateways vindt u in [Automatisch schalen en zoneredundantie voor toepassingsgateway (openbare preview)](application-gateway-autoscaling-zone-redundant.md).

> [!IMPORTANT]
> De SKU met automatisch schalen en zoneredundantie voor toepassingsgateways is momenteel in openbare preview. Deze preview wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De configuratieparameter voor automatisch schalen instellen
> * De zoneparameter gebruiken
> * Een statisch VIP gebruiken
> * De toepassingsgateway maken


Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Voor deze zelfstudie moet u Azure PowerShell lokaal uitvoeren. U hebt versie 6.9.0 of hoger van de Azure PowerShell-module nodig. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Nadat u de versie van PowerShell hebt gecontroleerd, voert u `Login-AzureRmAccount` uit om een verbinding op te zetten met Azure.

## <a name="sign-in-to-your-azure-account"></a>Aanmelden bij uw Azure-account

```azurepowershell
Connect-AzureRmAccount
Select-AzureRmSubscription -Subscription "<sub name>"
```
## <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een resourcegroep op een van de beschikbare locaties.

```azurepowershell
$location = "East US 2"
$rg = "<rg name>"

#Create a new Resource Group
New-AzureRmResourceGroup -Name $rg -Location $location
```

## <a name="create-a-vnet"></a>Een VNet maken
Maak een VNet met één gereserveerd subnet voor een toepassingsgateway met automatisch schalen. Op dit moment kan er in elk gereserveerd subnet maar één toepassingsgateway met automatisch schalen worden geïmplementeerd.

```azurepowershell
#Create VNet with two subnets
$sub1 = New-AzureRmVirtualNetworkSubnetConfig -Name "AppGwSubnet" -AddressPrefix "10.0.0.0/24"
$sub2 = New-AzureRmVirtualNetworkSubnetConfig -Name "BackendSubnet" -AddressPrefix "10.0.1.0/24"
$vnet = New-AzureRmvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg `
       -Location $location -AddressPrefix "10.0.0.0/16" -Subnet $sub1, $sub2
```

## <a name="create-a-reserved-public-ip"></a>Een gereserveerd openbaar IP-adres maken

Stel de toewijzingsmethode van PublicIPAddress in op **Static**. De VIP van een toepassingsgateway met automatisch schalen kan alleen statisch zijn. Dynamische VIP's worden niet ondersteund. Alleen PublicIpAddress uit de standaard-SKU wordt ondersteund.

```azurepowershell
#Create static public IP
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP" `
       -location $location -AllocationMethod Static -Sku Standard
```

## <a name="retrieve-details"></a>Gegevens ophalen

Haal gegevens op van de resourcegroep, het subnet en een IP-in een lokaal object om de IP-configuratiegegevens van de toepassingsgateway te maken.

```azurepowershell
$resourceGroup = Get-AzureRmResourceGroup -Name $rg
$publicip = Get-AzureRmPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP"
$vnet = Get-AzureRmvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg
$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "AppGwSubnet" -VirtualNetwork $vnet
```
## <a name="configure-application-gateway-infrastructure"></a>Infrastructuur van toepassingsgateway configureren
Configureer de waarden voor IP-configuratie, front-end-IP-configuratie, back-endpool, HTTP-instellingen, certificaat, poort, listener en regel met exact dezelfde indeling als de bestaande standaardtoepassingsgateway. De nieuwe SKU volgt hetzelfde objectmodel als de standaard-SKU.

```azurepowershell
$ipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "IPConfig" -Subnet $gwSubnet
$fip = New-AzureRmApplicationGatewayFrontendIPConfig -Name "FrontendIPCOnfig" -PublicIPAddress $publicip
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name "Pool1" `
       -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "SSLPort" -Port 443
$fp02 = New-AzureRmApplicationGatewayFrontendPort -Name "HTTPPort" -Port 80

$securepfxpwd = ConvertTo-SecureString -String "scrap" -AsPlainText -Force
$sslCert01 = New-AzureRmApplicationGatewaySslCertificate -Name "SSLCert" -Password $securepfxpwd `
            -CertificateFile "D:\Networking\ApplicationGateway\scrap.pfx"
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "SSLListener" `
             -Protocol Https -FrontendIPConfiguration $fip -FrontendPort $fp01 -SslCertificate $sslCert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "HTTPListener" `
             -Protocol Http -FrontendIPConfiguration $fip -FrontendPort $fp02

$setting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "BackendHttpSetting1" `
          -Port 80 -Protocol Http -CookieBasedAffinity Disabled
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "Rule1" -RuleType basic `
         -BackendHttpSettings $setting -HttpListener $listener01 -BackendAddressPool $pool
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "Rule2" -RuleType basic `
         -BackendHttpSettings $setting -HttpListener $listener02 -BackendAddressPool $pool
```

## <a name="specify-autoscale"></a>Automatisch schalen configureren

U kunt nu de configuratie voor automatisch schalen opgeven voor de toepassingsgateway. Er worden twee configuratietypen voor automatisch schalen ondersteund:

- **Modus voor vaste capaciteit**. In deze modus wordt er geen automatisch schalen uitgevoerd voor de toepassingsgateway en werkt deze met een vaste schaaleenheid-capaciteit.

   ```azurepowershell
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2 -Capacity 2
   ```
- **Modus voor automatisch schalen**. In deze modus wordt de schaal van de toepassingsgateway automatisch aangepast op basis van het patroon van het toepassingsverkeer.

   ```azurepowershell
   $autoscaleConfig = New-AzureRmApplicationGatewayAutoscaleConfiguration -MinCapacity 2
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2
   ```

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

Maak de toepassingsgateway en voeg redundantiezones toe. 

De zoneconfiguratie wordt alleen ondersteund in regio's waar Azure Zones beschikbaar is. In regio's waar Azure Zones niet beschikbaar is, moet de zoneparameter niet worden gebruikt. Een toepassingsgateway kan ook worden geïmplementeerd in een enkele zone, twee zones of alle drie de zones. Het PublicIPAddress voor een toepassingsgateway met één zone moet aan dezelfde zone worden gebonden. Voor een toepassingsgateway met twee of drie zones moet het PublicIPAddress ook zoneredundant zijn, en wordt er dus geen zone opgegeven.

```azurepowershell
$appgw = New-AzureRmApplicationGateway -Name "AutoscalingAppGw" -Zone 1,2,3 `
  -ResourceGroupName $rg -Location $location -BackendAddressPools $pool `
  -BackendHttpSettingsCollection $setting -GatewayIpConfigurations $ipconfig `
  -FrontendIpConfigurations $fip -FrontendPorts $fp01, $fp02 `
  -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 `
  -Sku $sku -sslCertificates $sslCert01 -AutoscaleConfiguration $autoscaleConfig
```

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

Gebruik [Get-AzureRmPublicIPAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) om het openbare IP-adres van de toepassingsgateway op te halen. Kopieer het openbare IP-adres of de DNS-naam en plak het adres of de naam in de adresbalk van de browser.

`Get-AzureRmPublicIPAddress -ResourceGroupName $rg -Name AppGwVIP`

## <a name="clean-up-resources"></a>Resources opschonen
Bekijk eerst de resources die met de toepassingsgateway zijn gemaakt en wanneer u deze niet meer nodig hebt, kunt u met de opdracht `Remove-AzureRmResourceGroup` de resourcegroep, de toepassingsgateway en alle gerelateerde resources verwijderen.

`Remove-AzureRmResourceGroup -Name $rg`

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een statisch VIP gebruiken
> * De configuratieparameter voor automatisch schalen instellen
> * De zoneparameter gebruiken
> * De toepassingsgateway maken

> [!div class="nextstepaction"]
> [Een toepassingsgateway maken met omleidingsregels op basis van een URL-pad](./tutorial-url-route-powershell.md)
