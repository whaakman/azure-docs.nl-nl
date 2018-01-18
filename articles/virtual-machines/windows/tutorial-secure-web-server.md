---
title: IIS beveiligen met SSL-certificaten in Azure | Microsoft Docs
description: Meer informatie over het beveiligen van de IIS-webserver met SSL-certificaten op een Windows-virtuele machine in Azure
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/14/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 43f06422e1120f1c3b2a9d9d5d4be515213c0937
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="secure-iis-web-server-with-ssl-certificates-on-a-windows-virtual-machine-in-azure"></a>IIS-webserver met SSL-certificaten op een virtuele Windows-machine in Azure beveiligen
Beveiligde webservers, kan een Secure Sockets Layer (SSL)-certificaat worden gebruikt voor het versleutelen van internetverkeer. Deze SSL-certificaten kunnen worden opgeslagen in Azure Sleutelkluis en beveiligde implementaties van certificaten aan Windows virtuele machines (VM's) in Azure toestaan. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maken van een Azure Sleutelkluis
> * Genereren of een certificaat uploaden naar de Sleutelkluis
> * Een virtuele machine maken en de IIS-webserver installeren
> * Het certificaat invoeren in de virtuele machine en IIS te configureren met een SSL-binding

Voor deze zelfstudie is moduleversie 3,6 of hoger van Azure PowerShell vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).


## <a name="overview"></a>Overzicht
Azure Sleutelkluis beschermt cryptografische sleutels en geheimen, zoals certificaten of wachtwoorden. Sleutelkluis helpt het beheerproces van het certificaat te stroomlijnen en kunt u controle houdt over sleutels die toegang hebben tot deze certificaten. U kunt een zelfondertekend certificaat in de Sleutelkluis maken of een bestaande, vertrouwd certificaat dat u al bezit uploaden.

In plaats van met een aangepaste VM-installatiekopie die certificaten bevat standaard uitbreidbaar module u certificaten kunt invoeren in een actieve virtuele machine. Dit proces zorgt ervoor dat de meest recente certificaten tijdens de implementatie op een webserver zijn geïnstalleerd. Als u vernieuwen of vervangen van een certificaat, hebt u niet ook een nieuwe aangepaste VM-installatiekopie maken. De meest recente certificaten worden automatisch toegevoegd als u extra virtuele machines maken. Tijdens het hele proces, de certificaten nooit laat u de Azure-platform of beschikbaar worden gesteld in een script, opdrachtregel geschiedenis of sjabloon.


