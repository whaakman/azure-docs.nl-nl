---
title: Een virtuele-machineschaalset maken met PowerShell | Microsoft Docs
description: Een virtuele-machineschaalset maken met PowerShell
services: virtual-machine-scale-sets
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7bb03323-8bcc-4ee4-9a3e-144ca6d644e2
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/18/2016
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 6d70338ebf918a3f9178a4f633dd46a607d72b1c


---
# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Een virtuele-machineschaalset voor Windows maken met Azure PowerShell
Met deze stappen kunt u virtuele-machineschaalsets in Azure maken door de blanco opties in te vullen. Raadpleeg [Overzicht van virtuele-machineschaalsets](virtual-machine-scale-sets-overview.md) voor meer informatie over schaalsets.

Het duurt circa 30 minuten om de stappen in de artikel te voltooien.

## <a name="step-1-install-azure-powershell"></a>Stap 1: Azure PowerShell installeren
Zie [Azure PowerShell installeren en configureren](../powershell-install-configure.md) voor informatie over het installeren van de nieuwste versie van Azure PowerShell, het selecteren van het abonnement en het aanmelden bij uw account.

## <a name="step-2-create-resources"></a>Stap 2: Resources maken
Maak de resources die nodig zijn voor uw nieuwe schaalset.

### <a name="resource-group"></a>Resourcegroep
Een virtuele-machineschaalset moet zijn opgenomen in een resourcegroep.

1. Haal een lijst op met beschikbare locaties waar resources kunnen worden geplaatst:
   
        Get-AzureLocation | Sort Name | Select Name
2. Kies een handige locatie, vervang de waarde van **$locName** door de naam van die locatie en maak de volgende variabele:
   
        $locName = "location name from the list, such as Central US"
3. Vervang de waarde van **$rgName** door de naam die u wilt gebruiken voor de nieuwe resourcegroep en maak de variabele: 
   
        $rgName = "resource group name"
4. De resourcegroep maken:
   
        New-AzureRmResourceGroup -Name $rgName -Location $locName
   
    U zou iets moeten zien zoals in dit voorbeeld wordt weergegeven:
   
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="storage-account"></a>Storage-account
Een virtuele machine gebruikt een opslagaccount voor het opslaan van de besturingssysteemschijf en diagnostische gegevens die voor schalen worden gebruikt. Indien mogelijk kunt u het beste een afzonderlijk opslagaccount gebruiken voor elke virtuele machine in een schaalset. Als dit niet mogelijk is, plan dan niet meer dan 20 VM's per opslagaccount. In het voorbeeld in dit artikel worden drie opslagaccounts gemaakt voor drie virtuele machines.

1. Vervang de waarde van **$saName** door de gewenste naam voor het opslagaccount. Test of de naam uniek is. 
   
        $saName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $saName
   
    Als het antwoord **True** is, is de gewenste naam uniek.
2. Vervang de waarde van **$saType** door het gewenste type voor het opslagaccount en maak de variabele:  
   
        $saType = "storage account type"
   
    Mogelijke waarden zijn: Standard_LRS, Standard_GRS, Standard_RAGRS en Premium_LRS.
3. Het account maken:
   
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName
   
    U zou iets moeten zien zoals in dit voorbeeld wordt weergegeven:
   
        ResourceGroupName   : myrg1
        StorageAccountName  : myst1
        Id                  : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft
                              .Storage/storageAccounts/myst1
        Location            : centralus
        AccountType         : StandardLRS
        CreationTime        : 3/15/2016 4:51:52 PM
        CustomDomain        :
        LastGeoFailoverTime :
        PrimaryEndpoints    : Microsoft.Azure.Management.Storage.Models.Endpoints
        PrimaryLocation     : centralus
        ProvisioningState   : Succeeded
        SecondaryEndpoints  :
        SecondaryLocation   :
        StatusOfPrimary     : Available
        StatusOfSecondary   :
        Tags                : {}
        Context             : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext
4. Herhaal stappen 1 tot 4 om drie opslagaccounts te maken, bijvoorbeeld myst1, myst2 en myst3.

### <a name="virtual-network"></a>Virtueel netwerk
Voor de virtuele machines in de schaalset is een virtueel netwerk vereist.

1. Vervang de waarde van **$subnetName** door de naam die u wilt gebruiken voor het subnet in het virtuele netwerk en maak de variabele: 
   
        $subnetName = "subnet name"
2. De subnetconfiguratie maken:
   
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
   
    Het adresvoorvoegsel kan anders zijn in uw virtuele netwerk.
3. Vervang de waarde van **$netName** door de naam die u wilt gebruiken voor het virtuele netwerk en maak de variabele: 
   
        $netName = "virtual network name"
