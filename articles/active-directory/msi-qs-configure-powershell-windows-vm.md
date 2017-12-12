---
title: Het configureren van MSI op een virtuele machine in Azure met behulp van PowerShell
description: Stap door stapsgewijze instructies voor het configureren van een beheerde Service identiteit (MSI) op een virtuele machine van Azure, met behulp van PowerShell.
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/27/2017
ms.author: bryanla
ms.openlocfilehash: 7d34e506eca609001ef6d10ce288c2531921f3d0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="configure-a-vm-managed-service-identity-msi-using-powershell"></a>Configureren van een VM beheerde Service identiteit (MSI) met behulp van PowerShell

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om alle services die Azure AD-verificatie ondersteunt, zonder referenties in uw code te verifiëren. 

In dit artikel leert u het inschakelen en het verwijderen van MSI voor een virtuele machine van Azure, met behulp van PowerShell.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

Ook installeren [de meest recente versie van Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) (versie 4.3.1 of hoger) als u dat nog niet gedaan hebt.

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>MSI bij het maken van een virtuele machine van Azure inschakelen

Maken van een VM MSI ingeschakeld:

1. Verwijzen naar een van de volgende Azure VM snelstartgidsen, alleen de benodigde secties voltooid ('Aanmelden bij Azure', 'Resourcegroep maken', 'Netwerken groep maken', 'De virtuele machine maken'). 

   > [!IMPORTANT] 
   > Wanneer u naar de sectie 'De VM maken' krijgen, moet u een kleine wijziging van de [nieuw AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvm) cmdlet-syntaxis. Zorg ervoor dat om toe te voegen een `-IdentityType "SystemAssigned"` parameter voor het inrichten van de virtuele machine met een MSI-bestand, bijvoorbeeld:
   >  
   > `$vmConfig = New-AzureRmVMConfig -VMName myVM -IdentityType "SystemAssigned" ...`

   - [Maken van een virtuele Windows-machine met behulp van PowerShell](../virtual-machines/windows/quick-create-powershell.md)
   - [Maken van een virtuele Linux-machine met behulp van PowerShell](../virtual-machines/linux/quick-create-powershell.md)



2. Toevoegen van de MSI VM uitbreiding met behulp van de `-Type` parameter bij de [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. U kunt doorgeven 'ManagedIdentityExtensionForWindows' of 'ManagedIdentityExtensionForLinux', afhankelijk van het type van de virtuele machine, en deze met de naam de `-Name` parameter. De `-Settings` parameter geeft u de poort die wordt gebruikt door het eindpunt van het OAuth-token voor de aanschaf van token:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

## <a name="enable-msi-on-an-existing-azure-vm"></a>MSI op een bestaande virtuele machine van Azure inschakelen

Als u inschakelen, MSI op een bestaande virtuele Machine wilt:

1. Meld u bij het gebruik van Azure `Login-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Controleer ook of uw account behoort tot een functie waarmee u schrijfmachtigingen heeft op de virtuele machine, zoals 'Virtual Machine Contributor':

   ```powershell
   Login-AzureRmAccount
   ```

2. Eerst ophalen van de eigenschappen van de virtuele machine met behulp van de `Get-AzureRmVM` cmdlet. Om in te schakelen MSI, gebruikt u de `-IdentityType` overschakelen op de [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm -IdentityType "SystemAssigned"
   ```

3. Toevoegen van de MSI VM uitbreiding met behulp van de `-Type` parameter bij de [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. U kunt doorgeven 'ManagedIdentityExtensionForWindows' of 'ManagedIdentityExtensionForLinux', afhankelijk van het type van de virtuele machine, en deze met de naam de `-Name` parameter. De `-Settings` parameter geeft u de poort die wordt gebruikt door het eindpunt van het OAuth-token voor de aanschaf van token. Zorg ervoor dat u de juiste `-Location` parameter die overeenkomt met de locatie van de bestaande virtuele machine:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

## <a name="remove-msi-from-an-azure-vm"></a>Verwijder de MSI van een Azure VM

Als u een virtuele Machine die een MSI-bestand niet meer nodig hebt, kunt u de `RemoveAzureRmVMExtension` cmdlet MSI verwijderen uit de virtuele machine:

1. Meld u bij het gebruik van Azure `Login-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Controleer ook of uw account behoort tot een functie waarmee u schrijfmachtigingen heeft op de virtuele machine, zoals 'Virtual Machine Contributor':

   ```powershell
   Login-AzureRmAccount
   ```

2. Gebruik de `-Name` switch met de [verwijderen AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension) cmdlet, geven de dezelfde naam die u hebt gebruikt toen u de extensie toegevoegd:

   ```powershell
   Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="related-content"></a>Gerelateerde inhoud

- [Overzicht van beheerde Service-identiteit](msi-overview.md)
- Voor het volledige virtuele machine in Azure maken snelstartgidsen, Zie:
  
  - [Een virtuele Windows-machine maken met PowerShell](../virtual-machines/windows/quick-create-powershell.md) 
  - [Een virtuele Linux-machine maken met PowerShell](../virtual-machines/linux/quick-create-powershell.md) 

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.
















