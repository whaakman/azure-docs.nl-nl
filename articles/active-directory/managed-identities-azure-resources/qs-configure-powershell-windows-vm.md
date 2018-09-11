---
title: Het configureren van beheerde identiteiten voor Azure-resources op een Azure-VM met behulp van PowerShell
description: Stapsgewijze beheerde instructies voor het configureren van identiteiten voor een Azure-resources op een Azure-VM met behulp van PowerShell.
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
ms.openlocfilehash: 347d4132756f7570865ab5bfb98cc959b7fa1f81
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44349072"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-powershell"></a>Configureren van beheerde identiteiten voor Azure-resources op een Azure-VM met behulp van PowerShell

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel, leert met behulp van PowerShell, u hoe u de volgende beheerde identiteiten voor bewerkingen van de Azure-resources op een Azure-VM uitvoeren:

## <a name="prerequisites"></a>Vereiste onderdelen

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md). **Lees de [verschil tussen een beheerde identiteit door het systeem is toegewezen en de gebruiker toegewezen](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende Azure op basis van rollen access control-toewijzingen:
    > [!NOTE]
    > Er zijn geen extra Azure AD directory-roltoewijzingen is vereist.
    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) aan een virtuele machine maken en inschakelen en het systeem en/of de gebruiker toegewezen beheerde identiteit verwijderen uit een Azure-VM.
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol te maken van een gebruiker toegewezen beheerde identiteit.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol die u wilt toewijzen en verwijderen van een gebruiker toegewezen beheerde identiteit van en naar een virtuele machine.
- Installeer [de meest recente versie van Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) als u dat nog niet gedaan hebt.

## <a name="system-assigned-managed-identity"></a>Het systeem toegewezen beheerde identiteit

In deze sectie leert u hoe u kunt in- en uitschakelen van de door het systeem toegewezen beheerde identiteit met Azure PowerShell.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm"></a>Het systeem toegewezen beheerde identiteit inschakelen tijdens het maken van een Azure VM

Een Azure-VM maken met het systeem toegewezen beheerde identiteit ingeschakeld:

1. Verwijzen naar een van de volgende snelstartgidsen voor Azure VM, alleen de benodigde secties voltooid ('Meld u aan bij Azure', 'Resourcegroep maken', 'Netwerken groep maken', 'Create VM').
    
    Wanneer u naar de sectie 'Maken van de virtuele machine', een kleine wijziging aanbrengt in de [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvm) cmdlet-syntaxis. Zorg ervoor dat u toevoegt een `-AssignIdentity:$SystemAssigned` parameter voor het inrichten van de virtuele machine met de door het systeem toegewezen identiteit die wordt ingeschakeld, bijvoorbeeld:
      
    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName myVM -AssignIdentity:$SystemAssigned ...
    ```

   - [Maak een Windows-machine met behulp van PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
   - [Maken van een virtuele Linux-machine met behulp van PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Optioneel) Toevoegen van de beheerde identiteiten voor het gebruik van Azure-resources VM extension (gepland voor de afschaffing in januari 2019) de `-Type` parameter op de [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. U kunt doorgeven "ManagedIdentityExtensionForWindows" of "ManagedIdentityExtensionForLinux", afhankelijk van het type virtuele machine, en geef de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens. De beheerde identiteiten voor VM-extensie voor Azure-resources is gepland voor de afschaffing in januari 2019. 

### <a name="enable-system-assigned-managed-identity-on-an-existing-azure-vm"></a>Het systeem toegewezen beheerde identiteit van een bestaande VM in Azure inschakelen

Als u nodig hebt om in te schakelen van een systeem toegewezen beheerde identiteit op een bestaande virtuele Machine:

1. Aanmelden bij Azure met `Login-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.

   ```powershell
   Login-AzureRmAccount
   ```

