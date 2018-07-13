---
title: MSI configureren op een Azure-VM met behulp van PowerShell
description: Voor stap door stap instructies voor het configureren van een Managed Service Identity (MSI) op een Azure-VM, met behulp van PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 9a466a5c695277a7b5833f997e2ad7281c962f3f
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38610996"
---
# <a name="configure-a-vm-managed-service-identity-msi-using-powershell"></a>Configureren van een virtuele machine Managed Service Identity (MSI) met behulp van PowerShell

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u inschakelen en verwijderen van MSI-bestand voor een Azure-VM met behulp van PowerShell.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Ook installeren [de meest recente versie van Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) (versie 4.3.1 of hoger) als u dat nog niet gedaan hebt.

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Inschakelen van MSI tijdens het maken van een Azure VM

Een VM MSI-functionaliteit maken:

1. Verwijzen naar een van de volgende snelstartgidsen voor Azure VM, alleen de benodigde secties voltooid ('Meld u aan bij Azure', 'Resourcegroep maken', 'Netwerken groep maken', 'Create VM'). 

   > [!IMPORTANT] 
   > Wanneer u naar de sectie 'Maken van de virtuele machine', een kleine wijziging aanbrengt in de [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvm) cmdlet-syntaxis. Zorg ervoor dat u toevoegt een `-IdentityType "SystemAssigned"` parameter voor het inrichten van de virtuele machine met een MSI-bestand, bijvoorbeeld:
   >  
   > `$vmConfig = New-AzureRmVMConfig -VMName myVM -IdentityType "SystemAssigned" ...`

   - [Maak een Windows-machine met behulp van PowerShell](~/articles/virtual-machines/windows/quick-create-powershell.md)
   - [Maken van een virtuele Linux-machine met behulp van PowerShell](~/articles/virtual-machines/linux/quick-create-powershell.md)



2. Toevoegen van de MSI VM-extensie via de `-Type` parameter op de [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. U kunt doorgeven "ManagedIdentityExtensionForWindows" of "ManagedIdentityExtensionForLinux", afhankelijk van het type virtuele machine, en geef de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

## <a name="enable-msi-on-an-existing-azure-vm"></a>MSI-bestand op een bestaande VM in Azure inschakelen

Als u nodig hebt om in te schakelen van MSI-bestand op een bestaande virtuele Machine:

1. Aanmelden bij Azure met `Connect-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele machine, zoals 'Inzender voor virtuele machines':

   ```powershell
   Connect-AzureRmAccount
   ```

2. Eerst ophalen van de eigenschappen van de virtuele machine met behulp van de `Get-AzureRmVM` cmdlet. Gebruik vervolgens om in te schakelen MSI-bestand, de `-IdentityType` overschakelen op de [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm -IdentityType "SystemAssigned"
   ```

3. Toevoegen van de MSI VM-extensie via de `-Type` parameter op de [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. U kunt doorgeven "ManagedIdentityExtensionForWindows" of "ManagedIdentityExtensionForLinux", afhankelijk van het type virtuele machine, en geef de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens. Geef de juiste `-Location` parameter, die overeenkomt met de locatie van de bestaande virtuele machine:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

## <a name="remove-msi-from-an-azure-vm"></a>MSI-bestand verwijderen uit een Azure-VM

Als u een virtuele Machine die een MSI-bestand niet meer nodig hebt, kunt u de `RemoveAzureRmVMExtension` cmdlet voor het verwijderen van MSI-bestand van de virtuele machine:

1. Aanmelden bij Azure met `Connect-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele machine, zoals 'Inzender voor virtuele machines':

   ```powershell
   Connect-AzureRmAccount
   ```

2. Gebruik de `-Name` overschakelen met de [Remove-AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension) cmdlet, op te geven de dezelfde naam die u hebt gebruikt toen u de extensie toegevoegd:

   ```powershell
   Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="related-content"></a>Gerelateerde inhoud

- [Overzicht van de beheerde Service-identiteit](msi-overview.md)
- Zie voor het volledige Azure-VM maken snelstartgidsen:
  
  - [Een Windows-machine maken met PowerShell](~/articles/virtual-machines/windows/quick-create-powershell.md) 
  - [Een virtuele Linux-machine maken met PowerShell](~/articles/virtual-machines/linux/quick-create-powershell.md) 

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.
















