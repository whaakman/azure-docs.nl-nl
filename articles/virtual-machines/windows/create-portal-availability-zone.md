---
title: Maak een zoned Windows-VM met de Azure portal | Microsoft Docs
description: Een virtuele Windows-machine maken in een zone beschikbaarheid met de Azure-portal
services: virtual-machines-windows
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/19/2017
ms.author: danlep
ms.custom: 
ms.openlocfilehash: 4d48aff7d29def9fa54438a11885b4ff4fba54cc
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="create-a-windows-virtual-machine-in-an-availability-zone-with-the-azure-portal"></a>Maken van een virtuele Windows-machine in een zone beschikbaarheid met de Azure-portal

In dit artikel wordt stapsgewijs via de Azure portal gebruiken voor het maken van een virtuele machine in een zone Azure beschikbaarheid. Een [beschikbaarheidszone](../../availability-zones/az-overview.md) is een fysiek afgescheiden zone in een Azure-regio. Gebruik beschikbaarheidszones om uw apps en gegevens te beschermen tegen het onwaarschijnlijke risico van een storing of het verloren gaan van een heel datacenter.

[!INCLUDE [availability-zones-preview-statement.md](../../../includes/availability-zones-preview-statement.md)]


## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Aanmelden bij de Azure portal op https://portal.azure.com.

## <a name="create-virtual-machine"></a>Virtuele machine maken

1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.

2. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. 

3. Geef de informatie van de virtuele machine op. De gebruikersnaam en het wachtwoord die u hier opgeeft, worden gebruikt voor aanmelding bij de virtuele machine. Na het voltooien klikt u op **OK**.

    ![Voer basisinformatie over uw virtuele machine in op de portalblade](./media/create-portal-availability-zone/create-windows-vm-portal-basic-blade.png)

4. Selecteer een grootte voor de VM. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. Selecteer een van de grootten die worden ondersteund, zoals in het voorbeeld van de zones beschikbaarheid behandelen *DS1_v2 standaard*. 

    ![Schermopname van VM-grootten](./media/create-portal-availability-zone/create-linux-vm-portal-sizes.png)  

5. Onder **instellingen** > **hoge beschikbaarheid**, selecteer een van de genummerde zones van de **beschikbaarheid zone** dropdown, blijven de overige standaardinstellingen, en Klik op **OK**.

    ![Selecteer een zone beschikbaarheid](./media/create-portal-availability-zone/create-linux-vm-portal-availability-zone.png)

6. Klik op de pagina overzicht **aankoop** implementatie van virtuele machine te starten.

7. De VM wordt aan het dashboard van de Azure Portal vastgemaakt. Zodra de implementatie is voltooid, wordt de samenvatting van de VM automatisch geopend.


## <a name="zone-for-ip-address-and-managed-disk"></a>Zone voor IP-adres en beheerde schijf

Wanneer de virtuele machine wordt geïmplementeerd in een zone beschikbaarheid, zijn de IP-adres en de beheerde schijfbronnen geïmplementeerd in dezelfde regio bevindt beschikbaarheid. U kunt bevestigen dat de zone-instellingen met Azure PowerShell. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

De volgende voorbeelden informatie ophalen over de resources in een resourcegroep met de naam *myResourceGroup*. Vervang de naam van de resourcegroep die u gebruikt voor het maken van de virtuele machine.

Vinden van de zone van het openbare IP-adres met [Get-AzureRmPublicIpAddress](/en-us/powershell/module/azurerm.network/get-azurermpublicipaddress):

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup
```
De `Zones` -instelling in de uitvoer geeft aan dat het openbare IP-adres in dezelfde regio bevindt beschikbaarheid als de virtuele machine:

```powershell
Name                     : myVM-ip
ResourceGroupName        : myResourceGroup
Location                 : eastus2
Id                       : /subscriptions/e44f251c-c67e-4760-9ed6-bf99a306ecff/resourceGroups/danlep0911/providers/Micr
                           osoft.Network/publicIPAddresses/myVM-ip
Etag                     : W/"b67e14c0-7e8a-4d12-91c5-da2a5dfad132"
ResourceGuid             : 314bf57d-9b25-4474-9282-db3561d536aa
ProvisioningState        : Succeeded
Tags                     :
PublicIpAllocationMethod : Dynamic
IpAddress                : 13.68.16.25
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                             "Id": "/subscriptions/e44f251c-c67e-4760-9ed6-bf99a306ecff/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVM11842/ipConfigurations/ipconfig1"
                           }
DnsSettings              : null
Zones                    : {2}
```


De resource van de beheerde schijf voor de virtuele machine wordt ook in dezelfde beschikbaarheidszone gemaakt. U kunt dit controleren met [Get-AzureRmDisk](/powershell/module/azurerm.compute/get-azurermdisk):

```powershell
Get-AzureRmDisk -ResourceGroupName myResourceGroup
```

In de uitvoer ziet u dat de beheerde schijf zich in dezelfde beschikbaarheidszone bevindt als de virtuele machine:

```powershell
ResourceGroupName  : myResourceGroup
AccountType        : PremiumLRS
OwnerId            : /subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.
                     Compute/virtualMachines/myVM
ManagedBy          : /subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.
                     Compute/virtualMachines/myVM
Sku                : Microsoft.Azure.Management.Compute.Models.DiskSku
Zones              : {2}
TimeCreated        : 9/7/2017 6:57:26 PM
OsType             : Windows
CreationData       : Microsoft.Azure.Management.Compute.Models.CreationData
DiskSizeGB         : 127
EncryptionSettings :
ProvisioningState  : Succeeded
Id                 : /subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.
                     Compute/disks/myVM_OsDisk_1_bd921920bb0a4650becfc2d830000000
Name               : myVM_OsDisk_1_bd921920bb0a4650becfc2d830000000
Type               : Microsoft.Compute/disks
Location           : eastus2
Tags               : {}
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u een virtuele machine maakt in een beschikbaarheidszone. Meer informatie over [regio's en beschikbaarheid](regions-and-availability.md) voor virtuele Azure-machines.
