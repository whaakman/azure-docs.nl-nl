---
title: 'Snelstart: webverkeer omleiden met Azure Application Gateway - Azure PowerShell | Microsoft Docs'
description: Lees hoe u Azure PowerShell kunt gebruiken om een Azure Application Gateway te maken die webverkeer omleidt naar virtuele machines in een back-endpool.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 1/11/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 9edfa85105bbc20cf7f149d4c31b60d9e570a7ad
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54243726"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-powershell"></a>Quickstart: Webverkeer omleiden met Azure Application Gateway - Azure PowerShell

In deze quickstart wordt beschreven hoe u de Azure-portal kunt gebruiken om snel een toepassingsgateway te maken met twee virtuele machines in zijn back-endpool. Vervolgens test u de gateway om er zeker van te zijn dat deze correct werkt. Met Azure Application Gateway kunt u webverkeer van uw toepassing omleiden naar specifieke resources door listeners toe te wijzen aan poorten, regels te maken en resources toe te voegen aan een back-endpool.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

## <a name="run-azure-powershell-locally"></a>Azure PowerShell lokaal uitvoeren

Als u Azure PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie moduleversie 3.6 of hoger van Azure PowerShell vereist.

1. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). 
2. Voer `Login-AzureRmAccount` uit om een verbinding met Azure tot stand te brengen.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

In Azure kunt u verwante resources toewijzen aan een resourcegroep. Maak als volgt een resourcegroep met de cmdlet [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup): 

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Netwerkbronnen maken

Maak een virtueel netwerk om de toepassingsgateway in staat te stellen te communiceren met andere resources. In dit voorbeeld worden twee subnetten gemaakt: één voor de toepassingsgateway en één voor de back-endservers. Het subnet van de toepassingsgateway kan alleen bestaan uit toepassingsgateways. Andere resources zijn niet toegestaan.

1. Maak de subnetconfiguraties door [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) aan te roepen.
2. Maak het virtuele netwerk met de subnetconfiguraties door [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) aan te roepen.
3. Maak het openbare IP-adres door [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) aan te roepen.

```azurepowershell-interactive
$agSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$backendSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.2.0/24
New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $agSubnetConfig, $backendSubnetConfig
New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```
## <a name="create-backend-servers"></a>Back-endservers maken

In dit voorbeeld maakt u twee virtuele machines die door Azure worden gebruikt als back-endservers voor de toepassingsgateway. U installeert ook IIS op de virtuele machines om te controleren of de toepassingsgateway in Azure is gemaakt.

### <a name="create-two-virtual-machines"></a>Twee virtuele machines maken

1. Maak een netwerkinterfacekaart met [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). 
2. Maak een virtuele-machineconfiguratie met [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig).
3. Maak de virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

Wanneer u het volgende codevoorbeeld uitvoert om de virtuele machines te maken, wordt u gevraagd om referenties. Voer *azureuser* in als gebruikersnaam en *Azure123456!* als wachtwoord:
    
```azurepowershell-interactive
$vnet   = Get-AzureRmVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myBackendSubnet
$cred = Get-Credential
for ($i=1; $i -le 2; $i++)
{
  $nic = New-AzureRmNetworkInterface `
    -Name myNic$i `
    -ResourceGroupName myResourceGroupAG `
    -Location EastUS `
    -SubnetId $subnet.Id
  $vm = New-AzureRmVMConfig `
    -VMName myVM$i `
    -VMSize Standard_DS2_v2
  Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred
  Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  Add-AzureRmVMNetworkInterface `
    -VM $vm `
    -Id $nic.Id
  Set-AzureRmVMBootDiagnostics `
    -VM $vm `
    -Disable
  New-AzureRmVM -ResourceGroupName myResourceGroupAG -Location EastUS -VM $vm
  Set-AzureRmVMExtension `
    -ResourceGroupName myResourceGroupAG `
    -ExtensionName IIS `
    -VMName myVM$i `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location EastUS
}
```

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

