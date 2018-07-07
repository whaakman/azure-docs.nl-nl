---
title: MSI configureren op een Azure-VMSS met behulp van PowerShell
description: Stap voor stap instructies voor het configureren van een systeem- en aan toegewezen identiteiten op een Azure-VMSS met behulp van PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/27/2017
ms.author: daveba
ms.openlocfilehash: 61fa6c94c0d717fe1e71bf8929f2e3b4a0982562
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37903876"
---
# <a name="configure-a-vmss-managed-service-identity-msi-using-powershell"></a>Configureren van een VMSS Managed Service Identity (MSI) met behulp van PowerShell

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u de beheerde Service-identiteit-bewerkingen op een virtuele Machine schaal ingesteld (VMSS), met behulp van PowerShell uit te voeren:
- In- en uitschakelen van het systeem toegewezen identiteit in een Azure VMSS
- Toevoegen en verwijderen van een gebruiker toegewezen identiteit in een Azure VMSS

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md). **Lees de [verschil tussen een systeem toegewezen en een gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u nog een Azure-account hebt [zich registreren voor een gratis account](https://azure.microsoft.com/free/) voordat u doorgaat.
- Installeer [de meest recente versie van Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) als u dat nog niet gedaan hebt. 

## <a name="system-assigned-managed-identity"></a>Het systeem toegewezen beheerde identiteit

In deze sectie leert u hoe u om te schakelen en te verwijderen van een systeem toegewezen identiteit met Azure PowerShell.

### <a name="enable-system-assigned-identity-during-the-creation-of-an-azure-vmss"></a>Systeem toegewezen identiteit tijdens het maken van een Azure-VMSS inschakelen

Voor het maken van een VMSS met het systeem toegewezen identiteit ingeschakeld:

1. Raadpleeg *voorbeeld 1* in de [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) naslagartikel voor cmdlet een VMSS met een systeem toegewezen identiteit maken.  De parameter toevoegen `-IdentityType SystemAssigned` naar de `New-AzureRmVmssConfig` cmdlet:

    ```powershell
    $VMSS = New-AzureRmVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```

2. (Optioneel) Het gebruik van MSI VMSS extensie toevoegen de `-Name` en `-Type` parameter op de [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet. U kunt doorgeven "ManagedIdentityExtensionForWindows" of "ManagedIdentityExtensionForLinux", afhankelijk van het type virtuele machine, en geef de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens:

    > [!NOTE]
    > Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens.

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

## <a name="enable-system-assigned-identity-on-an-existing-azure-vmss"></a>Inschakelen van systeem toegewezen identiteit in een bestaande Azure VMSS

Als u nodig hebt om in te schakelen van een systeem toegewezen identiteit in een bestaande Azure-VMSS:

1. Aanmelden bij Azure met `Login-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele machine, zoals 'Inzender voor virtuele machines':

   ```powershell
   Login-AzureRmAccount
   ```

2. Eerst ophalen van de VMSS-eigenschappen met behulp van de [ `Get-AzureRmVmss` ](/powershell/module/azurerm.compute/get-azurermvmss) cmdlet. Als u wilt een systeem toegewezen identiteit inschakelen, gebruikt u de `-IdentityType` overschakelen op de [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name -myVM -IdentityType "SystemAssigned"
   ```

3. Het gebruik van MSI VMSS extensie toevoegen de `-Name` en `-Type` parameter op de [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet. U kunt doorgeven "ManagedIdentityExtensionForWindows" of "ManagedIdentityExtensionForLinux", afhankelijk van het type virtuele machine, en geef de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-vmss"></a>Het systeem toegewezen identiteit van een Azure-VMSS uitschakelen

> [!NOTE]
> Beheerde Service-identiteit van een virtuele-Machineschaalset uitschakelen wordt momenteel niet ondersteund. In de tussentijd kunt u overschakelen tussen het gebruik van het systeem toegewezen en de gebruiker toegewezen identiteiten.

Als u een virtuele-Machineschaalset die niet meer nodig is op het systeem toegewezen identiteit maar nog steeds moet gebruiker toegewezen identiteiten hebt, gebruikt u de volgende cmdlet:

1. Aanmelden bij Azure met `Login-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele machine, zoals 'Inzender voor virtuele machines':

2. Voer de volgende cmdlet uit:

    ```powershell
    Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
    ```

## <a name="user-assigned-identity"></a>Door gebruiker toegewezen identiteit

In deze sectie leert u hoe u toevoegen en verwijderen van een gebruiker toegewezen identiteit van een VMSS met behulp van Azure PowerShell.

### <a name="assign-a-user-assigned-identity-during-creation-of-an-azure-vmss"></a>Een gebruiker toegewezen identiteit tijdens het maken van een Azure-VMSS toewijzen

Het maken van een nieuwe VMSS met een gebruiker toegewezen identiteit wordt momenteel niet ondersteund via PowerShell. Zie de volgende sectie over het toevoegen van de identiteit van een gebruiker toegewezen aan een bestaand VMSS. Kijk binnenkort voor updates.

### <a name="assign-a-user-identity-to-an-existing-azure-vmss"></a>De identiteit van een gebruiker toewijzen aan een bestaande Azure VMSS

Een gebruiker toegewezen identiteit aan een bestaande Azure VMSS toewijzen:

1. Aanmelden bij Azure met `Connect-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele machine, zoals 'Inzender voor virtuele machines':

   ```powershell
   Connect-AzureRmAccount
   ```

2. Eerst ophalen van de eigenschappen van de virtuele machine met behulp van de `Get-AzureRmVM` cmdlet. Als u wilt de identiteit van een gebruiker toegewezen aan de Azure-VMSS toewijst, gebruikt u de `-IdentityType` en `-IdentityID` overschakelen op de [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet. Vervang `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>`, `USER ASSIGNED ID2` door uw eigen waarden.

   [!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]


   ```powershell
   $vmss = Get-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME>
   Update-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -VM $vmss -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vmss"></a>Een gebruiker toegewezen identiteit van een Azure-VMSS verwijderen

> [!NOTE]
> Verwijderen van alle gebruiker toegewezen identiteiten uit een virtuele-Machineschaalset is momenteel niet ondersteund, tenzij er een systeem toegewezen identiteit. Kijk binnenkort voor updates.

Als uw VMSS meerdere gebruiker toegewezen identiteiten heeft, kunt u alles behalve het laatste item met de volgende opdrachten verwijderen. Vervang de `<RESOURCE GROUP>` en `<VMSS NAME>` parameterwaarden door uw eigen waarden. De `<MSI NAME>` is de eigenschap name van de identiteit van de gebruiker die is toegewezen, die op de VMSS moet blijven. Deze informatie kan worden gevonden door in de sectie van de identiteit van het gebruik van de VMSS `az vmss show`:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss
$vmss.Identity.IdentityIds = "<MSI NAME>"
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -VirtualMachineScaleSet $vmss
```

Als uw VMSS is zowel de systeem toegewezen als de gebruiker toegewezen identiteiten, kunt u alle gebruiker toegewezen identiteiten door over te schakelen voor het gebruik van alleen systeem toegewezen verwijderen. Gebruik de volgende opdracht:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss
$vmss.Identity.IdentityIds = $null
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -VirtualMachine $vmss -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>Gerelateerde inhoud

- [Overzicht van de beheerde Service-identiteit](overview.md)
- Zie voor het volledige Azure-VM maken snelstartgidsen:
  
  - [Een Windows-machine maken met PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Een virtuele Linux-machine maken met PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 

















