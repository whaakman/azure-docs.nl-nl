---
title: Het configureren van beheerde identiteiten voor Azure-resources op een virtuele-machineschaalset instellen met behulp van PowerShell
description: Stapsgewijze instructies voor het configureren van een systeem en de gebruiker toegewezen beheerde identiteiten op een virtuele-machineschaalset instellen met behulp van PowerShell.
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
ms.openlocfilehash: e426da9e48c24a83c0612d233923227cf057e0f8
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47106850"
---
# <a name="configure-managed-identities-for-azure-resources-on-virtual-machine-scale-sets-using-powershell"></a>Configureren van beheerde identiteiten voor Azure-resources op virtuele-machineschaalsets met behulp van PowerShell

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel, leert met behulp van PowerShell, u hoe u de beheerde identiteit voor bewerkingen van de Azure-resources op een virtuele-machineschaalset uitvoeren:
- In- en uitschakelen van de door het systeem toegewezen beheerde identiteit op een virtuele-machineschaalset
- Toevoegen en verwijderen van een gebruiker toegewezen beheerde identiteit op een virtuele-machineschaalset

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een door het systeem toegewezen en beheerd gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende Azure op basis van rollen access control-toewijzingen:

    > [!NOTE]
    > Er zijn geen extra Azure AD directory-roltoewijzingen is vereist.

    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) instellen voor een virtuele-machineschaalset maken en inschakelen en verwijderen door het systeem toegewezen beheerd en/of de gebruiker toegewezen beheerde identiteit van een virtuele-machineschaalset.
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol te maken van een gebruiker toegewezen beheerde identiteit.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol die u wilt toewijzen en verwijderen van een gebruiker toegewezen beheerde identiteit van en naar een virtuele-machineschaalset.
- Installeer [de meest recente versie van Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) als u dat nog niet gedaan hebt. 

## <a name="system-assigned-managed-identity"></a>Het systeem toegewezen beheerde identiteit

In deze sectie leert u hoe u om te schakelen en te verwijderen van een systeem toegewezen beheerde identiteit met Azure PowerShell.

### <a name="enable-system-assigned-managed-identity-during-the-creation-of-an-azure-virtual-machine-scale-set"></a>Beheerde identiteit systeem toegewezen tijdens het maken van een schaalset voor virtuele Azure-machine inschakelen

Een VMSS maken met de door het systeem toegewezen beheerde identiteit ingeschakeld:

1. Raadpleeg *voorbeeld 1* in de [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) naslagartikel voor cmdlet een VMSS maken met een door het systeem toegewezen beheerde identiteit.  De parameter toevoegen `-IdentityType SystemAssigned` naar de `New-AzureRmVmssConfig` cmdlet:

    ```powershell
    $VMSS = New-AzureRmVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```

