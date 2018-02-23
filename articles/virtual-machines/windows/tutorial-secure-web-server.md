---
title: IIS met SSL-certificaten beveiligen in Azure | Microsoft Docs
description: Meer informatie over het beveiligen van de IIS-webserver met SSL-certificaten op een Windows-VM in Azure
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/09/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: ada0703603df5ae5a324d38cda2b23a060a10992
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2018
---
# <a name="secure-iis-web-server-with-ssl-certificates-on-a-windows-virtual-machine-in-azure"></a>Een IIS-webserver met SSL-certificaten beveiligen op een virtuele Windows-machine in Azure
Om webservers te beveiligen, kan een Secure Sockets Layer (SSL)-certificaat worden gebruikt voor het versleutelen van internetverkeer. Deze SSL-certificaten kunnen worden opgeslagen in Azure Key Vault en beveiligde implementaties van certificaten aan virtuele Windows-machines (VM's) in Azure toestaan. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Azure Key Vault maken
> * Een certificaat genereren of uploaden naar de Key Vault
> * Een virtuele machine maken en de IIS-webserver installeren
> * Het certificaat invoeren in de virtuele machine en IIS configureren met een SSL-binding

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie moduleversie 5.3 of later van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure. 


## <a name="overview"></a>Overzicht
Azure Key Vault beschermt cryptografische sleutels en geheimen, zoals certificaten of wachtwoorden. Key Vault helpt het beheerproces voor certificaten te stroomlijnen en zorgt dat u de controle houdt over de sleutels waarmee deze certificaten toegankelijk zijn. U kunt een zelfondertekend certificaat in Key Vault maken of een bestaand, vertrouwd certificaat dat u al bezit uploaden.

In plaats van met een aangepaste VM-installatiekopie die standaard certificaten bevat, kunt u certificaten invoeren in een actieve virtuele machine. Dit proces zorgt ervoor dat de meest recente certificaten tijdens de implementatie op een webserver zijn geïnstalleerd. Als u een certificaat wilt vernieuwen of vervangen, hoeft u niet ook een nieuwe aangepaste VM-installatiekopie te maken. De meest recente certificaten worden automatisch toegevoegd als u extra virtuele machines maakt. Tijdens het hele proces verlaten de certificaten nooit het Azure-platform of zijn ze blootgesteld in een script, opdrachtregelgeschiedenis of sjabloon.


## <a name="create-an-azure-key-vault"></a>Een Azure Key Vault maken
Voordat u een Key Vault en certificaten kunt maken, moet u eerst een resourcegroep maken met [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupSecureWeb* gemaakt op de locatie *VS Oost*:

```azurepowershell-interactive
$resourceGroup = "myResourceGroupSecureWeb"
$location = "East US"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

Maak vervolgens een Key Vault met [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault). Elke Key Vault moet een unieke naam hebben van alleen kleine letters. Vervang `mykeyvault` in het volgende voorbeeld door de naam van uw eigen unieke Key Vault:

```azurepowershell-interactive
$keyvaultName="mykeyvault"
New-AzureRmKeyVault -VaultName $keyvaultName `
    -ResourceGroup $resourceGroup `
    -Location $location `
    -EnabledForDeployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Een certificaat genereren en opslaan in Key Vault
Voor gebruik in de productie, moet u een geldig certificaat importeren dat is ondertekend door een vertrouwde provider met [Import-AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/import-azurekeyvaultcertificate). Voor deze zelfstudie toont het volgende voorbeeld hoe u een zelfondertekend certificaat kunt genereren met [Add-AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificate) dat gebruikmaakt van het standaardbeleid voor certificaten van [New-AzureKeyVaultCertificatePolicy](/powershell/module/azurerm.keyvault/new-azurekeyvaultcertificatepolicy). 

```azurepowershell-interactive
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
Stel een beheerdersnaam en -wachtwoord in voor de virtuele machine met [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

U kunt de virtuele machine nu maken met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). In het volgende voorbeeld wordt een VM met de naam *myVM* gemaakt op de locatie *VS Oost*. Als ze nog niet bestaan, worden de ondersteunende netwerkresources gemaakt. Om beveiligd webverkeer mogelijk te maken, opent de cmdlet ook poort *443*.

```azurepowershell-interactive
# Create a VM
New-AzureRmVm `
    -ResourceGroupName $resourceGroup `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred `
    -OpenPorts 443

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

Het duurt enkele minuten voordat de virtuele machine wordt gemaakt. In de laatste stap wordt gebruikgemaakt van de Azure-extensie voor aangepaste scripts om de IIS-webserver te installeren met [Set AzureRmVmExtension](/powershell/module/azurerm.compute/set-azurermvmextension).


## <a name="add-a-certificate-to-vm-from-key-vault"></a>Een certificaat toevoegen aan de virtuele machine vanuit Key Vault
Om het certificaat vanuit Key Vault toe te voegen aan een virtuele machine, haalt u de id van het certificaat op met [Get-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/get-azurekeyvaultsecret). Voeg het certificaat toe aan de virtuele machine met [Add-AzureRmVMSecret](/powershell/module/azurerm.compute/add-azurermvmsecret):

```azurepowershell-interactive
$certURL=(Get-AzureKeyVaultSecret -VaultName $keyvaultName -Name "mycert").id

$vm=Get-AzureRMVM -ResourceGroupName $resourceGroup -Name "myVM"
$vaultId=(Get-AzureRmKeyVault -ResourceGroupName $resourceGroup -VaultName $keyVaultName).ResourceId
$vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $vaultId -CertificateStore "My" -CertificateUrl $certURL

Update-AzureRmVM -ResourceGroupName $resourceGroup -VM $vm
```


## <a name="configure-iis-to-use-the-certificate"></a>IIS configureren voor gebruik van het certificaat
Gebruik de extensie voor aangepaste scripts opnieuw met [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) om de IIS-configuratie bij te werken. Deze update is van toepassing op het certificaat dat is ingevoerd vanuit Key Vault en configureert de webbinding:

```azurepowershell-interactive
$PublicSettings = '{
    "fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/secure-iis.ps1"],
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
Haal het openbare IP-adres van uw virtuele machine op met [Get-AzureRmPublicIPAddress](/powershell/resourcemanager/azurerm.network/get-azurermpublicipaddress). In het volgende voorbeeld wordt het IP-adres opgehaald voor de `myPublicIP` die eerder is gemaakt:

```azurepowershell-interactive
Get-AzureRmPublicIPAddress -ResourceGroupName $resourceGroup -Name "myPublicIPAddress" | select "IpAddress"
```

Nu kunt u een webbrowser openen en `https://<myPublicIP>` in de adresbalk invoeren. Voor het accepteren van de beveiligingswaarschuwing als u een zelfondertekend certificaat hebt gebruikt, selecteert u **Details** en vervolgens **Ga verder naar de webpagina**:

![Beveiligingswaarschuwing voor web browser accepteren](./media/tutorial-secure-web-server/browser-warning.png)

Uw beveiligde IIS-website wordt vervolgens weergegeven zoals in het volgende voorbeeld:

![Beveiligde actieve IIS-site weergeven](./media/tutorial-secure-web-server/secured-iis.png)


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u een IIS-webserver beveiligd met een SSL-certificaat dat is opgeslagen in Azure Key Vault. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een Azure Key Vault maken
> * Een certificaat genereren of uploaden naar de Key Vault
> * Een virtuele machine maken en de IIS-webserver installeren
> * Het certificaat invoeren in de virtuele machine en IIS configureren met een SSL-binding

Volg deze link om voorbeelden te zien van vooraf gemaakte virtuele machinescripts.

> [!div class="nextstepaction"]
> [Scriptvoorbeelden van virtuele Windows-machines](./powershell-samples.md)
