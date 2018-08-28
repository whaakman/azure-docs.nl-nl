---
title: Webverkeer beheren - Azure PowerShell
description: Leer hier hoe u een toepassingsgateway met een virtuele-machineschaalset maakt om webverkeer te beheren met behulp van Azure PowerShell.
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 6/5/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 0ece839777747ac7f3683f6f475f3af9d050190e
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41918083"
---
# <a name="manage-web-traffic-with-an-application-gateway-using-azure-powershell"></a>Webverkeer met een toepassingsgateway beheren met behulp van Azure PowerShell

De toepassingsgateway wordt gebruikt voor het beheren en beveiligen van webverkeer naar servers die door u worden onderhouden. U kunt Azure PowerShell gebruiken om een ​​[toepassingsgateway](overview.md) te maken die gebruikmaakt van een [virtuele-machineschaalset](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) voor back-endservers om webverkeer te beheren. In dit voorbeeld bevat de schaalset twee virtuele-machine-instanties die zijn toegevoegd aan de standaard back-endpool van de toepassingsgateway.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Het netwerk instellen
> * Een toepassingsgateway maken
> * Een virtuele-machineschaalset maken met de standaard back-endpool

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie versie 3.6 of hoger van de Azure PowerShell-module vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Maak een Azure-resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).  

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Netwerkbronnen maken 