2. (Optioneel) De beheerde identiteiten toevoegen voor Azure-resources virtuele-met behulp van extensie machineschaalset de `-Name` en `-Type` parameter op de [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet. U kunt een van beide "ManagedIdentityExtensionForWindows" doorgeven of "ManagedIdentityExtensionForLinux", afhankelijk van het type van virtuele-machineschaalset instellen en de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens:

    > [!NOTE]
    > Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens.

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

## <a name="enable-system-assigned-managed-identity-on-an-existing-azure-virtual-machine-scale-set"></a>De beheerde inschakelen door het systeem toegewezen identiteit op een bestaande schaalset voor virtuele machine van Azure

Als u nodig hebt om in te schakelen van een systeem toegewezen beheerde identiteit op een bestaande schaalset voor virtuele Azure-machine:

1. Aanmelden bij Azure met `Login-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele-machineschaalset, zoals 'Inzender voor virtuele machines':

   ```powershell
   Login-AzureRmAccount
   ```

2. Eerste ophalen, instellen van de virtuele-machineschaalset met behulp van de [ `Get-AzureRmVmss` ](/powershell/module/azurerm.compute/get-azurermvmss) cmdlet. Gebruik vervolgens om in te schakelen op een beheerde identiteit voor het systeem is toegewezen, de `-IdentityType` overschakelen op de [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) cmdlet:

   ```powershell
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name -myVmss -IdentityType "SystemAssigned"
   ```

3. Toevoegen van de beheerde identiteiten voor het gebruik van Azure-resources VMSS extension de `-Name` en `-Type` parameter op de [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) cmdlet. U kunt een van beide "ManagedIdentityExtensionForWindows" doorgeven of "ManagedIdentityExtensionForLinux", afhankelijk van het type van virtuele-machineschaalset instellen en de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

### <a name="disable-the-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>De door het systeem toegewezen beheerde identiteit van een schaalset voor virtuele Azure-machine uitschakelen

Als u een virtuele-machineschaalset die niet meer nodig is op het systeem toegewezen beheerde identiteit maar nog steeds moet gebruiker toegewezen identiteiten op beheerde hebt, gebruikt u de volgende cmdlet:

1. Aanmelden bij Azure met `Login-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele-machineschaalset, zoals 'Inzender voor virtuele machines':

2. Voer de volgende cmdlet uit:

   ```powershell
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
   ```

Als u een virtuele-machineschaalset die beheerde identiteit systeem toegewezen niet meer nodig hebt en er geen beheerde gebruiker toegewezen identiteiten, gebruikt u de volgende opdrachten:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```

## <a name="user-assigned-managed-identity"></a>de gebruiker toegewezen beheerde identiteit

In deze sectie leert u hoe u toevoegen en verwijderen van een gebruiker toegewezen beheerde identiteit van een VM-schaalset met behulp van Azure PowerShell.

### <a name="assign-a-user-assigned-managed-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Een gebruiker toegewezen beheerde identiteit toewijzen tijdens het maken van een schaalset voor virtuele Azure-machine

Het maken van een nieuwe virtuele-machineschaalset met een gebruiker toegewezen beheerde identiteit wordt momenteel niet ondersteund via PowerShell. Zie de volgende sectie over het toevoegen van een beheerde identiteit voor de gebruiker toegewezen aan een bestaande virtuele-machineschaalset. Controleer later op updates.

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Een gebruiker toegewezen beheerde identiteit toewijzen aan een bestaande schaalset voor virtuele Azure-machine

Een gebruiker toegewezen beheerde identiteit toewijzen aan een bestaande schaalset voor virtuele Azure-machine:

1. Aanmelden bij Azure met `Connect-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele-machineschaalset, zoals 'Inzender voor virtuele machines':

   ```powershell
   Connect-AzureRmAccount
   ```

2. Eerste ophalen, instellen van de virtuele-machineschaalset met behulp van de `Get-AzureRmVM` cmdlet. Als u wilt een beheerde identiteit voor de gebruiker toegewezen aan de virtuele-machineschaalset toewijst, gebruikt u de `-IdentityType` en `-IdentityID` overschakelen op de [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) cmdlet. Vervang `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>`, `USER ASSIGNED ID2` door uw eigen waarden.

   [!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```powershell
   Update-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME> -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Een gebruiker toegewezen beheerde identiteit verwijderen uit een schaalset voor virtuele Azure-machine

Als uw virtuele-machineschaalset meerdere gebruiker toegewezen beheerde identiteiten heeft, kunt u alles behalve het laatste item met de volgende opdrachten verwijderen. Vervang de parameterwaarden `<RESOURCE GROUP>` en `<VMSS NAME>` door uw eigen waarden. De `<USER ASSIGNED IDENTITY NAME>` is van de gebruiker toegewezen beheerde identiteit van de naam van eigenschap, die op de virtuele-machineschaalset moet blijven. Deze informatie kan worden gevonden in de sectie van de identiteit van de VM-schaalset met behulp van `az vmss show`:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType UserAssigned -IdentityID "<USER ASSIGNED IDENTITY NAME>"
```
Als uw virtuele-machineschaalset beschikt niet over een systeem toegewezen beheerd wilt identiteits- en u alle beheerde identiteiten die door de gebruiker toegewezen van het apparaat verwijderen, gebruikt u de volgende opdracht uit:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```
Als uw virtuele-machineschaalset beide door het systeem is toegewezen en gebruiker toegewezen beheerde identiteiten, kunt u alle de gebruiker toegewezen beheerde identiteiten verwijderen door over te schakelen voor het gebruik van alleen het systeem toegewezen beheerde identiteit.

```powershell 
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Volgende stappen

- [Beheerde identiteiten voor een overzicht van Azure-resources](overview.md)
- Zie voor het volledige Azure-VM maken snelstartgidsen:
  
  - [Een Windows-machine maken met PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Een virtuele Linux-machine maken met PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 

















