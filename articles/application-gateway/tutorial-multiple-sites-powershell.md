---
title: Een toepassingsgateway maken waarop meerdere websites worden gehost - Azure PowerShell
description: Informatie over het maken van een toepassingsgateway waarop meerdere websites worden gehost met Azure PowerShell.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 7/13/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 5c9d02cf1bfb9b8226328d1923859dd97ba2b79d
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67502043"
---
# <a name="create-an-application-gateway-that-hosts-multiple-web-sites-using-azure-powershell"></a>Een toepassingsgateway maken waarop meerdere websites worden gehost met behulp van Azure PowerShell

U kunt Azure Powershell gebruiken om [het hosten van meerdere websites te configureren](multiple-site-overview.md) als u een [toepassingsgateway](overview.md) maakt. In deze zelfstudie definieert u back-endadresgroepen met behulp van schaalsets voor virtuele machines. Vervolgens configureert u listeners en regels op basis van domeinen waarvan u eigenaar bent om er zeker van te zijn dat webverkeer bij de juiste servers in de pools binnenkomen. In deze zelfstudie wordt ervan uitgegaan dat u eigenaar bent van meerdere domeinen en voorbeelden gebruikt van *www.contoso.com* en *www.fabrikam.com*.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Het netwerk instellen
> * Een toepassingsgateway maken
> * Back-endlisteners maken
> * Routeringsregels maken
> * Schaalsets voor virtuele machines maken met de back-endpools
> * Een CNAME-record in uw domein maken

![Voorbeeld van routeren voor meerdere sites](./media/tutorial-multiple-sites-powershell/scenario.png)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de PowerShell lokaal, in deze zelfstudie is vereist voor de Azure PowerShell-moduleversie 1.0.0 of hoger. Voer `Get-Module -ListAvailable Az` uit om de versie te vinden. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Maak een Azure-resource-groep met [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup).  

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Netwerkbronnen maken

Maak de subnetconfiguraties met behulp van [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig). Maken van het virtuele netwerk met [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) met de subnetconfiguraties. En maak ten slotte het openbare IP-adres met [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress). Deze resources worden gebruikt om de netwerkverbinding naar de toepassingsgateway en de bijbehorende bronnen te leveren.

```azurepowershell-interactive
$backendSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24

$agSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.2.0/24

$vnet = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $backendSubnetConfig, $agSubnetConfig

$pip = New-AzPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

### <a name="create-the-ip-configurations-and-frontend-port"></a>IP-configuraties en front-endpoort maken

Koppelen van het subnet dat u eerder hebt gemaakt met de application gateway via [New-AzApplicationGatewayIPConfiguration](/powershell/module/az.network/new-azapplicationgatewayipconfiguration). Het openbare IP-adres toewijzen aan de application gateway met [New-AzApplicationGatewayFrontendIPConfig](/powershell/module/az.network/new-azapplicationgatewayfrontendipconfig).

```azurepowershell-interactive
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet

$subnet=$vnet.Subnets[0]

$gipconfig = New-AzApplicationGatewayIPConfiguration `
  -Name myAGIPConfig `
  -Subnet $subnet

$fipconfig = New-AzApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -PublicIPAddress $pip

$frontendport = New-AzApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 80
```

### <a name="create-the-backend-pools-and-settings"></a>Back-endpools en instellingen maken

Maken van de eerste back-end-adresgroep voor de application gateway met [New-AzApplicationGatewayBackendAddressPool](/powershell/module/az.network/new-azapplicationgatewaybackendaddresspool). Configureer de instellingen voor de groep van toepassingen met [New-AzApplicationGatewayBackendHttpSettings](/powershell/module/az.network/new-azapplicationgatewaybackendhttpsetting).

```azurepowershell-interactive
$contosoPool = New-AzApplicationGatewayBackendAddressPool `
  -Name contosoPool

$fabrikamPool = New-AzApplicationGatewayBackendAddressPool `
  -Name fabrikamPool

$poolSettings = New-AzApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-listeners-and-rules"></a>Listeners en regels maken

Listeners zijn vereist om de toepassingsgateway verkeer op de juiste manier naar de back-endadresgroepen te kunnen laten doorsturen. In deze zelfstudie maakt u twee listeners voor de twee domeinen. In dit voorbeeld worden listeners gemaakt voor de domeinen van *www.contoso.com* en *www.fabrikam.com*.

Maken van de eerste listener met behulp van [New-AzApplicationGatewayHttpListener](/powershell/module/az.network/new-azapplicationgatewayhttplistener) met de front-end-configuratie en de frontend-poort die u eerder hebt gemaakt. Er is een regel vereist, zodat de listener weet welke back-endpool moet worden gebruikt voor binnenkomend verkeer. Maak een eenvoudige regel met de naam *contosoRule* met behulp van [New-AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/new-azapplicationgatewayrequestroutingrule).

```azurepowershell-interactive
$contosolistener = New-AzApplicationGatewayHttpListener `
  -Name contosoListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport `
  -HostName "www.contoso.com"

$fabrikamlistener = New-AzApplicationGatewayHttpListener `
  -Name fabrikamListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport `
  -HostName "www.fabrikam.com"

$contosoRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name contosoRule `
  -RuleType Basic `
  -HttpListener $contosoListener `
  -BackendAddressPool $contosoPool `
  -BackendHttpSettings $poolSettings

$fabrikamRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name fabrikamRule `
  -RuleType Basic `
  -HttpListener $fabrikamListener `
  -BackendAddressPool $fabrikamPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>De toepassingsgateway maken

