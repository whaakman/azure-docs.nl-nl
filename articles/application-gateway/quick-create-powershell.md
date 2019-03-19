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
ms.openlocfilehash: de1162be381a905a3b04fbf7ae7dec59eb7d209a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57549591"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-powershell"></a>Quickstart: Webverkeer omleiden met Azure Application Gateway - Azure PowerShell

Deze quickstart laat zien hoe u snel een toepassingsgateway maken met de Azure-portal.  Nadat de toepassingsgateway is gemaakt, testen u vervolgens deze om te controleren of dat deze correct werkt. Met Azure Application Gateway, kunt u uw toepassing beveiligd webverkeer specifieke resources sturen door listeners toewijzen aan poorten, het maken van regels en resources toe te voegen aan een back-endpool. Om het eenvoudig, te worden in dit artikel wordt een eenvoudige configuratie met een openbaar front-end-IP-adres, een basislistener naar één site op deze application gateway-host, twee virtuele machines die worden gebruikt voor de back endpool en een regel voor het doorsturen van een algemene aanvraag.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

## <a name="prerequisites"></a>Vereisten

### <a name="azure-powershell-module"></a>Azure PowerShell-module

Als u wilt installeren en gebruiken van Azure PowerShell lokaal, in deze zelfstudie is vereist voor de Azure PowerShell-moduleversie 1.0.0 of hoger.

1. Voer `Get-Module -ListAvailable Az` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps). 
2. Voer `Login-AzAccount` uit om een verbinding met Azure tot stand te brengen.

### <a name="resource-group"></a>Resourcegroep

