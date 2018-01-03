---
title: Een virtuele machine en opslag voor een schaalbare toepassing maakt in Azure | Microsoft Docs
description: Informatie over het implementeren van een virtuele machine moet worden gebruikt voor het uitvoeren van een schaalbare toepassing met behulp van Azure blob-opslag
services: storage
documentationcenter: 
author: georgewallace
manager: jeconnoc
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 0fd1cd93ca6faabcbe0007136fe427028e722733
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/23/2017
---
# <a name="create-a-virtual-machine-and-storage-account-for-a-scalable-application"></a>Een virtuele machine en storage-account voor een schaalbare toepassing maken

Deze zelfstudie maakt deel uit een reeks. Deze zelfstudie ziet dat u een toepassing implementeren die uploaden en downloaden van grote hoeveelheden willekeurige gegevens met Azure storage-account. Wanneer u klaar bent, hebt u een consoletoepassing die wordt uitgevoerd op een virtuele machine u uploaden en downloaden van grote hoeveelheden gegevens naar een opslagaccount.

Deel een van de reeks, leert u hoe:

> [!div class="checklist"]
> * Een opslagaccount maken
> * Een virtuele machine maken
> * Een extensie voor aangepaste scripts configureren

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, wordt voor deze zelfstudie moduleversie 3.6 of hoger van Azure PowerShell vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-storage-account"></a>Een opslagaccount maken
 
Het voorbeeld uploadt 50 grote bestanden naar een blobcontainer in Azure Storage-account. Een opslagaccount biedt een unieke naamruimte voor het opslaan en toegang tot uw Azure storage-gegevensobjecten. Een opslagaccount maken in de resourcegroep die u hebt gemaakt met behulp van de [nieuw AzureRmStorageAccount](/powershell/module/AzureRM.Storage/New-AzureRmStorageAccount) opdracht.

In de volgende opdracht te vervangen door uw eigen globaal unieke naam op voor de Blob storage-account waarin u zien hoe de `<blob_storage_account>` tijdelijke aanduiding.

```powershell-interactive
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName myResourceGroup `
  -Name "<blob_storage_account>" `
  -Location EastUS `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Maak een virtuele-machineconfiguratie. Deze configuratie bevat de instellingen die worden gebruikt bij het implementeren van de virtuele machine, zoals een installatiekopie van de virtuele machine, de grootte en de verificatieconfiguratie. Als deze stap wordt uitgevoerd, wordt u gevraagd referenties op te geven. De waarden die u invoert, worden geconfigureerd als de gebruikersnaam en het wachtwoord voor de virtuele machine.

Maak de virtuele machine met [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```azurepowershell-interactive
# Variables for common values
$resourceGroup = "myResourceGroup"
$location = "eastus"
$vmName = "myVM"

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Location $location `
  -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup -Location $location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

# Create a virtual network card and associate with public IP address
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -Location $location `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS14_v2 | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create a virtual machine
New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig

Write-host "Your public IP address is $($pip.IpAddress)"
```

## <a name="deploy-configuration"></a>Configuratie implementeren

Er zijn vereisten die moeten worden geïnstalleerd op de virtuele machine voor deze zelfstudie. De extensie voor aangepaste scripts wordt gebruikt voor het uitvoeren van een PowerShell-script dat de volgende taken:

> [!div class="checklist"]
> * .NET core 2.0 installeren
> * Chocolatey installeren
> * Installeer GIT
> * Kloon de opslagplaats voorbeeld
> * NuGet-pakketten herstellen
> * Hiermee maakt u 50 1 GB-bestanden met willekeurige gegevens

Voer de volgende cmdlet als configuratie van de virtuele machine wilt voltooien. Deze stap neemt 5 tot 15 minuten in beslag.

```azurepowershell-interactive
# Start a CustomScript extension to use a simple PowerShell script to install .NET core, dependencies, and pre-create the files to upload.
Set-AzureRMVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location EastUS `
    -FileUri https://raw.githubusercontent.com/azure-samples/storage-dotnet-perf-scale-app/master/setup_env.ps1 `
    -Run 'setup_env.ps1' `
    -Name DemoScriptExtension
```

## <a name="next-steps"></a>Volgende stappen

In deel 1 van de reeks, hebt u geleerd over het maken van een opslagaccount, het implementeren van een virtuele machine en het configureren van de virtuele machine met de vereiste vereisten zoals het:

> [!div class="checklist"]
> * Een opslagaccount maken
> * Een virtuele machine maken
> * Een extensie voor aangepaste scripts configureren

Ga naar deel twee van de reeks grote hoeveelheden gegevens uploaden naar een opslagaccount met behulp van de exponentiële opnieuw proberen en parallelle uitvoering.

> [!div class="nextstepaction"]
> [Uploaden van grote hoeveelheden grote bestanden naar een opslagaccount parallel](storage-blob-scalable-app-upload-files.md)
