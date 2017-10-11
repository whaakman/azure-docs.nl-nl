---
title: Beheren van virtuele machines in een virtuele-Machineschaalset | Microsoft Docs
description: Virtuele machines in een virtuele-machineschaalset ingesteld met Azure PowerShell beheren.
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d35fa77a-de96-4ccd-a332-eb181d1f4273
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: d09a020b903e5f43afe03b86c675bcc1eb536cbc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a>Beheren van virtuele machines in een virtuele-machineschaalset
De taken in dit artikel gebruiken voor het beheren van virtuele machines in uw virtuele-machineschaalset.

De meeste taken die betrekking hebben op het beheren van een virtuele machine in een schaalset vereisen dat u weet dat de exemplaar-ID van de computer die u wilt beheren. U kunt [Azure Resource Explorer](https://resources.azure.com) de exemplaar-ID van een virtuele machine in een schaalset vinden. U kunt ook Resource Explorer gebruiken om te controleren of de status van de taken die u hebt voltooid.

Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview) voor informatie over het installeren van de nieuwste versie van Azure PowerShell, het selecteren van het abonnement en het aanmelden bij uw account.

## <a name="display-information-about-a-scale-set"></a>Informatie weergeven over een schaalset
U kunt algemene informatie over een schaalset, die ook wordt aangeduid als de instantieweergave ophalen. Of u krijgt meer specifieke informatie, zoals informatie over de resources in de schaalset.

Vervang de waarden tussen aanhalingstekens met de naam of de resourcegroep en schaal ingesteld en voer vervolgens de opdracht:

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

Deze retourneert ongeveer het volgende:

    Id                                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                                        : myvmss1
    Type                                        : Microsoft.Compute/virtualMachineScaleSets
    Location                                    : centralus
    Sku                                         :
      Name                                      : Standard_A0
      Tier                                      : Standard
      Capacity                                  : 3
    UpgradePolicy                               :
      Mode                                      : Manual
    VirtualMachineProfile                       :
      OsProfile                                 :
        ComputerNamePrefix                      : vmss1
        AdminUsername                           : admin1
        WindowsConfiguration                    :
          ProvisionVMAgent                      : True
          EnableAutomaticUpdates                : True
    StorageProfile                              :
      ImageReference                            :
        Publisher                               : MicrosoftWindowsServer
        Offer                                   : WindowsServer
        Sku                                     : 2012-R2-Datacenter
        Version                                 : latest
      OsDisk                                    :
        Name                                    : vmssosdisk
        Caching                                 : ReadOnly
        CreateOption                            : FromImage
        VhdContainers[0]                        : https://astore.blob.core.windows.net/vmss
        VhdContainers[1]                        : https://gstore.blob.core.windows.net/vmss
        VhdContainers[2]                        : https://mstore.blob.core.windows.net/vmss
        VhdContainers[3]                        : https://sstore.blob.core.windows.net/vmss
        VhdContainers[4]                        : https://ystore.blob.core.windows.net/vmss
    NetworkProfile                              :
      NetworkInterfaceConfigurations[0]         :
        Name                                    : mync1
        Primary                                 : True
        IpConfigurations[0]                     :
          Name                                  : ip1
          Subnet                                :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/virtualNetworks/myvn1/subnets/mysn1
          LoadBalancerBackendAddressPools[0]    :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/backendAddressPools/bepool1
        LoadBalancerInboundNatPools[0]          :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/inboundNatPools/natpool1
    ExtensionProfile                            :
      Extensions[0]                             :
        Name                                    : Microsoft.Insights.VMDiagnosticsSettings
        Publisher                               : Microsoft.Azure.Diagnostics
        Type                                    : IaaSDiagnostics
        TypeHandlerVersion                      : 1.5
        AutoUpgradeMinorVersion                 : True
        Settings                                : {"xmlCfg":"...","storageAccount":"astore"}
    ProvisioningState                           : Succeeded