In Azure kunt u verwante resources toewijzen aan een resourcegroep. U kunt een bestaande resourcegroep gebruiken of een nieuwe maken. In dit voorbeeld wordt er een nieuwe resourcegroep maken met behulp van de [New-AzResourceGroup](/powershell/module/Az.resources/new-Azresourcegroup) cmdlet als volgt te werk: 

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupAG -Location eastus
```

### <a name="required-network-resources"></a>Vereiste netwerkbronnen

Er is een virtueel netwerk nodig voor communicatie tussen de resources die u maakt.  Het subnet van de toepassingsgateway kan alleen bestaan uit toepassingsgateways. Andere resources zijn niet toegestaan.  U kunt een nieuw subnet maken voor Application Gateway of gebruik een bestaande resourcegroep. In dit voorbeeld maakt u twee subnetten in dit voorbeeld: één voor de application gateway en een voor de back-endservers. U kunt configureren dat de Frontend-IP van de toepassingsgateway moet openbaar of privé aan de hand van uw situatie. In dit voorbeeld kiezen we een openbare front-end-IP-adres.

1. Maak de subnetconfiguraties door het aanroepen van [New-AzVirtualNetworkSubnetConfig](/powershell/module/Az.network/new-Azvirtualnetworksubnetconfig).
2. Het virtuele netwerk met de subnetconfiguraties maken door het aanroepen van [New-AzVirtualNetwork](/powershell/module/Az.network/new-Azvirtualnetwork). 
3. Het openbare IP-adres maken door het aanroepen van [New-AzPublicIpAddress](/powershell/module/Az.network/new-Azpublicipaddress). 

```azurepowershell-interactive
$agSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$backendSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.2.0/24
New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $agSubnetConfig, $backendSubnetConfig
New-AzPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```
### <a name="backend-servers"></a>Back-endservers

Back-end kan bestaan uit NIC's, virtuele-machineschaalsets, openbare IP-adressen, namen van interne IP-adressen, de volledig gekwalificeerde domeinnaam (FQDN) en multitenant back-ends, zoals Azure Appservice. In dit voorbeeld maakt u twee virtuele machines die door Azure worden gebruikt als back-endservers voor de toepassingsgateway. U installeert ook IIS op de virtuele machines om te controleren of de toepassingsgateway in Azure is gemaakt.

#### <a name="create-two-virtual-machines"></a>Twee virtuele machines maken

1. Maken van een netwerkinterface met [New-AzNetworkInterface](/powershell/module/Az.network/new-Aznetworkinterface). 
2. Maken van een virtuele-machineconfiguratie met [New-AzVMConfig](/powershell/module/Az.compute/new-Azvmconfig).
3. Maak de virtuele machine met [New-AzVM](/powershell/module/Az.compute/new-Azvm).

Wanneer u het volgende codevoorbeeld uitvoert om de virtuele machines te maken, wordt u gevraagd om referenties. Voer *azureuser* in als gebruikersnaam en *Azure123456!* als wachtwoord:
    
```azurepowershell-interactive
$vnet   = Get-AzVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myBackendSubnet
$cred = Get-Credential
for ($i=1; $i -le 2; $i++)
{
  $nic = New-AzNetworkInterface `
    -Name myNic$i `
    -ResourceGroupName myResourceGroupAG `
    -Location EastUS `
    -SubnetId $subnet.Id
  $vm = New-AzVMConfig `
    -VMName myVM$i `
    -VMSize Standard_DS2_v2
  Set-AzVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred
  Set-AzVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  Add-AzVMNetworkInterface `
    -VM $vm `
    -Id $nic.Id
  Set-AzVMBootDiagnostics `
    -VM $vm `
    -Disable
  New-AzVM -ResourceGroupName myResourceGroupAG -Location EastUS -VM $vm
  Set-AzVMExtension `
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

1. Gebruik [New-AzApplicationGatewayIPConfiguration](/powershell/module/Az.network/new-Azapplicationgatewayipconfiguration) om de configuratie die wordt gekoppeld aan het subnet te maken die u hebt gemaakt met de application gateway. 
2. Gebruik [New-AzApplicationGatewayFrontendIPConfig](/powershell/module/Az.network/new-Azapplicationgatewayfrontendipconfig) om de configuratie die het openbare IP-adres dat u eerder hebt gemaakt toegewezen aan de toepassingsgateway te maken. 
3. Gebruik [New-AzApplicationGatewayFrontendPort](/powershell/module/Az.network/new-Azapplicationgatewayfrontendport) om toe te wijzen van poort 80 voor toegang tot de toepassingsgateway.

```azurepowershell-interactive
$vnet   = Get-AzVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myAGSubnet
$pip    = Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress 
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

### <a name="create-the-backend-pool"></a>Back-endpool maken

1. Gebruik [New-AzApplicationGatewayBackendAddressPool](/powershell/module/Az.network/new-Azapplicationgatewaybackendaddresspool) te maken van de back-endpool voor de toepassingsgateway. 
2. Configureer de instellingen voor de back endadresgroep met [New-AzApplicationGatewayBackendHttpSettings](/powershell/module/Az.network/new-Azapplicationgatewaybackendhttpsettings).

```azurepowershell-interactive
$address1 = Get-AzNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic1
$address2 = Get-AzNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic2
$backendPool = New-AzApplicationGatewayBackendAddressPool `
  -Name myAGBackendPool `
  -BackendIPAddresses $address1.ipconfigurations[0].privateipaddress, $address2.ipconfigurations[0].privateipaddress
$poolSettings = New-AzApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-listener-and-add-a-rule"></a>Listener maken en regel toevoegen

Als u de toepassingsgateway wilt inschakelen om het verkeer op de juiste manier naar de back-endpool te routeren, is in Azure een listener vereist. In Azure is ook een regel vereist, zodat de listener weet welke back-endpool moet worden gebruikt voor binnenkomend verkeer. 

1. Maak een listener met behulp van [New-AzApplicationGatewayHttpListener](/powershell/module/Az.network/new-Azapplicationgatewayhttplistener) met de front-end-configuratie en de frontend-poort die u eerder hebt gemaakt. 
2. Gebruik [New-AzApplicationGatewayRequestRoutingRule](/powershell/module/Az.network/new-Azapplicationgatewayrequestroutingrule) maken van een regel met de naam *rule1*. 

```azurepowershell-interactive
$defaultlistener = New-AzApplicationGatewayHttpListener `
  -Name myAGListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$frontendRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultlistener `
  -BackendAddressPool $backendPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>De toepassingsgateway maken

Nu u de benodigde ondersteunende resources hebt gemaakt, kunt u de toepassingsgateway maken:

1. Gebruik [New-AzApplicationGatewaySku](/powershell/module/Az.network/new-Azapplicationgatewaysku) om op te geven van de parameters voor de toepassingsgateway.
2. Gebruik [New-AzApplicationGateway](/powershell/module/Az.network/new-Azapplicationgateway) om de toepassingsgateway te maken.

```azurepowershell-interactive
$sku = New-AzApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2
New-AzApplicationGateway `
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

1. Voer [Get-AzPublicIPAddress](/powershell/module/Az.network/get-Azpublicipaddress) om op te halen van het openbare IP-adres van de toepassingsgateway. 
2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser. Als u de browser vernieuwt, ziet u de naam van de virtuele machine. Een geldige reactie wordt gecontroleerd of de application gateway is gemaakt en uitgevoerd wordt om verbinding te maken met de back-end.

```azurepowershell-interactive
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Toepassingsgateway testen](./media/quick-create-powershell/application-gateway-iistest.png)


## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de bij de toepassingsgateway gemaakte resources niet meer nodig hebt, verwijdert u de resourcegroep. Als u de resourcegroep verwijdert, worden ook de toepassingsgateway en alle gerelateerde resources verwijderd. 

Als u wilt verwijderen van de resourcegroep, Roep de [Remove-AzResourceGroup](/powershell/module/Az.resources/remove-Azresourcegroup) cmdlet als volgt te werk:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Manage web traffic with an application gateway using Azure PowerShell](./tutorial-manage-web-traffic-powershell.md) (Webverkeer met een toepassingsgateway beheren met behulp van Azure PowerShell)