2. Eerst ophalen van de eigenschappen van de virtuele machine met behulp van de `Get-AzureRmVM` cmdlet. Gebruik vervolgens om in te schakelen op een beheerde identiteit voor het systeem is toegewezen, de `-AssignIdentity` overschakelen op de [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm -AssignIdentity:$SystemAssigned
   ```

3. (Optioneel) Toevoegen van de beheerde identiteiten voor het gebruik van Azure-resources VM extension (gepland voor de afschaffing in januari 2019) de `-Type` parameter op de [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. U kunt doorgeven "ManagedIdentityExtensionForWindows" of "ManagedIdentityExtensionForLinux", afhankelijk van het type virtuele machine, en geef de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens. Geef de juiste `-Location` parameter, die overeenkomt met de locatie van de bestaande virtuele machine:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens.

## <a name="disable-system-assigned-managed-identity-from-an-azure-vm"></a>Uitschakelen door het systeem toegewezen identiteit van een Azure VM beheerd

Als u een virtuele Machine die niet meer nodig is op het systeem toegewezen beheerde identiteit maar nog steeds moet gebruiker toegewezen identiteiten op beheerde hebt, gebruikt u de volgende cmdlet:

1. Aanmelden bij Azure met `Login-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.

   ```powershell
   Login-AzureRmAccount
   ```

2. Ophalen van de eigenschappen van de virtuele machine met behulp van de `Get-AzureRmVM` cmdlet en stel de `-IdentityType` parameter `UserAssigned`:

   ```powershell   
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM    
   Update-AzureRmVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType "UserAssigned"
   ```

Als u een virtuele machine die niet langer nodig door het systeem toegewezen beheerde identiteit heeft en er geen beheerde gebruiker toegewezen identiteiten, gebruikt u de volgende opdrachten:

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzureRmVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```

Verwijderen van de beheerde identiteiten voor VM-extensie voor Azure-resources, gebruiker de - de naam switch met de [Remove-AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension) cmdlet, op te geven de dezelfde naam die u hebt gebruikt toen u de extensie toegevoegd:

   ```powershell
   Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="user-assigned-managed-identity"></a>de gebruiker toegewezen beheerde identiteit

In deze sectie leert u hoe u kunt toevoegen en verwijderen van een gebruiker toegewezen beheerde identiteit van een virtuele machine met behulp van Azure PowerShell.

### <a name="assign-a-user-assigned-managed-identity-to-a-vm-during-creation"></a>Een gebruiker toegewezen beheerde identiteit toewijzen aan een virtuele machine tijdens het maken van

Een beheerde identiteit voor de gebruiker toegewezen aan een Azure-VM toewijzen bij het maken van de virtuele machine:

1. Verwijzen naar een van de volgende snelstartgidsen voor Azure VM, alleen de benodigde secties voltooid ('Meld u aan bij Azure', 'Resourcegroep maken', 'Netwerken groep maken', 'Create VM'). 
  
    Wanneer u naar de sectie 'Maken van de virtuele machine', een kleine wijziging aanbrengt in de [ `New-AzureRmVMConfig` ](/powershell/module/azurerm.compute/new-azurermvm) cmdlet-syntaxis. Voeg de `-IdentityType UserAssigned` en `-IdentityID ` parameters voor het inrichten van de virtuele machine met de identiteit van een gebruiker toegewezen.  Vervang `<VM NAME>`,`<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, en `<USER ASSIGNED IDENTITY NAME>` door uw eigen waarden.  Bijvoorbeeld:
    
    ```powershell 
    $vmConfig = New-AzureRmVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>..."
    ```
    
    - [Maak een Windows-machine met behulp van PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
    - [Maken van een virtuele Linux-machine met behulp van PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Optioneel) Toevoegen van de beheerde identiteit voor het gebruik van Azure-resources VM extension de `-Type` parameter op de [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. U kunt doorgeven "ManagedIdentityExtensionForWindows" of "ManagedIdentityExtensionForLinux", afhankelijk van het type virtuele machine, en geef de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens. Geef de juiste `-Location` parameter, die overeenkomt met de locatie van de bestaande virtuele machine:
      > [!NOTE]
    > Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens. De beheerde identiteiten voor VM-extensie voor Azure-resources is gepland voor de afschaffing in januari 2019.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-vm"></a>Een gebruiker toegewezen beheerde identiteit toewijzen aan een bestaande VM in Azure

Een gebruiker toegewezen beheerde identiteit toewijzen aan een bestaande VM in Azure:

1. Aanmelden bij Azure met `Connect-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.

   ```powershell
   Connect-AzureRmAccount
   ```

2. Maak een beheerde identiteit gebruiker toegewezen met de [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) cmdlet.  Houd er rekening mee de `Id` in de uitvoer omdat u dit in de volgende stap moet.

   > [!IMPORTANT]
   > Het maken van beheerde identiteiten gebruiker toegewezen ondersteunt alleen alfanumerieke tekens en afbreekstreepjes (0-9 of a-z of A-Z of -) tekens. Bovendien moet de naam zijn beperkt tot 24 tekens voor de toewijzing van VM/VMSS goed te laten werken. Controleer later op updates. Zie voor meer informatie [Veelgestelde vragen en bekende problemen](known-issues.md)

   ```powershell
   New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
   ```
3. Ophalen van de eigenschappen van de virtuele machine met behulp van de `Get-AzureRmVM` cmdlet. Als u wilt een beheerde identiteit voor de gebruiker toegewezen aan de Azure-VM toewijst, gebruikt u de `-IdentityType` en `-IdentityID` overschakelen op de [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet.  De waarde voor de`-IdentityId` parameter wordt de `Id` u in de vorige stap hebt genoteerd.  Vervang `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, en `<USER ASSIGNED IDENTITY NAME>` door uw eigen waarden.

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
   Update-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
   ```

4. Toevoegen van de beheerde identiteit voor het gebruik van Azure-resources VM extension (gepland voor de afschaffing in januari 2019) de `-Type` parameter op de [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. U kunt doorgeven "ManagedIdentityExtensionForWindows" of "ManagedIdentityExtensionForLinux", afhankelijk van het type virtuele machine, en geef de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens. Geef de juiste `-Location` parameter, die overeenkomt met de locatie van de bestaande virtuele machine.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Verwijderen van een gebruiker toegewezen beheerde identiteit van een Azure VM

Als uw virtuele machine meerdere beheerde identiteiten gebruiker toegewezen, kunt u alles behalve het laatste item met de volgende opdrachten verwijderen. Vervang de parameterwaarden `<RESOURCE GROUP>` en `<VM NAME>` door uw eigen waarden. De `<USER ASSIGNED IDENTITY NAME>` is van de gebruiker toegewezen beheerde identiteit van de naam van eigenschap, die op de virtuele machine moet blijven. Deze informatie kan worden gevonden in de sectie van de identiteit van het gebruik van de virtuele machine `az vm show`:

```powershell
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzureRmVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType UserAssigned -IdentityID <USER ASSIGNED IDENTITY NAME>
```
Als uw virtuele machine beschikt niet over een systeem toegewezen beheerd wilt identiteits- en u alle beheerde identiteiten die door de gebruiker toegewezen van het apparaat verwijderen, gebruikt u de volgende opdracht uit:

```powershell
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzureRmVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```
Als uw virtuele machine zowel door het systeem toegewezen als de gebruiker toegewezen beheerde identiteiten is, kunt u alle de gebruiker toegewezen beheerde identiteiten verwijderen door over te schakelen voor het gebruik van beheerde identiteiten alleen door het systeem is toegewezen.

```powershell 
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzureRmVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Volgende stappen

- [Beheerde identiteiten voor een overzicht van Azure-resources](overview.md)
- Zie voor het volledige Azure-VM maken snelstartgidsen:
  
  - [Een Windows-machine maken met PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Een virtuele Linux-machine maken met PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 