De aanhalingstekens waarden vervangt door de naam van de resource-groep en scale set. Vervang  *#*  met exemplaar-id van de virtuele machine die u wilt u informatie wilt over en vervolgens uit te voeren:

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Het resultaat dat lijkt op het volgende voorbeeld:

    Id                            : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/
                                    virtualMachineScaleSets/myvmss1/virtualMachines/0
    Name                          : myvmss1_0
    Type                          : Microsoft.Compute/virtualMachineScaleSets/virtualMachines
    Location                      : centralus
    InstanceId                    : 0
    Sku                           :
      Name                        : Standard_A0
      Tier                        : Standard
    LatestModelApplied            : True
    StorageProfile                :
      ImageReference              :
        Publisher                 : MicrosoftWindowsServer
        Offer                     : WindowsServer
        Sku                       : 2012-R2-Datacenter
        Version                   : 4.0.20160617
      OsDisk                      :
        OsType                    : Windows
        Name                      : vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8
        Vhd                       :
          Uri                     : https://astore.blob.core.windows.net/vmss/vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8.vhd
        Caching                   : ReadOnly
        CreateOption              : FromImage
    OsProfile                     :
      ComputerName                : myvmss1-0
      AdminUsername               : admin1
      WindowsConfiguration        :
        ProvisionVMAgent          : True
        EnableAutomaticUpdates    : True
    NetworkProfile                :
      NetworkInterfaces[0]        :
        Id                        : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/
                                    myvmss1/virtualMachines/0/networkInterfaces/mync1
    ProvisioningState             : Succeeded
    Resources[0]                  :
      Id                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachines/
                                    myvmss1_0/extensions/Microsoft.Insights.VMDiagnosticsSettings
      Name                        : Microsoft.Insights.VMDiagnosticsSettings
      Type                        : Microsoft.Compute/virtualMachines/extensions
      Location                    : centralus
      Publisher                   : Microsoft.Azure.Diagnostics
      VirtualMachineExtensionType : IaaSDiagnostics
      TypeHandlerVersion          : 1.5
      AutoUpgradeMinorVersion     : True
      Settings                    : {"xmlCfg":"...","storageAccount":"astore"}
      ProvisioningState           : Succeeded

## <a name="start-a-virtual-machine-in-a-scale-set"></a>Een virtuele machine te starten in een schaalset
De aanhalingstekens waarden vervangt door de naam van de resource-groep en scale set. Vervang  *#*  met de id van de virtuele machine die u wilt starten en vervolgens uit te voeren:

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

In Resource Explorer, ziet u de status van het exemplaar **met**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T02:10:08.0730839+00:00"
      },
      {
        "code": "PowerState/running",
        "level": "Info",
        "displayStatus": "VM running"
      }
    ]

U kunt alle virtuele machines in de schaal instelt met behulp van de parameter - InstanceId niet starten.

## <a name="stop-a-virtual-machine-in-a-scale-set"></a>Een virtuele machine in een scale-verzameling stoppen
De aanhalingstekens waarden vervangt door de naam van de resource-groep en scale set. Vervang  *#*  met de id van de virtuele machine die u wilt stoppen en vervolgens uit te voeren:

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

In Resource Explorer, ziet u de status van het exemplaar **ongedaan**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T01:25:17.8792929+00:00"
      },
      {
        "code": "PowerState/deallocated",
        "level": "Info",
        "displayStatus": "VM deallocated"
      }
    ]

Als u wilt stoppen van een virtuele machine en deze niet ongedaan gemaakt, gebruikt u de parameter - StayProvisioned. U kunt de virtuele machines in de set met behulp van de exemplaar-id - parameter niet stoppen.

## <a name="restart-a-virtual-machine-in-a-scale-set"></a>Opnieuw opstarten van een virtuele machine in een schaalset
De aanhalingstekens waarden vervangt door de naam van de resourcegroep en de schaal is ingesteld. Vervang  *#*  met de id van de virtuele machine die u wilt starten en vervolgens uit te voeren:

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

U kunt alle virtuele machines in de set met behulp van de parameter - InstanceId niet opnieuw.

## <a name="remove-a-virtual-machine-from-a-scale-set"></a>Een virtuele machine verwijderen uit een schaalset
De aanhalingstekens waarden vervangt door de naam van de resourcegroep en de schaal is ingesteld. Vervang  *#*  met de id van de virtuele machine die u wilt verwijderen en vervolgens uit te voeren:  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

U kunt de virtuele-machineschaalset in één keer verwijderen met behulp van de exemplaar-id - parameter niet.

## <a name="change-the-capacity-of-a-scale-set"></a>De capaciteit van een schaalset wijzigen
U kunt toevoegen of verwijderen van virtuele machines met het wijzigen van de capaciteit van de set. Haal de schaalaanpassingsset die u wilt wijzigen, de capaciteit aan wat u wilt dat het instellen en werk vervolgens de schaal instelt met de nieuwe capaciteit. In deze opdrachten kunt u de tussen aanhalingstekens waarden vervangt door de naam van de resourcegroep en de schaal is ingesteld.

    $vmss = Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
    $vmss.sku.capacity = 5
    Update-AzureRmVmss -ResourceGroupName "resource group name" -Name "scale set name" -VirtualMachineScaleSet $vmss 

Als u virtuele machines van de schaalaanpassingsset verwijdert, worden de virtuele machines met de hoogste id eerst verwijderd.