## <a name="create-an-azure-key-vault"></a>Maken van een Azure Sleutelkluis
Voordat u certificaten en een Sleutelkluis maken kunt, maakt u een resourcegroep met [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupSecureWeb* in de *VS-Oost* locatie:

```powershell
$resourceGroup = "myResourceGroupSecureWeb"
$location = "East US"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

Maak vervolgens een Sleutelkluis met [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault). Elke Sleutelkluis moet een unieke naam hebben en moet alle kleine letters. Vervang `<mykeyvault>` in het volgende voorbeeld met de naam van uw eigen unieke Sleutelkluis:

```powershell
$keyvaultName="<mykeyvault>"
New-AzureRmKeyVault -VaultName $keyvaultName `
    -ResourceGroup $resourceGroup `
    -Location $location `
    -EnabledForDeployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Een certificaat genereren en opslaan in de Sleutelkluis
Voor gebruik in productieomgevingen, moet u een geldig certificaat dat is ondertekend door een vertrouwde provider met importeren [importeren AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/import-azurekeyvaultcertificate). Voor deze zelfstudie, het volgende voorbeeld toont hoe kunt u een zelfondertekend certificaat met genereren [toevoegen AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificate) die gebruikmaakt van het standaardbeleid voor het certificaat van [ Nieuwe AzureKeyVaultCertificatePolicy](/powershell/module/azurerm.keyvault/new-azurekeyvaultcertificatepolicy). 

```powershell
$policy = New-AzureKeyVaultCertificatePolicy `
    -SubjectName "CN=www.contoso.com" `
    -SecretContentType "application/x-pkcs12" `
    -IssuerName Self `
    -ValidityInMonths 12

Add-AzureKeyVaultCertificate `
    -VaultName $keyvaultName `
    -Name "mycert" `
    -CertificatePolicy $policy 
```


## <a name="create-a-virtual-machine"></a>Een virtuele machine maken
Stel dat een beheerder gebruikersnaam en wachtwoord voor de virtuele machine met [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Nu kunt u de virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Het volgende voorbeeld maakt de vereiste virtuele netwerkonderdelen, de configuratie van het besturingssysteem, en maakt vervolgens een virtuele machine met de naam *myVM*:

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name "myVnet" `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRuleRDP"  `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

# Create an inbound network security group rule for port 443
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRuleWWW"  `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name "myNic" `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2" | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName "myVM" -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2016-Datacenter" -Version "latest" | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create virtual machine
New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig

# Use the Custom Script Extension to install IIS
Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server -IncludeManagementTools"}'
```

Het duurt enkele minuten duren voordat de virtuele machine moet worden gemaakt. De laatste stap de Azure-extensie voor aangepaste scripts gebruikt voor het installeren van de IIS-webserver met [Set AzureRmVmExtension](/powershell/module/azurerm.compute/set-azurermvmextension).


## <a name="add-a-certificate-to-vm-from-key-vault"></a>Een certificaat toevoegen aan de virtuele machine uit Sleutelkluis
Om het certificaat van de Sleutelkluis toevoegen aan een virtuele machine, krijgen de ID van het certificaat met [Get-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/get-azurekeyvaultsecret). Het certificaat toevoegen aan de virtuele machine met [toevoegen AzureRmVMSecret](/powershell/module/azurerm.compute/add-azurermvmsecret):

```powershell
$certURL=(Get-AzureKeyVaultSecret -VaultName $keyvaultName -Name "mycert").id

$vm=Get-AzureRMVM -ResourceGroupName $resourceGroup -Name "myVM"
$vaultId=(Get-AzureRmKeyVault -ResourceGroupName $resourceGroup -VaultName $keyVaultName).ResourceId
$vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $vaultId -CertificateStore "My" -CertificateUrl $certURL

Update-AzureRmVM -ResourceGroupName $resourceGroup -VM $vm
```


## <a name="configure-iis-to-use-the-certificate"></a>IIS configureren voor het gebruik van het certificaat
Gebruik de aangepaste Scriptextensie opnieuw met [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) de IIS-configuratie bij te werken. Deze update van toepassing is het certificaat van de Sleutelkluis ingevoegd op IIS en configureert u de web-binding:

```powershell
$PublicSettings = '{
    "fileUris":["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/secure-iis.ps1"],
    "commandToExecute":"powershell -ExecutionPolicy Unrestricted -File secure-iis.ps1"
}'

Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString $publicSettings
```


### <a name="test-the-secure-web-app"></a>Testen van de beveiligde web-app
Het openbare IP-adres van uw virtuele machine met [Get-AzureRmPublicIPAddress](/powershell/resourcemanager/azurerm.network/get-azurermpublicipaddress). Het volgende voorbeeld verkrijgt het IP-adres voor `myPublicIP` eerder hebt gemaakt:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName $resourceGroup -Name "myPublicIP" | select "IpAddress"
```

Nu kunt u een webbrowser openen en voer `https://<myPublicIP>` in de adresbalk. Voor het accepteren van de beveiligingswaarschuwing als u een zelfondertekend certificaat gebruikt, selecteert u **Details** en vervolgens **gaat u naar de webpagina**:

![Beveiligingswaarschuwing voor web browser accepteren](./media/tutorial-secure-web-server/browser-warning.png)

Uw beveiligde IIS-website wordt weergegeven zoals in het volgende voorbeeld:

![Weergave actief beveiligde IIS-website](./media/tutorial-secure-web-server/secured-iis.png)


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt beveiligd u een IIS-webserver met een SSL-certificaat dat is opgeslagen in Azure Sleutelkluis. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Maken van een Azure Sleutelkluis
> * Genereren of een certificaat uploaden naar de Sleutelkluis
> * Een virtuele machine maken en de IIS-webserver installeren
> * Het certificaat invoeren in de virtuele machine en IIS te configureren met een SSL-binding

Volg deze link om te zien van de vooraf gemaakte virtuele machine scriptvoorbeelden.

> [!div class="nextstepaction"]
> [Windows virtuele machine scriptvoorbeelden](./powershell-samples.md)
