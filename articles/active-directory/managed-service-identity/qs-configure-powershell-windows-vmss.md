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
ms.openlocfilehash: b82785d0f4b6a5952334e891e7adec570c624f2d
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39238128"
---
# <a name="configure-a-vmss-managed-service-identity-msi-using-powershell"></a>Configureren van een VMSS Managed Service Identity (MSI) met behulp van PowerShell

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u de bewerkingen van de beheerde Service-identiteit in een schaalset van virtuele machine van Azure uit te voeren met behulp van PowerShell:
- In- en uitschakelen van het systeem toegewezen identiteit op een virtuele-machineschaalset
- Toevoegen en verwijderen van een gebruiker toegewezen identiteit op een virtuele-machineschaalset

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md). **Lees de [verschil tussen een systeem toegewezen en een gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende roltoewijzingen:
    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) aan een virtuele-machineschaalset maken en inschakelen en verwijderen van systeem toegewezen beheerd en/of de identiteit van de gebruiker van een virtuele-machineschaalset ingesteld.
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol te maken van de identiteit van een gebruiker toegewezen.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol die u wilt toewijzen en verwijderen van een gebruiker toegewezen identiteit van en naar een virtuele-machineschaalset.
- Installeer [de meest recente versie van Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) als u dat nog niet gedaan hebt. 

## <a name="system-assigned-managed-identity"></a>Het systeem toegewezen beheerde identiteit

In deze sectie leert u hoe u om te schakelen en te verwijderen van een systeem toegewezen identiteit met Azure PowerShell.

### <a name="enable-system-assigned-identity-during-the-creation-of-an-azure-virtual-machine-scale-set"></a>Systeem toegewezen identiteit inschakelen tijdens het maken van een schaalset voor virtuele Azure-machine

Voor het maken van een VMSS met het systeem toegewezen identiteit ingeschakeld:

1. Raadpleeg *voorbeeld 1* in de [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) naslagartikel voor cmdlet een VMSS met een systeem toegewezen identiteit maken.  De parameter toevoegen `-IdentityType SystemAssigned` naar de `New-AzureRmVmssConfig` cmdlet:

    ```powershell
    $VMSS = New-AzureRmVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```

2. (Optioneel) Het gebruik van MSI VMSS extensie toevoegen de `-Name` en `-Type` parameter op de [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet. U kunt een van beide "ManagedIdentityExtensionForWindows" doorgeven of "ManagedIdentityExtensionForLinux", afhankelijk van het type van virtuele-machineschaalset instellen en de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens:

    > [!NOTE]
    > Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens.

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

## <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Systeem toegewezen identiteit op een bestaande schaalset voor virtuele Azure-machine inschakelen

Als u nodig hebt om in te schakelen van een systeem toegewezen identiteit op een bestaande schaalset voor virtuele Azure-machine:

1. Aanmelden bij Azure met `Login-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele-machineschaalset, zoals 'Inzender voor virtuele machines':

   ```powershell
   Login-AzureRmAccount
   ```

2. Eerste ophalen, instellen van de virtuele-machineschaalset met behulp van de [ `Get-AzureRmVmss` ](/powershell/module/azurerm.compute/get-azurermvmss) cmdlet. Als u wilt een systeem toegewezen identiteit inschakelen, gebruikt u de `-IdentityType` overschakelen op de [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) cmdlet:

   ```powershell
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name -myVmss -IdentityType "SystemAssigned"
   ```

3. Het gebruik van MSI VMSS extensie toevoegen de `-Name` en `-Type` parameter op de [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet. U kunt een van beide "ManagedIdentityExtensionForWindows" doorgeven of "ManagedIdentityExtensionForLinux", afhankelijk van het type van virtuele-machineschaalset instellen en de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Het systeem toegewezen identiteit van een schaalset voor virtuele Azure-machine uitschakelen

Als u een virtuele-machineschaalset die niet meer nodig is op het systeem toegewezen identiteit maar nog steeds moet gebruiker toegewezen identiteiten hebt, gebruikt u de volgende cmdlet:

1. Aanmelden bij Azure met `Login-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele-machineschaalset, zoals 'Inzender voor virtuele machines':

2. Voer de volgende cmdlet uit:

   ```powershell
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
   ```

Als u een virtuele-machineschaalset die systeem toegewezen identiteit niet meer nodig hebt en er geen gebruiker toegewezen identiteiten, gebruikt u de volgende opdrachten:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```

## <a name="user-assigned-identity"></a>Door gebruiker toegewezen identiteit

In deze sectie leert u hoe u toevoegen en verwijderen van een gebruiker toegewezen identiteit van een VM-schaalset met behulp van Azure PowerShell.

### <a name="assign-a-user-assigned-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Een gebruiker toegewezen identiteit toewijzen tijdens het maken van een schaalset voor virtuele Azure-machine

Het maken van een nieuwe virtuele-machineschaalset met een gebruiker toegewezen identiteit wordt momenteel niet ondersteund via PowerShell. Zie de volgende sectie over het toevoegen van de identiteit van een gebruiker toegewezen aan een bestaande virtuele-machineschaalset. Controleer later op updates.

### <a name="assign-a-user-identity-to-an-existing-azure-virtual-machine-scale-set"></a>De identiteit van een gebruiker toewijzen aan een bestaande schaalset voor virtuele Azure-machine

Een gebruiker toegewezen identiteit toewijzen aan een bestaande schaalset voor virtuele Azure-machine:

1. Aanmelden bij Azure met `Connect-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele-machineschaalset, zoals 'Inzender voor virtuele machines':

   ```powershell
   Connect-AzureRmAccount
   ```

2. Eerste ophalen, instellen van de virtuele-machineschaalset met behulp van de `Get-AzureRmVM` cmdlet. Als u wilt de identiteit van een gebruiker toegewezen aan de virtuele-machineschaalset toewijst, gebruikt u de `-IdentityType` en `-IdentityID` overschakelen op de [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) cmdlet. Vervang `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>`, `USER ASSIGNED ID2` door uw eigen waarden.

   [!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```powershell
   Update-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME> -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Een beheerde identiteit van een schaalset voor virtuele Azure-machine toegewezen gebruiker verwijderen

Als uw virtuele-machineschaalset meerdere gebruiker toegewezen identiteiten heeft, kunt u alles behalve het laatste item met de volgende opdrachten verwijderen. Vervang de parameterwaarden `<RESOURCE GROUP>` en `<VMSS NAME>` door uw eigen waarden. De `<MSI NAME>` is de eigenschap name van de identiteit van de gebruiker die is toegewezen, die op de virtuele-machineschaalset moet blijven. Deze informatie kan worden gevonden in de sectie van de identiteit van de VM-schaalset met behulp van `az vmss show`:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType UserAssigned -IdentityID "<MSI NAME>"
```
Als uw virtuele-machineschaalset heeft geen een systeem toegewezen identiteit en u wilt verwijderen van alle gebruiker toegewezen identiteiten uit, gebruikt u de volgende opdracht uit:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```
Als uw virtuele-machineschaalset is zowel de systeem toegewezen als de gebruiker toegewezen identiteiten, kunt u alle gebruiker toegewezen identiteiten door over te schakelen voor het gebruik van alleen systeem toegewezen verwijderen.

```powershell 
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>Gerelateerde inhoud

- [Overzicht van de beheerde Service-identiteit](overview.md)
- Zie voor het volledige Azure-VM maken snelstartgidsen:
  
  - [Een Windows-machine maken met PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Een virtuele Linux-machine maken met PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 

