Nu dat u de benodigde ondersteunende netwerkbronnen gemaakt, Geef parameters op voor de application gateway met [New-AzApplicationGatewaySku](/powershell/module/az.network/new-azapplicationgatewaysku), en maak vervolgens met behulp van [New-AzApplicationGateway](/powershell/module/az.network/new-azapplicationgateway).

```azurepowershell-interactive
$sku = New-AzApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2

$appgw = New-AzApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $contosoPool, $fabrikamPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $contosoListener, $fabrikamListener `
  -RequestRoutingRules $contosoRule, $fabrikamRule `
  -Sku $sku
```

## <a name="create-virtual-machine-scale-sets"></a>Virtuele-machineschaalset maken

In dit voorbeeld maakt u twee schaalsets voor virtuele machines die ondersteuning bieden voor de twee back-end-pools die u hebt gemaakt. De schaalsets die u maakt, hebben de namen *myvmss1* en *myvmss2*. Elke schaalset bevat twee instanties van virtuele machines waarop u IIS installeert. U wijst de schaalset toe aan de back-endpool wanneer u de IP-instellingen configureert.

```azurepowershell-interactive
$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet

$appgw = Get-AzApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway

$contosoPool = Get-AzApplicationGatewayBackendAddressPool `
  -Name contosoPool `
  -ApplicationGateway $appgw

$fabrikamPool = Get-AzApplicationGatewayBackendAddressPool `
  -Name fabrikamPool `
  -ApplicationGateway $appgw

for ($i=1; $i -le 2; $i++)
{
  if ($i -eq 1) 
  {
    $poolId = $contosoPool.Id
  }
  if ($i -eq 2)
  {
    $poolId = $fabrikamPool.Id
  }

  $ipConfig = New-AzVmssIpConfig `
    -Name myVmssIPConfig$i `
    -SubnetId $vnet.Subnets[1].Id `
    -ApplicationGatewayBackendAddressPoolsId $poolId

  $vmssConfig = New-AzVmssConfig `
    -Location eastus `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic

  Set-AzVmssStorageProfile $vmssConfig `
    -ImageReferencePublisher MicrosoftWindowsServer `
    -ImageReferenceOffer WindowsServer `
    -ImageReferenceSku 2016-Datacenter `
    -ImageReferenceVersion latest
    -OsDiskCreateOption FromImage

  Set-AzVmssOsProfile $vmssConfig `
    -AdminUsername azureuser `
    -AdminPassword "Azure123456!" `
    -ComputerNamePrefix myvmss$i

  Add-AzVmssNetworkInterfaceConfiguration `
    -VirtualMachineScaleSet $vmssConfig `
    -Name myVmssNetConfig$i `
    -Primary $true `
    -IPConfiguration $ipConfig

  New-AzVmss `
    -ResourceGroupName myResourceGroupAG `
    -Name myvmss$i `
    -VirtualMachineScaleSet $vmssConfig
}
```

### <a name="install-iis"></a>IIS installeren

```azurepowershell-interactive
$publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }

for ($i=1; $i -le 2; $i++)
{
  $vmss = Get-AzVmss `
    -ResourceGroupName myResourceGroupAG `
    -VMScaleSetName myvmss$i

  Add-AzVmssExtension -VirtualMachineScaleSet $vmss `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings

  Update-AzVmss `
    -ResourceGroupName myResourceGroupAG `
    -Name myvmss$i `
    -VirtualMachineScaleSet $vmss
}
```

## <a name="create-cname-record-in-your-domain"></a>CNAME-record in uw domein maken

Als de toepassingsgateway met het bijbehorende openbare IP-adres is gemaakt, kunt u het DNS-adres ophalen en dit gebruiken om een CNAME-record in uw domein te maken. U kunt [Get-AzPublicIPAddress](/powershell/module/az.network/get-azpublicipaddress) om op te halen van de DNS-adres van de toepassingsgateway. Kopieer de waarde *fqdn* van DNSSettings en gebruik deze als de waarde van de CNAME-record die u maakt. Het gebruik van A-records wordt niet aanbevolen, omdat de VIP kan veranderen wanneer de toepassingsgateway opnieuw wordt gestart.

```azurepowershell-interactive
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

## <a name="test-the-application-gateway"></a>Toepassingsgateway testen

Voer uw domeinnaam in de adresbalk van de browser in. Zoals http:\//www.contoso.com.

![Contoso-site testen in toepassingsgateway](./media/tutorial-multiple-sites-powershell/application-gateway-iistest.png)

Wijzig het adres in uw andere domein. U krijgt iets te zien zoals in het volgende voorbeeld:

![Fabrikam-site testen in toepassingsgateway](./media/tutorial-multiple-sites-powershell/application-gateway-iistest2.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de resourcegroep, application-gateway en alle gerelateerde resources met behulp van [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup).

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Het netwerk instellen
> * Een toepassingsgateway maken
> * Back-endlisteners maken
> * Routeringsregels maken
> * Schaalsets voor virtuele machines maken met de back-endpools
> * Een CNAME-record in uw domein maken

> [!div class="nextstepaction"]
> [Een toepassingsgateway maken met omleidingsregels op basis van een URL-pad](./tutorial-url-route-powershell.md)
