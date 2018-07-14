---
title: Een toepassingsgateway maken met HTTP naar HTTPS-omleiding - Azure PowerShell | Microsoft Docs
description: Informatie over het maken van een toepassingsgateway met omgeleide verkeer van HTTP naar HTTPS met behulp van Azure PowerShell.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 7/13/2018
ms.author: victorh
ms.openlocfilehash: 372ada504ddfe90b48ace35acfeb934d67fa81a6
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035425"
---
# <a name="create-an-application-gateway-with-http-to-https-redirection-using-azure-powershell"></a>Een toepassingsgateway maken met HTTP naar HTTPS-omleiding met behulp van Azure PowerShell

U kunt de Azure PowerShell gebruiken om te maken een [toepassingsgateway](overview.md) met een certificaat voor SSL-beëindiging. Een regel voor doorsturen wordt gebruikt om HTTP-verkeer omleiden naar de HTTPS-poort in uw application gateway. In dit voorbeeld maakt u ook een [virtuele-machineschaalset](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) voor de back-endpool van de toepassingsgateway met twee exemplaren van de virtuele machine. 

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een zelfondertekend certificaat maken
> * Een netwerk instellen
> * Een toepassingsgateway maken met behulp van het certificaat
> * Een listener en omleiding van-regel toevoegen
> * Een virtuele-machineschaalset maken met de standaard back-endpool

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Voor deze zelfstudie is moduleversie 3,6 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). De opdrachten in deze zelfstudie, hebt u ook nodig om uit te voeren `Login-AzureRmAccount` voor het maken van een verbinding met Azure.

## <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken

Voor gebruik in productieomgevingen, moet u een geldig certificaat dat is ondertekend door een vertrouwde provider importeren. Voor deze zelfstudie maakt u een zelfondertekend certificaat met behulp van [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate). U kunt [Export-PfxCertificate ](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate) gebruiken met de Thumbprint die is geretourneerd om een ​​PFX-bestand uit het certificaat te exporteren.

```powershell
New-SelfSignedCertificate `
  -certstorelocation cert:\localmachine\my `
  -dnsname www.contoso.com
```

U zou iets moeten zien dat lijkt op dit resultaat:

```
PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\my

Thumbprint                                Subject
----------                                -------
E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630  CN=www.contoso.com
```

Gebruik de vingerafdruk om het PFX-bestand te maken:

```powershell
$pwd = ConvertTo-SecureString -String "Azure123456!" -Force -AsPlainText
Export-PfxCertificate `
  -cert cert:\localMachine\my\E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630 `
  -FilePath c:\appgwcert.pfx `
  -Password $pwd
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Maken van een Azure-resourcegroep met de naam *myResourceGroupAG* met behulp van [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). 

```powershell
New-AzureRmResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Netwerkbronnen maken

Maak de subnetconfiguraties voor *myBackendSubnet* en *myAGSubnet* met behulp van [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Maak het virtuele netwerk met de naam *myVNet* met behulp van [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) met de subnetconfiguraties. En maak ten slotte het openbare IP-adres met de naam *myAGPublicIPAddress* met behulp van [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). Deze resources worden gebruikt om de netwerkverbinding naar de toepassingsgateway en de bijbehorende bronnen te leveren.

```powershell
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

### <a name="create-the-ip-configurations-and-frontend-port"></a>IP-configuraties en front-endpoort maken

Koppel *myAGSubnet* dat u eerder hebt gemaakt aan de toepassingsgateway met behulp van [New-AzureRmApplicationGatewayIPConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration). Wijs *myAGPublicIPAddress* toe aan de toepassingsgateway met behulp van [New-AzureRmApplicationGatewayFrontendIPConfig](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig). Vervolgens kunt u de HTTPS-poort gebruikt [New-AzureRmApplicationGatewayFrontendPort](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendport).

```powershell
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
$frontendPort = New-AzureRmApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 443
```

### <a name="create-the-backend-pool-and-settings"></a>Back-endpool en instellingen maken

Maak de back-endpool met de naam *appGatewayBackendPool* voor de toepassingsgateway met [New-AzureRmApplicationGatewayBackendAddressPool](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool). Configureer de instellingen voor de back-endpool met [New-AzureRmApplicationGatewayBackendHttpSettings](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings).

```powershell
$defaultPool = New-AzureRmApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool 
$poolSettings = New-AzureRmApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-default-listener-and-rule"></a>De standaard-listener en regel maken

Een listener is nodig om de toepassingsgateway in stat te stellen het verkeer op de juiste manier naar de back-endpool door te sturen. In dit voorbeeld maakt u een basis-listener die luistert naar HTTPS-verkeer op de basis-URL. 

Maak een certificaat-object met [New-AzureRmApplicationGatewaySslCertificate](/powershell/module/azurerm.network/new-azurermapplicationgatewaysslcertificate) en maak vervolgens een listener met de naam *appGatewayHttpListener* met behulp van [ Nieuwe AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) met de front-end-configuratie, de frontend-poort en het certificaat dat u eerder hebt gemaakt. Er is een regel vereist, zodat de listener weet welke back-endpool moet worden gebruikt voor binnenkomend verkeer. Maak een basisregel met de naam *rule1* met behulp van [New-AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule).

```powershell
$pwd = ConvertTo-SecureString `
  -String "Azure123456!" `
  -Force `
  -AsPlainText
$cert = New-AzureRmApplicationGatewaySslCertificate `
  -Name "appgwcert" `
  -CertificateFile "c:\appgwcert.pfx" `
  -Password $pwd