Configureer de subnetten *myBackendSubnet* en *myAGSubnet* met [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Maak het virtuele netwerk *myVNet* met [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) met de subnetconfiguraties. En maak ten slotte het openbare IP-adres *myAGPublicIPAddress* met [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). Deze resources worden gebruikt om de netwerkverbinding naar de toepassingsgateway en de bijbehorende bronnen te leveren.

```azurepowershell-interactive
$backendSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24

$agSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.2.0/24

$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $backendSubnetConfig, $agSubnetConfig

$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

In deze sectie kunt u resources maken die ondersteuning bieden voor de toepassingsgateway. Ten slotte maakt u deze. De resources die u maakt, zijn onder andere:

- *IP-configuraties en front-endpoort*: hiermee koppelt u het subnet dat u eerder hebt gemaakt aan de toepassingsgateway en wijst u een poort toe die u gebruikt om de gateway te openen.
- *Standaardpool*: alle toepassingsgateways moeten ten minste één back-endpool met servers hebben.
- *Standaard-listener en regel*: de standaard-listener luistert naar verkeer op de poort die is toegewezen en de standaardregel verzendt verkeer naar de standaardpool.

### <a name="create-the-ip-configurations-and-frontend-port"></a>IP-configuraties en front-endpoort maken

Koppel *myAGSubnet* dat u eerder hebt gemaakt aan de toepassingsgateway met behulp van [New-AzureRmApplicationGatewayIPConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration). Wijs *myAGPublicIPAddress* toe aan de toepassingsgateway met behulp van [New-AzureRmApplicationGatewayFrontendIPConfig](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig).

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet

$subnet=$vnet.Subnets[0]

$gipconfig = New-AzureRmApplicationGatewayIPConfiguration `
  -Name myAGIPConfig `
  -Subnet $subnet

$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -PublicIPAddress $pip

$frontendport = New-AzureRmApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 80
```

### <a name="create-the-backend-pool-and-settings"></a>Back-endpool en instellingen maken

Maak de back-endpool met de naam *appGatewayBackendPool* voor de toepassingsgateway met behulp van [New-AzureRmApplicationGatewayBackendAddressPool](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool). Configureer de instellingen voor de back-end-adrespools met behulp van [New-AzureRmApplicationGatewayBackendHttpSettings](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings).

```azurepowershell-interactive
$defaultPool = New-AzureRmApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool

$poolSettings = New-AzureRmApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-default-listener-and-rule"></a>Standaardlistener en -regel maken

Een listener is nodig om de toepassingsgateway in staat te stellen het verkeer op de juiste manier naar de back-endpool door te sturen. In dit voorbeeld maakt u een eenvoudige listener die luistert naar verkeer op de basis-URL. 

Maak een listener met de naam *mydefaultListener* met behulp van [New-AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) met de front-endconfiguratie en de front-endpoort die u eerder hebt gemaakt. Er is een regel vereist zodat de listener weet welke back-endpool moet worden gebruikt voor binnenkomend verkeer. Maak een basisregel met de naam *rule1* met behulp van [New-AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule).

```azurepowershell-interactive
$defaultlistener = New-AzureRmApplicationGatewayHttpListener `
  -Name mydefaultListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport

$frontendRule = New-AzureRmApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultlistener `
  -BackendAddressPool $defaultPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>De toepassingsgateway maken

Nu u de benodigde ondersteunende resources hebt gemaakt, geeft u parameters op voor de toepassingsgateway met [New-AzureRmApplicationGatewaySku](/powershell/module/azurerm.network/new-azurermapplicationgatewaysku) en maakt u deze vervolgens met [New-AzureRmApplicationGateway](/powershell/module/azurerm.network/new-azurermapplicationgateway).

```azurepowershell-interactive
$sku = New-AzureRmApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2

$appgw = New-AzureRmApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $defaultPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $defaultlistener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku
```

## <a name="create-a-virtual-machine-scale-set"></a>Een virtuele-machineschaalset maken

In dit voorbeeld maakt u een virtuele-machineschaalset om servers op te geven voor de back-endpool in de toepassingsgateway. U wijst de schaalset toe aan de back-endpool wanneer u de IP-instellingen configureert.

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet

$appgw = Get-AzureRmApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway

$backendPool = Get-AzureRmApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool `
  -ApplicationGateway $appgw

$ipConfig = New-AzureRmVmssIpConfig `
  -Name myVmssIPConfig `
  -SubnetId $vnet.Subnets[1].Id `
  -ApplicationGatewayBackendAddressPoolsId $backendPool.Id

$vmssConfig = New-AzureRmVmssConfig `
  -Location eastus `
  -SkuCapacity 2 `
  -SkuName Standard_DS2 `
  -UpgradePolicyMode Automatic

Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher MicrosoftWindowsServer `
  -ImageReferenceOffer WindowsServer `
  -ImageReferenceSku 2016-Datacenter `
  -ImageReferenceVersion latest `
  -OsDiskCreateOption FromImage

Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword "Azure123456!" `
  -ComputerNamePrefix myvmss

Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name myVmssNetConfig `
  -Primary $true `
  -IPConfiguration $ipConfig

New-AzureRmVmss `
  -ResourceGroupName myResourceGroupAG `
  -Name myvmss `
  -VirtualMachineScaleSet $vmssConfig
```

### <a name="install-iis"></a>IIS installeren

```azurepowershell-interactive
$publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }

$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroupAG -VMScaleSetName myvmss

Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss `
  -Name "customScript" `
  -Publisher "Microsoft.Compute" `
  -Type "CustomScriptExtension" `
  -TypeHandlerVersion 1.8 `
  -Setting $publicSettings

Update-AzureRmVmss `
  -ResourceGroupName myResourceGroupAG `
  -Name myvmss `
  -VirtualMachineScaleSet $vmss
```

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

Gebruik [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) om het openbare IP-adres van de toepassingsgateway op te halen. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser.

```azurepowershell-interactive
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Basis-URL testen in de toepassingsgateway](./media/tutorial-manage-web-traffic-powershell/tutorial-iistest.png)

## <a name="clean-up-resources"></a>Resources opschonen

U kunt [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) gebruiken om de resourcegroep, de toepassingsgateway en alle gerelateerde resources te verwijderen wanneer u deze niet meer nodig hebt.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Het netwerk instellen
> * Een toepassingsgateway maken
> * Een virtuele-machineschaalset maken met de standaard back-endpool

> [!div class="nextstepaction"]
> [Webverkeer beperken met een firewall voor webtoepassingen](./tutorial-restrict-web-traffic-powershell.md)
