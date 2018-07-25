---
title: MSI configureren op een Azure-VM met behulp van PowerShell
description: Voor stap door stap instructies voor het configureren van een Managed Service Identity (MSI) op een Azure-VM, met behulp van PowerShell.
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
ms.openlocfilehash: 9a40ad66f104a33230484f24e20a5f3bd9ed6175
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39237652"
---
# <a name="configure-a-vm-managed-service-identity-msi-using-powershell"></a>Configureren van een virtuele machine Managed Service Identity (MSI) met behulp van PowerShell

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

<a name="in-this-article-you-learn-how-to-perform-the-following-managed-service-identity-operations-on-an-azure-vm-using-powershell"></a>In dit artikel leert u hoe u de volgende bewerkingen van de beheerde Service-identiteit op een Azure-VM, met behulp van PowerShell uit te voeren:
- 

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md). **Lees de [verschil tussen een systeem toegewezen en een gebruiker toegewezen identiteit](overview.md#how-does-it-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende roltoewijzingen:
    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) toegewezen aan een virtuele machine maken en inschakelen en verwijderen van systeem en/of de gebruiker beheerde identiteit van een Azure-VM.
    - [Beheerde identiteit Inzender](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rol te maken van de identiteit van een gebruiker toegewezen.
    - [Beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol die u wilt toewijzen en verwijderen van een gebruiker toegewezen identiteit van en naar een virtuele machine.
- Installeer [de meest recente versie van Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) als u dat nog niet gedaan hebt.

## <a name="system-assigned-identity"></a>Systeem toegewezen identiteit

In deze sectie leert u hoe u kunt in- en uitschakelen van het systeem toegewezen identiteit met Azure PowerShell.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Systeem toegewezen identiteit tijdens het maken van een virtuele Azure-machine inschakelen

Voor het maken van een Azure-VM met het systeem toegewezen identiteit ingeschakeld:

1. Verwijzen naar een van de volgende snelstartgidsen voor Azure VM, alleen de benodigde secties voltooid ('Meld u aan bij Azure', 'Resourcegroep maken', 'Netwerken groep maken', 'Create VM').
    
    Wanneer u naar de sectie 'Maken van de virtuele machine', een kleine wijziging aanbrengt in de [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvm) cmdlet-syntaxis. Zorg ervoor dat u toevoegt een `-AssignIdentity:$SystemAssigned` parameter voor het inrichten van de virtuele machine met het systeem toegewezen identiteit ingeschakeld, bijvoorbeeld:
      
    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName myVM -AssignIdentity:$SystemAssigned ...
    ```

   - [Maak een Windows-machine met behulp van PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
   - [Maken van een virtuele Linux-machine met behulp van PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Optioneel) Toevoegen van de MSI VM-extensie via de `-Type` parameter op de [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. U kunt doorgeven "ManagedIdentityExtensionForWindows" of "ManagedIdentityExtensionForLinux", afhankelijk van het type virtuele machine, en geef de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens.

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Systeem toegewezen identiteit van een bestaande VM in Azure inschakelen

Als u nodig hebt om in te schakelen van een systeem toegewezen identiteit op een bestaande virtuele Machine:

1. Aanmelden bij Azure met `Login-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.

   ```powershell
   Login-AzureRmAccount
   ```

2. Eerst ophalen van de eigenschappen van de virtuele machine met behulp van de `Get-AzureRmVM` cmdlet. Als u wilt een systeem toegewezen identiteit inschakelen, gebruikt u de `-AssignIdentity` overschakelen op de [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet:

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
   Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm -AssignIdentity:$SystemAssigned
   ```

3. (Optioneel) Toevoegen van de MSI VM-extensie via de `-Type` parameter op de [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. U kunt doorgeven "ManagedIdentityExtensionForWindows" of "ManagedIdentityExtensionForLinux", afhankelijk van het type virtuele machine, en geef de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens. Geef de juiste `-Location` parameter, die overeenkomt met de locatie van de bestaande virtuele machine:

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```
    > [!NOTE]
    > Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens.

## <a name="disable-the-system-assigned-identity-from-an-azure-vm"></a>Uitschakelen van het systeem toegewezen identiteit van een Azure VM

Als u een virtuele Machine die niet meer nodig is op het systeem toegewezen identiteit maar nog steeds moet gebruiker toegewezen identiteiten hebt, gebruikt u de volgende cmdlet:

1. Aanmelden bij Azure met `Login-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.

   ```powershell
   Login-AzureRmAccount
   ```

2. Ophalen van de eigenschappen van de virtuele machine met behulp van de `Get-AzureRmVM` cmdlet en stel de `-IdentityType` parameter `UserAssigned`:

   ```powershell   
   $vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM    
   Update-AzureRmVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType "UserAssigned"
   ```

Als u een virtuele machine die systeem toegewezen identiteit niet meer nodig hebt en er geen gebruiker toegewezen identiteiten, gebruikt u de volgende opdrachten:

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
Update-AzureRmVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```

Verwijderen van de MSI-VM-extensie, gebruiker de - de naam switch met de [Remove-AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension) cmdlet, op te geven de dezelfde naam die u hebt gebruikt toen u de extensie toegevoegd:

   ```powershell
   Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
   ```

## <a name="user-assigned-identity"></a>Door gebruiker toegewezen identiteit

In deze sectie leert u hoe u toevoegen en verwijderen van een gebruiker toegewezen identiteit van een virtuele machine met behulp van Azure PowerShell.

### <a name="assign-a-user-assigned-identity-to-a-vm-during-creation"></a>Een gebruiker toegewezen identiteit aan een virtuele machine tijdens het maken van toewijzen

De identiteit van een gebruiker toegewezen aan een Azure-VM toewijzen bij het maken van de virtuele machine:

1. Verwijzen naar een van de volgende snelstartgidsen voor Azure VM, alleen de benodigde secties voltooid ('Meld u aan bij Azure', 'Resourcegroep maken', 'Netwerken groep maken', 'Create VM'). 
  
    Wanneer u naar de sectie 'Maken van de virtuele machine', een kleine wijziging aanbrengt in de [ `New-AzureRmVMConfig` ](/powershell/module/azurerm.compute/new-azurermvm) cmdlet-syntaxis. Voeg de `-IdentityType UserAssigned` en `-IdentityID ` parameters voor het inrichten van de virtuele machine met de identiteit van een gebruiker toegewezen.  Vervang `<VM NAME>`,`<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, en `<MSI NAME>` door uw eigen waarden.  Bijvoorbeeld:
    
    ```powershell 
    $vmConfig = New-AzureRmVMConfig -VMName <VM NAME> -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>..."
    ```
    
    - [Maak een Windows-machine met behulp van PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
    - [Maken van een virtuele Linux-machine met behulp van PowerShell](../../virtual-machines/linux/quick-create-powershell.md)

2. (Optioneel) Toevoegen van de MSI VM-extensie via de `-Type` parameter op de [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. U kunt doorgeven "ManagedIdentityExtensionForWindows" of "ManagedIdentityExtensionForLinux", afhankelijk van het type virtuele machine, en geef de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens. Geef de juiste `-Location` parameter, die overeenkomt met de locatie van de bestaande virtuele machine:
      > [!NOTE]
    > Deze stap is optioneel als u het eindpunt van de identiteit Azure Instance Metadata Service (IMDS) gebruiken kunt voor het ophalen en tokens.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="assign-a-user-identity-to-an-existing-azure-vm"></a>De identiteit van een gebruiker toewijzen aan een bestaande VM in Azure

Als u wilt toewijzen van een gebruiker toegewezen identiteit aan een bestaande Azure-VM:

1. Aanmelden bij Azure met `Connect-AzureRmAccount`. Gebruik een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.

   ```powershell
   Connect-AzureRmAccount
   ```

2. Maken van een gebruiker toegewezen identiteit via de [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) cmdlet.  Houd er rekening mee de `Id` in de uitvoer omdat u dit in de volgende stap moet.

   > [!IMPORTANT]
   > Het maken van een gebruiker toegewezen identiteiten ondersteunt alleen alfanumerieke tekens en afbreekstreepjes (0-9 of a-z of A-Z of -) tekens. Bovendien moet de naam zijn beperkt tot 24 tekens voor de toewijzing van VM/VMSS goed te laten werken. Controleer later op updates. Zie voor meer informatie [Veelgestelde vragen en bekende problemen](known-issues.md)

   ```powershell
   New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
   ```
3. Ophalen van de eigenschappen van de virtuele machine met behulp van de `Get-AzureRmVM` cmdlet. Als u wilt de identiteit van een gebruiker toegewezen aan de Azure-VM toewijst, gebruikt u de `-IdentityType` en `-IdentityID` overschakelen op de [Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) cmdlet.  De waarde voor de`-IdentityId` parameter wordt de `Id` u in de vorige stap hebt genoteerd.  Vervang `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, en `<USER ASSIGNED IDENTITY NAME>` door uw eigen waarden.

   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -Name <VM NAME>
   Update-AzureRmVM -ResourceGroupName <RESOURCE GROUP> -VM $vm -IdentityType UserAssigned -IdentityID "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESROURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>"
   ```

4. Toevoegen van de MSI VM-extensie via de `-Type` parameter op de [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) cmdlet. U kunt doorgeven "ManagedIdentityExtensionForWindows" of "ManagedIdentityExtensionForLinux", afhankelijk van het type virtuele machine, en geef de naam met behulp van de `-Name` parameter. De `-Settings` parameter geeft u de poort die door het eindpunt van de OAuth-token gebruikt voor het ophalen van tokens. Geef de juiste `-Location` parameter, die overeenkomt met de locatie van de bestaande virtuele machine.

   ```powershell
   $settings = @{ "port" = 50342 }
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Een beheerde identiteit van een Azure VM toegewezen gebruiker verwijderen

Als uw virtuele machine meerdere gebruiker toegewezen identiteiten heeft, kunt u alles behalve het laatste item met de volgende opdrachten verwijderen. Vervang de parameterwaarden `<RESOURCE GROUP>` en `<VM NAME>` door uw eigen waarden. De `<MSI NAME>` is de eigenschap name van de identiteit van de gebruiker die is toegewezen, die op de virtuele machine moet blijven. Deze informatie kan worden gevonden in de sectie van de identiteit van het gebruik van de virtuele machine `az vm show`:

```powershell
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzureRmVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType UserAssigned -IdentityID "<MSI NAME>"
```
Als uw virtuele machine heeft geen een systeem toegewezen identiteit en u wilt verwijderen van alle gebruiker toegewezen identiteiten uit, gebruikt u de volgende opdracht uit:

```powershell
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzureRmVm -ResourceGroupName myResourceGroup -VM $vm -IdentityType None
```
Als uw virtuele machine is zowel de systeem toegewezen als de gebruiker toegewezen identiteiten, kunt u alle gebruiker toegewezen identiteiten door over te schakelen voor het gebruik van alleen systeem toegewezen verwijderen.

```powershell 
$vm = Get-AzureRmVm -ResourceGroupName myResourceGroup -Name myVm
Update-AzureRmVm -ResourceGroupName myResourceGroup -VirtualMachine $vm -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>Gerelateerde inhoud

- [Overzicht van de beheerde Service-identiteit](overview.md)
- Zie voor het volledige Azure-VM maken snelstartgidsen:
  
  - [Een Windows-machine maken met PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Een virtuele Linux-machine maken met PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 