4. Het virtuele netwerk maken:
   
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="configuration-of-the-scale-set"></a>Configuratie van de schaalset
U hebt alle benodigde resources voor het configureren van de schaalset. We kunnen deze nu gaan maken.  

1. Vervang de waarde van **$ipName** door de naam die u wilt gebruiken voor de IP-configuratie en maak de variabele: 
   
        $ipName = "IP configuration name"
2. De IP-configuratie maken:
   
        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id
3. Vervang de waarde van **$vmssConfig** door de naam die u wilt gebruiken voor de schaalsetconfiguratie en maak de variabele:   
   
        $vmssConfig = "Scale set configuration name"
4. De configuratie voor de schaalset maken:
   
        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
   
    Dit voorbeeld toont een schaalset die wordt gemaakt met drie virtuele machines. Raadpleeg [Overzicht van virtuele-machineschaalsets](virtual-machine-scale-sets-overview.md) voor meer informatie over de capaciteit van schaalsets. Deze stap omvat ook het instellen van de grootte (waarnaar wordt verwezen met SkuName) van de virtuele machines in de set. Bekijk [Grootten voor virtuele machines](../virtual-machines/virtual-machines-windows-sizes.md) om een grootte te vinden die aansluit bij uw behoeften.
5. De configuratie van de netwerkinterface toevoegen aan de configuratie van de schaalset:
   
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
   
    U zou iets moeten zien zoals in dit voorbeeld wordt weergegeven:
   
        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>Profiel van besturingssysteem
1. Vervang de waarde van **$computerName** door het voorvoegsel van de computernaam dat u wilt gebruiken en maak de variabele: 
   
        $computerName = "computer name prefix"
2. Vervang de waarde van **$adminName** door de naam van het administrator-account op de virtuele machines en maak de variabele:
   
        $adminName = "administrator account name"
3. Vervang de waarde van **$adminPassword** door het accountwachtwoord en maak de variabele:
   
        $adminPassword = "password for administrator accounts"
4. Het profiel van het besturingssysteem maken:
   
        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Opslagprofiel
1. Vervang de waarde van **$storageProfile** door de naam die u wilt gebruiken voor het opslagprofiel en maak de variabele:  
   
        $storageProfile = "storage profile name"
2. De variabelen maken die de gewenste installatiekopie definiëren:  
   
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
   
    Raadpleeg [Door installatiekopieën van virtuele Azure-machines navigeren en deze selecteren met Windows PowerShell en Azure CLI](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md) voor meer informatie over het gebruiken van andere installatiekopieën.
3. Vervang de waarden van **$vhdContainers** door een lijst met paden naar de locaties waar de virtuele vaste schijven zijn opgeslagen, zoals https://mystorage.blob.core.windows.net/vhds, en maak de variabele:
   
        $vhdContainers = @("https://myst1.blob.core.windows.net/vhds","https://myst2.blob.core.windows.net/vhds","https://myst3.blob.core.windows.net/vhds")
4. Het opslagprofiel maken:
   
        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storageProfile -VhdContainer $vhdContainers -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>Schaalset voor virtuele machines
Nu kunt u de schaalset maken.

1. Vervang de waarde van **$vmssName** door de naam van de virtuele-machineschaalset en maak de variabele:
   
        $vmssName = "scale set name"
2. De schaalset maken:
   
        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss
   
    U zou iets moeten zien dat lijkt op dit voorbeeld van een succesvolle implementatie:
   
        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3-explore-resources"></a>Stap 3: Resources verkennen
Gebruik deze resources om de door u gemaakte virtuele-machineschaalset te verkennen:

* Azure Portal: er is maar beperkte informatie beschikbaar via de portal.
* [Azure Resource Explorer](https://resources.azure.com/): u kunt het beste dit programma gebruiken om de huidige status van uw schaalset te bekijken.
* Azure PowerShell: gebruik de volgende opdracht om informatie op te halen:
  
        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
  
        Or 
  
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

## <a name="next-steps"></a>Volgende stappen
* Gebruik de informatie in [Virtuele machines beheren in een virtuele-machineschaalset](virtual-machine-scale-sets-windows-manage.md) om de schaalset die u net hebt gemaakt, te beheren
* Ga na of u automatisch schalen wilt inschakelen voor uw schaalset aan de hand van [Automatisch schalen en virtuele-machineschaalsets](virtual-machine-scale-sets-autoscale-overview.md)
* Raadpleeg [Verticaal automatisch schalen met virtuele-machineschaalsets](virtual-machine-scale-sets-vertical-scale-reprovision.md) voor meer informatie over verticaal schalen




<!--HONumber=Nov16_HO2-->