$defaultListener = New-AzureRmApplicationGatewayHttpListener `
  -Name appGatewayHttpListener `
  -Protocol Https `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendPort `
  -SslCertificate $cert
$frontendRule = New-AzureRmApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultListener `
  -BackendAddressPool $defaultPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>De toepassingsgateway maken

Nu u de benodigde ondersteunende resources hebt gemaakt, geeft u parameters op voor de toepassingsgateway met de naam *myAppGateway* met behulp van [New-AzureRmApplicationGatewaySku](/powershell/module/azurerm.network/new-azurermapplicationgatewaysku) en maakt u deze vervolgens door [New-AzureRmApplicationGateway](/powershell/module/azurerm.network/new-azurermapplicationgateway) te gebruiken bij het certificaat.

```powershell
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
  -FrontendPorts $frontendPort `
  -HttpListeners $defaultListener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku `
  -SslCertificates $cert
```

## <a name="add-a-listener-and-redirection-rule"></a>Een listener en omleiding van-regel toevoegen

### <a name="add-the-http-port"></a>De HTTP-poort toevoegen

De HTTP-poort toevoegen aan de application gateway met [toevoegen AzureRmApplicationGatewayFrontendPort](/powershell/module/azurerm.network/add-azurermapplicationgatewayfrontendport).

```powershell
$appgw = Get-AzureRmApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG
Add-AzureRmApplicationGatewayFrontendPort `
  -Name httpPort  `
  -Port 80 `
  -ApplicationGateway $appgw
```

### <a name="add-the-http-listener"></a>De HTTP-listener toevoegen

Toevoegen van de HTTP-listener met de naam *myListener* aan de application gateway met [toevoegen AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/add-azurermapplicationgatewayhttplistener).

```powershell
$fipconfig = Get-AzureRmApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -ApplicationGateway $appgw
$fp = Get-AzureRmApplicationGatewayFrontendPort `
  -Name httpPort `
  -ApplicationGateway $appgw
Add-AzureRmApplicationGatewayHttpListener `
  -Name myListener `
  -Protocol Http `
  -FrontendPort $fp `
  -FrontendIPConfiguration $fipconfig `
  -ApplicationGateway $appgw
```

### <a name="add-the-redirection-configuration"></a>De configuratie van omleiding toevoegen

De HTTP toevoegen aan HTTPS-omleiding configuratie aan de application gateway met [Add-azurermapplicationgatewayredirectconfiguration is](/powershell/module/azurerm.network/add-azurermapplicationgatewayredirectconfiguration).

```powershell
$defaultListener = Get-AzureRmApplicationGatewayHttpListener `
  -Name appGatewayHttpListener `
  -ApplicationGateway $appgw
Add-AzureRmApplicationGatewayRedirectConfiguration -Name httpToHttps `
  -RedirectType Permanent `
  -TargetListener $defaultListener `
  -IncludePath $true `
  -IncludeQueryString $true `
  -ApplicationGateway $appgw
```

### <a name="add-the-routing-rule"></a>Toevoegen van de regel voor doorsturen

De routeringsregel met de configuratie van omleiding toevoegen aan de application gateway met [toevoegen AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/add-azurermapplicationgatewayrequestroutingrule).

```powershell
$myListener = Get-AzureRmApplicationGatewayHttpListener `
  -Name myListener `
  -ApplicationGateway $appgw
$redirectConfig = Get-AzureRmApplicationGatewayRedirectConfiguration `
  -Name httpToHttps `
  -ApplicationGateway $appgw
Add-AzureRmApplicationGatewayRequestRoutingRule `
  -Name rule2 `
  -RuleType Basic `
  -HttpListener $myListener `
  -RedirectConfiguration $redirectConfig `
  -ApplicationGateway $appgw
Set-AzureRmApplicationGateway -ApplicationGateway $appgw
```

## <a name="create-a-virtual-machine-scale-set"></a>Een virtuele-machineschaalset maken

In dit voorbeeld maakt u een virtuele-machineschaalset om servers op te geven voor de back-endpool in de toepassingsgateway. U wijst de schaalset toe aan de back-endpool wanneer u de IP-instellingen configureert.

```powershell
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
  -ImageReferenceVersion latest
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

```powershell
$publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/davidmu1/samplescripts/master/appgatewayurl.ps1"); 
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

U kunt [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) gebruiken om het openbare IP-adres van de toepassingsgateway op te halen. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser. Bijvoorbeeld: http://52.170.203.149

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Beveiligingswaarschuwing](./media/redirect-http-to-https-powershell/application-gateway-secure.png)

Voor het accepteren van de beveiligingswaarschuwing als u een zelfondertekend certificaat hebt gebruikt, selecteert u **Details** en vervolgens **Ga verder naar de webpagina**. Uw beveiligde IIS-website wordt vervolgens weergegeven zoals in het volgende voorbeeld:

![Basis-URL testen in de toepassingsgateway](./media/redirect-http-to-https-powershell/application-gateway-iistest.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een zelfondertekend certificaat maken
> * Een netwerk instellen
> * Een toepassingsgateway maken met behulp van het certificaat
> * Een listener en omleiding van-regel toevoegen
> * Een virtuele-machineschaalset maken met de standaard back-endpool