### <a name="create-the-ip-configurations-and-frontend-port"></a>IP-configuraties en front-endpoort maken

1. Gebruik [New-AzureRmApplicationGatewayIPConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration) voor het maken van de configuratie die het subnet dat u hebt gemaakt, koppelt aan de toepassingsgateway. 
2. Gebruik [New-AzureRmApplicationGatewayFrontendIPConfig](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig) voor het maken van de configuratie die het openbare IP-adres dat u eerder hebt gemaakt, toewijst aan de toepassingsgateway. 
3. Gebruik [New-AzureRmApplicationGatewayFrontendPort](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendport) om poort 80 toe te wijzen als toegangspoort voor de toepassingsgateway.

```azurepowershell-interactive
$vnet   = Get-AzureRmVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myAGSubnet
$pip    = Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress 
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

### <a name="create-the-backend-pool"></a>Back-endpool maken

1. Gebruik [New-AzureRmApplicationGatewayBackendAddressPool](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool) om de back-endpool voor de toepassingsgateway te maken. 
2. Configureer de instellingen voor de back-endpool met [New-AzureRmApplicationGatewayBackendHttpSettings](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings).

```azurepowershell-interactive
$address1 = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic1
$address2 = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic2
$backendPool = New-AzureRmApplicationGatewayBackendAddressPool `
  -Name myAGBackendPool `
  -BackendIPAddresses $address1.ipconfigurations[0].privateipaddress, $address2.ipconfigurations[0].privateipaddress
$poolSettings = New-AzureRmApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-listener-and-add-a-rule"></a>Listener maken en regel toevoegen

Als u de toepassingsgateway wilt inschakelen om het verkeer op de juiste manier naar de back-endpool te routeren, is in Azure een listener vereist. In Azure is ook een regel vereist, zodat de listener weet welke back-endpool moet worden gebruikt voor binnenkomend verkeer. 

1. Maak een listener met [New-AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) en de front-endconfiguratie en front-endpoort die u eerder hebt gemaakt. 
2. Gebruik [New-AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule) om regel *rule1* te maken. 

```azurepowershell-interactive
$defaultlistener = New-AzureRmApplicationGatewayHttpListener `
  -Name myAGListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$frontendRule = New-AzureRmApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultlistener `
  -BackendAddressPool $backendPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>De toepassingsgateway maken

Nu u de benodigde ondersteunende resources hebt gemaakt, kunt u de toepassingsgateway maken:

1. Gebruik [New-AzureRmApplicationGatewaySku](/powershell/module/azurerm.network/new-azurermapplicationgatewaysku) om parameters voor de toepassingsgateway op te geven.
2. Gebruik [New-AzureRmApplicationGateway](/powershell/module/azurerm.network/new-azurermapplicationgateway) om de toepassingsgateway te maken.

```azurepowershell-interactive
$sku = New-AzureRmApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2
New-AzureRmApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $backendPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $defaultlistener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku
```

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

Het is niet nodig IIS te installeren om de toepassingsgateway te maken, maar u hebt het in deze quickstart geïnstalleerd om te controleren of het maken van de toepassingsgateway in Azure is geslaagd. Gebruik IIS om de toepassingsgateway te testen:

1. Voer [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) uit om het openbare IP-adres van de toepassingsgateway op te halen. 
2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser. Als u de browser vernieuwt, ziet u de naam van de virtuele machine.

```azurepowershell-interactive
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Toepassingsgateway testen](./media/quick-create-powershell/application-gateway-iistest.png)


## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de bij de toepassingsgateway gemaakte resources niet meer nodig hebt, verwijdert u de resourcegroep. Als u de resourcegroep verwijdert, worden ook de toepassingsgateway en alle gerelateerde resources verwijderd. 

U kunt de resourcegroep als volgt verwijderen met de cmdlet [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup):

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Manage web traffic with an application gateway using Azure PowerShell](./tutorial-manage-web-traffic-powershell.md) (Webverkeer met een toepassingsgateway beheren met behulp van Azure PowerShell)

