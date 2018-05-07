---
title: Maken van een toepassingsgateway - Azure PowerShell | Microsoft Docs
description: Informatie over het maken van een toepassingsgateway met behulp van Azure PowerShell.
services: application-gateway
author: vhorne
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: azurepowershell
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/25/2018
ms.author: victorh
ms.openlocfilehash: 1ad0fcd135e664b2b97fd16ebe851c817f567d7f
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
---
# <a name="create-an-application-gateway-using-azure-powershell"></a>Een toepassingsgateway met Azure PowerShell maken

U kunt Azure PowerShell gebruiken voor het maken of beheren van Toepassingsgateways vanaf de opdrachtregel of in scripts. Deze snelstartgids wordt beschreven hoe u netwerkbronnen, back-endservers en een toepassingsgateway maken.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, wordt voor deze zelfstudie moduleversie 3.6 of hoger van Azure PowerShell vereist. Ga voor de versie uitvoeren `Get-Module -ListAvailable AzureRM` . Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een Azure-resource groep met [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Netwerkbronnen maken 

De subnetconfiguraties met maken [nieuw AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Maken van het virtuele netwerk met [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) met de subnetconfiguraties. En maak ten slotte het openbare IP-adres met [nieuw AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). Deze resources worden gebruikt om de netwerkverbinding met de toepassingsgateway en de bijbehorende bronnen opgeven.

```azurepowershell-interactive
$backendSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$agSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.2.0/24
New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $backendSubnetConfig, $agSubnetConfig
New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```
## <a name="create-backend-servers"></a>Back-endservers maken

In dit voorbeeld maakt u twee virtuele machines moet worden gebruikt als back-endservers voor de toepassingsgateway. U kunt ook IIS installeren op de virtuele machines om te controleren of de toepassingsgateway is gemaakt.

### <a name="create-two-virtual-machines"></a>Twee virtuele machines maken

Maken van een netwerkinterface met [nieuw AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). Maken van een virtuele-machineconfiguratie met [nieuw AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). Wanneer u de volgende opdrachten uitvoert, wordt u gevraagd referenties op te geven. Voer *azureuser* voor de gebruikersnaam en *Azure123456!* voor het wachtwoord. Maak de virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$cred = Get-Credential
for ($i=1; $i -le 2; $i++)
{
  $nic = New-AzureRmNetworkInterface `
    -Name myNic$i `
    -ResourceGroupName myResourceGroupAG `
    -Location EastUS `
    -SubnetId $vnet.Subnets[1].Id
  $vm = New-AzureRmVMConfig `
    -VMName myVM$i `
    -VMSize Standard_DS2
  $vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred
  $vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  $vm = Add-AzureRmVMNetworkInterface `
    -VM $vm `
    -Id $nic.Id
  $vm = Set-AzureRmVMBootDiagnostics `
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

### <a name="create-the-ip-configurations-and-frontend-port"></a>De IP-configuraties en de front-endpoort maken

Gebruik [nieuw AzureRmApplicationGatewayIPConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration) om de configuratie die wordt gekoppeld aan het subnet dat u eerder hebt gemaakt met de toepassingsgateway te maken. Gebruik [nieuw AzureRmApplicationGatewayFrontendIPConfig](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig) om de configuratie waarmee het openbare IP-adres dat u ook een eerder gemaakt worden toegewezen aan de toepassingsgateway te maken. Gebruik [nieuw AzureRmApplicationGatewayFrontendPort](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendport) poort 80 moet worden gebruikt voor toegang tot de toepassingsgateway toewijzen.

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$pip = Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress 
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

### <a name="create-the-backend-pool"></a>De back-endpool maken

Gebruik [nieuw AzureRmApplicationGatewayBackendAddressPool](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool) om de groep back-end voor de toepassingsgateway te maken. Configureer de instellingen voor de back-end-pool met [nieuw AzureRmApplicationGatewayBackendHttpSettings](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings).

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

### <a name="create-the-listener-and-add-a-rule"></a>De listener maken en een regel toevoegen

Een listener is vereist voor het inschakelen van de toepassingsgateway voor het verkeer op de juiste wijze te routeren naar de back-endpool. Maak een listener met [nieuw AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) met de frontend-configuratie en de frontend-poort die u eerder hebt gemaakt. Een regel is vereist voor de listener te weten welke back-endpool moet worden gebruikt voor binnenkomend verkeer. Gebruik [nieuw AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule) voor het maken van een regel met naam *rule1*.

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

Nu u de benodigde ondersteunende resources hebt gemaakt, gebruiken [nieuw AzureRmApplicationGatewaySku](/powershell/module/azurerm.network/new-azurermapplicationgatewaysku) Geef parameters op voor de toepassingsgateway en gebruik daarna [New-AzureRmApplicationGateway](/powershell/module/azurerm.network/new-azurermapplicationgateway) naar Deze maken.

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

## <a name="test-the-application-gateway"></a>Testen van de toepassingsgateway

Gebruik [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) ophalen van het openbare IP-adres van de toepassingsgateway. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser.

```azurepowershell-interactive
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Test toepassingsgateway](./media/application-gateway-create-gateway-arm/application-gateway-iistest.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, kunt u de [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) opdracht de resourcegroep, een toepassingsgateway verwijderen en alle bijbehorende resources.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een resourcegroep, netwerkbronnen en backend-servers gemaakt. Vervolgens gebruikt u deze resources om een toepassingsgateway te maken. Blijven de artikelen voor meer informatie over Toepassingsgateways en de bijbehorende resources.

